---
title: Xamarin.Android 的 Android 可调用包装器
ms.prod: xamarin
ms.assetid: C33E15FA-1E2B-819A-C656-CA588D611492
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/15/2018
ms.openlocfilehash: ef2f8e0375786ba7b627fdf75545cbb48318c1aa
ms.sourcegitcommit: 854798de42566750d9c70b6d0539b7ee73ff6ddc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/20/2020
ms.locfileid: "81646601"
---
# <a name="android-callable-wrappers-for-xamarinandroid"></a>Xamarin.Android 的 Android 可调用包装器

只要 Android 运行时调用托管代码，就需要 Android 可调用包装器 (ACW)。 这些包装器是必需的，因为在运行时无法向 ART（Android 运行时）注册类。 （具体而言，Android 运行时不支持 [JNI DefineClass() 函数](https://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html#wp15986)。） 因此，Android 可调用包装器弥补了缺少的运行时类型注册支持。 

每当  Android 代码需要执行在托管代码中 `overridden` 或实现的 `virtual` 或接口方法时，Xamarin.Android 都必须提供 Java 代理，以便将此方法调度到相应托管类型。 这些 Java 代理类型是 Java 代码，它们具有与托管类型“相同”的基类和 Java 接口列表，实现相同的构造函数，并声明任何重写的基类和接口方法。 

Android 可调用包装器是由 monodroid.exe  程序在[生成流程](~/android/deploy-test/building-apps/build-process.md)中生成的：它们是为（直接或间接）继承 [Java.Lang.Object](xref:Java.Lang.Object) 的所有类型生成的。 

## <a name="android-callable-wrapper-naming"></a>Android 可调用包装器命名

Android 可调用包装器的包名称基于，要导出的类型的程序集限定名称的 MD5SUM。 借助此命名技术，不同的程序集可以提供相同的完全限定类型名称，而不会引入打包错误。 

鉴于此 MD5SUM 命名方案，不能直接按名称访问类型。 例如，下面的 `adb` 命令无法运行，因为类型名称 `my.ActivityType` 默认不会生成： 

```shell
adb shell am start -n My.Package.Name/my.ActivityType
```

此外，如果尝试按名称引用类型，可能会看到如下错误：

```shell
java.lang.ClassNotFoundException: Didn't find class "com.company.app.MainActivity"
on path: DexPathList[[zip file "/data/app/com.company.App-1.apk"] ...
```

如果确实  需要按名称访问类型，可以在特性声明中声明相应类型的名称。 例如，下面的代码声明使用完全限定名称 `My.ActivityType` 的活动：

```csharp
namespace My {
    [Activity]
    public partial class ActivityType : Activity {
        /* ... */
    }
}
```

可以设置 `ActivityAttribute.Name` 属性来显式声明此活动的名称： 

```csharp
namespace My {
    [Activity(Name="my.ActivityType")]
    public partial class ActivityType : Activity {
        /* ... */
    }
}
```

添加此属性设置后，可以通过外部代码和 `adb` 脚本按名称访问 `my.ActivityType`。 可以为多种不同的类型（包括 `Activity`、`Application`、`Service`、`BroadcastReceiver` 和 `ContentProvider`）设置 `Name` 特性： 

- [ActivityAttribute.Name](xref:Android.App.ActivityAttribute.Name)
- [ApplicationAttribute.Name](xref:Android.App.ApplicationAttribute.Name)
- [ServiceAttribute.Name](xref:Android.App.ServiceAttribute.Name)
- [BroadcastReceiverAttribute.Name](xref:Android.Content.BroadcastReceiverAttribute.Name)
- [ContentProviderAttribute.Name](xref:Android.Content.ContentProviderAttribute.Name)

Xamarin.Android 5.0 中引入了基于 MD5SUM 的 ACW 命名。 若要详细了解特性命名，请参阅 [RegisterAttribute](xref:Android.Runtime.RegisterAttribute)。 

## <a name="implementing-interfaces"></a>实现接口

有时，可能需要实现 Android 接口，如 [Android.Content.IComponentCallbacks](xref:Android.Content.IComponentCallbacks)。 由于所有 Android 类和接口都扩展了 [Android.Runtime.IJavaObject](xref:Android.Runtime.IJavaObject) 接口，因此会出现以下问题：如何实现 `IJavaObject`？ 

上文已经回答了这个问题：所有 Android 类型都需要实现 `IJavaObject` 的原因是，这样 Xamarin.Android 就可以有提供给 Android 的 Android 可调用包装器，即给定类型的 Java 代理。 由于 monodroid.exe 仅查找 `Java.Lang.Object` 子类，且 `Java.Lang.Object` 实现 `IJavaObject`，因此答案很明显，就是子类 `Java.Lang.Object` ： 

```csharp
class MyComponentCallbacks : Java.Lang.Object, Android.Content.IComponentCallbacks {

    public void OnConfigurationChanged (Android.Content.Res.Configuration newConfig)
    {
        // implementation goes here...
    } 

    public void OnLowMemory ()
    {
        // implementation goes here...
    }
}
```

## <a name="implementation-details"></a>实现详细信息

本页的其余部分提供了实现细节，如有更改，恕不另行通知  （仅在此处显示是因为开发人员会对所发生的情况感到好奇）。 

例如，假设为以下 C# 源：

```csharp
using System;
using Android.App;
using Android.OS;

namespace Mono.Samples.HelloWorld
{
    public class HelloAndroid : Activity
    {
        protected override void OnCreate (Bundle savedInstanceState)
        {
            base.OnCreate (savedInstanceState);
            SetContentView (R.layout.main);
        }
    }
}
```

mandroid.exe  程序会生成以下 Android 可调用包装器： 

```java
package mono.samples.helloWorld;

public class HelloAndroid
    extends android.app.Activity
{
    static final String __md_methods;
    static {
        __md_methods = "n_onCreate:(Landroid/os/Bundle;)V:GetOnCreate_Landroid_os_Bundle_Handler\n" + "";
        mono.android.Runtime.register (
            "Mono.Samples.HelloWorld.HelloAndroid, HelloWorld, Version=1.0.0.0, 
            Culture=neutral, PublicKeyToken=null", HelloAndroid.class, __md_methods);
    }

    public HelloAndroid ()
    {
        super ();
        if (getClass () == HelloAndroid.class)
            mono.android.TypeManager.Activate (
                "Mono.Samples.HelloWorld.HelloAndroid, HelloWorld, Version=1.0.0.0, 
                Culture=neutral, PublicKeyToken=null", "", this, new java.lang.Object[] {  });
    }

    @Override
    public void onCreate (android.os.Bundle p0)
    {
        n_onCreate (p0);
    }

    private native void n_onCreate (android.os.Bundle p0);
}
```

请注意，不仅会暂留基类，还会为在托管代码中重写的每个方法提供 `native` 方法声明。 
