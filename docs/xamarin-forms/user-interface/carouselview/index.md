---
title: Xamarin.FormsCarouselView
description: CarouselView 是一个视图，用于以可滚动的布局显示数据，用户可在此查看一系列项目中的数据。
ms.prod: xamarin
ms.assetid: 5b673347-cdba-4532-820f-fb5f070c86bc
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/08/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 891f1ff8ad8f254ff3a2805d08d0f7e115bb0fff
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/18/2020
ms.locfileid: "84137366"
---
# <a name="xamarinforms-carouselview"></a>Xamarin.FormsCarouselView

![](~/media/shared/preview.png "This API is currently pre-release")

## <a name="introduction"></a>[简介](introduction.md)

[`CarouselView`](xref:Xamarin.Forms.CarouselView)是一个视图，用于以可滚动的布局显示数据，用户可在此视图中轻扫以便在项集合中移动。

## <a name="data"></a>[数据](populate-data.md)

[`CarouselView`](xref:Xamarin.Forms.CarouselView)使用数据填充数据，方法是将其 [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) 属性设置为任何实现的集合 `IEnumerable` 。 可以通过将属性设置为来定义每个项的外观 [`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 。

## <a name="layout"></a>[布局](layout.md)

默认情况下， [`CarouselView`](xref:Xamarin.Forms.CarouselView) 将在水平列表中显示其项。 但是，它还可以访问 CollectionView 的相同布局，包括垂直方向。

## <a name="interaction"></a>[交互](interaction.md)

[`CarouselView`](xref:Xamarin.Forms.CarouselView)可以通过和属性访问中当前显示的项 `CurrentItem` `Position` 。

## <a name="empty-views"></a>[空视图](emptyview.md)

在中 [`CarouselView`](xref:Xamarin.Forms.CarouselView) ，可以指定一个空视图，在没有可显示的数据时向用户提供反馈。 空视图可以是字符串、视图或多个视图。

## <a name="scrolling"></a>[滚动](scrolling.md)

当用户 swipes 启动滚动时，可以控制滚动的结束位置，以便完全显示项。 此外，还 [`CarouselView`](xref:Xamarin.Forms.CarouselView) 定义了两种 [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) 方法，这些方法以编程方式将项滚动到视图中。 其中一个重载将指定索引处的项滚动到视图中，而另一个重载将指定项滚动到视图中。
