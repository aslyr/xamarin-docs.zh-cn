---
title: title:“摘要：第 28 章. 位置和地图”说明："《使用 Xamarin.Forms 创建移动应用》- 摘要：摘要：第 28 章.
description: '位置和地图”ms.prod: xamarin ms.technology: xamarin-forms ms.assetid:F6E20077-687C-45C4-A375-31D4F49BBFA4 author: davidbritch ms.author: dabritch ms.date:2018 年 7 月 19 日 no-loc: [Xamarin.Forms, Xamarin.Essentials] 摘要：第 28 章.'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F6E20077-687C-45C4-A375-31D4F49BBFA4
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 301dc65c7909603e117717a993959e3c73fa2d32
ms.sourcegitcommit: ea9269b5d9e3d68b61bb428560a10034117ee457
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/10/2020
ms.locfileid: "84133401"
---
# <a name="summary-of-chapter-28-location-and-maps"></a>位置和地图 [![下载示例](~/media/shared/download.png) 下载示例](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28)

此页上的“注意”指出了 Xamarin.Forms 与书中所述内容的不同之处。

> [!NOTE]
> Xamarin.Forms 支持派生自 `View` 的 [`Map`](xref:Xamarin.Forms.Maps.Map) 元素。

由于使用地图涉及到特殊的平台要求，因此它们在单独的程序集 (Xamarin.Forms.Maps) 中实现，并涉及到不同的命名空间：`Xamarin.Forms.Maps`。 地理坐标系统

## <a name="the-geographic-coordinate-system"></a>地理坐标系统确定球形（或将近球形）对象（如地球）上的位置。

坐标由纬度和经度组成，用角度表示。 称为 `equator` 的大圆位于地球的两极之间，从概念上讲，地轴就是从两极延伸出去的。

纬线和纬度

### <a name="parallels-and-latitude"></a>从地球中心到赤道以北或以南测得的角度标示等纬度线，称为纬线。

范围从赤道的 0 度到南北极的 90 度。 按照惯例，赤道以北的纬度为正值，赤道以南的纬度为负值。 经度和经线

### <a name="longitude-and-meridians"></a>从北极到南极的大圆的一半是等经度线，也被称为经线。

它们相对于英格兰格林威治的本初子午线。 按照惯例，本初子午线以东的经度是 0 度到 180 度之间的正值，而本初子午线以西的经度是 0 度到 &ndash;180 度之间的负值。 Equirectangular 投影

### <a name="the-equirectangular-projection"></a>地球的任何平面地图都会产生变形。

如果纬度和经度的所有线都是直线，并且纬度和经度的相等差异与地图上的相等距离相对应，结果就是 Equirectangular 投影。 这张地图使靠近两极的区域发生了形变，因为它们是水平延伸的。 墨卡托投影

### <a name="the-mercator-projection"></a>常见的墨卡托投影同样尝试通过垂直拉伸这些区域来补偿水平拉伸。

这会生成一个地图，其中靠近两极的区域会看起来比实际大得多，但任何局部区域都与实际区域非常接近。 地图服务和磁贴

### <a name="map-services-and-tiles"></a>地图服务使用称为 `Web Mercator` 的墨卡托投影的变体。

地图服务根据位置和缩放级别将位图磁贴传送到客户端。 获取用户位置

## <a name="getting-the-users-location"></a>Xamarin.Forms `Map` 类不包含用于获取用户地理位置的工具，但在使用地图时，这通常是需要的，因此依赖项服务必须处理好这一情况。

Xamarin.Forms 应用程序可以改用 Xamarin.Essentials 中包含的 [`Geolocation`](~/essentials/geolocation.md) 类。

> [!NOTE]
> 位置跟踪器 API

### <a name="the-location-tracker-api"></a>[Xamarin.FormsBook.Platform](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform) 解决方案包含位置跟踪器 API 代码。

[`GeographicLocation`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/GeographicLocation.cs) 结构封装纬度和经度。 [`ILocationTracker`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/ILocationTracker.cs) 接口定义了用于启动和暂停位置跟踪器的两种方法，并定义了新位置可用时的事件。 iOS 位置管理器

#### <a name="the-ios-location-manager"></a>`ILocationTracker` 的 iOS 实现是使用 iOS [`CLLocationManager`](xref:CoreLocation.CLLocationManager) 的 [`LocationTracker`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/LocationTracker.cs) 类。

Android 位置管理器

#### <a name="the-android-location-manager"></a>`ILocationTracker` 的 Android 实现是使用 Android [`LocationManager`](xref:Android.Locations.LocationManager) 类的 [`LocationTracker`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/LocationTracker.cs) 类。

UWP 地理位置定位器

#### <a name="the-uwp-geo-locator"></a>`ILocationTracker` 的通用 Windows 平台实现是使用 UWP [`Geolocator`](/uwp/api/Windows.Devices.Geolocation.Geolocator) 的 [`LocationTracker`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/LocationTracker.cs) 类。

显示手机位置

### <a name="display-the-phones-location"></a>[**WhereAmI**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28/WhereAmI) 示例使用位置跟踪器在文本和 Equirectangular 地图上显示手机位置。

必需的开销

### <a name="the-required-overhead"></a>WhereAmI 使用位置跟踪器时需要一些开销。

首先，WhereAmI 解决方案中的所有项目都必须引用 Xamarin.FormsBook.Platform 中的相应项目，并且每个 WhereAmI 项目都必须调用 `Toolkit.Init` 方法。 还需要一些特定于平台的额外开销（以位置权限的形式）。

iOS 位置权限

#### <a name="location-permission-for-ios"></a>对于 iOS，info.plist 文件必须包括包含问题文本的项，以请求用户允许获取该用户的位置。

Android 位置权限

#### <a name="location-permissions-for-android"></a>获取用户位置的 Android 应用程序必须在 AndroidManifest.xml 文件中具有 ACCESS_FILE_LOCATION 权限。

UWP 位置权限

#### <a name="location-permissions-for-the-uwp"></a>通用 Windows 平台应用程序必须在 Package.appxmanifest 文件中标记 `location` 设备功能。

使用 Xamarin.Forms.Maps

## <a name="working-with-xamarinformsmaps"></a>使用 `Map` 类涉及几个要求。

NuGet 包

### <a name="the-nuget-package"></a>必须将 Xamarin.Forms.Maps NuGet 库添加到应用程序解决方案。

版本号应与当前安装的 Xamarin.Forms 包相同。 初始化 Maps 包

### <a name="initializing-the-maps-package"></a>在调用 `Xamarin.Forms.Forms.Init` 之后，应用程序项目必须调用 `Xamarin.FormsMaps.Init` 方法。

启用地图服务

### <a name="enabling-map-services"></a>由于 `Map` 可以获取用户的位置，因此应用程序必须按照本章节前面所述的方式获取用户权限：

启用 iOS 地图

#### <a name="enabling-ios-maps"></a>使用 `Map` 的 iOS 应用程序需要 info.plist 文件中的两行。

启用 Android 地图

#### <a name="enabling-android-maps"></a>使用 Google Map 服务需要授权密钥。

此密钥插入到 AndroidManifest.xml 文件中。 此外，AndroidManifest.xml 文件需要使用 `manifest` 标记来获取用户位置。 启用 UWP 地图

#### <a name="enabling-uwp-maps"></a>通用 Windows 平台应用程序需要授权密钥才能使用必应地图。

此密钥将作为参数传递给 `Xamarin.FormsMaps.Init` 方法。 还必须为位置服务启用该应用程序。 未经修饰的地图

### <a name="the-unadorned-map"></a>[MapDemos](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28/MapDemos) 示例包含 [MapsDemoHomePage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapDemosHomePage.xaml) 文件和 [MapsDemoHomePage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapDemosHomePage.xaml.cs) 代码隐藏文件，该文件允许导航到各种演示程序。

[BasicMapPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/BasicMapPage.xaml) 文件展示了如何显示 [`Map`](xref:Xamarin.Forms.Maps.Map) 视图。

默认情况下，它会显示罗马城，但该地图可以由用户操作。 若要禁用水平滚动和垂直滚动，请将 [`HasScrollEnabled`](xref:Xamarin.Forms.Maps.Map.HasScrollEnabled) 属性设置为 `false`。

若要禁用缩放，请将 [`HasZoomEnabled`](xref:Xamarin.Forms.Maps.Map.HasZoomEnabled) 设置为 `false`。 这些属性可能不适用于所有平台。 街道和地形

### <a name="streets-and-terrain"></a>可以通过设置 [`MapType`](xref:Xamarin.Forms.Maps.MapType) 类型的 `Map` 属性 [`MapType`](xref:Xamarin.Forms.Maps.Map.MapType)（包含三个成员的枚举）来显示不同类型的地图：

[`Street`](xref:Xamarin.Forms.Maps.MapType.Street)（默认值）

- [MapTypesPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapTypesPage.xaml) 文件展示了如何使用单选按钮来选择地图类型。
- [`Satellite`](xref:Xamarin.Forms.Maps.MapType.Satellite)
- [`Hybrid`](xref:Xamarin.Forms.Maps.MapType.Hybrid)

它利用 [Xamarin.FormsBook.Toolkit](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 库中的 [`RadioButtonManager`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/RadioButtonManager.cs) 类，以及基于 [MapTypeRadioButton.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapTypeRadioButton.xaml) 文件的类。 地图坐标

### <a name="map-coordinates"></a>程序可以获取 `Map` 通过 [`VisibleRegion`](xref:Xamarin.Forms.Maps.Map.VisibleRegion) 属性显示的当前区域。

可绑定属性不支持此属性，并且没有用于指示其发生更改的通知机制，因此希望监视属性的程序应为此使用计时器。 `VisibleRegion` 是 [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) 类型，一个具有四个只读属性的类：

[`Position`](xref:Xamarin.Forms.Maps.Position) 类型的 [`Center`](xref:Xamarin.Forms.Maps.MapSpan.Center)

- `double` 类型的 [`LatitudeDegrees`](xref:Xamarin.Forms.Maps.MapSpan.LatitudeDegrees)，指示地图显示区域的高度
- `double` 类型的 [`LongitudeDegrees`](xref:Xamarin.Forms.Maps.MapSpan.LongitudeDegrees)，指示地图的显示区域的宽度
- [`Distance`](xref:Xamarin.Forms.Maps.Distance) 类型的 [`Radius`](xref:Xamarin.Forms.Maps.MapSpan.Radius)，指示在地图上可见的最大圆形区域的大小
- `Position` 和 `Distance` 都是结构。

`Position` 定义两个通过 [`Position` 构造函数](xref:Xamarin.Forms.Maps.Position.%23ctor(System.Double,System.Double))设置的只读属性： `Distance` 旨在通过公制和英制单位之间的转换来提供独立于单位的距离。

- [`Latitude`](xref:Xamarin.Forms.Maps.Position.Latitude)
- [`Longitude`](xref:Xamarin.Forms.Maps.Position.Longitude)

`Distance` 值可以通过以下几种方式创建： 距离单位为米的 [`Distance` 构造函数](xref:Xamarin.Forms.Maps.Distance.%23ctor(System.Double))

- [`Distance.FromMeters`](xref:Xamarin.Forms.Maps.Distance.FromMeters(System.Double)) 静态方法
- [`Distance.FromKilometers`](xref:Xamarin.Forms.Maps.Distance.FromKilometers(System.Double)) 静态方法
- [`Distance.FromMiles`](xref:Xamarin.Forms.Maps.Distance.FromMiles(System.Double)) 静态方法
- 可从以下三个属性获取该值：

`double` 类型的 [`Meters`](xref:Xamarin.Forms.Maps.Distance.Meters)

- `double` 类型的 [`Kilometers`](xref:Xamarin.Forms.Maps.Distance.Kilometers)
- `double` 类型的 [`Miles`](xref:Xamarin.Forms.Maps.Distance.Miles)
- [MapCoordinatesPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapCoordinatesPage.xaml) 文件包含若干个用于显示 `MapSpan` 信息的 `Label` 元素。

[MapCoordinatesPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapCoordinatesPage.xaml.cs) 代码隐藏文件在用户操作地图时使用计时器来更新信息。 位置扩展

### <a name="position-extensions"></a>本书新增了名为 [Xamarin.FormsBook.Toolkit.Maps](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit.Maps) 的库，包含特定于地图但独立于平台的类型。

[`PositionExtensions`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit.Maps/Xamarin.FormsBook.Toolkit.Maps/PositionExtensions.cs) 类具有用于 `Position` 的 `ToString` 方法，以及用于计算两个 `Position` 值之间的距离的方法。 设置初始位置

### <a name="setting-an-initial-location"></a>可以调用 `Map` 的 [`MoveToRegion`](xref:Xamarin.Forms.Maps.Map.MoveToRegion(Xamarin.Forms.Maps.MapSpan)) 方法，以编程方式设置地图上的位置和缩放级别。

此参数类型为 `MapSpan`。 可以使用下列任一方法创建 `MapSpan` 对象： [`MapSpan` constructor](xref:Xamarin.Forms.Maps.MapSpan.%23ctor(Xamarin.Forms.Maps.Position,System.Double,System.Double)) 方法，其中包含 `Position` 以及纬度和经度范围

- [`MapSpan.FromCenterAndRadius`](xref:Xamarin.Forms.Maps.MapSpan.FromCenterAndRadius(Xamarin.Forms.Maps.Position,Xamarin.Forms.Maps.Distance)) 方法，其中包含 `Position` 和半径
- 还可以使用 [`ClampLatitude`](xref:Xamarin.Forms.Maps.MapSpan.ClampLatitude(System.Double,System.Double)) 或 [`WithZoom`](xref:Xamarin.Forms.Maps.MapSpan.WithZoom(System.Double)) 方法从现有对象创建新的 `MapSpan`。

[WyomingPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/WyomingPage.xaml) 文件和 [WyomingPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/WyomingPage.xaml.cs) 代码隐藏文件演示如何使用 `MoveToRegion` 方法来显示怀俄明州的状态。

也可以将 [`Map` constructor](xref:Xamarin.Forms.Maps.Map.%23ctor(Xamarin.Forms.Maps.MapSpan)) 与 `MapSpan` 对象结合使用来初始化地图的位置。

[XamarinHQPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/XamarinHQPage.xaml) 文件演示了如何完全在 XAML 中执行此操作，以显示 Xamarin 在旧金山的总部。 动态缩放

### <a name="dynamic-zooming"></a>可以使用 `Slider` 来动态缩放地图。

[RadiusZoomPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/RadiusZoomPage.xaml) 文件和 [RadiusZoomPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/RadiusZoomPage.xaml.cs) 代码隐藏文件演示如何根据 `Slider` 值更改地图半径。 [LongitudeZoomPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LongitudeZoomPage.xaml) 文件和 [LongitudeZoomPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LongitudeZoomPage.xaml.cs) 代码隐藏文件演示了更适用于 Android 的另一种方法，但这两种方法在 Windows 平台上都很有用。

手机位置

### <a name="the-phones-location"></a>`Map` 的 [`IsShowingUser`](xref:Xamarin.Forms.Maps.Map.IsShowingUser) 属性在每个平台上的工作方式稍有不同，如 [ShowLocationPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/ShowLocationPage.xaml) 文件所示：

在 iOS 上，蓝点指示手机位置，但你必须手动导航到该位置

- 在 Android 上，当推送提示将地图移至手机位置时，会显示一个图标
- UWP 类似于 iOS，但有时会自动导航到位置
- MapDemos 项目尝试通过先定义基于图标的按钮来模拟 Android 方法，此做法基于 [MyLocationButton.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MyLocationButton.xaml) 文件和 [MyLocationButton.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MyLocationButton.xaml.cs) 代码隐藏文件。

[GoToLocationPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GoToLocationPage.xaml) 文件和 [GoToLocationPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GoToLocationPage.xaml.cs) 代码隐藏文件使用此按钮导航到手机位置。

图钉和科学博物馆

### <a name="pins-and-science-museums"></a>最后，`Map` 类定义 `IList<Pin>` 类型的 [`Pins`](xref:Xamarin.Forms.Maps.Map.Pins) 属性。

[`Pin`](xref:Xamarin.Forms.Maps.Pin) 类定义以下四个属性： `string` 类型的 [`Label`](xref:Xamarin.Forms.Maps.Pin.Label)，一个必需属性

- `string` 类型的 [`Address`](xref:Xamarin.Forms.Maps.Pin.Address)，一个可选的人工可读地址
- `Position` 类型的 [`Position`](xref:Xamarin.Forms.Maps.Pin.Position)，指示在地图上显示图钉的位置
- [`PinType`](xref:Xamarin.Forms.Maps.PinType) 类型的 [`Type`](xref:Xamarin.Forms.Maps.Pin.Type)，不使用的枚举
- MapDemos 项目包含 [ScienceMuseums.xml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/Data/ScienceMuseums.xml) 文件，该文件列出了美国的科学博物馆，[`Locations`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/Locations.cs) 和 [`Site`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/Site.cs) 类用于反序列化此数据。

[ScienceMuseumsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/ScienceMuseumsPage.xaml) 文件和 [ScienceMuseumsPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/ScienceMuseumsPage.xaml.cs) 代码隐藏文件在地图中显示表示这些科学博物馆的图钉。

当用户点击图钉时，它将显示博物馆的地址和网站。 两点之间的距离

### <a name="the-distance-between-two-points"></a>[`PositionExtensions`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit.Maps/Xamarin.FormsBook.Toolkit.Maps/PositionExtensions.cs) 类包含 [`DistanceTo`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit.Maps/Xamarin.FormsBook.Toolkit.Maps/PositionExtensions.cs#L88) 方法，并简化了两个地理位置之间距离的计算。

在 [LocalMuseumsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LocalMuseumsPage.xaml) 文件和 [LocalMuseumsPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LocalMuseumsPage.xaml.cs) 代码隐藏文件中使用此类还可以显示从用户位置到博物馆的距离：

[![本地博物馆页面的三倍屏幕截图](images/ch28fg28-small.png "到位置的距离")](images/ch28fg28-large.png#lightbox "到位置的距离")

该程序还演示了如何根据地图的位置动态限制图钉数。

地理位置编码，然后返回

## <a name="geocoding-and-back-again"></a>[Xamarin.Forms.Maps](xref:Xamarin.Forms.Maps) 程序集还包含 [`Geocoder`](xref:Xamarin.Forms.Maps.Geocoder) 类，其中 [`GetPositionsForAddressAsync`](xref:Xamarin.Forms.Maps.Geocoder.GetPositionsForAddressAsync(System.String)) 方法用于将文本地址转换为零个或多个可能的地理位置，另一个方法 [`GetAddressesForPositionAsync`](xref:Xamarin.Forms.Maps.Geocoder.GetAddressesForPositionAsync(Xamarin.Forms.Maps.Position)) 用于转换为其他方向。

[GeocoderRoundTrip.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GeocoderRoundTripPage.xaml) 文件和 [GeocoderRoundTrip.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GeocoderRoundTripPage.xaml.cs) 代码隐藏文件演示了此功能。

相关链接

## <a name="related-links"></a>[第 28 章全文 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch28-Aug2016.pdf)

- [第 28 章示例](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28)
- [Xamarin.Forms 地图](~/xamarin-forms/user-interface/map/index.md)
- 到位置的距离
