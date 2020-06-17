---
标题： " Xamarin.Forms CollectionView" 说明： "CollectionView 是一种灵活且高性能的视图，可使用不同的布局规范呈现数据列表。"
ms-chap： xamarin assetid：2BC9B223-2D5C-4B09-849C-B9D578954557： xamarin 窗体作者： davidbritch： dabritch ms. 日期：07/24/2019 非 loc： [ Xamarin.Forms ， Xamarin.Essentials ]
---

# <a name="xamarinforms-collectionview"></a>Xamarin.Forms CollectionView

## <a name="introduction"></a>[简介](introduction.md)

[`CollectionView`](xref:Xamarin.Forms.CollectionView)是一个灵活且高性能的视图，可使用不同的布局规范呈现数据列表。

## <a name="data"></a>[数据](populate-data.md)

[`CollectionView`](xref:Xamarin.Forms.CollectionView)使用数据填充数据，方法是将其 [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) 属性设置为任何实现的集合 `IEnumerable` 。 可以通过将属性设置为来定义列表中每个项的外观 [`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 。

## <a name="layout"></a>[布局](layout.md)

默认情况下， [`CollectionView`](xref:Xamarin.Forms.CollectionView) 会在垂直列表中显示其项。 但是，可以指定垂直和水平列表以及网格。

## <a name="selection"></a>[选择](selection.md)

默认情况下， [`CollectionView`](xref:Xamarin.Forms.CollectionView) 选定内容处于禁用状态。 但是，可以启用单个和多个选择。

## <a name="empty-views"></a>[空视图](emptyview.md)

在中 [`CollectionView`](xref:Xamarin.Forms.CollectionView) ，可以指定一个空视图，在没有可显示的数据时向用户提供反馈。 空视图可以是字符串、视图或多个视图。

## <a name="scrolling"></a>[滚动](scrolling.md)

当用户 swipes 启动滚动时，可以控制滚动的结束位置，以便完全显示项。 此外，还 [`CollectionView`](xref:Xamarin.Forms.CollectionView) 定义了两种 [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) 方法，这些方法以编程方式将项滚动到视图中。 其中一个重载将指定索引处的项滚动到视图中，而另一个重载将指定项滚动到视图中。

## <a name="grouping"></a>[分组](grouping.md)

[`CollectionView`](xref:Xamarin.Forms.CollectionView)可以通过将其属性设置为，来显示正确分组的数据 `IsGrouped` `true` 。
