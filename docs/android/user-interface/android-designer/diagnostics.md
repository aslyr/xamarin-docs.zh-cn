---
title: Android 布局诊断
description: 介绍 Android 布局诊断和入门方式
ms.prod: xamarin
ms.assetid: BD252EA7-7E69-4DB4-96AB-D52CC0510C8F
ms.technology: xamarin-android
author: decriptor
ms.author: stepsha
ms.date: 03/24/2020
ms.openlocfilehash: 5c29a1a80d8c1f599f0bbc750d22d8334ddb3494
ms.sourcegitcommit: d83c6af42ed26947aa7c0ecfce00b9ef60f33319
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/25/2020
ms.locfileid: "80247814"
---
# <a name="android-layout-diagnostics"></a>Android 布局诊断

Android 布局诊断旨在通过突出显示常见质量问题和有用的优化来帮助提高 Android 布局文件的质量。 此功能适用于 Visual Studio 16.5 + 和 Visual Studio for Mac 8.5 +。

提供了一组默认的分析器来解决各种问题，并且每个分析器都可以进行自定义，以满足项目的特定需求。 分析器基于 Android linting 系统松散。

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

## <a name="enable-android-layout-diagnostics-on-visual-studio-2019"></a>在 Visual Studio 2019 上启用 Android 布局诊断

确保启用布局诊断设置 "**启用布局诊断**"。 若要访问此选项页，请选择 "**工具**" > **选项**"，然后选择"**文本编辑器**" > **Android XML** > **高级**：

![显示如何启用诊断选项的 "选项" 对话框](diagnostics-images/AndroidDiagnosticsEnableOption.png)

启用后，Android 布局编辑器将显示问题：

![在 Visual Studio 2019 上启用 Android 诊断](diagnostics-images/AndroidDiagnosticsEnabled.png)

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

## <a name="enable-android-layout-diagnostics-on-visual-studio-for-mac"></a>启用 Visual Studio for Mac 上的 Android 布局诊断

确保启用布局诊断设置 "**启用布局诊断**"。 若要访问此选项页，请选择 " **Visual Studio** > **首选项 ...** "，然后选择 "**文本编辑器**" > **Android XML**：

![显示如何启用诊断选项的 "首选项" 对话框](diagnostics-images/AndroidDiagnosticsEnableOptionVSmac.png)

启用后，Android 布局编辑器将显示问题：

![Visual Studio for Mac 上启用 Android 诊断](diagnostics-images/AndroidDiagnosticsEnabledVSmac.png)

-----

## <a name="features"></a>功能

以下部分概述了 Android 布局诊断中的可用功能。

### <a name="analyzers"></a>分析器

分析器用于帮助检测布局文件中的问题。 某些方法可减少硬编码值，提高性能并标记错误。

### <a name="diagnostic-configuration"></a>诊断配置

可以使用 XML 文件配置分析器，使您可以更改默认的严重性级别、忽略某些文件和传入变量。

如果你有一组要在多个 Android 应用间共享的配置，则可以使用基线文件。 若要使用此功能，请创建新的配置文件并将 `-baseline` 追加到文件名。 首先应用基线配置，然后应用其余配置文件。

> [!TIP]
> 如果要忽略新的或现有的 Android 应用中的一组问题，这会很有用。

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
> 目前，只有 `TooManyViews` 和 `TooDeepLayout` 分别 `MAX_VIEW_COUNT` （默认值：80）和 `MAX_DEPTH` （默认值：10）。

严重级别包括：

- 建议
- 信息
- 警告
- 错误
- 忽略

### <a name="add-a-configuration-file"></a>添加配置文件

在 Android 应用程序项目的根目录中创建新的 XML 文件。 该文件的名称并不重要，但此示例使用 `AndroidLayoutDiagnostics.xml`：

![添加新项](diagnostics-images/AndroidDiagnosticsNewFileDialog.png)

添加新的 XML 文件后，该文件应显示在 Android 应用项目树中：

![Android 应用项目树](diagnostics-images/AndroidDiagnosticsFileAddToTree.png)

请确保 "属性" 面板中的 "生成" 操作设置为 " **AndroidResourceAnalysisConfig** "。
若要获取新文件的属性面板，最简单的方法是右键单击该文件，然后选择 "属性"。 显示 "属性" 面板后，应将 "**生成操作**" 更改为 " **AndroidResourceAnalysisConfig**"：

![在项目属性中设置生成操作](diagnostics-images/AndroidDiagnosticsSetBuildAction.png)

现在，你已有一个空白 XML 文件，需要添加 `<configuration>` 根元素。 此时，你可以调整任何支持的问题的默认行为。
如果要确保将硬编码字符串视为错误，请执行以下操作：

```xml
<issue="HardcodedText" severity="error">
</issue>
```

![诊断配置文件](diagnostics-images/AndroidDiagnosticsConfigurationFileExample.png)

由于硬编码文本被视为错误，现在它在布局编辑器中被标记为红色波形曲线：

![使用诊断配置布局](diagnostics-images/AndroidDiagnosticsUsingConfiguration.png)

> [!NOTE]
> 为了使任何新的配置文件更改生效，需要重新打开任何当前打开的布局文件。
>

## <a name="troubleshooting"></a>故障排除

下面是一些可能出现的常见问题。

- 请确保不存在 XML 格式错误。
- 生成操作已正确设置为**AndroidResourceAnalysisConfig**。

## <a name="known-issues"></a>已知问题

- 第一次更改文件后，才会填充错误板。

## <a name="related-links"></a>相关链接

- [Android 不起毛检查](http://tools.android.com/tips/lint-checks)
- [通过不起毛的检查改善代码](https://developer.android.com/studio/write/lint)
