---
title: “第 4 章： 滚动堆叠”摘要
description: 使用 Xamarin.Forms 创建移动应用：“第 4 章： 滚动堆叠”摘要
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 7A39FD4F-15AD-4F94-960E-9FEEB63FFD44
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
ms.openlocfilehash: bda9d5cb323524981bed9c3bb55998513dd69aab
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/10/2020
ms.locfileid: "73032879"
---
# <a name="summary-of-chapter-4-scrolling-the-stack"></a>“第 4 章： 滚动堆叠”摘要

[![下载示例](~/media/shared/download.png) 下载示例](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04)

本章节主要介绍了“布局”  概念，这是 Xamarin.Forms 用于在页面上组织多个视图的可视化显示的类和技术的总称。

布局涉及多个派生自 [`Layout`](xref:Xamarin.Forms.Layout) 和 [`Layout<T>`](xref:Xamarin.Forms.Layout`1) 的类。 本章节重点介绍了 [`StackLayout`](xref:Xamarin.Forms.StackLayout)。

> [!NOTE]
> Xamarin.Forms 3.0 中引入的 [`FlexLayout`](~/xamarin-forms/user-interface/layouts/flex-layout.md) 的使用方式与 `StackLayout` 相似，但更加灵活。

本章节还介绍了 [`ScrollView`](xref:Xamarin.Forms.ScrollView)、[`Frame`](xref:Xamarin.Forms.Frame) 和 [`BoxView`](xref:Xamarin.Forms.BoxView) 类。

## <a name="stacks-of-views"></a>视图堆叠

[`StackLayout`](xref:Xamarin.Forms.StackLayout) 派生自 `Layout<View>`，并继承类型 `IList<View>` 的 [`Children`](xref:Xamarin.Forms.Layout`1) 属性。 可以向此集合添加多个视图项，`StackLayout` 在水平或垂直堆叠中显示它们。

将 `StackLayout` 的 [`Orientation`](xref:Xamarin.Forms.StackLayout.Orientation) 属性设置为 [`StackOrientation`](xref:Xamarin.Forms.StackOrientation) 枚举的成员（[`Vertical`](xref:Xamarin.Forms.StackOrientation.Vertical) 或 [`Horizontal`](xref:Xamarin.Forms.StackOrientation.Horizontal)）。 默认值为 `Vertical`。

将 `StackLayout` 的 [`Spacing`](xref:Xamarin.Forms.StackLayout.Spacing) 属性设置为 `double` 值，可以指定子元素之间的间距。 默认值为 6。

在代码中，可以在 `for` 或 `foreach` 循环中将项添加到 `StackLayout` 的 `Children` 集合，如 [ColorLoop  ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorLoop) 示例所示；也可以使用单独视图的列表来初始化 `Children` 集合，如 [ColorList  ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorList) 所示。 子元素必须派生自 `View`，但也可以包含其他 `StackLayout` 对象。

## <a name="scrolling-content"></a>滚动内容

如果 `StackLayout` 包含过多子元素，无法显示在一个页面上，可以将 `StackLayout` 置于 [`ScrollView`](xref:Xamarin.Forms.ScrollView) 中，以允许滚动。

将 `ScrollView` 的 [`Content`](xref:Xamarin.Forms.ScrollView.Content) 属性设置为要滚动的视图。 这通常是 `StackLayout`，但它可以是任意视图。

将 `ScrollView` 的 [`Orientation`](xref:Xamarin.Forms.ScrollView.Orientation) 属性设置为 [`ScrollOrientation`](xref:Xamarin.Forms.ScrollOrientation) 属性的成员（[`Vertical`](xref:Xamarin.Forms.ScrollOrientation.Vertical)、[`Horizontal`](xref:Xamarin.Forms.ScrollOrientation.Horizontal) 或 [`Both`](xref:Xamarin.Forms.ScrollOrientation.Both)）。 默认值为 `Vertical`。 如果 `ScrollView` 的内容是 `StackLayout`，这两个方向应一致。

[ReflectedColors  ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ReflectedColors) 示例展示了如何使用 `ScrollView` 和 `StackLayout` 显示可用颜色。 此示例还展示了如何使用 .NET 反射来获取 `Color` 结构的所有公共静态属性和字段，而无需显式列出它们。

## <a name="the-expands-option"></a>Expands 选项

当 `StackLayout` 堆叠它的子元素时，每个子元素都会在 `StackLayout` 的总高度中占据一个特定位置，具体视子元素的尺寸及其 `HorizontalOptions` 和 `VerticalOptions` 属性设置而定。 这些属性分配有 [`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions) 类型的值。

`LayoutOptions` 结构定义了两个属性：

- [`LayoutAlignment`](xref:Xamarin.Forms.LayoutAlignment) 枚举类型的 [`Alignment`](xref:Xamarin.Forms.LayoutOptions.Alignment)：有四个成员，分别为 [`Start`](xref:Xamarin.Forms.LayoutAlignment.Start)、[`Center`](xref:Xamarin.Forms.LayoutAlignment.Center)、[`End`](xref:Xamarin.Forms.LayoutAlignment.End) 和 [`Fill`](xref:Xamarin.Forms.LayoutAlignment.Fill)
- `bool` 类型的 [`Expands`](xref:Xamarin.Forms.LayoutOptions.Expands)

为了方便起见，`LayoutOptions` 结构还定义了 `LayoutOptions` 类型的八个静态只读字段，它们包含两个实例属性的所有组合：

- [`LayoutOptions.Start`](xref:Xamarin.Forms.LayoutOptions.Start)
- [`LayoutOptions.Center`](xref:Xamarin.Forms.LayoutOptions.Center)
- [`LayoutOptions.End`](xref:Xamarin.Forms.LayoutOptions.End)
- [`LayoutOptions.Fill`](xref:Xamarin.Forms.LayoutOptions.Fill)
- [`LayoutOptions.StartAndExpand`](xref:Xamarin.Forms.LayoutOptions.StartAndExpand)
- [`LayoutOptions.CenterAndExpand`](xref:Xamarin.Forms.LayoutOptions.CenterAndExpand)
- [`LayoutOptions.EndAndExpand`](xref:Xamarin.Forms.LayoutOptions.EndAndExpand)
- [`LayoutOptions.FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand)

下面的讨论涉及采用默认垂直方向的 `StackLayout`。 水平 `StackLayout` 是类似的。

对于垂直 `StackLayout`，`HorizontalOptions` 设置决定了子元素在 `StackLayout` 宽度内的水平位置。 如果 `Alignment` 设置为 `Start`、`Center` 或 `End`，子元素在水平方向不受约束。 子元素确定自己的宽度，并位于 `StackLayout` 的左侧、中间或右侧。 `Fill` 选项会令子元素在水平方向受约束，并占满 `StackLayout` 的宽度。

对于垂直 `StackLayout`，每个子元素在垂直方向都不受约束，并根据子元素的高度获取垂直槽（在这种情况下，`VerticalOptions` 设置不相关）。

如果垂直 `StackLayout` 本身不受约束（即如果它的 `VerticalOptions` 设置为 `Start`、`Center` 或 `End`），则 `StackLayout` 的高度就是它的子元素的总高度。

不过，如果垂直 `StackLayout` 在垂直方向受约束（即如果它的 `VerticalOptions` 设置为 `Fill`），则 `StackLayout` 的高度就是其容器的高度，可能大于其子元素的总高度。 在这种情况下，如果至少有一个子元素有 `VerticalOptions` 设置和值为 `true` 的 `Expands` 标志，则 `StackLayout` 中的额外空间会在所有这些有值为 `true` 的 `Expands` 标志的子元素之间平均分配。 子元素的总高度等于 `StackLayout` 的高度，而 `VerticalOptions` 设置的 `Alignment` 部分则决定了子元素在槽中的垂直定位。

[VerticalOptionsDemo  ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/VerticalOptionsDemo) 示例对此进行了展示。

## <a name="frame-and-boxview"></a>Frame 和 BoxView

这两个矩形视图通常用于呈现目的。

[`Frame`](xref:Xamarin.Forms.Frame) 视图在另一个视图（可以是诸如 `StackLayout` 之类的布局）周围显示矩形框架。 `Frame` 从 [`ContentView`](xref:Xamarin.Forms.ContentView)（设置为要在 `Frame` 中显示的视图）继承 [`Content`](xref:Xamarin.Forms.ContentView.Content) 属性。 默认情况下，`Frame` 是透明的。 设置以下三个属性，可以自定义框架外观：

- [`OutlineColor`](xref:Xamarin.Forms.Frame.OutlineColor) 属性：让框架可见。 如果不知道基础配色方案，通常将 `OutlineColor` 设置为 `Color.Accent`。
- [`HasShadow`](xref:Xamarin.Forms.Frame.HasShadow) 属性：可以设置为 `true`，以便在 iOS 设备上显示黑色阴影。
- [`Padding`](xref:Xamarin.Forms.Layout.Padding) 属性：设置为 `Thickness` 值可以在框架和框架内容之间留出空间。 默认值是每边 20 个单位。

`Frame` 的 `HorizontalOptions` 和 `VerticalOptions` 默认值为 `LayoutOptions.Fill`（即 `Frame` 会填充它的容器）。 在其他设置中，`Frame` 的尺寸取决于其内容的尺寸。

[FramedText  ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/FramedText) 示例展示了 `Frame`。

[`BoxView`](xref:Xamarin.Forms.BoxView) 会显示矩形区域，颜色由其 [`Color`](xref:Xamarin.Forms.BoxView.Color) 属性指定。

如果 `BoxView` 受约束（即它的 `HorizontalOptions` 和 `VerticalOptions` 属性的默认设置为 `LayoutOptions.Fill`），则 `BoxView` 填充可用空间。 如果 `BoxView` 不受约束（即 `HorizontalOptions` 和 `LayoutOptions` 设置为 `Start`、`Center` 或 `End`），则它的默认尺寸为 40 个平方单位。 `BoxView` 可以在一个维度上受约束，而在另一个维度上不受约束。

通常会设置 `BoxView` 的 [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) 和 [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) 属性，以为它指定具体尺寸。 [SizedBoxView  ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/SizedBoxView) 示例对此进行了展示。

可以使用 `StackLayout` 的多个实例，在 `Frame` 中组合一个 `BoxView` 和多个 `Label` 实例，以显示特定颜色，然后将每个视图置于 `ScrollView` 的 `StackLayout` 中，以创建有吸引力的颜色列表，如 [ColorBlocks  ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorBlocks) 示例所示：

[![颜色块的三倍屏幕截图](images/ch04fg11-small.png "颜色列表")](images/ch04fg11-large.png#lightbox "颜色列表")

## <a name="a-scrollview-in-a-stacklayout"></a>StackLayout 中的 ScrollView？

将 `StackLayout` 置于 `ScrollView` 中很常见，但将 `ScrollView` 置于 `StackLayout` 中有时也很方便。 理论上，这是不可能的，因为垂直 `StackLayout` 的子元素在垂直方向不受约束。 但 `ScrollView` 必须在垂直方向受约束。 必须为它指定具体高度，这样它才能确定用于滚动的子元素的尺寸。

诀窍是为 `StackLayout` 的 `ScrollView` 子元素指定 `FillAndExpand` 的 `VerticalOptions` 设置。 [BlackCat  ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/BlackCat) 示例对此进行了展示。

BlackCat  示例还展示了如何定义和访问嵌入到共享库中的程序资源。 这也可以通过共享资产项目 (SAP) 来实现，但过程有点棘手，如 [BlackCatSap  ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/BlackCatSap) 示例所示。

## <a name="related-links"></a>相关链接

- [第 4 章全文 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch04-Apr2016.pdf)
- [第 4 章示例](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04)
- [第 4 章 F# 示例](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/FS)
- [StackLayout](~/xamarin-forms/user-interface/layouts/stack-layout.md)
- [ScrollView](~/xamarin-forms/user-interface/layouts/scroll-view.md)
- [BoxView](~/xamarin-forms/user-interface/boxview.md)
