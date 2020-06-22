---
title: title:“摘要：第 21 章. 转换”说明："《使用 Xamarin.Forms 创建移动应用》- 摘要：摘要：第 21 章.
description: '转换”ms.prod: xamarin ms.technology: xamarin-forms ms.assetid:3642F112-C7FA-4A74-9000-F9087BA89AD9 author: davidbritch ms.author: dabritch ms.date:2017/11/07 no-loc: [Xamarin.Forms, Xamarin.Essentials] 摘要：第 21 章.'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 3642F112-C7FA-4A74-9000-F9087BA89AD9
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 32393108f84ea3a57079c86b6a9a8e628ceca03a
ms.sourcegitcommit: ea9269b5d9e3d68b61bb428560a10034117ee457
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/10/2020
ms.locfileid: "84136664"
---
# <a name="summary-of-chapter-21-transforms"></a>转换 [![下载示例](~/media/shared/download.png) 下载示例](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21)

Xamarin.Forms 视图以其父级（这通常是 `Layout` 或 `Layout<View>` 导数）确定的位置和大小出现在屏幕上。

转换是一项 Xamarin.Forms 功能，可以修改该位置、大小甚至方向。 Xamarin.Forms 支持三种基本转换类型：

*平移* &mdash; 水平或垂直移动元素

- *缩放* &mdash; 更改元素的大小
- *旋转* &mdash; 围绕点或轴旋转元素
- 在 Xamarin.Forms 中，缩放具有等向性；它对宽度和高度的影响是一致的。

在屏幕的二维表面和 3D 空间中都支持旋转。 没有偏斜（或垂直）转换，也没有广义矩阵转换。 由 `VisualElement` 类定义的八个类型为 `double` 的属性支持转换：

所有这些属性都由可绑定属性提供支持。

- [`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX)
- [`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY)
- [`Scale`](xref:Xamarin.Forms.VisualElement.Scale)
- [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation)
- [`RotationX`](xref:Xamarin.Forms.VisualElement.RotationX)
- [`RotationY`](xref:Xamarin.Forms.VisualElement.RotationY)
- [`AnchorX`](xref:Xamarin.Forms.VisualElement.AnchorX)
- [`AnchorY`](xref:Xamarin.Forms.VisualElement.AnchorY)

它们可以成为数据绑定和样式设置的目标。 **第 22 章.[动画](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter22.md)演示了如何对这些属性进行动画处理，但本章中的一些示例演示了如何使用 Xamarin.Forms [计时器](~/xamarin-forms/platform/device.md#devicestarttimer)对其进行动画处理**。 转换属性仅影响元素的呈现方式，而不影响布局中元素的感知方式。

平移转换

## <a name="the-translation-transform"></a>[`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX) 和 [`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY) 属性的非零值可水平或垂直移动元素。

借助 [TranslationDemo](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/TranslationDemo) 程序，可以使用两个 `Slider` 元素来试验这些属性，这两个元素可控制 `Frame` 的 `TranslationX` 和 `TranslationY` 属性。

转换也会影响该 `Frame` 的所有子级。 文本效果

### <a name="text-effects"></a>平移属性的一种常见用法是稍微偏移文本的呈现。

[TextOffsets](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/TextOffsets) 示例对此进行了演示： [![文本偏移的三倍屏幕截图](images/ch21fg03-small.png "文本偏移")](images/ch21fg03-large.png#lightbox "文本偏移")

另一个效果是呈现 `Label` 的多个副本，使其类似于 3D 块，例如 [BlockText](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/BlockText) 示例中所示。

跳转和动画

### <a name="jumps-and-animations"></a>[ButtonJump](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ButtonJump) 示例使用平移在每次点击 `Button` 时移动它，但是主要目的是演示 `Button` 在呈现按钮的位置接收到用户输入。

[ButtonGlide](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ButtonGlide) 示例类似，但使用计时器对 `Button` 进行动画处理，从一个点移动到另一个点。

缩放转换

## <a name="the-scale-transform"></a>[`Scale`](xref:Xamarin.Forms.VisualElement.Scale) 转换可以增加或减小元素的呈现大小。

默认值为 1。 值为 0 会导致元素不可见。 负值会导致元素看起来旋转 180 度。 `Scale` 属性不会影响元素的 `Width` 或 `Height` 属性。 这些值保持不变。 可以使用 [SimpleScaleDemo](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/SimpleScaleDemo) 示例来试验 `Scale` 属性。

[ButtonScaler](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ButtonScaler) 示例演示了对 `Button` 的 `Scale` 属性进行动画处理与对 `FontSize` 属性进行动画处理之间的区别。

`FontSize` 属性会影响 `Button` 在布局中的感知方式；`Scale` 属性不会。 [ScaleToSize](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ScaleToSize) 示例计算应用于 `Label` 元素的 `Scale` 属性，以使其尽可能大，但同时仍适合页面。

定位缩放

### <a name="anchoring-the-scale"></a>在前面三个示例中缩放的元素，其大小相对于元素中心都已增加或减小。

换言之，元素在所有方向上的增加或减少大小都相同。 在缩放过程中，只有元素中心的点保持在相同位置。 可以通过设置 [`AnchorX`](xref:Xamarin.Forms.VisualElement.AnchorX) 和 [`AnchorY`](xref:Xamarin.Forms.VisualElement.AnchorY) 属性来更改缩放中心。

这些属性是相对于元素本身而言的。 对于 `AnchorX`，值为 0 表示元素的左侧，1 表示右侧。 与 `AnchorY` 类似，0 表示顶部，而 1 表示底部。 这两个属性的默认值均为 0.5，即中心。 借助 [AnchoredScaleDemo](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/AnchoredScaleDemo) 示例，可以试验 `AnchorX`、`AnchorY` 属性以及 `Scale` 属性。

在 iOS 上，使用 `AnchorX` 和 `AnchorY` 属性的非默认值通常与手机方向更改不兼容。

旋转转换

## <a name="the-rotation-transform"></a>以度为单位指定 [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) 属性，并指示围绕由 `AnchorX` 和 `AnchorY` 定义的元素的点顺时针旋转。

借助 [PlaneRotationDemo](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/PlaneRotationDemo)，可以试验这三个属性。 旋转的文本效果

### <a name="rotated-text-effects"></a>[BoxViewCircle](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/BoxViewCircle) 示例演示了使用 64 个略微旋转的 `BoxView` 元素绘制圆所需的数学方法。

[RotatedText](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/RotatedText) 示例显示了多个 `Label` 元素，并将相同的文本字符串旋转以显示为类似轮辐的形式。

[CircularText](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/CircularText) 示例显示一个文本字符串，该文本字符串显示为围成一个圆圈。

模拟时钟

### <a name="an-analog-clock"></a>[Xamarin.FormsBook.Toolkit](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 库包含一个 [`AnalogClockViewModel`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AnalogClockViewModel.cs) 类，该类计算时钟指针的角度。

为了避免 ViewModel 中的平台依赖项，该类使用 `Task.Delay`（而不是计时器）来查找新的 `DateTime` 值。 Xamarin.FormsBook.Toolkit 中还包括一个 [`SecondTickConverter`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/SecondTickConverter.cs) 类，该类实现 `IValueConverter` 并用于将第二个角度舍入到最接近的秒。

[MinimalBoxViewClock](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/MinimalBoxViewClock) 使用三个旋转的 `BoxView` 元素绘制一个模拟时钟。

[BoxViewClock](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/BoxViewClock) 使用 `BoxView` 获得更多图形，包括时钟钟面上的刻度线以及从其末端旋转一点距离的指针：

[![BoxView 时钟的三倍屏幕截图](images/ch21fg17-small.png "模拟时钟钟面")](images/ch21fg17-large.png#lightbox "模拟时钟钟面")

此外，Xamarin.FormsBook.Toolkit 中的 [`SecondBackEaseConverter`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/SecondBackEaseConverter.cs) 类使秒针似乎在向前跳之前先向后拉一点，然后再移回到其正确的位置。

垂直滑块？

### <a name="vertical-sliders"></a>[VerticalSliders](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/VerticalSliders) 示例演示了 `Slider` 元素可以旋转 90 度，并仍可正常工作。

但是，很难定位这些旋转的 `Slider` 元素，因为在布局中它们仍然是水平的。 以 3D 方式旋转

## <a name="3d-ish-rotations"></a>[`RotationX`](xref:Xamarin.Forms.VisualElement.RotationX) 属性似乎使元素围绕 3D X 轴旋转，因此元素的顶部和底部似乎朝着观看者或远离观看者移动。

同样，[`RotationY`](xref:Xamarin.Forms.VisualElement.RotationY) 似乎使元素围绕 Y 轴旋转，以使元素的左侧和右侧似乎朝着观看者或远离观看者移动。 `AnchorX` 属性影响 `RotationY`，但不影响 `RotationX`。

`AnchorY` 属性影响 `RotationX`，但不影响 `RotationY`。 可以试用 [ThreeDeeRotationDemo](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ThreeDeeRotationDemo) 示例来探索这些属性之间的交互。 Xamarin.Forms 所隐含的 3D 坐标系统适用于左手操作。

如果将左手食指指向 X 坐标增加的方向（向右），而中指指向 Y 坐标增加的方向（向下），那么拇指指向 Z 坐标增加的方向（在屏幕之外）。 另外，对于三个轴中的任何一个轴，如果将左手拇指指向增加值的方向，则手指的曲线将指示正旋转角度的旋转方向。

相关链接

## <a name="related-links"></a>[第 21 章全文 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch21-Apr2016.pdf)

- [第 21 章示例](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21)
- 文本偏移
