---
title: title:“摘要：第 22 章. 动画”说明："《使用 Xamarin.Forms 创建移动应用》- 摘要：第 22 章摘要。
description: '动画”ms.prod: xamarin ms.technology: xamarin-forms ms.assetid:47C2B9AB-E688-4412-8AF5-9F633B3DA695 author: davidbritch ms.author: dabritch ms.date:2017/11/07 no-loc: [Xamarin.Forms, Xamarin.Essentials] 第 22 章摘要。'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 47C2B9AB-E688-4412-8AF5-9F633B3DA695
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 2a8a089c210a3fe2f48dbe32bf8cda6179af2a78
ms.sourcegitcommit: ea9269b5d9e3d68b61bb428560a10034117ee457
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/10/2020
ms.locfileid: "84136625"
---
# <a name="summary-of-chapter-22-animation"></a>动画 [![下载示例](~/media/shared/download.png) 下载示例](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22)

你已经知道可以使用 Xamarin.Forms 计时器或 `Task.Delay` 创建自己的动画，但使用 Xamarin.Forms 提供的动画工具通常更容易。

有三个类可以实现这些动画： [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions)（高级方法）

- [`Animation`](xref:Xamarin.Forms.Animation)（更通用，但较难）
- [`AnimationExtension`](xref:Xamarin.Forms.AnimationExtensions)（最常用、最基础的方法）
- 通常，动画以可绑定属性支持的属性为目标。

这不是必需的，但这些是对更改做出动态反应的唯一属性。 这些动画没有 XAML 接口，但可以使用[第 23 章触发器和行为](chapter23.md)中讨论的方法将动画集成到 XAML。

探索基本动画

## <a name="exploring-basic-animations"></a>基本动画功能是 [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) 类中的扩展方法。

这些方法适用于派生自 `VisualElement` 的任何对象。 此类最简单的动画以 [`Chapter 21. Transforms`](chapter21.md) 中讨论的转换属性为目标。 [AnimationTryout](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/AnimationTryout) 演示了 `Button` 的 `Clicked` 事件处理程序如何调用 [`RotateTo`](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) 扩展方法来旋转圆形中的按钮。

`RotateTo` 方法在四分之一秒的时间内（默认情况下）将 `Button` 的 `Rotation` 属性从 0 更改为 360。

但是，如果再次点击 `Button`，则不会执行任何操作，因为 `Rotation` 属性已经是 360 度。 设置动画持续时间

### <a name="setting-the-animation-duration"></a>`RotateTo` 的第二个自变量是持续时间（毫秒）。

如果设置为较大的值，则在动画演示过程中点击 `Button` 将以当前角度开始一个新的动画。 相对动画

### <a name="relative-animations"></a>`RelRotateTo` 方法通过将指定的值添加到现有值来执行相对旋转。

此方法允许多次点击 `Button`，每次 `Rotation` 属性增加 360 度。 等待动画

### <a name="awaiting-animations"></a>`ViewExtensions` 中的所有动画方法都返回 `Task<bool>` 对象。

这意味着可以使用 `ContinueWith` 或 `await` 来定义一系列连续动画。 如果动画在没有中断的情况下完成，则 `bool` 完成返回值为 `false`；如果通过 [`CancelAnimation`](xref:Xamarin.Forms.ViewExtensions.CancelAnimations(Xamarin.Forms.VisualElement)) 方法（该方法将取消在同一元素上设置的 `ViewExtensions` 中的其他方法启动的所有动画）取消了该动画，返回值则为 `true`。 复合动画

### <a name="composite-animations"></a>可以混合等待和非等待的动画来创建复合动画。

这些是 `ViewExtensions` 中面向 `TranslationX`、`TranslationY` 和 `Scale` 转换属性的动画： [`TranslateTo`](xref:Xamarin.Forms.ViewExtensions.TranslateTo(Xamarin.Forms.VisualElement,System.Double,System.Double,System.UInt32,Xamarin.Forms.Easing))

- [`ScaleTo`](xref:Xamarin.Forms.ViewExtensions.ScaleTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))
- [`RelScaleTo`](xref:Xamarin.Forms.ViewExtensions.RelScaleTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))
- 请注意，`TranslateTo` 可能会影响 `TranslationX` 和 `TranslationY` 的属性。

Task.WhenAll 和 Task.WhenAny

### <a name="taskwhenall-and-taskwhenany"></a>还可以使用 [`Task.WhenAll`](xref:System.Threading.Tasks.Task.WhenAll*)（在多个任务都结束时发出信号）和 [`Task.WhenAny`](xref:System.Threading.Tasks.Task.WhenAny*)（在多个任务中的第一个任务结束后发出信号）来管理同步动画。

旋转和定位点

### <a name="rotation-and-anchors"></a>调用 `ScaleTo`、`RelScaleTo`、`RotateTo` 和 `RelRotateTo` 方法时，可以将 [`AnchorX`](xref:Xamarin.Forms.VisualElement.AnchorX) 和 [`AnchorY`](xref:Xamarin.Forms.VisualElement.AnchorY) 属性设置为指示缩放和旋转的中心。

[CircleButton](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/CircleButton) 通过围绕页面中心旋转 `Button` 来演示此技术。

缓动函数

### <a name="easing-functions"></a>通常，动画从开始值到结束值都是线性的。

缓动函数会导致动画在整个演示过程中加速或减速。 动画方法的最后一个可选自变量为 [`Easing`](xref:Xamarin.Forms.Easing) 类型，该类定义了 11 个 `Easing` 类型的静态只读字段： [`Linear`](xref:Xamarin.Forms.Easing.Linear)（默认值）

- [`SinIn`](xref:Xamarin.Forms.Easing.SinIn)、[`SinOut`](xref:Xamarin.Forms.Easing.SinOut) 和 [`SinInOut`](xref:Xamarin.Forms.Easing.SinInOut)
- [`CubicIn`](xref:Xamarin.Forms.Easing.CubicIn)、[`CubicOut`](xref:Xamarin.Forms.Easing.CubicOut) 和 [`CubicInOut`](xref:Xamarin.Forms.Easing.CubicInOut)
- [`BounceIn`](xref:Xamarin.Forms.Easing.BounceIn) 和 [`BounceOut`](xref:Xamarin.Forms.Easing.BounceOut)
- [`SpringIn`](xref:Xamarin.Forms.Easing.SpringIn) 和 [`SpringOut`](xref:Xamarin.Forms.Easing.SpringOut)
- `In` 后缀表示该效果位于动画的开头，`Out` 表示该效果位于结尾，`InOut` 表示该效果位于动画的开头和结尾。

[BounceButton](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/BounceButton) 示例演示了如何使用缓动函数。

自己的缓动函数

### <a name="your-own-easing-functions"></a>还可以通过将 [`Func<double, double>`](xref:System.Func`2) 传递到 [`Easing` 构造函数](xref:Xamarin.Forms.Easing.%23ctor(System.Func{System.Double,System.Double}))来定义自己的缓动函数。

`Easing` 还定义了从 `Func<double, double>` 到 `Easing` 的隐式转换。 缓动函数的自变量始终处于 0 到 1 的范围内，因为动画会从头到尾以线性方式演示。 函数通常返回介于 0 到 1 之间的值，但可能短暂地为负值或大于 1（与 `SpringIn` 和 `SpringOut` 函数的情况相似），或者，如果你很清楚当前执行的操作，则可能不会遵循该规则。 [UneasyScale](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/UneasyScale) 示例演示了一个自定义缓动函数，[CustomCubicEase](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/CustomCubicEase) 演示了另一个函数 。

[SwingButton](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SwingButton) 示例也演示了一个自定义缓动函数，还演示了在一系列旋转动画中更改 `AnchorX` 和 `AnchorY` 属性的方法。

[Xamarin.FormsBook.Toolkit](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 库具有 [`JiggleButton`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/JiggleButton.cs) 类，该类使用自定义缓动函数使按钮在单击时抖动。

[JiggleButtonDemo](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/JiggleButtonDemo) 示例对此进行了演示。 进入动画

### <a name="entrance-animations"></a>一种常见的动画类型，在页面第一次显示时出现。

此类动画可以在页面的 [`OnAppearing`](xref:Xamarin.Forms.Page.OnAppearing) 重写中启动。 对于这些动画，最佳做法是针对动画后所需的页面显示方式设置 XAML，然后通过代码初始化布局并对其进行动画处理。 [FadingEntrance](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/FadingEntrance) 示例使用 [`FadeTo`](xref:Xamarin.Forms.ViewExtensions.FadeTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) 扩展方法淡入页面内容。

[SlidingEntrance](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SlidingEntrance) 示例使用 [`TranslateTo`](xref:Xamarin.Forms.ViewExtensions.TranslateTo(Xamarin.Forms.VisualElement,System.Double,System.Double,System.UInt32,Xamarin.Forms.Easing)) 扩展方法从侧面滑入页面内容。

[SwingingEntrance](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SwingingEntrance) 示例使用 [`RotateYTo`](xref:Xamarin.Forms.ViewExtensions.RotateYTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) 扩展方法为 `RotationY` 属性赋予动画效果。

也可以使用 [`RotateXTo`](xref:Xamarin.Forms.ViewExtensions.RotateXTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) 方法。 永久动画

### <a name="forever-animations"></a>“永久”动画是另一种极端情况，会一直运行到程序终止为止。

此类动画通常供演示之用。 [FadingTextAnimation](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/FadingTextAnimation) 示例使用 [`FadeTo`](xref:Xamarin.Forms.ViewExtensions.FadeTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) 动画来淡入和淡出两部分文本。

[PalindromeAnimation](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/PalindromeAnimation) 显示回文，然后按顺序将各个字母旋转 180 度，使它们全部上下颠倒。

然后，整个字符串将翻转 180 度，以读取与原始字符串相同的字符串。 [CopterAnimation](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/CopterAnimation) 示例将旋转出一个简单的 `BoxView` 直升机形状，同时围绕屏幕中心旋转。

[RotatingSpokes](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/RotatingSpokes) 使 `BoxView` 轮辐围绕屏幕中心旋转，然后旋转每个轮辐本身以创建有趣的图案：

[![旋转轮辐的三重屏幕截图](images/ch22fg21-small.png "旋转轮辐")](images/ch22fg21-large.png#lightbox "旋转轮辐")

但是，如 [RotationBreakdown](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/RotationBreakdown) 示例中所示，从长远来看，逐渐增加元素的 `Rotation` 属性可能不起作用。

[SpinningImage](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SpinningImage) 示例使用 [`RotateTo`](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))、[`RotateXTo`](xref:Xamarin.Forms.ViewExtensions.RotateXTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) 和 [`RotateYTo`](xref:Xamarin.Forms.ViewExtensions.RotateYTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) 设置效果，使其看起来像是在 3D 空间中旋转位图。

对 bounds 属性进行动画处理

### <a name="animating-the-bounds-property"></a>尚未演示的 `ViewExtensions` 中唯一的扩展方法是 [`LayoutTo`](xref:Xamarin.Forms.ViewExtensions.LayoutTo(Xamarin.Forms.VisualElement,Xamarin.Forms.Rectangle,System.UInt32,Xamarin.Forms.Easing))，该方法通过调用 [`Layout`](xref:Xamarin.Forms.VisualElement.Layout(Xamarin.Forms.Rectangle)) 方法有效地对只读的 [`Bounds`](xref:Xamarin.Forms.VisualElement.Bounds) 属性进行动画处理。

此方法通常由 `Layout` 派生项调用，将在[第 26 章 CustomLayouts](chapter26.md) 中进行讨论。 `LayoutTo` 方法应仅限于特殊用途。

[BouncingBox](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/BouncingBox) 程序在 `BoxView` 从页面侧面反弹时使用该方法对其进行压缩和展开。 [XamagonXuzzle](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/XamagonXuzzle) 示例使用 `LayoutTo` 在经典的 15-16 拼图的实现中移动磁贴，该拼图显示杂乱的图像而不是编号磁贴：

[![Xamarin Xuzzle 的三重屏幕截图](images/ch22fg26-small.png "Xuzzle 益智解谜游戏")](images/ch22fg26-large.png#lightbox "Xuzzle 益智解谜游戏")

自己的可等待动画

### <a name="your-own-awaitable-animations"></a>[TryAwaitableAnimation](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/TryAwaitableAnimation) 示例创建可等待动画。

在动画完成时，可以从方法返回 `Task` 对象并发出信号的关键类是 [`TaskCompletionSource`](xref:System.Threading.Tasks.TaskCompletionSource`1)。 深入了解动画

## <a name="deeper-into-animations"></a>Xamarin.Forms 动画系统可能会有些混乱。

除了 `Easing` 类，该动画系统还包括 `ViewExtensions`、`Animation` 和 `AnimationExtension` 类。 ViewExtensions 类

### <a name="viewextensions-class"></a>你已经了解了 [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions)。

它定义了九种返回 `Task<bool>` 和 [`CancelAnimations`](xref:Xamarin.Forms.ViewExtensions.CancelAnimations(Xamarin.Forms.VisualElement)) 的方法。 9 个方法中的 7 个方法以转换属性为目标。 另外两种方法是 [`FadeTo`](xref:Xamarin.Forms.ViewExtensions.FadeTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))（面向 [`Opacity`](xref:Xamarin.Forms.VisualElement.Opacity) 属性） 和 [`LayoutTo`](xref:Xamarin.Forms.ViewExtensions.LayoutTo(Xamarin.Forms.VisualElement,Xamarin.Forms.Rectangle,System.UInt32,Xamarin.Forms.Easing))（调用 [`Layout`](xref:Xamarin.Forms.VisualElement.Layout(Xamarin.Forms.Rectangle)) 方法）。 Animation 类

### <a name="animation-class"></a>[`Animation`](xref:Xamarin.Forms.AnimationExtensions) 类具有一个[构造函数](xref:Xamarin.Forms.Animation.%23ctor(System.Action{System.Double},System.Double,System.Double,Xamarin.Forms.Easing,System.Action))，其中有 5 个实参（用于定义回叫和完成方法）和动画形参。

可以通过 [`Add`](xref:Xamarin.Forms.Animation.Add(System.Double,System.Double,Xamarin.Forms.Animation))、[`Insert`](xref:Xamarin.Forms.Animation.Insert(System.Double,System.Double,Xamarin.Forms.Animation))、[`WithConcurrent`](xref:Xamarin.Forms.Animation.WithConcurrent(Xamarin.Forms.Animation,System.Double,System.Double)) 和重载 [`WithConcurrent`](xref:Xamarin.Forms.Animation.WithConcurrent(System.Action{System.Double},System.Double,System.Double,Xamarin.Forms.Easing,System.Double,System.Double)) 来添加子动画。

然后，通过调用 [`Commit`](xref:Xamarin.Forms.Animation.Commit(Xamarin.Forms.IAnimatable,System.String,System.UInt32,System.UInt32,Xamarin.Forms.Easing,System.Action{System.Double,System.Boolean},System.Func{System.Boolean})) 方法启动动画对象。

AnimationExtensions 类

### <a name="animationextensions-class"></a>[`AnimationExtensions`](xref:Xamarin.Forms.AnimationExtensions) 类主要包含扩展方法。

有多种版本的 `Animate` 方法，泛型 [`Animate`](xref:Xamarin.Forms.AnimationExtensions.Animate*) 方法非常通用，因为它实际上是所需的唯一动画函数。 使用 Animation 类

## <a name="working-with-the-animation-class"></a>[ConcurrentAnimations](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ConcurrentAnimations) 示例演示了具有多个不同动画的 [`Animation`](xref:Xamarin.Forms.Animation) 类。

子动画

### <a name="child-animations"></a>[ConcurrentAnimations](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ConcurrentAnimations) 示例还演示了子动画，这些子动画使用了（非常相似的）[`Add`](xref:Xamarin.Forms.Animation.Add(System.Double,System.Double,Xamarin.Forms.Animation)) 和 [`Insert`](xref:Xamarin.Forms.Animation.Insert(System.Double,System.Double,Xamarin.Forms.Animation)) 方法。

高级动画方法之外的内容

### <a name="beyond-the-high-level-animation-methods"></a>[ConcurrentAnimations](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ConcurrentAnimations) 示例还演示了如何执行超出 `ViewExtensions` 方法所针对的属性的动画。

在一个示例中，一系列周期变长；在另一个示例中，将对 `BackgroundColor` 属性进行动画处理。 自己的可等待方法的更多信息

### <a name="more-of-your-own-awaitable-methods"></a>`ViewExtensions` 的 [`TranslateTo`](xref:Xamarin.Forms.ViewExtensions.TranslateTo(Xamarin.Forms.VisualElement,System.Double,System.Double,System.UInt32,Xamarin.Forms.Easing)) 方法不适用于 [`Easing.SpringOut`](xref:Xamarin.Forms.Easing.SpringOut) 函数。

当缓动输出大于 1 时，它将停止。 [Xamarin.FormsBook.Toolkit](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 库包含 [`MoreViewExtensions`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs) 类，该类具有不存在此问题的 [`TranslateXTo`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L12) 和 [`TranslateYTo`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L49) 扩展方法，以及用于取消这些动画的 [`CancelTranslateXTo`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L44) 和 [`CancelTranslateYTo`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L71) 方法。

[SpringSlidingEntrance](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SpringSlidingEntrance) 演示了 `TranslateXTo` 方法。

`MoreExtensions` 类还包含将 X 和 Y 转换结合的 [`TranslateXYTo`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L76) 扩展方法以及 [`CancelTranslateXYTo`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L113) 方法。

实现贝塞尔动画

### <a name="implementing-a-bezier-animation"></a>也可以开发一个动画，使动画沿贝塞尔曲线的路径移动。

[Xamarin.FormsBook.Toolkit](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 库包含 [`BezierSpline`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BezierSpline.cs) 结构，该结构封装贝塞尔曲线和 [`BezierTangent`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BezierTangent.cs) 枚举来控制方向。 [`MoreViewExtensions`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs) 类包含 [`BezierPathTo`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L118) 扩展方法和 [`CancelBezierPathTo`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L161) 方法。

[BezierLoop](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/BezierLoop) 示例演示如何沿着贝塞尔路径对元素进行动画处理。

使用 AnimationExtensions

## <a name="working-with-animationextensions"></a>彩色动画是标准集合中缺少的一种动画类型。

问题在于，在两个 `Color` 值之间没有正确的插入方法。 可以插入单独的 RGB 值，但与插入 HSL 值的有效性相同。 因此，[Xamarin.FormsBook.Toolkit](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 库中的 [`MoreViewExtensions`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs) 类包含两个 `Color` 动画方法：[`RgbColorAnimation`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L166) 和 [`HslColorAnimation`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L188)。

（还有两种取消方法：[`CancelRgbColorAnimation`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L183) 和 [`CancelHslColorAnimation`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L206)）。 这两种方法都使用 [`ColorAnimation`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L211)（通过调用 [`AnimationExtensions`](xref:Xamarin.Forms.AnimationExtensions) 中广泛的泛型 [`Animate`](xref:Xamarin.Forms.AnimationExtensions.Animate*) 方法来执行动画）。

[ColorAnimations](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ColorAnimations) 示例演示如何使用这两种彩色动画类型。

构造动画

## <a name="structuring-your-animations"></a>用 XAML 表达动画并将动画与 MVVM 结合使用有时会很有用。

这将在下一章[第 23 章触发器和行为](chapter23.md)中进行介绍。 相关链接

## <a name="related-links"></a>[第 22 章全文 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch22-Apr2016.pdf)

- [第 22 章示例](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22)
- [动画](~/xamarin-forms/user-interface/animation/index.md)
- 旋转轮辐
