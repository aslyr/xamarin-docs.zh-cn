---
title: Android 布局诊断
description: 解释 Android 布局诊断以及如何入门
ms.prod: xamarin
ms.assetid: BD252EA7-7E69-4DB4-96AB-D52CC0510C8F
ms.technology: xamarin-android
author: decriptor
ms.author: stepsha
ms.date: 03/24/2020
ms.openlocfilehash: 746f74e68fa4816f1f7979980af9506dc0173542
ms.sourcegitcommit: 765b69ed451a0f48625ea597c3f39de95f3ae693
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/09/2020
ms.locfileid: "80987577"
---
# <a name="android-layout-diagnostics"></a>Android 布局诊断

Android 布局诊断旨在通过突出显示常见的质量问题和有用的优化来帮助提高 Android 布局文件的质量。 此功能适用于适用于 Visual Studio 16.5+ 和适用于 Mac 8.5+ 的可视化工作室。

为各种问题提供了一组默认的分析器，每个分析器都可以自定义以满足项目的特定需求。 分析仪松散地基于 Android 林丁系统。

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

## <a name="enable-android-layout-diagnostics-on-visual-studio-2019"></a>在视觉工作室 2019 启用 Android 布局诊断

确保启用布局诊断设置，**启用布局诊断**。 要访问此选项页，请选择 **"工具** > **选项**"，然后选择**文本编辑器** > **Android XML** > **高级**：

![演示如何启用诊断选项的选项对话框](diagnostics-images/AndroidDiagnosticsEnableOption.png)

启用后，Android 布局编辑器将显示问题：

![在视觉工作室 2019 启用的 Android 诊断](diagnostics-images/AndroidDiagnosticsEnabled.png)

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

## <a name="enable-android-layout-diagnostics-on-visual-studio-for-mac"></a>在 Mac 视觉工作室启用 Android 布局诊断

确保启用布局诊断设置，**启用布局诊断**。 要访问此选项页面，请选择**可视化工作室** > **首选项...** **Text Editor** > **Android XML**

![显示如何启用诊断选项的首选项对话框](diagnostics-images/AndroidDiagnosticsEnableOptionVSmac.png)

启用后，Android 布局编辑器将显示问题：

![在 Mac 视觉工作室启用的 Android 诊断](diagnostics-images/AndroidDiagnosticsEnabledVSmac.png)

-----

## <a name="features"></a>功能

以下各节概述了 Android 布局诊断中的可用功能。

### <a name="analyzers"></a>分析器

分析器用于帮助检测布局文件中的问题、减少硬编码值、提高性能和标记错误。 有关分析仪列表，请参阅 Android[设计器诊断分析仪](diagnostic-analyzers.md)

### <a name="diagnostic-configuration"></a>诊断配置

可以使用 XML 文件配置分析器，允许您更改默认严重性级别、忽略某些文件以及传递变量。

如果您有一组要跨多个 Android 应用共享的配置，则可以使用基准文件。 要使用此功能，请创建新的配置文件并追加`-baseline`文件名。 首先应用基线配置，然后应用其余配置文件。

> [!TIP]
> 如果要忽略新或现有 Android 应用上的一组问题，这非常有用。

格式为：

```xml
<?xml version="1.0" encoding="utf-8" ?> 
<configuration>
    <issue id="DuplicateIDs" severity="warning">
        <ignore path="Resources/layout/layout1.xml" />
    </issue>
    <issue id="HardcodedText" severity="informational">
        <ignore path="Resources/layout/layout1.xml" />
        <ignore path="Resource/layout/layout2.xml" />
    </issue>
    <issue id="TooManyViews">
        <variable name="MAX_VIEW_COUNT" value="12" />
    </issue>
    <issue id="TooDeepLayout">
        <variable name="MAX_DEPTH" value="12" />
    </issue>
</configuration>
```

> [!NOTE]
> 目前，唯一的变量`MAX_VIEW_COUNT`是 （默认值： 80） 和`MAX_DEPTH`（默认值`TooManyViews`： `TooDeepLayout` 10） 和

严重级别包括：

- 建议
- 信息
- 警告
- 错误
- 忽略

### <a name="add-a-configuration-file"></a>添加配置文件

在 Android 应用项目的根目录中创建新的 XML 文件。 文件的名称并不重要，但此示例使用`AndroidLayoutDiagnostics.xml`：

![添加新项](diagnostics-images/AndroidDiagnosticsNewFileDialog.png)

添加新的 XML 文件后，该文件应出现在 Android 应用项目树中：

![Android 应用程序项目树](diagnostics-images/AndroidDiagnosticsFileAddToTree.png)

确保生成操作设置为属性面板中的**AndroidResourceAnalysis Config。**
拉上新文件的属性面板的最简单方法是右键单击该文件并选择属性。 显示属性面板后，应将**生成操作**更改为**Android 资源分析配置**：

![在项目属性中设置生成操作](diagnostics-images/AndroidDiagnosticsSetBuildAction.png)

现在，您有一个空白的XML文件，您需要添加`<configuration>`根元素。 此时，您可以调整任何受支持问题的默认行为。
如果要确保硬编码字符串被视为错误，请添加：

```xml
<issue="HardcodedText" severity="error">
</issue>
```

![诊断配置文件](diagnostics-images/AndroidDiagnosticsConfigurationFileExample.png)

现在硬编码文本被视为错误，现在它在布局编辑器中用红色波浪标记：

![使用诊断配置进行布局](diagnostics-images/AndroidDiagnosticsUsingConfiguration.png)

> [!NOTE]
> 要使任何新的配置文件更改生效，需要重新打开任何当前打开的布局文件。
>

## <a name="troubleshooting"></a>故障排除

下面是一些可能的常见问题。

- 确保没有 XML 格式错误。
- 生成操作正确设置为**Android 资源分析分析配置**。

## <a name="known-issues"></a>已知问题

- 直到第一次更改文件后，才会填充错误键盘。

## <a name="related-links"></a>相关链接

- [安卓林特检查](http://tools.android.com/tips/lint-checks)
- [使用绒毛检查改进代码](https://developer.android.com/studio/write/lint)
