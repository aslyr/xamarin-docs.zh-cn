---
title: 使用 Android 清单
ms.prod: xamarin
ms.assetid: CB7CCF60-FEF1-3B28-215F-159391E74347
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/05/2018
ms.openlocfilehash: 1438c012608b367c21ebcc401c058b186b917f53
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/10/2020
ms.locfileid: "73027799"
---
# <a name="working-with-the-android-manifest"></a>使用 Android 清单

Androidmanifest.xml  是 Android 平台上功能非常强大的文件，可便于向 Android 描述应用程序的功能和要求。 然而，使用它并不容易。 通过允许你向类添加自定义特性（这些特性然后会自动为你生成清单），Xamarin.Android 有助于最大限度地降低使用难度。 我们的目标是，99% 的用户永远都不需要手动修改 Androidmanifest.xml  。 

Androidmanifest.xml  是在生成流程中生成，Properties/Androidmanifest.xml  中的 XML 会与通过自定义特性生成的 XML 合并在一起。 生成的合并 Androidmanifest.xml  驻留在 obj  子目录中；例如，对于调试内部版本，它驻留在 obj/Debug/android/Androidmanifest.xml  中。 合并过程很简单：它使用代码中的自定义特性来生成 XML 元素，并将这些元素插入  Androidmanifest.xml  中。 

## <a name="the-basics"></a>基本知识

在编译时，扫描程序集中是否有派生自 [Activity](xref:Android.App.Activity) 且声明了 [`[Activity]`](xref:Android.App.ActivityAttribute) 特性的非 `abstract` 类。 然后，它使用这些类和特性来生成清单。 例如，考虑以下代码： 

```csharp
namespace Demo
{
    public class MyActivity : Activity
    {
    }
}
```

这不会在 AndroidManifest.xml  中生成任何内容。 若要生成 `<activity/>` 元素，需要使用自定义特性 [`[Activity]`](xref:Android.App.Activity) 
： 

```csharp
namespace Demo
{
    [Activity]
    public class MyActivity : Activity
    {
    }
}
```

此示例将下面的 xml 片段添加到 AndroidManifest.xml  中：

```xml
<activity android:name="md5a7a3c803e481ad8926683588c7e9031b.MainActivity" />
```

`[Activity]` 特性对 `abstract` 类型没有任何影响；将忽略 `abstract` 类型。

### <a name="activity-name"></a>活动名称

自 Xamarin.Android 5.1 起，活动的类型名称基于要导出的类型的程序集限定名称的 MD5SUM。 这样，可以从两个不同的程序集提供相同的完全限定名称，而不会出现打包错误。 （在低于 Xamarin.Android 5.1 的版本中，活动的默认类型名称是根据小写的命名空间和类名创建的。） 

若要重写此默认值，并显式指定活动名称，请使用 [`Name`](xref:Android.App.ActivityAttribute.Name) 属性： 

```csharp
[Activity (Name="awesome.demo.activity")]
public class MyActivity : Activity
{
}
```

此示例生成下面的 xml 片段：

```xml
<activity android:name="awesome.demo.activity" />
```

> [!NOTE]
> 只能出于向后兼容性原因，才使用 `Name` 属性，因为此类重命名可能会在运行时减慢类型查找速度。 若有旧代码需要活动的默认类型名称基于小写的命名空间和类名，请参阅 [Android 可调用包装器命名](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/android/xamarin.android_5/xamarin.android_5.1/index.md#Android_Callable_Wrapper_Naming)，以获取关于维持兼容性的提示与技巧。 

### <a name="activity-title-bar"></a>活动标题栏

默认情况下，Android 会运行的应用程序提供标题栏。 本教程使用的值为 [`/manifest/application/activity/@android:label`](https://developer.android.com/guide/topics/manifest/activity-element.html#label)。 在大多数情况下，此值与类名不同。 若要在标题栏上指定应用的标签，请使用 [`Label`](xref:Android.App.ActivityAttribute.Label) 属性。
例如： 

```csharp
[Activity (Label="Awesome Demo App")]
public class MyActivity : Activity
{
}
```

此示例生成下面的 xml 片段：

```xml
<activity android:label="Awesome Demo App" 
          android:name="md5a7a3c803e481ad8926683588c7e9031b.MainActivity" />
```

### <a name="launchable-from-application-chooser"></a>可从应用程序选择器启动

默认情况下，活动不会显示在 Android 的应用程序启动器屏幕中。 这是因为应用程序可能有很多活动，你不希望每个活动都有图标。 若要指定哪个活动应可从应用程序启动器启动，请使用 [`MainLauncher`](xref:Android.App.ActivityAttribute.MainLauncher) 属性。 例如： 

```csharp
[Activity (Label="Awesome Demo App", MainLauncher=true)] 
public class MyActivity : Activity
{
}
```

此示例生成下面的 xml 片段：

```xml
<activity android:label="Awesome Demo App" 
          android:name="md5a7a3c803e481ad8926683588c7e9031b.MainActivity">
  <intent-filter>
    <action android:name="android.intent.action.MAIN" />
    <category android:name="android.intent.category.LAUNCHER" />
  </intent-filter>
</activity>
```

### <a name="activity-icon"></a>活动图标

默认情况下，活动分配有系统提供的默认启动器图标。 若要使用自定义图标，请先将 .png  添加到 Resources/drawable  ，将它的“生成操作”设置为“AndroidResource”  ，然后使用 [`Icon`](xref:Android.App.ActivityAttribute.Icon) 属性指定要使用的图标。 例如： 

```csharp
[Activity (Label="Awesome Demo App", MainLauncher=true, Icon="@drawable/myicon")] 
public class MyActivity : Activity
{
}
```

此示例生成下面的 xml 片段：

```xml
<activity android:icon="@drawable/myicon" android:label="Awesome Demo App" 
          android:name="md5a7a3c803e481ad8926683588c7e9031b.MainActivity">
  <intent-filter>
    <action android:name="android.intent.action.MAIN" />
    <category android:name="android.intent.category.LAUNCHER" />
  </intent-filter>
</activity>
```

### <a name="permissions"></a>权限

如果你将权限添加到 Android 清单（如[向 Android 清单添加权限](https://github.com/xamarin/recipes/tree/master/Recipes/android/general/projects/add_permissions_to_android_manifest)中所述），这些权限记录在 Properties/Androidmanifest.xml  中。 例如，如果你设置 `INTERNET` 权限，下面的元素会添加到 Properties/Androidmanifest.xml  中： 

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

调试内部版本自动设置一些权限（如 `INTERNET` 和 `READ_EXTERNAL_STORAGE`），以简化调试 &ndash; 这些设置只在生成的 obj/Debug/android/AndroidManifest.xml  中设置，不会在“必需权限”  设置中显示为“已启用”。 

例如，如果检查 obj/Debug/android/AndroidManifest.xml  处生成的清单文件，可能会看到以下添加的权限元素： 

```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
```

在（obj/Debug/android/AndroidManifest.xml  处）清单的发布内部版本中，这些权限不会  自动配置。 如果发现切换到发布内部版本导致应用丢失在调试内部版本中可用的权限，请验证是否已在应用的“必需权限”  设置中显式设置此权限（在 Visual Studio for Mac 中，依次转到“内部版本”>“Android 应用程序”  ；在 Visual Studio 中，依次转到“属性”>“Android 清单”  ）。 

## <a name="advanced-features"></a>高级功能

### <a name="intent-actions-and-features"></a>意向操作和功能

使用 Android 清单，可以描述活动的功能。 这是通过 [Intent](https://developer.android.com/guide/topics/manifest/intent-filter-element.html) 和自定义特性 [`[IntentFilter]`](xref:Android.App.IntentFilterAttribute)
完成的。 可以使用 [`IntentFilter`](xref:Android.App.IntentFilterAttribute#ctor*) 构造函数来指定哪些操作适合你的活动，
并使用 [`Categories`](xref:Android.App.IntentFilterAttribute.Categories) 属性来指定哪些类别适合
。 至少必须提供一个活动（正因为此，活动是在构造函数中提供的）。 可以多次提供 `[IntentFilter]`，每次使用都会在 `<activity/>` 中生成单独的 `<intent-filter/>` 元素。 例如：

```csharp
[Activity (Label="Awesome Demo App", MainLauncher=true, Icon="@drawable/myicon")] 
[IntentFilter (new[]{Intent.ActionView}, 
        Categories=new[]{Intent.CategorySampleCode, "my.custom.category"})]
public class MyActivity : Activity
{
}
```

此示例生成下面的 xml 片段：

```xml
<activity android:icon="@drawable/myicon" android:label="Awesome Demo App" 
          android:name="md5a7a3c803e481ad8926683588c7e9031b.MainActivity">
  <intent-filter>
    <action android:name="android.intent.action.MAIN" />
    <category android:name="android.intent.category.LAUNCHER" />
  </intent-filter>
  <intent-filter>
    <action android:name="android.intent.action.VIEW" />
    <category android:name="android.intent.category.SAMPLE_CODE" />
    <category android:name="my.custom.category" />
  </intent-filter>
</activity>
```

### <a name="application-element"></a>Application 元素

使用 Android 清单，还可以为整个应用程序声明属性。 这是通过 `<application>` 元素及其对应项 [Application](xref:Android.App.ApplicationAttribute) 自定义特性完成的。 请注意，这些是应用程序范围内（程序集范围内）设置，而不是每活动设置。 通常是先为整个应用程序声明 `<application>` 属性，再（根据需要）每活动重写这些设置。 

例如，下面的 `Application` 特性添加到 AssemblyInfo.cs  中，以指明应用程序是可调试的、应用程序的用户可读名称是“我的应用”  ，以及应用程序使用 `Theme.Light` 样式作为所有活动的默认主题： 

```csharp
[assembly: Application (Debuggable=true,   
                        Label="My App",   
                        Theme="@android:style/Theme.Light")]
```

此声明在 obj/Debug/android/AndroidManifest.xml  中生成下面的 XML 片段：

```xml
<application android:label="My App" 
             android:debuggable="true" 
             android:theme="@android:style/Theme.Light"
                ... />
```

在此示例中，应用中的所有活动都默认使用 `Theme.Light` 样式。 如果你将某活动的主题设置为 `Theme.Dialog`，只有此活动会使用 `Theme.Dialog` 样式，而应用中的其他所有活动则都会默认使用 `<application>` 元素中设置的 `Theme.Light` 样式。 

`Application` 元素不是配置 `<application>` 特性的唯一方法。 也可以直接将特性插入 Properties/Androidmanifest.xml  的 `<application>` 元素中。 这些设置合并到在 obj/Debug/android/AndroidManifest.xml  处驻留的最终 `<application>` 元素。 请注意，Properties/Androidmanifest.xml  的内容始终重写自定义特性提供的数据。 

可以在 `<application>` 元素中配置的应用程序范围内特性有很多；若要详细了解这些设置，请参阅 [ApplicationAttribute](xref:Android.App.ApplicationAttribute) 的[公共特性](xref:Android.App.ApplicationAttribute)部分。 

## <a name="list-of-custom-attributes"></a>自定义特性列表

- [Android.App.ActivityAttribute](xref:Android.App.ActivityAttribute)：生成 [/manifest/application/activity](https://developer.android.com/guide/topics/manifest/activity-element.html) XML 片段 
- [Android.App.ApplicationAttribute](xref:Android.App.ApplicationAttribute)：生成 [/manifest/application](https://developer.android.com/guide/topics/manifest/application-element.html) XML 片段 
- [Android.App.InstrumentationAttribute](xref:Android.App.InstrumentationAttribute)：生成 [/manifest/instrumentation](https://developer.android.com/guide/topics/manifest/instrumentation-element.html) XML 片段 
- [Android.App.IntentFilterAttribute](xref:Android.App.IntentFilterAttribute)：生成 [//intent-filter](https://developer.android.com/guide/topics/manifest/intent-filter-element.html) XML 片段 
- [Android.App.MetaDataAttribute](xref:Android.App.MetaDataAttribute)：生成 [//meta-data](https://developer.android.com/guide/topics/manifest/meta-data-element.html) XML 片段 
- [Android.App.PermissionAttribute](xref:Android.App.PermissionAttribute)：生成 [//permission](https://developer.android.com/guide/topics/manifest/permission-element.html) XML 片段 
- [Android.App.PermissionGroupAttribute](xref:Android.App.PermissionGroupAttribute)：生成 [//permission-group](https://developer.android.com/guide/topics/manifest/permission-group-element.html) XML 片段 
- [Android.App.PermissionTreeAttribute](xref:Android.App.PermissionTreeAttribute)：生成 [//permission-tree](https://developer.android.com/guide/topics/manifest/permission-tree-element.html) XML 片段 
- [Android.App.ServiceAttribute](xref:Android.App.ServiceAttribute)：生成 [/manifest/application/service](https://developer.android.com/guide/topics/manifest/service-element.html) XML 片段 
- [Android.App.UsesLibraryAttribute](xref:Android.App.UsesLibraryAttribute)：生成 [/manifest/application/uses-library](https://developer.android.com/guide/topics/manifest/uses-library-element.html) XML 片段 
- [Android.App.UsesPermissionAttribute](xref:Android.App.UsesPermissionAttribute)：生成 [/manifest/uses-permission](https://developer.android.com/guide/topics/manifest/uses-permission-element.html) XML 片段 
- [Android.Content.BroadcastReceiverAttribute](xref:Android.Content.BroadcastReceiverAttribute)：生成 [/manifest/application/receiver](https://developer.android.com/guide/topics/manifest/receiver-element.html) XML 片段 
- [Android.Content.ContentProviderAttribute](xref:Android.Content.ContentProviderAttribute)：生成 [/manifest/application/provider](https://developer.android.com/guide/topics/manifest/provider-element.html) XML 片段 
- [Android.Content.GrantUriPermissionAttribute](xref:Android.Content.GrantUriPermissionAttribute)：生成 [/manifest/application/provider/grant-uri-permission](https://developer.android.com/guide/topics/manifest/grant-uri-permission-element.html) XML 片段
