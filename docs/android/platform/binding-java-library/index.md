---
title: 绑定 Java 库
description: Android 社区包含很多可供你在应用中使用的 Java 库；本指南介绍如何通过创建绑定库将 Java 库合并到 Xamarin Android 应用程序中。
ms.prod: xamarin
ms.assetid: B39FF1D5-69C3-8A76-D268-C227A23C9485
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 05/01/2017
ms.openlocfilehash: d40a23076ec8f405e57ec40de47ec9ad2261d85d
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/13/2020
ms.locfileid: "73027609"
---
# <a name="binding-a-java-library"></a>绑定 Java 库

_Android 社区包含很多可供你在应用中使用的 Java 库；本指南介绍如何通过创建绑定库将 Java 库合并到 Xamarin Android 应用程序中。_

## <a name="overview"></a>概述

Android 的第三方库生态系统非常庞大。 正因为此，使用现有的 Android 库通常比创建一个新的库更合理。 Xamarin.Android 提供两种方法来使用这些库：

- 创建一个绑定库，该库使用 C# 包装器自动包装库，以便通过 C# 调用来调用 Java 代码  。

- 使用 Java 本机接口 (JNI) 直接调用 Java 库代码中的调用   。 JNI 是一个编程框架，它使 Java 代码能够调用本机应用程序或库并反过来由其调用。

本指南介绍第一种方法：  如何创建可将一个或多个现有 Java 库包装到可在应用程序中链接到的程序集中的绑定库。 有关使用 JNI 的详细信息，请参阅[使用 JNI](~/android/platform/java-integration/working-with-jni.md)。

Xamarin.Android 通过使用托管可调用包装器  (MCW  ) 实现绑定。 MCW 是一个 JNI 桥，在托管代码需要调用 Java 代码时会使用它。 托管可调用包装器还支持对 Java 类型进行子类化以及覆盖 Java 类型的虚拟方法。 同样，每当 Android 运行时 (ART) 代码需要调用托管代码时，它都会通过另一个称为 Android 可调用包装器 (ACW) 的 JNI 桥来实现。 下图说明了此[体系结构](~/android/internals/architecture.md)：

[![Android JNI 桥体系结构](images/architecture.png)](images/architecture.png#lightbox)

绑定库是一个程序集，其中包含 Java 类型的托管可调用包装器。 例如，下面是我们要在绑定库中包装的 Java 类型 `MyClass`：

```java
package com.xamarin.mycode;

public class MyClass
{
    public String myMethod (int i) { ... }
}
```

为包含 `MyClass`的 .jar  生成绑定库之后，可以对其进行实例化并通过 C# 对其调用方法：

```csharp
var instance = new MyClass ();

string result = instance.MyMethod (42);
```

若要创建此绑定库，请使用 Xamarin.Android Java 绑定库  模板。 生成的绑定项目将创建一个包含 MCW 类的 .NET 程序集、一个或多个 .jar 文件以及嵌入其中的 Android 库项目的资源  。 你还可以为 Android 存档 (.AAR) 文件和 Eclipse Android 库项目创建绑定库。 通过引用生成的绑定库 DLL 程序集，可以在 Xamarin.Android 项目中重用现有的 Java 库。

在绑定库中引用类型时，必须使用绑定库的命名空间。 通常，会在作为 Java 包名称的 .NET 命名空间版本的 C# 源文件的顶部添加一个 `using` 指令。 例如，假设绑定的 .jar  的 Java 包名称如下：

```csharp
com.company.package
```

然后，将以下 `using` 语句放在 C# 源文件的顶部，以访问绑定的 .jar  文件中的类型：

```csharp
using Com.Company.Package;
```

绑定现有 Android 库时，需要注意以下几点：

- **库是否有任何外部依赖项？** &ndash; Android 库所需的任何 Java 依赖项都必须作为 ReferenceJar  或 EmbeddedReferenceJar  包含在 Xamarin.Android 项目中。 任何本机程序集都必须作为 EmbeddedNativeLibrary  添加到绑定项目中。  

- **Android 库面向哪个版本的 Android API？** &ndash; 不能对 Android API 级别“降级”；确保 Xamarin.Android 绑定项目的目标 API 级别与 Android 库相同（或处于更高级别）。

- **使用哪个版本的 JDK 编译库？** &ndash; 如果用于生成 Android 库的 JDK 版本不同于 Xamarin.Android 所用的版本，则可能会发生绑定错误。 如有可能，请使用安装 Xamarin.Android 所用的同一版本的 JDK 重新编译 Android 库。

## <a name="build-actions"></a>生成操作

创建绑定库时，你对合并到绑定库项目中的 .jar 或 .AAR 文件设置了生成操作，每个生成操作都决定了如何将 .jar 或 .AAR 文件嵌入到你的绑定库中（或如何使你的绑定库引用它们）    。 下面的列表总结了这些生成操作：

- `EmbeddedJar` &ndash; 将 .jar  作为嵌入式资源嵌入到生成的绑定库 .DLL 中。 这是最简单、最常用的生成操作。 如果需要将 .jar  自动编译为字节代码并打包到绑定库中，请使用此选项。

- `InputJar` &ndash; 不将 .jar  嵌入到生成的绑定库 .DLL 中。 绑定库 .DLL 在运行时将对此 .jar  具有依赖性。 如果不需要在绑定库中包含 .jar  （例如，出于许可原因），请使用此选项。 如果使用此选项，必须确保输入 .jar  在运行应用的设备上可用。

- `LibraryProjectZip` &ndash; 将 .AAR 文件嵌入到生成的绑定库 .DLL 中。 这类似于 EmbeddedJar，不同之处在于，你可以访问绑定的 .AAR 文件中的资源（以及代码）。 如果要将 .AAR 嵌入到绑定库中，请使用此选项。

- `ReferenceJar` &ndash; 指定引用 .jar  ：引用 .jar  是任一绑定的 .jar 或 .AAR 文件依赖的那个 .jar   。 这种引用 .jar  仅用于满足编译时依赖性。 当你使用此生成操作时，不会为引用 .jar 创建 C# 绑定，并且该 C# 绑定不会嵌入到生成的绑定库 .DLL 中  。 如果要为引用 .jar 生成绑定库，  但目前尚未这样做，请使用此选项。 如果将多个 .jar  （和/或 .AAR）打包到多个相互依赖的绑定库中，则此生成操作很有用。

- `EmbeddedReferenceJar` &ndash; 将引用 .jar  嵌入到生成的绑定库 .DLL 中。 若要同时为输入 .jar  （或 .AAR）及绑定库中的所有其引用 .jar 创建 C# 绑定，  请使用此生成操作。

- `EmbeddedNativeLibrary` &ndash; 将本机 .so  嵌入到绑定中。 此生成操作适用于绑定的 .jar 文件所需的 .so 文件   。 从 Java 库执行代码之前，可能有必要手动加载 .so 库  。 下面对此进行了介绍。

以下指南中更详细地介绍了这些生成操作。

此外，以下生成操作用于帮助导入 Java API 文档并将其转换为 C# XML 文档：

- `JavaDocJar` 用于指向符合 Maven 包样式的 Java 库的 Javadoc 存档 Jar（通常为 `FOOBAR-javadoc**.jar**`）。
- `JavaDocIndex` 用于指向 API 参考文档 HTML 中的 `index.html` 文件。
- `JavaSourceJar` 用于补充 `JavaDocJar`，以便首先从源生成 JavaDoc，然后将结果视为 `JavaDocIndex`，用于符合 Maven 包样式的 Java 库（通常为 `FOOBAR-sources**.jar**`）。

API 文档应该是 Java8、Java7 或 Java6 SDK 中的默认 doclet（都具有不同的格式）或 DroidDoc 样式。

## <a name="including-a-native-library-in-a-binding"></a>在绑定中包含本机库

在绑定 Java 库的过程中，可能有必要在 Xamarin.Android 绑定项目中包含 .so 库  。 当包装的 Java 代码执行时，Xamarin.Android 将无法进行 JNI 调用，并且应用程序的 logcat 中将出现错误消息“java.lang.UnsatisfiedLinkError:  找不到本机方法:”。

解决此问题的方法是通过调用 `Java.Lang.JavaSystem.LoadLibrary` 手动加载 .so  库。 例如，假设某个 Xamarin.Android 项目的绑定项目中包含共享库 libpocketsphinx_jni.so  ，并且具有 EmbeddedNativeLibrary  的生成操作，那么（在使用共享库之前执行的）以下代码片段将加载 .so  库：

```csharp
Java.Lang.JavaSystem.LoadLibrary("pocketsphinx_jni");
```

## <a name="adapting-java-apis-to-ceparsl"></a>使 Java API 适应 C&eparsl;

Xamarin.Android 绑定生成器会将一些 Java 惯例和模式更改为与 .NET 模式相对应。 以下列表介绍如何将 Java 映射到 C#/.NET：

- Java 中  的“Setter/Getter 方法”是 .NET 中的  “属性”。

- Java 中  的“字段”是 .NET 中的  “属性”。

- Java 中  的“侦听器/侦听器接口”是 .NET 中的  “事件”。 回调接口中的方法的参数将由 `EventArgs` 子类表示。

- Java 中的“静态嵌套类”  是 .NET 中的“嵌套类”  。

- Java 中的“内部类”  是 C# 中包含实例构造函数的“嵌套类”  。

## <a name="binding-scenarios"></a>绑定方案

以下绑定方案指南可帮助你绑定一个或多个 Java 库以将其合并到应用中：

- [绑定 .JAR](~/android/platform/binding-java-library/binding-a-jar.md) 介绍如何为 .jar  文件创建绑定库。

- [绑定 .AAR](~/android/platform/binding-java-library/binding-an-aar.md) 介绍如何为 .AAR 文件创建绑定库。 阅读本演练可以了解如何绑定 Android Studio 库。

- [绑定 Eclipse 库项目](~/android/platform/binding-java-library/binding-a-library-project.md)介绍如何从 Android 库项目创建绑定库。 阅读本演练可以了解如何绑定 Eclipse Android 库项目。

- [自定义绑定](~/android/platform/binding-java-library/customizing-bindings/index.md)说明了如何对绑定进行手动修改，以解决生成错误并调整生成的 API 的外观，从而使其“更像 C#”。

- [绑定疑难解答](~/android/platform/binding-java-library/troubleshooting-bindings.md)列出常见绑定错误场景、说明可能的原因，并提供用于解决这些错误的建议。

## <a name="related-links"></a>相关链接

- [使用 JNI](~/android/platform/java-integration/working-with-jni.md)
- [GAPI 元数据](https://www.mono-project.com/docs/gui/gtksharp/gapi/#metadata)
- [使用本机库](~/android/platform/native-libraries.md)
