---
title: 绑定 .JAR
description: 本演练将提供从 Android .JAR 文件创建 Xamarin.Android Java 绑定库的分步说明。
ms.prod: xamarin
ms.assetid: 93F1D5C5-E2AF-46EA-8460-485A0860C176
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 04/11/2018
ms.openlocfilehash: 336462176813b9985e2c269273d08c4aacbea9b5
ms.sourcegitcommit: a3f13a216fab4fc20a9adf343895b9d6a54634a5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/02/2020
ms.locfileid: "85853100"
---
# <a name="binding-a-jar"></a>绑定 .JAR

> [!IMPORTANT]
> 我们当前正在调查 Xamarin 平台上的自定义绑定使用情况。 请参与[此调查](https://www.surveymonkey.com/r/KKBHNLT)，告诉我们将来应该进行哪些开发工作。

本演练将提供从 Android .JAR 文件创建 Xamarin.Android Java 绑定库的分步说明。

## <a name="overview"></a>概述

Android 社区提供很多你可能需要在应用中使用的 Java 库。 这些 Java 库通常以 .JAR（Java 存档）格式打包，但你可以将 .JAR 打包到 Java 绑定库，使 Xamarin.Android 应用可以使用其功能。 Java 绑定库用于通过自动生成的代码包装器为 C# 代码提供 .JAR 文件中的 API。

Xamarin 工具可以从一个或多个输入 .JAR 文件生成绑定库。 绑定库（.DLL 程序集）包含以下内容： 

- 初始 .JAR 文件的内容。

- 托管的可调用包装器 (MCW)，该包装器为 C# 类型，可包装 .JAR 文件中相应的 Java 类型。

生成的 MCW 代码使用 JNI（Java 本机接口）将 API 调用转发到基础 .JAR 文件。 可以为最初用于 Android 的任何 .JAR 文件创建绑定库（请注意，Xamarin 工具当前不支持非 Android Java 库的绑定）。 你还可以选择生成绑定库，且不包括 .JAR 文件的内容，使 DLL 在运行时在 .JAR 上具有依赖项。

本指南将逐步介绍为单个 .JAR 文件创建绑定库的基础知识。 我们将通过一个现成可用（即不需要自定义或调试绑定）的示例进行说明。 
[使用元数据创建绑定](~/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata.md)举例说明了一种更高级的场景：绑定流程并不完全自动，需要一定的手动干预。 有关 Java 库绑定的常规概述（包含基本代码示例），请参阅[绑定 Java 库](~/android/platform/binding-java-library/index.md)。 

## <a name="walkthrough"></a>演练

在以下演练中，我们将为 [Picasso](https://square.github.io/picasso/)（一种提供图像加载和缓存功能的常用 Android .JAR）创建绑定库。 我们将按以下步骤绑定 picasso-2.x.x.jar，新建可在 Xamarin Android 项目中使用的 .NET 程序集： 

1. 新建 Java 绑定库项目。

2. 向项目添加 .JAR 文件。

3. 为 .JAR 文件设置适当的生成操作。

4. 选择 .JAR 支持的目标框架。

5. 生成绑定库。

创建绑定库后，我们将开发一个小型 Android 应用，用于展示在绑定库中调用 API 的能力。 在此示例中，我们想要访问 picasso-2.x.x.jar 的方法：

```java
package com.squareup.picasso

public class Picasso
{ 
    ...
    public static Picasso with (Context context) { ... };
    ...
    public RequestCreator load (String path) { ... };
    ...
}
```

生成 picasso-2.x.x.jar 的绑定库后，可以从 C# 调用这些方法。 例如：

```csharp
using Com.Squareup.Picasso;
...
Picasso.With (this)
    .Load ("http://mydomain.myimage.jpg")
    .Into (imageView);

```

### <a name="creating-the-bindings-library"></a>创建绑定库

开始执行以下步骤之前，请下载 [picasso-2](http://repo1.maven.org/maven2/com/squareup/picasso/picasso/2.5.2/picasso-2.5.2.jar)。

首先，新建绑定库项目。 在 Visual Studio for Mac 或 Visual Studio 中，新建解决方案并选择 Android 绑定库模板。 （本演练中的屏幕截图使用的是 Visual Studio，但 Visual Studio for Mac 非常类似。）将该解决方案命名为 JarBinding： 

[![创建 JarBinding 库项目](binding-a-jar-images/01-new-bindings-library-sml.w157.png)](binding-a-jar-images/01-new-bindings-library.w157.png#lightbox)

该模板包括一个 Jars 文件夹，可以在其中将 .JAR 添加到绑定库项目中。 右键单击 Jars 文件夹，并选择“添加”>“现有项” ： 

[![添加现有项](binding-a-jar-images/02-add-existing-item-sml.png)](binding-a-jar-images/02-add-existing-item.png#lightbox)

导航到之前下载的 picasso-2.x.x.jar 文件，选中它，然后单击“添加” ： 

[![选择 jar 文件，然后单击“添加”](binding-a-jar-images/03-select-jar-file-sml.png)](binding-a-jar-images/03-select-jar-file.png#lightbox)

验证 picasso-2.x.x.jar 文件是否已成功添加到项目中： 

[![添加到项目的 Jar](binding-a-jar-images/04-jar-added-sml.png)](binding-a-jar-images/04-jar-added.png#lightbox)

创建 Java 绑定库项目时，必须指定是将 JAR 嵌入绑定库中还是单独打包。 为此，请指定以下生成操作之一： 

- EmbeddedJar &ndash; .JAR 将嵌入绑定库中。

- InputJar &ndash; .JAR 将与绑定库分离。

通常使用的是 EmbeddedJar 生成操作，使 .JAR 自动打包到绑定库。 这是最简单的方法 &ndash; .JAR 中的 Java 字节码转换为 Dex 字节码，然后（与托管的可调用包装器一起）嵌入 APK。 如果要将 .JAR 与绑定库分离，可以使用 InputJar 选项；但必须确保 .JAR 文件在运行应用的设备上可用。 

将生成操作设置为 EmbeddedJar： 

[![选择 EmbeddedJar 生成操作](binding-a-jar-images/05-embeddedjar-sml.png)](binding-a-jar-images/05-embeddedjar.png#lightbox)

接下来，打开项目属性，配置“目标框架”。 如果 .JAR 使用任何 Android API，请将目标框架设置为 .JAR 预计的 API 级别。 通常，.JAR 文件的开发人员将指示 .JAR 所兼容的 API 级别。 （有关目标框架设置和 Android API 级别的更多概要信息，请参阅[了解 Android API 级别](~/android/app-fundamentals/android-api-levels.md)。）

设置绑定库的目标 API 级别（在本示例中，使用的是 API 级别 19）： 

[![目标 API 级别已设置为 API 19](binding-a-jar-images/06-set-target-framework-sml.png)](binding-a-jar-images/06-set-target-framework.png#lightbox)

最后，生成绑定库。 尽管可能会显示一些警告消息，但绑定库项目应成功构建并在以下位置生成输出 .DLL：JarBinding/bin/Debug/JarBinding.dll

### <a name="using-the-bindings-library"></a>使用绑定库

若要在 Xamarin.Android 应用中使用此 .DLL，请执行以下操作：

1. 添加对绑定库的引用。

2. 通过托管的可调用包装器对 .JAR 进行调用。 

在以下步骤中，我们将创建一个最小的应用，用于使用绑定库下载并显示 `ImageView` 中的图像；“繁重工作”由 .JAR 文件中驻留的代码完成。 

首先，新建使用绑定库的 Xamarin.Android 应用。 右键单击“解决方案”并选择“添加新项目”；将新项目命名为 BindingTest 。 我们将在与绑定库相同的解决方案中创建此应用，以简化此演练；但使用绑定库的应用可以驻留在其他解决方案中： 

[![添加新的 BindingTest 项目](binding-a-jar-images/07-add-new-project-sml.w157.png)](binding-a-jar-images/07-add-new-project.w157.png#lightbox)

右键单击 BindingTest 项目的“引用”节点并选择“添加引用...”  ：

[![右键添加引用](binding-a-jar-images/08-add-reference.png)](binding-a-jar-images/08-add-reference.png#lightbox)

勾选之前创建的 JarBinding 项目，然后单击“确定” ：

[![选择 JarBinding 项目](binding-a-jar-images/09-choose-jar-binding-sml.png)](binding-a-jar-images/09-choose-jar-binding.png#lightbox)

打开 BindingTest 项目的“引用”节点，验证是否存在 JarBinding 引用  ： 

[![JarBinding 显示在“引用”下](binding-a-jar-images/10-references-shows-jarbinding-sml.png)](binding-a-jar-images/10-references-shows-jarbinding.png#lightbox)

修改 BindingTest 布局 (Main.axml)，使其具有单个 `ImageView` ：

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:minWidth="25px"
    android:minHeight="25px">
    <ImageView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/imageView" />
</LinearLayout>
```

将以下 `using` 语句添加到 MainActivity.cs &ndash; 这使得可以轻松访问基于 Java 的 `Picasso` 类（驻留在绑定库中）的方法：

```csharp
using Com.Squareup.Picasso;
```

修改 `OnCreate` 方法，以便其使用 `Picasso` 类从 URL 加载图像并在 `ImageView` 中显示： 

```csharp
public class MainActivity : Activity
{
    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);
        SetContentView(Resource.Layout.Main);
        ImageView imageView = FindViewById<ImageView>(Resource.Id.imageView);

        // Use the Picasso jar library to load and display this image:
        Picasso.With (this)
            .Load ("http://i.imgur.com/DvpvklR.jpg")
            .Into (imageView);
    }
}
```

编译并运行 BindingTest 项目。 应用将启动，并在短暂延迟后（取决于网络条件），应下载并显示与以下屏幕截图类似的图像：

[![运行的 BindingTest 的屏幕截图](binding-a-jar-images/11-result-sml.png)](binding-a-jar-images/11-result.png#lightbox)

祝贺你！ 你成功绑定了 Java 库 .JAR 并在 Xamarin Android 应用中使用了它。

## <a name="summary"></a>总结

在本演练中，我们为第三方 .JAR 文件创建了一个绑定库，将绑定库添加到最小的测试应用中，然后运行该应用以验证我们的 C# 代码是否可以调用驻留在 .JAR 文件中的 Java 代码。 

## <a name="related-links"></a>相关链接

- [生成 Java 绑定库（视频）](https://university.xamarin.com/classes#10090)
- [绑定 Java 库](~/android/platform/binding-java-library/index.md)
