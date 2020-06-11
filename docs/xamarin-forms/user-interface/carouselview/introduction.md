---
标题： " Xamarin.Forms CarouselView 简介" 说明： "CarouselView 是一个视图，用于以可滚动的布局显示数据，用户可在此视图中翻阅项目集合。"
ms-chap： xamarin assetid： 2a96e4bd-c29b-4658-bb4c-ab00872b0f8f 毫秒： xamarin 窗体作者： davidbritch 毫秒. 作者： dabritch 毫秒。日期：10/08/2019 非 loc： [ Xamarin.Forms ， Xamarin.Essentials ]
---

# <a name="xamarinforms-carouselview-introduction"></a>Xamarin.FormsCarouselView 简介

![](~/media/shared/preview.png "This API is currently pre-release")

[`CarouselView`](xref:Xamarin.Forms.CarouselView)是一个视图，用于以可滚动的布局显示数据，用户可以在该视图中轻扫以便在项集合中移动。 默认情况下， `CarouselView` 会以水平方向显示其项。 屏幕上将显示一项，其中包含滑动手势，导致在项集合中向前和向后导航。 此外，可以显示表示中的每一项的指示器 `CarouselView` ：

[![IOS 和 Android 上的 CarouselView 和 IndicatorView 屏幕截图](populate-data-images/indicators.png "IndicatorView 圆圈")](populate-data-images/indicators-large.png#lightbox "IndicatorView 圆圈")

[`CarouselView`](xref:Xamarin.Forms.CarouselView)在4.3 中提供 Xamarin.Forms 。 但是，它当前是实验性的，只能通过将以下代码行添加到 `AppDelegate` iOS 上的类，或者添加到 `MainActivity` Android 上的类，然后再调用 `Forms.Init` ：

```csharp
Forms.SetFlags("CarouselView_Experimental");
```

> [!IMPORTANT]
> [`CarouselView`](xref:Xamarin.Forms.CarouselView)在 iOS 和 Android 上提供，但某些功能可能仅部分可用通用 Windows 平台。

[`CarouselView`](xref:Xamarin.Forms.CarouselView)与的大部分实现共享 [`CollectionView`](xref:Xamarin.Forms.CollectionView) 。 但这两个控件具有不同的用例。 `CollectionView`通常用于显示任意长度的数据列表，而 `CarouselView` 通常用于突出显示有限长度列表中的信息。
