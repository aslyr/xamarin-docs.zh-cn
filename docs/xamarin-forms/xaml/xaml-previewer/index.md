---
title: 的 XAML 预览器Xamarin.Forms
description: 本文介绍如何使用 XAML 预览器来查看你 Xamarin.Forms 键入时呈现的布局。 Visual Studio 2019 和 Visual Studio 2019 for Mac 中提供了 XAML 预览器。
zone_pivot_groups: platform
ms.prod: xamarin
ms.assetid: 84769ff1-72fd-4c44-8251-dd6d5bf8c7b2
ms.technology: xamarin-forms
author: maddyleger1
ms.author: maleger
ms.date: 03/16/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 6916d5fde688c5b1162f12db0d36bc3ca27156d8
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/18/2020
ms.locfileid: "84137314"
---
# <a name="xaml-previewer-for-xamarinforms"></a>的 XAML 预览器Xamarin.Forms

_查看在 Xamarin.Forms 键入时呈现的布局_

## <a name="overview"></a>概述

XAML 预览器显示 Xamarin.Forms xaml 页面在 iOS 和 Android 上的外观。 当你对 XAML 进行更改时，你会看到它们会立即与你的代码一起预览。 Visual Studio 和 Visual Studio for Mac 中提供了 XAML 预览器。

## <a name="getting-started"></a>入门

::: zone pivot="windows"

### <a name="visual-studio-2019"></a>Visual Studio 2019

可以通过单击 "拆分视图" 窗格中的箭头打开 XAML 预览器。 如果要更改默认拆分视图行为，请使用**工具 > 选项 "> Xamarin > Xamarin.Forms XAML 预览**器" 对话框。 在此对话框中，可以选择默认文档视图和拆分方向。

[![Xamarin.FormsVisual Studio 中的预览器选项](xaml-previewer-images/xamlp-options-vs-sm.png "[!基金.Visual Studio 中的非 LOC （Xamarin）] 预览器选项")](xaml-previewer-images/xamlp-options-vs-lg.png#lightbox)

打开 XAML 文件时，编辑器将根据 "**工具" > 选项 "> Xamarin > Xamarin.Forms XAML 预览**器" 对话框中选择的设置，打开预览器的完整大小或旁边的。 但是，可以在编辑器窗口中为每个文件更改拆分。

#### <a name="xaml-preview-controls"></a>XAML 预览控件

选择是否要查看代码和/或 XAML 预览器，方法是在 "拆分视图" 窗格中选择这些按钮。 中间按钮交换预览器和代码的位置：

[![Xamarin.Forms在 Visual Studio 中的 "设计"、"源" 和 "拆分" 视图之间切换的预览器控件](xaml-previewer-images/xamlp-controls-splitview-vs-sm.png "[!基金.无 LOC （Xamarin）：在 Visual Studio 中的 "设计"、"源" 和 "拆分" 视图之间切换的控件")](xaml-previewer-images/xamlp-controls-splitview-vs-lg.png#lightbox)

您可以更改是垂直拆分屏幕还是水平拆分屏幕，或完全折叠一个窗格：

[![Xamarin.FormsVisual Studio 中的 "预览器" 窗格方向控件](xaml-previewer-images/xamlp-controls-orientation-vs-sm.png "[!基金.无 LOC （Xamarin）] 在 Visual Studio 中预览窗格方向控件")](xaml-previewer-images/xamlp-controls-orientation-vs-lg.png#lightbox)

#### <a name="enable-or-disable-the-xaml-previewer"></a>启用或禁用 XAML 预览器

您可以通过选择 "**默认 XML 编辑器**" **，将 "****工具" > Xamarin.Forms "工具" > Xamarin >** 这也会关闭文档大纲、属性面板和 XAML 工具箱。 若要打开 XAML 预览器并重新打开这些工具，请将**默认 XAML 编辑器**更改为** Xamarin.Forms 预览**器。

::: zone-end
::: zone pivot="macos"

### <a name="visual-studio-for-mac"></a>Visual Studio for Mac

当您打开 XAML 页时，"**预览**" 按钮将显示在编辑器中。 按任意 XAML 文档窗口左下角的 "**预览**" 或 "**拆分**" 按钮显示或隐藏预览器：

[![Xamarin.Forms用预览或拆分按钮启用的预览器](xaml-previewer-images/xamlp-list-sml.png)](xaml-previewer-images/xamlp-list.png#lightbox)

> [!NOTE]
> 在较旧版本的 Visual Studio for Mac 中，"**预览**" 按钮位于窗口的右上方。

#### <a name="enable-or-disable-the-xaml-previewer"></a>启用或禁用 XAML 预览器

通过选择 "**默认 XML 编辑器**" 作为**默认的 XAML 编辑器**，可以在 VISUAL Studio 中关闭 Xaml 预览器 **> 首选项 > 文本编辑器 > XAML** "对话框。 这也会关闭文档大纲、属性面板和 XAML 工具箱。 若要打开 XAML 预览器并重新打开这些工具，请将**默认 XAML 编辑器**更改为** Xamarin.Forms 预览**器。

::: zone-end

## <a name="xaml-previewer-options"></a>XAML 预览器选项

预览窗格顶部的选项如下：

* **Android** –显示 android 版本的屏幕
* **ios** –显示 ios 版本的屏幕（*注意：如果你在 Windows 上使用 Visual Studio，则必须[配对到 Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md)才能使用此模式*）
* **设备**-Android 或 iOS 设备的下拉列表，包括分辨率和屏幕大小
* **纵向（图标）** -使用纵向预览
* **横向（图标）** –使用预览版的横向方向

## <a name="detect-design-mode"></a>检测设计模式

静态 [`DesignMode.IsDesignModeEnabled`](xref:Xamarin.Forms.DesignMode.IsDesignModeEnabled) 属性告诉你应用程序是否正在预览程序中运行。 使用它，你可以指定仅在应用程序处于或未在预览程序中运行时执行的代码：

```csharp
if (DesignMode.IsDesignModeEnabled)
{
  // Previewer only code  
}

if (!DesignMode.IsDesignModeEnabled)
{
  // Don't run in the Previewer  
}
```

如果在无法在设计时运行的页面构造函数中初始化库，此属性将非常有用。

## <a name="troubleshooting"></a>疑难解答

如果预览器不起作用，请检查以下问题和[Xamarin 论坛](https://forums.xamarin.com/categories/xamarin-forms)。

### <a name="xaml-previewer-isnt-showing-or-shows-an-error"></a>XAML 预览器没有显示或显示错误

* 预览器启动可能需要一些时间，你会看到 "正在初始化呈现"，直到它就绪。
* 尝试关闭并重新打开该 XAML 文件。
* 确保你的 `App` 类具有无参数的构造函数。
* 检查 Xamarin.Forms 版本-必须至少为 Xamarin.Forms 3.6。 可以通过 NuGet 更新到最新 Xamarin.Forms 版本。
* 检查 JDK 安装-预览 Android 至少需要[JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/index.html)。
* 尝试在中隐藏页的 c # 代码中的任何已初始化的类 `if (!DesignMode.IsDesignModeEnabled)` 。

### <a name="custom-controls-arent-rendering"></a>自定义控件不呈现

尝试生成项目。 如果该控件的基类未能呈现控件，或者该控件的创建者选择退出设计时呈现，则预览器会显示该控件的基类。 有关详细信息，请参阅[在 XAML 预览程序中呈现自定义控件](render-custom-controls.md)。
