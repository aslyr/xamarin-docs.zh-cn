---
title: “第 17 章： 掌握网格
description: 使用 Xamarin.Forms 创建移动应用：“第 17 章： 掌握网格”的摘要
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 71EDEF9C-4220-4D2E-A235-43F1EC8746C1
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 6dd13c0f592831c6488afac6727bcac734e9136a
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/18/2020
ms.locfileid: "84136716"
---
# <a name="summary-of-chapter-17-mastering-the-grid"></a>“第 17 章： 掌握网格”的摘要

[![下载示例](~/media/shared/download.png) 下载示例](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17)

[`Grid`](xref:Xamarin.Forms.Grid) 是一种功能强大的布局机制，可以将自己的子元素排列为成行、成列的单元格。 与类似的 HTML `table` 元素不同，`Grid` 仅用于布局，而不用于呈现目的。

## <a name="the-basic-grid"></a>基本网格

`Grid` 派生自 [`Layout<View>`](xref:Xamarin.Forms.Layout`1)，后者定义了 `Grid` 继承的 [`Children`](xref:Xamarin.Forms.Layout`1.Children) 属性。 可以使用 XAML 或代码填充此集合。

### <a name="the-grid-in-xaml"></a>XAML 中的网格

XAML 中的 `Grid` 定义通常始于使用 [`RowDefinition`](xref:Xamarin.Forms.RowDefinition) 和 [`ColumnDefinition`](xref:Xamarin.Forms.ColumnDefinition) 对象填充 `Grid` 的 [`RowDefinitions`](xref:Xamarin.Forms.Grid.RowDefinitions) 和 [`ColumnDefinitions`](xref:Xamarin.Forms.Grid.ColumnDefinitions) 集合。 这样，可以确定 `Grid` 的行数和列数及其属性。

`RowDefinition` 有 [`Height`](xref:Xamarin.Forms.RowDefinition.Height) 属性，`ColumnDefinition` 有 [`Width`](xref:Xamarin.Forms.ColumnDefinition.Width) 属性，这两个集合都是类型为 [`GridLength`](xref:Xamarin.Forms.GridLength) 的结构。

在 XAML 中，[`GridLengthTypeConverter`](xref:Xamarin.Forms.GridLengthTypeConverter) 将简单文本字符串转换为 `GridLength` 值。 在后台，[`GridLength` constructor](xref:Xamarin.Forms.GridLength.%23ctor(System.Double,Xamarin.Forms.GridUnitType)) 根据类型 [`GridUnitType`](xref:Xamarin.Forms.GridUnitType)（包含以下三个成员的枚举）的数字和值创建 `GridLength` 值：

- [`Absolute`](xref:Xamarin.Forms.GridUnitType.Absolute) &mdash; 宽度或高度以设备无关单位指定（XAML 中的数字）
- [`Auto`](xref:Xamarin.Forms.GridUnitType.Auto) &mdash; 高度或宽度根据单元格内容自动调整大小（XAML 中的“Auto”）
- [`Star`](xref:Xamarin.Forms.GridUnitType.Star) &mdash; 剩余高度或宽度按比例分配（XAML 中的带“\*”（称为“星号”**）的数字）

另外，还必须为 `Grid` 的每个子元素（显式或隐式）分配行和列。 行和列跨度是可选的。 这些全都是使用附加的可绑定属性指定的 &mdash; 这些属性虽然是由 `Grid` 定义的，但却是对 `Grid` 的子元素设置的。 `Grid` 定义了四个附加的可绑定静态属性：

- [`RowProperty`](xref:Xamarin.Forms.Grid.RowProperty) &mdash; 从零开始的行；默认值为 0
- [`ColumnProperty`](xref:Xamarin.Forms.Grid.ColumnProperty) &mdash; 从零开始的列；默认值为 0
- [`RowSpanProperty`](xref:Xamarin.Forms.Grid.RowSpanProperty) &mdash; 子元素跨越的行数；默认值为 1
- [`ColumnSpanProperty`](xref:Xamarin.Forms.Grid.ColumnSpanProperty) &mdash; 子元素跨越的列数；默认值为 1

在代码中，程序可以使用以下八个静态方法来设置和获取这些值：

- [`Grid.SetRow`](xref:Xamarin.Forms.Grid.SetRow(Xamarin.Forms.BindableObject,System.Int32)) 和 [`Grid.GetRow`](xref:Xamarin.Forms.Grid.GetRow(Xamarin.Forms.BindableObject))
- [`Grid.SetColumn`](xref:Xamarin.Forms.Grid.SetColumn(Xamarin.Forms.BindableObject,System.Int32)) 和 [`Grid.GetColumn`](xref:Xamarin.Forms.Grid.GetColumn(Xamarin.Forms.BindableObject))
- [`Grid.SetRowSpan`](xref:Xamarin.Forms.Grid.SetRowSpan(Xamarin.Forms.BindableObject,System.Int32)) 和 [`Grid.GetRowSpan`](xref:Xamarin.Forms.Grid.GetRowSpan(Xamarin.Forms.BindableObject))
- [`Grid.SetColumnSpan`](xref:Xamarin.Forms.Grid.SetColumnSpan(Xamarin.Forms.BindableObject,System.Int32)) 和 [`Grid.GetColumnSpan`](xref:Xamarin.Forms.Grid.GetColumnSpan(Xamarin.Forms.BindableObject))

在 XAML 中，可以使用以下特性来设置这些值：

- `Grid.Row`
- `Grid.Column`
- `Grid.RowSpan`
- `Grid.ColumnSpan`

[SimpleGridDemo****](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/SimpleGridDemo) 示例展示了如何使用 XAML 创建和初始化 `Grid`。

`Grid` 继承自 `Layout` 的 [`Padding`](xref:Xamarin.Forms.Layout.Padding) 属性，并定义了其他两个提供行间距和列间距的属性：

- [`RowSpacing`](xref:Xamarin.Forms.Grid.RowSpacing)：默认值为 6
- [`ColumnSpacing`](xref:Xamarin.Forms.Grid.ColumnSpacing)：默认值为 6

并不严格要求使用 `RowDefinitions` 和 `ColumnDefinitions` 集合。 如果没有这两个集合，`Grid` 会为 `Grid` 的子元素创建行和列，并为它们全都提供默认 `GridLength`“\*”（星号）。

### <a name="the-grid-in-code"></a>代码中的网格

[GridCodeDemo****](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridCodeDemo) 示例展示了如何使用代码创建和填充 `Grid`。 可以直接设置每个子元素的附加属性，也可以通过调用附加 `Add` 方法（如 [Grid.IGridList<T>](xref:Xamarin.Forms.Grid.IGridList`1) 定义的 [`Add`](xref:Xamarin.Forms.Grid.IGridList`1.Add*)）间接设置。

### <a name="the-grid-bar-chart"></a>网格条形图

[GridBarChart****](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridBarChart) 示例展示了如何使用批量 [`AddHorizontal`](xref:Xamarin.Forms.Grid.IGridList`1.AddHorizontal*) 方法将多个 `BoxView` 元素添加到 `Grid` 中。 默认情况下，这些 `BoxView` 元素的宽度相等。 然后，可以控制每个 `BoxView` 的高度，使其像条形图一样。

GridBarChart**** 示例中的 `Grid` 与最初不可见的 `Frame` 共用 `AbsoluteLayout` 父元素。 此程序还对每个 `BoxView` 设置 `TapGestureRecognizer`，以使用 `Frame` 显示用户点击的条形的相关信息。

### <a name="alignment-in-the-grid"></a>网格中的对齐方式

[GridAlignment****](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridAlignment) 示例展示了如何使用 `VerticalOptions` 和 `HorizontalOptions` 属性对齐 `Grid` 单元格中的子元素。

[SpacingButtons****](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/SpacingButtons) 示例对 `Grid` 单元格内居中的 `Button` 元素采用相等间距。

### <a name="cell-dividers-and-borders"></a>单元格分隔条和边框

`Grid` 不包含单元格分隔条或边框绘制功能。 不过，你可以自己创建。

[GridCellDividers****](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridCellDividers) 展示了如何专门为精简 `BoxView` 元素定义其他行和列来模拟分隔线。

[GridCellBorders****](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridCellBorders) 程序不创建其他任何单元格，而是改为在每个单元格中对齐 `BoxView` 元素，以模拟单元格边框。

## <a name="almost-real-life-grid-examples"></a>接近真实的网格示例

[KeypadGrid****](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/KeypadGrid) 示例使用 `Grid` 显示键盘：

[![键盘网格的三倍屏幕截图](images/ch17fg12-small.png "键盘网格")](images/ch17fg12-large.png#lightbox "键盘网格")

### <a name="responding-to-orientation-changes"></a>响应方向更改

`Grid` 有助于构建程序来响应方向更改。 [GridRgbSliders****](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridRgbSliders) 示例展示了一种技术，用于在纵向显示手机的第二行和横向显示手机的第二列之间移动元素。

此程序将 `Slider` 元素初始化为介于 0-255 范围之间，并使用数据绑定以十六进制显示滑块值。 由于 `Slider` 值为浮点型，且十六进制的 .NET 格式设置字符串只能用于整数，因此 [Xamarin.FormsBook.Toolkit****](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 库中的 [`DoubleToIntConvert`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/DoubleToIntConverter.cs) 类可以派上用场。

## <a name="related-links"></a>相关链接

- [第 17 章全文 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch17-Apr2016.pdf)
- [第 17 章示例](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17)
- [网格](~/xamarin-forms/user-interface/layouts/grid.md)
