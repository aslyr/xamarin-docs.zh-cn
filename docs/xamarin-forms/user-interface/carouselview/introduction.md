---
title: Xamarin.FormsCarouselView 简介
description: CarouselView 是一个视图，用于以可滚动的布局显示数据，用户可在此查看项的集合。
ms.prod: xamarin
ms.assetid: 2a96e4bd-c29b-4658-bb4c-ab00872b0f8f
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/08/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 49aa0ffcf7e7fa4d22024ee08f8bdf509cafd73a
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/22/2020
ms.locfileid: "86937795"
---
# <a name="xamarinforms-carouselview-introduction"></a>Xamarin.FormsCarouselView 简介

![预发布 API](~/media/shared/preview.png "此 API 当前为预发布版本")

[`CarouselView`](xref:Xamarin.Forms.CarouselView)是一个视图，用于以可滚动的布局显示数据，用户可以在该视图中轻扫以便在项集合中移动。 默认情况下， `CarouselView` 会以水平方向显示其项。 屏幕上将显示一项，其中包含滑动手势，导致在项集合中向前和向后导航。 此外，可以显示表示中的每一项的指示器 `CarouselView` ：

[![IOS 和 Android 上的 CarouselView 和 IndicatorView 屏幕截图](populate-data-images/indicators.png "IndicatorView 圆圈")](populate-data-images/indicators-large.png#lightbox "IndicatorView 圆圈")

[`CarouselView`](xref:Xamarin.Forms.CarouselView)在4.3 中提供 Xamarin.Forms 。 但是，它当前是实验性的，只能通过将以下代码行添加到 `AppDelegate` iOS 上的类，或者添加到 `MainActivity` Android 上的类，然后再调用 `Forms.Init` ：

```csharp
Forms.SetFlags("CarouselView_Experimental");
```

> [!IMPORTANT]
> [`CarouselView`](xref:Xamarin.Forms.CarouselView)在 iOS 和 Android 上提供，但某些功能可能仅部分可用通用 Windows 平台。

[`CarouselView`](xref:Xamarin.Forms.CarouselView)与的大部分实现共享 [`CollectionView`](xref:Xamarin.Forms.CollectionView) 。 但这两个控件具有不同的用例。 `CollectionView`通常用于显示任意长度的数据列表，而 `CarouselView` 通常用于突出显示有限长度列表中的信息。
