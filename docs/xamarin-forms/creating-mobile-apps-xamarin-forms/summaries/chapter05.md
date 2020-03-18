---
title: 摘要：第 5 章. 处理大小
description: 使用 Xamarin.Forms 创建移动应用：摘要：第 5 章. 处理大小
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 486800E9-C09F-4B95-9AC2-C0F8FE563BCF
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
ms.openlocfilehash: c082bdb10732e42b37511cf050e50f46990a5b5b
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/10/2020
ms.locfileid: "70771145"
---
# <a name="summary-of-chapter-5-dealing-with-sizes"></a>摘要：第 5 章. 处理大小

[![下载示例](~/media/shared/download.png) 下载示例](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05)

> [!NOTE]
> 此页上的“注意”指出了 Xamarin.Forms 与书中所述内容的不同之处。

到目前为止，在 Xamarin.Forms 中遇到了几种大小的尺寸：

- iOS 状态栏的高度为 20
- `BoxView` 的默认宽度和高度均为 40
- `Frame` 中的默认 `Padding` 为 20
- `StackLayout` 上的默认 `Spacing` 为 6
- `Device.GetNamedSize` 方法返回数值形式的字体大小

这些不是像素的大小。 相反，它们是每个平台独立识别的与设备无关的单元。

## <a name="pixels-points-dps-dips-and-dius"></a>像素、点、DPS、DIP、DIU

在 Apple Mac 和 Microsoft Windows 初期，程序员在工作中使用像素作为单位。 但是，较高分辨率显示器的出现要求对屏幕坐标采用更加虚拟化和抽象的方法。 在 Mac 环境下，程序员在工作中以点  （传统意义上是 1/72 英寸）为单位，而 Windows 开发人员则使用基于 1/96 英寸的与设备无关的单位 (DIU)  。

但是，与桌面屏幕相比，移动设备通常更靠近脸部并且具有更高的分辨率，这意味着可以容忍更大的像素密度。

面向 Apple iPhone 和 iPad 设备的程序员在工作中仍以点为单位，但每英寸有 160 点。  对于不同的设备，点可能包含 1 个、2 个或 3 个像素不等。

Android 也是类似的。 程序员通过与密度无关的像素  (DPS) 为单位开展工作，并且 DPS 和像素之间的关系为每英寸 160 DPS。

Windows 手机和移动设备还建立了一些比例因数，表示每英寸接近 160 的设备无关单位。

> [!NOTE]
> Xamarin.Forms 不再支持任何基于 Windows 的电话或移动设备。

总之，面向手机和平板电脑的 Xamarin.Forms 程序员可以假设所有度量单位都基于以下标准：

- 每英寸 160 个单位，相当于
- 每厘米 64 个单位

由 `VisualElement` 定义的只读 [`Width`](xref:Xamarin.Forms.VisualElement.Width) 和 [`Height`](xref:Xamarin.Forms.VisualElement.Height) 属性的“模拟”值默认为 &ndash;1。 只有在调整了元素的大小并使其适合布局后，这些属性才会以与设备无关的单位反映元素的实际大小。 此大小包括在元素上设置的任何 `Padding`，而不是 `Margin`。

视觉对象元素在它的 `Width` 或 `Height` 发生更改时会触发 [`SizeChanged`](xref:Xamarin.Forms.VisualElement.SizeChanged) 事件。 [WhatSize  ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/WhatSize) 示例使用此事件来显示程序屏幕的大小。

## <a name="metrical-sizes"></a>度量大小

[MetricalBoxView  ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/MetricalBoxView) 使用 [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) 和 [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) 显示一英寸高度和一厘米宽度的 `BoxView`。

## <a name="estimated-font-sizes"></a>估计字体大小

[FontSizes  ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/FontSizes) 示例展示了如何使用 160 个单位/英寸规则以点为单位指定字体大小。 使用此技术的平台之间的视觉一致性优于 `Device.GetNamedSize`。

## <a name="fitting-text-to-available-size"></a>使文本适合可用大小

通过使用以下条件计算 `Label` 的 `FontSize`，可以在特定矩形中容纳文本块：

- 行距为字号的 120%（在 Windows 平台上为 130%）。
- 平均字符宽度为字号的 50%。

[EstimatedFontSize  ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/EstimatedFontSize) 示例展示了此技术。 在编写此程序时，[`Margin`](xref:Xamarin.Forms.View.Margin) 属性尚未提供，因此，此程序使用包含 [`Padding`](xref:Xamarin.Forms.Layout.Padding) 设置的 [`ContentView`](xref:Xamarin.Forms.ContentView) 来模拟边距。

[![估计字体大小的三倍屏幕截图](images/ch05fg07-small.png "使文本适合可用大小")](images/ch05fg07-large.png#lightbox "使文本适合可用大小")

## <a name="a-fit-to-size-clock"></a>大小适中的时钟

[FitToSizeClock  ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/FitToSizeClock) 示例展示了如何使用 [`Device.StartTimer`](xref:Xamarin.Forms.Device.StartTimer(System.TimeSpan,System.Func{System.Boolean})) 启动计时器，以便定期通知应用程序更新时钟。 字号设置为页面宽度的六分之一，尽可能大地显示内容。

## <a name="accessibility-issues"></a>辅助功能问题

EstimatedFontSize  程序和 FitToSizeClock  程序都包含一个细微的缺陷：如果用户在 Android 或 Windows 10 移动版上更改了手机的辅助功能设置，程序将无法再根据字号来估算文本的呈现大小。 [AccessibilityTest  ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/AccessibilityTest) 示例展示了此问题。

## <a name="empirically-fitting-text"></a>根据经验调整文本大小

使文本适合矩形的另一种方法是根据经验计算呈现的文本大小，并相应调整。 本书中的程序调用视觉对象元素的 [`GetSizeRequest`](xref:Xamarin.Forms.VisualElement.GetSizeRequest(System.Double,System.Double))，以获取元素的所需大小。 这种方法已弃用，程序应改为调用 [`Measure`](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags))。

对于 `Label`，第一个参数应为容器的宽度（以允许换行），第二个参数应设置为 `Double.PositiveInfinity` 以使高度不受限制。 [EmpiricalFontSize  ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/EmpiricalFontSize) 示例展示了此技术。

## <a name="related-links"></a>相关链接

- [第 5 章全文 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch05-Apr2016.pdf)
- [第 5 章示例](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05)
- [第 5 章 F# 示例](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/FS)
