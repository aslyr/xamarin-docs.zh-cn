---
title: title:“摘要：第 14 章. 绝对布局”说明："《使用 Xamarin.Forms 创建移动应用》- 摘要：摘要：第 14 章.
description: '绝对布局”ms.prod: xamarin ms.technology: xamarin-forms ms.assetid:88882A48-3226-42D1-96ED-241250B64A84 author: davidbritch ms.author: dabritch ms.date:2018 年 7 月 19 日 no-loc: [Xamarin.Forms, Xamarin.Essentials] 摘要：第 14 章.'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 88882A48-3226-42D1-96ED-241250B64A84
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 72ee9c4a481388e69aeeb52dbd5b8eeaabb164f6
ms.sourcegitcommit: ea9269b5d9e3d68b61bb428560a10034117ee457
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/10/2020
ms.locfileid: "84136755"
---
# <a name="summary-of-chapter-14-absolute-layout"></a>绝对布局 [![下载示例](~/media/shared/download.png) 下载示例](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14)

与 `StackLayout` 一样，[`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout) 从 `Layout<View>` 派生并继承 `Children` 属性。

`AbsoluteLayout` 实现了一个布局系统，该系统要求程序员指定其子级的位置及其大小（可选）。 该位置由子级的左上角（相对于与设备无关的单元中 `AbsoluteLayout` 的左上角）指定。 `AbsoluteLayout` 还实现了按比例定位和调整大小的功能。 `AbsoluteLayout` 应被视为特殊用途的布局系统，仅在程序员可以设置子级（例如，`BoxView` 元素）的大小或元素的大小不影响其他子级的位置时使用。

`HorizontalOptions` 和 `VerticalOptions` 属性对 `AbsoluteLayout` 的子级不起作用。 本章还将介绍附加的可绑定属性的重要功能，该属性允许将一个类（在本例中为 `AbsoluteLayout`）中定义的属性附加到其他类（`AbsoluteLayout` 的子级）。

代码中的 AbsoluteLayout

## <a name="absolutelayout-in-code"></a>可以使用标准 [`Add`](xref:System.Collections.Generic.ICollection`1.Add*) 方法将子级添加到 `AbsoluteLayout` 的 `Children` 集合中，但是 `AbsoluteLayout` 还提供了扩展的 [`Add`](xref:Xamarin.Forms.AbsoluteLayout.IAbsoluteList`1.Add*) 方法，以便用户可以指定 [`Rectangle`](xref:Xamarin.Forms.Rectangle)。

另一个 [`Add`](xref:Xamarin.Forms.AbsoluteLayout.IAbsoluteList`1.Add*) 方法仅需要 [`Point`](xref:Xamarin.Forms.Point)，在这种情况下，子级不受约束并可以自行调整大小。 可以使用[构造函数](xref:Xamarin.Forms.Rectangle.%23ctor(System.Double,System.Double,System.Double,System.Double))创建一个 `Rectangle` 值，该构造函数需要四个值 &mdash; 前两个指示子级相对于其父级的左上角的位置，后两个指示子级的大小。

或者，可以使用要求 `Point` 和 [`Size`](xref:Xamarin.Forms.Size) 值的 [constructor](xref:Xamarin.Forms.Rectangle.%23ctor(Xamarin.Forms.Point,Xamarin.Forms.Size))。 这些 `Add` 方法在 [AbsoluteDemo](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/AbsoluteDemo) 中进行演示，后者使用 `Rectangle` 值定位 `BoxView` 元素，而仅使用 `Point` 值定位 `Label` 元素。

[ChessboardFixed](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardFixed) 示例使用 32 个 `BoxView` 元素来创建棋盘图案。

该程序为 `BoxView` 元素提供一个硬编码大小为 35 个单位的正方形。 `AbsoluteLayout` 的 `HorizontalOptions` 和 `VerticalOptions` 设置为 `LayoutOptions.Center`，这会导致 `AbsoluteLayout` 的总大小为 280 个单位的正方形。 附加的可绑定属性

## <a name="attached-bindable-properties"></a>在使用静态方法 [`AbsoluteLayout.SetLayoutBounds`](xref:Xamarin.Forms.AbsoluteLayout.SetLayoutBounds(Xamarin.Forms.BindableObject,Xamarin.Forms.Rectangle)) 将 `AbsoluteLayout` 的子级添加到 `Children` 集合中之后，还可以设置其位置及其子级的大小（可选）。

第一个参数是子级；第二个参数是 `Rectangle` 对象。 可以通过将宽度和高度值设置为 [`AbsoluteLayout.AutoSize`](xref:Xamarin.Forms.AbsoluteLayout.AutoSize) 常量来指定子级自身在水平和/或垂直方向上的大小。 [ChessboardDynamic](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardDynamic) 示例将 `AbsoluteLayout` 置于 `ContentView` 中，并使用 `SizeChanged` 处理程序对所有子级调用 `AbsoluteLayout.SetLayoutBounds` 以使其尽可能大。

`AbsoluteLayout` 定义的附加可绑定属性是类型为 `BindableProperty` 且名称为 [`AbsoluteLayout.LayoutBoundsProperty`](xref:Xamarin.Forms.AbsoluteLayout.LayoutBoundsProperty) 的静态只读字段。  

静态 `AbsoluteLayout.SetLayoutBounds` 方法是通过使用 `AbsoluteLayout.LayoutBoundsProperty` 对子级调用 `SetValue` 来实现的。 该子级包含一个字典，其中存储了附加的可绑定属性及其值。 在布局期间，`AbsoluteLayout` 可以通过调用 [`AbsoluteLayout.GetLayoutBounds`](xref:Xamarin.Forms.AbsoluteLayout.GetLayoutBounds(Xamarin.Forms.BindableObject))（通过 `GetValue` 调用实现）获取该值。 按比例调整大小和定位

## <a name="proportional-sizing-and-positioning"></a>`AbsoluteLayout` 实现了按比例调整大小和定位的功能。

类使用相关的静态方法 [`AbsoluteLayout.SetLayoutFlags`](xref:Xamarin.Forms.AbsoluteLayout.SetLayoutFlags(Xamarin.Forms.BindableObject,Xamarin.Forms.AbsoluteLayoutFlags)) 和 [`AbsoluteLayout.GetLayoutFlags`](xref:Xamarin.Forms.AbsoluteLayout.GetLayoutFlags(Xamarin.Forms.BindableObject)) 定义了第二个附加的可绑定属性 [`LayoutFlagsProperty`](xref:Xamarin.Forms.AbsoluteLayout.LayoutFlagsProperty)。 `AbsoluteLayout.SetLayoutFlags` 的自变量和 `AbsoluteLayout.GetLayoutFlags` 的返回值是类型为 [`AbsoluteLayoutFlags`](xref:Xamarin.Forms.AbsoluteLayoutFlags) 的值，它是一个包含以下成员的枚举：

[`None`](xref:Xamarin.Forms.AbsoluteLayoutFlags.None)（等于 0）

- [`XProportional`](xref:Xamarin.Forms.AbsoluteLayoutFlags.XProportional) (1)
- [`YProportional`](xref:Xamarin.Forms.AbsoluteLayoutFlags.YProportional) (2)
- [`PositionProportional`](xref:Xamarin.Forms.AbsoluteLayoutFlags.PositionProportional) (3)
- [`WidthProportional`](xref:Xamarin.Forms.AbsoluteLayoutFlags.WidthProportional) (4)
- [`HeightProportional`](xref:Xamarin.Forms.AbsoluteLayoutFlags.HeightProportional) (8)
- [`SizeProportional`](xref:Xamarin.Forms.AbsoluteLayoutFlags.SizeProportional) (12)
- [`All`](xref:Xamarin.Forms.AbsoluteLayoutFlags.All) (\xFFFFFFFF)
- 可以将它们与 C# 位 OR 运算符结合使用。

设置这些标记后，将按比例解释用于定位和设置子级大小的 `Rectangle` 布局边界结构的某些属性。

设置 `WidthProportional` 标记后，`Width` 的值为 1 表示子级与 `AbsoluteLayout` 的宽度相同。

对高度使用类似的方法。 按比例定位时会考虑大小。

设置 `XProportional` 标记时，`Rectangle` 布局边界的 `X` 属性是成比例的。 值为 0 表示子级的左边缘位于 `AbsoluteLayout` 的左边缘，而位置为 1 则意味着子级的右边缘位于 `AbsoluteLayout` 的右边缘，不超出 `AbsoluteLayout` 的右边缘，正如你预料的一样。 `X` 属性的值为 0.5，则子级在 `AbsoluteLayout` 中水平居中。 [ChessboardProportional](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardProportional) 示例演示了按比例调整大小和定位的使用方法。

使用按比例坐标

## <a name="working-with-proportional-coordinates"></a>有时，与 `AbsoluteLayout` 中实现定位的方式相比，更容易想到按比例定位。

你可能更愿意使用按比例坐标，其中 `X` 属性为 1 会相对于 `AbsoluteLayout` 的右边缘定位子级的左边缘（而不是右边缘）。 该替代定位方案可以称为“分数子坐标”。

可以使用以下公式将分数子坐标转换为 `AbsoluteLayout` 所需的布局边界： layoutBounds.X = (fractionalChildCoordinate.X / (1 - layoutBounds.Width))

layoutBounds.Y = (fractionalChildCoordinate.Y / (1 - layoutBounds.Height))

[ProportionalCoordinateCalc](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/PropCoordCalc) 示例对此进行了演示。

AbsoluteLayout 和 XAML

## <a name="absolutelayout-and-xaml"></a>可以在 XAML 中使用 `AbsoluteLayout`，并使用属性值 `AbsoluteLayout.LayoutBounds` 和 `AbsoluteLayout.LayoutFlags` 在 `AbsoluteLayout` 的子级上设置附加的可绑定属性。

[AbsoluteXamlDemo](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/AbsoluteXamlDemo) 和 [ChessboardXaml](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardXaml) 示例对此进行了演示。 后一个程序包含 32 个 `BoxView` 元素，但使用包含 `AbsoluteLayout.LayoutFlags` 属性的隐式 `Style` 来将标记保持为最小值。 XAML 中由类名称、点和属性名称组成的属性始终为附加的可绑定属性。

叠加

## <a name="overlays"></a>可以使用 `AbsoluteLayout` 来构造覆盖，它用其他控件覆盖页面，可能是为了防止用户与页面上的常规控件进行交互。

[SimpleOverlay](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/SimpleOverlay) 示例演示了此技术，还演示了 [`ProgressBar`](xref:Xamarin.Forms.ProgressBar)，它显示了程序完成任务的程度。

一些有趣的内容

## <a name="some-fun"></a>[DotMatrixClock](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/DotMatrixClock) 示例使用模拟的 5x7 点矩阵显示来显示当前时间。

每个点的大小为 `BoxView`（其中包含 228 个），位于 `AbsoluteLayout` 上。 [![点阵时钟的三倍屏幕截图](images/ch14fg08-small.png "点阵时钟")](images/ch14fg08-large.png#lightbox "点阵时钟")

[BouncingText](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/BouncingText) 程序对两个 `Label` 对象进行动画处理，使其在屏幕上水平和垂直弹跳。

相关链接

## <a name="related-links"></a>[第 14 章全文 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch14-Apr2016.pdf)

- [第 14 章示例](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14)
- [AbsoluteLayout](~/xamarin-forms/user-interface/layouts/absolute-layout.md)
- [附加属性](~/xamarin-forms/xaml/attached-properties.md)
- 点阵时钟
