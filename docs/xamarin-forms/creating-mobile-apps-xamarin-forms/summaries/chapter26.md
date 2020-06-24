---
title: “第 26 章： 自定义布局
description: 使用 Xamarin.Forms 创建移动应用：“第 26 章： 自定义布局
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 2B7F4346-414E-49FF-97FB-B85E92D98A21
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: deb46d1a70e7c707c998be8669b4af3b8e8d7ead
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/18/2020
ms.locfileid: "84136599"
---
# <a name="summary-of-chapter-26-custom-layouts"></a>“第 26 章： 自定义布局

[![下载示例](~/media/shared/download.png) 下载示例](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26)

Xamarin.Forms 包含多个派生自 [`Layout<View>`](xref:Xamarin.Forms.Layout`1) 的类：

- `StackLayout`,
- `Grid`,
- `AbsoluteLayout` 和
- `RelativeLayout`。

本章节介绍了如何创建你自己的派生自 `Layout<View>` 的类。

## <a name="an-overview-of-layout"></a>布局概述

没有处理 Xamarin.Forms 布局的集中系统。 每个元素都负责确定自身尺寸，以及如何在特定区域中呈现自身。

### <a name="parents-and-children"></a>父元素和子元素

每个有子元素的元素都负责在自身内部定位这些子元素。 最终由父元素根据可用尺寸和子元素所需尺寸来决定子元素的尺寸。

### <a name="sizing-and-positioning"></a>调整大小和定位

布局从页面的可视树的顶部开始，然后遍历所有分支。 布局中最重要的公共方法是由 `VisualElement` 定义的 [`Layout`](xref:Xamarin.Forms.VisualElement.Layout(Xamarin.Forms.Rectangle))。 作为其他元素的父元素，每个元素都会对自己的所有子元素调用 `Layout`，以 [`Rectangle`](xref:Xamarin.Forms.Rectangle) 值的形式为子元素指定相对于自身的尺寸和位置。 这些 `Layout` 调用通过可视化树传播。

若要让某元素出现在屏幕上，必须调用 `Layout`，这会设置以下只读属性。 它们与传递给方法的 `Rectangle` 一致：

- `Rectangle` 类型的 [`Bounds`](xref:Xamarin.Forms.VisualElement.Bounds)
- `double` 类型的 [`X`](xref:Xamarin.Forms.VisualElement.X)
- `double` 类型的 [`Y`](xref:Xamarin.Forms.VisualElement.Y)
- `double` 类型的 [`Width`](xref:Xamarin.Forms.VisualElement.Width)
- `double` 类型的 [`Height`](xref:Xamarin.Forms.VisualElement.Height)

在 `Layout` 调用前，`Height` 和 `Width` 的 mock 值为 &ndash;1。

调用 `Layout` 还会触发对以下受保护方法的调用：

- [`SizeAllocated`](xref:Xamarin.Forms.VisualElement.SizeAllocated(System.Double,System.Double))：调用
- [`OnSizeAllocated`](xref:Xamarin.Forms.VisualElement.OnSizeAllocated(System.Double,System.Double))：可重写。

最后，以下事件触发：

- [`SizeChanged`](xref:Xamarin.Forms.VisualElement.SizeChanged)

`OnSizeAllocated` 方法由 `Page` 和 `Layout` 替代，这两个类是 Xamarin.Forms 中唯一可以有子元素的类。 重写的方法调用

- [`UpdateChildrenLayout`](xref:Xamarin.Forms.Page.UpdateChildrenLayout)：对于 `Page` 衍生物；[`UpdateChildrenLayout`](xref:Xamarin.Forms.Layout.UpdateChildrenLayout)：对于 `Layout` 衍生物；调用
- [`LayoutChildren`](xref:Xamarin.Forms.Page.LayoutChildren(System.Double,System.Double,System.Double,System.Double))：对于 `Page` 衍生物；[`LayoutChildren`](xref:Xamarin.Forms.Layout.LayoutChildren(System.Double,System.Double,System.Double,System.Double))：对于 `Layout` 衍生物。

然后，`LayoutChildren` 对每个元素的子元素调用 `Layout`。 如果至少一个子元素有新的 `Bounds` 设置，则会触发以下事件：

- [`LayoutChanged`](xref:Xamarin.Forms.Page.LayoutChanged)：对于 `Page` 衍生物；[`LayoutChanged`](xref:Xamarin.Forms.Layout.LayoutChanged)：对于 `Layout` 衍生物

### <a name="constraints-and-size-requests"></a>约束和尺寸请求

为了能够智能地对自己的所有子元素调用 `Layout`，`LayoutChildren` 必须知道子元素的首选** 或所需** 尺寸。 因此，对每个子元素调用 `Layout` 之前，通常先调用

- [`GetSizeRequest`](xref:Xamarin.Forms.VisualElement.GetSizeRequest(System.Double,System.Double))

在本书出版后，`GetSizeRequest` 方法已遭弃用，并替换为

- [`Measure`](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags))

`Measure` 方法充分考虑 [`Margin`](xref:Xamarin.Forms.View.Margin) 属性，并包含 [`MeasureFlag`](xref:Xamarin.Forms.MeasureFlags) 类型的参数，它有两个成员：

- [`IncludeMargins`](xref:Xamarin.Forms.MeasureFlags.IncludeMargins)
- [`None`](xref:Xamarin.Forms.MeasureFlags.None)：没有边距

对于许多元素，`GetSizeRequest` 或 `Measure` 从呈现器获取元素的本机尺寸。 这两种方法都有宽度和高度约束** 参数。 例如，`Label` 使用宽度约束来确定如何换行多行文本。

`GetSizeRequest` 和 `Measure` 都返回 [`SizeRequest`](xref:Xamarin.Forms.SizeRequest) 类型的值，它有两个属性：

- `Size` 类型的 [`Request`](xref:Xamarin.Forms.SizeRequest.Request)
- `Size` 类型的 [`Minimum`](xref:Xamarin.Forms.SizeRequest.Minimum)

这两个值通常是相同的，且一般可以忽略 `Minimum` 值。

`VisualElement` 还定义了类似于 `GetSizeRequest` 的受保护方法，它是从 `GetSizeRequest` 调用：

- [`OnSizeRequest`](xref:Xamarin.Forms.VisualElement.OnSizeRequest(System.Double,System.Double))：返回 `SizeRequest` 值

此方法现已遭弃用，并替换为：

- [`OnMeasure`](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double))

每个派生自 `Layout` 或 `Layout<T>` 的类都必须重写 `OnSizeRequest` 或 `OnMeasure`。 正是在这一方面，布局类确定自己的尺寸，这通常基于子元素的尺寸（通过对子元素调用 `GetSizeRequest` 或 `Measure` 来获取）。 在调用 `OnSizeRequest` 或 `OnMeasure` 前后，`GetSizeRequest` 或 `Measure` 根据以下属性进行调整：

- `double` 类型的 [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest)：影响的 `SizeRequest` 的 `Request` 属性
- `double` 类型的 [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest)：影响的 `SizeRequest` 的 `Request` 属性
- `double` 类型的 [`MinimumWidthRequest`](xref:Xamarin.Forms.VisualElement.MinimumWidthRequest)：影响的 `SizeRequest` 的 `Minimum` 属性
- `double` 类型的 [`MinimumHeightRequest`](xref:Xamarin.Forms.VisualElement.MinimumHeightRequest)：影响的 `SizeRequest` 的 `Minimum` 属性

### <a name="infinite-constraints"></a>无限约束

传递给 `GetSizeRequest`（或 `Measure`）和 `OnSizeRequest`（或 `OnMeasure`）的约束参数可以是无限的（即值为 `Double.PositiveInfinity`）。 不过，从这些方法返回的 `SizeRequest` 不得包含无限尺寸。

无限约束表明，请求的尺寸应反映元素的自然尺寸。 垂直 `StackLayout` 对其子元素调用 `GetSizeRequest`（或 `Measure`），设有无限高度约束。 水平堆积布局对其子元素调用 `GetSizeRequest`（或 `Measure`），设有无限宽度约束。 `AbsoluteLayout` 对其子元素调用 `GetSizeRequest`（或 `Measure`），设有无限宽度和高度约束。

### <a name="peeking-inside-the-process"></a>过程一窥

[ExploreChildSize****](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/ExploreChildSizes) 显示简单布局的约束和尺寸请求信息。

## <a name="deriving-from-layoutview"></a>派生自 Layout\<View>

自定义布局类派生自 `Layout<View>`。 它有两个责任：

- 将 `OnMeasure` 重写为对布局的所有子元素调用 `Measure`。 返回布局本身的请求尺寸
- 将 `LayoutChildren` 重写为对布局的所有子元素调用 `Layout`

这些重写中的 `for` 或 `foreach` 循环应跳过 `IsVisible` 属性设置为 `false` 的任何子元素。

不保证调用 `OnMeasure`。 如果布局的父元素控制布局尺寸（例如，填充页面的布局），则不会调用 `OnMeasure`。 因此，`LayoutChildren` 无法依赖在 `OnMeasure` 调用期间获得的子元素尺寸。 通常情况下，`LayoutChildren` 本身必须对布局的子元素调用 `Measure`，你也可以实现某种尺寸缓存逻辑（稍后将进行介绍）。

### <a name="an-easy-example"></a>简单示例

[VerticalStackDemo****](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/VerticalStackDemo) 示例包含简化的 [`VerticalStack`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter26/VerticalStackDemo/VerticalStackDemo/VerticalStackDemo/VerticalStack.cs) 类及其用法演示。

### <a name="vertical-and-horizontal-positioning-simplified"></a>简化了垂直和水平定位

`VerticalStack` 必须执行的作业之一发生在 `LayoutChildren` 重写期间。 此方法使用子元素的 `HorizontalOptions` 属性，以确定如何在 `VerticalStack` 中的槽内定位子元素。 你可以改为调用静态方法 [`Layout.LayoutChildIntoBoundingRect`](xref:Xamarin.Forms.Layout.LayoutChildIntoBoundingRegion(Xamarin.Forms.VisualElement,Xamarin.Forms.Rectangle))。 此方法对子元素调用 `Measure`，并使用它的 `HorizontalOptions` 和 `VerticalOptions` 属性在指定矩形中定位子元素。

### <a name="invalidation"></a>失效

更改元素属性通常会影响此元素在布局中的显示方式。 必须让布局无效，才能触发新布局。

`VisualElement` 定义了受保护方法 [`InvalidateMeasure`](xref:Xamarin.Forms.VisualElement.InvalidateMeasure)，它通常由更改会影响元素尺寸的任何可绑定属性的属性更改处理程序调用。 `InvalidateMeasure` 方法触发 [`MeasureInvalidated`](xref:Xamarin.Forms.VisualElement.MeasureInvalidated) 事件。

`Layout` 类定义了类似的受保护方法，它名为 [`InvalidateLayout`](xref:Xamarin.Forms.Layout.InvalidateLayout) 且是 `Layout` 衍生物，应对任何影响子元素定位和尺寸的更改调用它。

### <a name="some-rules-for-coding-layouts"></a>编码布局的一些规则

1. `Layout<T>` 衍生物定义的属性应由可绑定属性支持，且属性更改处理程序应调用 `InvalidateLayout`。

2. 定义附加的可绑定属性的 `Layout<T>` 衍生物应将 [`OnAdded`](xref:Xamarin.Forms.Layout`1.OnAdded*) 重写为将属性更改处理程序添加到子元素中，并将 [`OnRemoved`](xref:Xamarin.Forms.Layout`1.OnRemoved*) 重写为删除此处理程序。 此处理程序应检查这些附加的可绑定属性是否有更改，并通过调用 `InvalidateLayout` 来响应。

3. 实现子元素尺寸缓存的 `Layout<T>` 衍生物应重写 `InvalidateLayout` 和 [`OnChildMeasureInvalidated`](xref:Xamarin.Forms.Layout.OnChildMeasureInvalidated)，并在这些方法获得调用时清除缓存。

### <a name="a-layout-with-properties"></a>带属性的布局

[Xamarin.FormsBook.Toolkit****](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 中的 [`WrapLayout`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/WrapLayout.cs) 类假设所有子元素都相同，并将子元素从一行（或列）换到下一行。 它定义了 `Orientation` 属性（像 `StackLayout` 一样），定义了 `ColumnSpacing` 和 `RowSpacing` 属性（像 `Grid` 一样），并缓存了子元素尺寸。

[PhotoWrap****](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/PhotoWrap) 示例将 `WrapLayout` 置于用于显示图库照片的 `ScrollView` 中。

### <a name="no-unconstrained-dimensions-allowed"></a>不允许使用不受约束的尺寸！

[Xamarin.FormsBook.Toolkit****](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 库中的 [`UniformGridLayout`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/UniformGridLayout.cs) 旨在在自身内部显示所有子元素。 因此，它无法处理不受约束的尺寸；如果遇到，就会抛出异常。

[PhotoGrid****](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/PhotoGrid) 示例展示了 `UniformGridLayout`：

[![照片网格的三倍屏幕截图](images/ch26fg08-small.png "统一网格布局")](images/ch26fg08-large.png#lightbox "统一网格布局")

### <a name="overlapping-children"></a>重叠子元素

`Layout<T>` 衍生物可能会与其子元素重叠。 不过，子元素按其在 `Children` 集合中的顺序呈现，而不是按其 `Layout` 方法的调用顺序。

`Layout` 类定义了两个方法，可便于在集合中移动子元素：

- [`LowerChild`](xref:Xamarin.Forms.Layout.LowerChild(Xamarin.Forms.View))：将子元素移动到集合开头
- [`RaiseChild`](xref:Xamarin.Forms.Layout.RaiseChild(Xamarin.Forms.View))：将子元素移动到集合末尾

对于重叠子元素，从视觉上看，集合末尾的子元素会显示在集合开头的子元素之上。

[Xamarin.FormsBook.Toolkit****](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 库中的 [`OverlapLayout`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/OverlapLayout.cs) 类定义了附加属性来指明呈现顺序，这样它的一个子元素就能显示在其他子元素之上。 [StudentCardFile****](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/StudentCardFile) 示例对此进行了展示：

[![学生卡文件网格的三倍屏幕截图](images/ch26fg10-small.png "重叠布局子元素")](images/ch26fg10-large.png#lightbox "重叠布局子元素")

### <a name="more-attached-bindable-properties"></a>更多附加的可绑定属性

[Xamarin.FormsBook.Toolkit****](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 库中的 [`CartesianLayout`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CartesianLayout.cs) 类定义了附加的可绑定属性，以指定两个 `Point` 值和粗细值，并控制 `BoxView` 元素，使其类似于线条。

[UnitCube****](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/UnitCube) 示例使用此类绘制 3D 立方体。

### <a name="layout-and-layoutto"></a>Layout 和 LayoutTo

`Layout<T>` 衍生物可以调用 `LayoutTo`（而不是 `Layout`），以为布局添加动画效果。 [`AnimatedCartesianLayout`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AnimatedCartesianLayout.cs) 类可实现此目的，[AnimatedUnitCube****](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/AnimatedUnitCube) 示例对此进行了展示。

## <a name="related-links"></a>相关链接

- [第 26 章全文 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch26-Apr2016.pdf)
- [第 26 章示例](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26)
- [创建自定义布局](~/xamarin-forms/user-interface/layouts/custom.md)
