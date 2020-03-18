---
title: 绑定疑难解答
description: 本文汇总了生成绑定时可能会出现的一些常见错误和可能的原因以及建议的解决方法。
ms.prod: xamarin
ms.assetid: BB81FCCF-F7BF-4C78-884E-F02C49AA819A
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/01/2018
ms.openlocfilehash: 0c273797d7512f062260e49e0f71fdd1132f037b
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/10/2020
ms.locfileid: "76723801"
---
# <a name="troubleshooting-bindings"></a>绑定疑难解答

_本文汇总了生成绑定时可能会出现的一些常见错误和可能的原因以及建议的解决方法。_

## <a name="overview"></a>概述

多数情况下，绑定 Android 库（.aar 或 .jar）文件都绝非易事；通常它需要花费额外的精力来缓解 Java 和 .NET 之间的差异导致的问题。  
这些问题会使 Xamarin.Android 无法绑定 Android 库，并在生成日志中显示为错误消息。 此指南将提供解决这些问题的一些技巧，列出一些较为常见的问题/场景，并提供成功绑定 Android 库的可能的解决方案。

绑定现有 Android 库时，需要注意以下几点：

- **此库的外部依赖项** &ndash; Android 库所需的任何 Java 依赖项都必须作为 ReferenceJar 或 EmbeddedReferenceJar  包含在 Xamarin.Android 项目中。 

- **Android 库的目标 Android API 级别** &ndash; 不能对 Android API 级别“降级”；确保 Xamarin.Android 绑定项目的目标 API 级别与 Android 库相同（或处于更高级别）。

- **用于打包 Android 库的 Android JDK 的版本** &ndash; 若生成 Android 库的 JDK 版本不同于 Xamarin.Android 所使用的版本，可能会出现绑定错误。 如有可能，请使用安装 Xamarin.Android 所用的同一版本的 JDK 重新编译 Android 库。

解决 Xamarin.Android 库绑定问题的第一步是启用[诊断 MSBuild 输出](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output)。
启用此诊断输出后，重新生成 Xamarin.Android 绑定项目，并查看生成日志，以找到相关线索确定问题起因。

此外，以下做法也有帮助：反向编译 Android 库，然后查看 Xamarin.Android 要绑定的类型和方法。 本指南后面的部分将更详细地介绍这一做法。

## <a name="decompiling-an-android-library"></a>反向编译 Android 库

检查 Java 类的类和方法可提供有助于绑定库的有用信息。
[JD-GUI](http://jd.benow.ca/) 是一个图形实用程序，它可以显示 JAR 中包含的类文件的 Java 源代码。  它可作为独立应用程序或者 IntelliJ 和 Eclipse 的插件运行。

使用 Java 反向编译器打开 .JAR 文件，以反向编译 Android 库。  若此库是 .AAR 文件，需要从存档文件中提取 classes.jar 文件。   下面是使用 JD-GUI 分析 [Picasso](https://square.github.io/picasso/) JAR 的示例屏幕截图：

![使用 Java 反向编译程序分析 picasso-2.5.2.jar](troubleshooting-bindings-images/troubleshoot-bindings-01.png)

反向编译 Android 库后，查看源代码。 通常情况下，查找以下内容：

- **具有模糊处理特性的类** &ndash; 模糊处理的类的特性包括：

  - 类名包括 $  ，即 a$.class 
  - 类名完全由小写字母组成，即 a.class       

- **未引用的库的 `import` 语句** &ndash; 确定未引用的库，并使用 ReferenceJar 或 EmbedddedReferenceJar 的生成操作将这些依赖项添加到 Xamarin.Android 绑定项目。   

> [!NOTE]
> 根据当地法规或发布 Java 库所依托的许可证，反向编译 Java 库可能会被禁止或者受到法律限制。 必要时，先向法律从业者寻求服务，然后再反向编译 Java 库并查看源代码。

## <a name="inspect-apixml"></a>查看 API.XML

在生成绑定项目的过程中，Xamarin.Android 将生成 XML 文件名 obj/Debug/api.xml： 

![在 obj/Debug 下生成的 api.xml](troubleshooting-bindings-images/troubleshoot-bindings-02.png)

此文件提供 Xamarin.Android 要绑定的所有 Java API 的列表。 此文件中的内容有助于确定缺失的类型或方法，以及重复的绑定。 查看此文件的过程乏味并且耗时，但可以提供相关线索，从而确定导致绑定问题的可能原因。 例如，api.xml 可能会显示某个属性返回的类型不正确，或者两个类型的托管名称相同。 

## <a name="known-issues"></a>已知问题

此部分将列出绑定 Android 库时可能会出现的一些常见错误消息或症状。

### <a name="problem-java-version-mismatch"></a>问题：Java 版本不匹配

有时，由于你所使用的 Java 版本高于或低于编译库的版本，可能会无法生成类型，或者可能会出现意外的故障。 请使用 Xamarin.Android 所使用的同一个 JDK 版本重新编译 Android 库。

### <a name="problem-at-least-one-java-library-is-required"></a>问题：至少需要一个 Java 库

已添加 .JAR，但仍收到错误“至少需要一个 Java 库”。

#### <a name="possible-causes"></a>可能的原因：

确保将生成操作设置为 `EmbeddedJar`。 有多个针对 .JAR 文件的生成操作（如 `InputJar`、`EmbeddedJar`、`ReferenceJar` 和 `EmbeddedReferenceJar`），因此默认情况下绑定生成器无法自动推测要使用哪一个。 有关生成操作的详细信息，请参阅[生成操作](~/android/platform/binding-java-library/index.md)。

### <a name="problem-binding-tools-cannot-load-the-jar-library"></a>问题：绑定工具无法加载 .JAR 库

绑定库生成器无法加载 .JAR 库。

#### <a name="possible-causes"></a>可能的原因

Java 工具无法加载使用代码模糊处理的一些 .JAR 库（通过 Proguard 等工具）。 我们的工具使用 Java 反射和 ASM 字节代码工程库，因此这些依赖工具可能会拒绝模糊处理的库，而 Android 运行时工具可能会批准它们。 此问题的解决方法是手动绑定这些库，而不使用绑定生成器。

### <a name="problem-missing-c-types-in-generated-output"></a>问题：生成的输出中缺少 C# 类型。

绑定 .dll 生成 Java 类型但缺少一些，或者提示类型缺失的错误导致生成的 C# 源无法构建。 

#### <a name="possible-causes"></a>可能的原因：

此错误可能由多种原因导致，如下所示：

- 要绑定的库可能引用了第二个 Java 库。 若绑定的库的公共 API 使用第二个库中的类型，必须也引用第二个库的托管绑定。

- 可能是 Java 反射导致注入了库，原因类似于上面的库加载错误，造成意外加载了元数据。 Xamarin.Android 的工具目前无法解决此状况。 在此情况下，必须手动绑定该库。

- .NET 4.0 运行时出现了 bug，在应加载程序集时未能加载。 .NET 4.5 运行时中已修复此问题。

- Java 允许从非公共类派生公共类，但 .NET 中不支持此操作。 绑定生成器无法从非公共类生成绑定，因此无法正确生成诸如这些的派生类。 若要修复此问题，请使用 Metadata.xml 中的删除节点删除这些派生类的元数据项，或修复使非公共类成为公共类的元数据。  后面的解决方案会创建绑定从而生成 C# 源，但不应使用非公共类。

  例如：

  ```xml
  <attr path="/api/package[@name='com.some.package']/class[@name='SomeClass']"
      name="visibility">public</attr>
  ```

- 模糊处理 Java 库的工具可能会影响 Xamarin.Android 绑定生成器及其生成 C# 包装类的功能。 下面的代码片段显示如何更新 Metadata.xml 以取消模糊处理类名称： 

  ```xml
  <attr path="/api/package[@name='{package_name}']/class[@name='{name}']"
      name="obfuscated">false</attr>
  ```

### <a name="problem-generated-c-source-does-not-build-due-to-parameter-type-mismatch"></a>问题：参数类型不匹配导致生成的 C# 源无法生成

生成的 C# 源无法生成。 重写的方法的参数类型不匹配。

#### <a name="possible-causes"></a>可能的原因：

Xamarin.Android 包括各种映射到 C# 绑定中枚举的 Java 字段。 这些可能会导致生成的绑定中的类型不兼容。 若要解决此问题，必须修改绑定生成器创建的方法签名以使用枚举。 有关详细信息，请参阅[更正枚举](~/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata.md)。

### <a name="problem-noclassdeffounderror-in-packaging"></a>问题：打包过程中出现 NoClassDefFoundError

在打包步骤中引发了 `java.lang.NoClassDefFoundError`。

#### <a name="possible-causes"></a>可能的原因：

出现此错误时，可能性最大的原因是必需的 Java 库需要添加到应用程序项目 (.csproj  )。 .JAR 文件无法自动解析。 并非始终会为目标设备或模拟器中不存在的用户程序集（如 Google Maps maps.jar）生成 Java 库绑定。  Android 库项目支持不属于这种情况，因为库 .JAR 嵌入到库 dll。 例如：[Bug 4288](https://bugzilla.xamarin.com/show_bug.cgi?id=4288)

### <a name="problem-duplicate-custom-eventargs-types"></a>问题：自定义 EventArgs 类型重复

自定义 EventArgs 类型重复导致生成失败。 出现如下错误：

```shell
error CS0102: The type `Com.Google.Ads.Mediation.DismissScreenEventArgs' already contains a definition for `p0'
```

#### <a name="possible-causes"></a>可能的原因：

这是因为，如果事件类型来自多个共享相同名称的方法的接口“侦听器”类型，它们之间会存在一些冲突。 例如，当存在以下示例所示的两个 Java 接口时，生成器同时为 `MediationBannerListener` 和 `MediationInterstitialListener` 生成 `DismissScreenEventArgs`，导致错误。

```java
// Java:
public interface MediationBannerListener {
    void onDismissScreen(MediationBannerAdapter p0);
}
public interface MediationInterstitialListener {
    void onDismissScreen(MediationInterstitialAdapter p0);
}
```

这是有意的设计，以避免事件参数类型的名称冗长。 要避免这些冲突，必须完成一些数据转换。 编辑 Transforms\Metadata.xml，并在其中的一个接口（或接口方法）中添加 `argsType` 属性： 

```xml
<attr path="/api/package[@name='com.google.ads.mediation']/
        interface[@name='MediationBannerListener']/method[@name='onDismissScreen']"
        name="argsType">BannerDismissScreenEventArgs</attr>

<attr path="/api/package[@name='com.google.ads.mediation']/
        interface[@name='MediationInterstitialListener']/method[@name='onDismissScreen']"
        name="argsType">IntersitionalDismissScreenEventArgs</attr>

<attr path="/api/package[@name='android.content']/
        interface[@name='DialogInterface.OnClickListener']"
        name="argsType">DialogClickEventArgs</attr>
```

### <a name="problem-class-does-not-implement-interface-method"></a>问题：类无法实现接口方法

生成错误消息，提示生成的类无法实现生成的类实现的接口需要的方法。 但是查看生成的代码时，可以看到实现了该方法。

下面是此错误的示例：

```shell
obj\Debug\generated\src\Oauth.Signpost.Basic.HttpURLConnectionRequestAdapter.cs(8,23):
error CS0738: 'Oauth.Signpost.Basic.HttpURLConnectionRequestAdapter' does not
implement interface member 'Oauth.Signpost.Http.IHttpRequest.Unwrap()'.
'Oauth.Signpost.Basic.HttpURLConnectionRequestAdapter.Unwrap()' cannot implement
'Oauth.Signpost.Http.IHttpRequest.Unwrap()' because it does not have the matching
return type of 'Java.Lang.Object'
```

#### <a name="possible-causes"></a>可能的原因：

这是绑定具有协变返回类型的 Java 方法时会发生的问题。 在此示例中，方法 `Oauth.Signpost.Http.IHttpRequest.UnWrap()` 需要返回 `Java.Lang.Object`。 但是方法 `Oauth.Signpost.Basic.HttpURLConnectionRequestAdapter.UnWrap()` 具有返回类型 `HttpURLConnection`。 可通过两种方法解决此问题：

- 添加 `HttpURLConnectionRequestAdapter` 的分部类声明，并显式实现 `IHttpRequest.Unwrap()`：

  ```csharp
  namespace Oauth.Signpost.Basic {
      partial class HttpURLConnectionRequestAdapter {
          Java.Lang.Object OauthSignpost.Http.IHttpRequest.Unwrap() {
              return Unwrap();
          }
      }
  }
  ```

- 从生成的 C# 代码删除此协变。 这涉及将以下转换添加到 Transforms\Metadata.xml，从而使生成的 C# 代码具有返回类型  `Java.Lang.Object`：

  ```xml
  <attr
      path="/api/package[@name='oauth.signpost.basic']/class[@name='HttpURLConnectionRequestAdapter']/method[@name='unwrap']"
      name="managedReturn">Java.Lang.Object
  </attr>
  ```

### <a name="problem-name-collisions-on-inner-classes--properties"></a>问题：内部类/属性名称冲突

继承对象的可见性冲突。

在 Java 中，派生类与其父类的可见性无需相同。 Java 会修复此问题。 在 C# 中，这一点必须是显式的，因此你需要确保层次结构中的所有类拥有适当的可见性。 下面的示例说明了如何将 Java 程序包名称从 `com.evernote.android.job` 更改为 `Evernote.AndroidJob`：

```xml
<!-- Change the visibility of a class -->
<attr path="/api/package[@name='namespace']/class[@name='ClassName']" name="visibility">public</attr>

<!-- Change the visibility of a method -->
<attr path="/api/package[@name='namespace']/class[@name='ClassName']/method[@name='MethodName']" name="visibility">public</attr>
```

### <a name="problem-a-so-library-required-by-the-binding-is-not-loading"></a>问题：绑定需要的 .so 库无法加载 

一些绑定项目可能还依赖 .so 库中的功能。  可能 Xamarin.Android 无法自动加载 .so 库。  当包装的 Java 代码执行时，Xamarin.Android 将无法进行 JNI 调用，并且应用程序的 logcat 中将出现错误消息“java.lang.UnsatisfiedLinkError:  找不到本机方法:”。

解决此问题的方法是通过调用 `Java.Lang.JavaSystem.LoadLibrary` 手动加载 .so  库。 例如，假设某个 Xamarin.Android 项目的绑定项目中包含共享库 libpocketsphinx_jni.so  ，并且具有 EmbeddedNativeLibrary  的生成操作，那么（在使用共享库之前执行的）以下代码片段将加载 .so  库：

```csharp
Java.Lang.JavaSystem.LoadLibrary("pocketsphinx_jni");
```

## <a name="summary"></a>总结

在本文中，我们列出了与 Java 绑定相关的常见疑难问题，并说明了如何解决它们。

## <a name="related-links"></a>相关链接

- [库项目](https://developer.android.com/tools/projects/index.html#LibraryProjects)
- [使用 JNI](~/android/platform/java-integration/working-with-jni.md)
- [启用诊断输出](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output)
- [面向 Android 开发人员的 Xamarin](~/android/get-started/java-developers.md)
- [JD-GUI](http://jd.benow.ca/)
