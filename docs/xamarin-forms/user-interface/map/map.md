---
title: Xamarin Map Control
description: 地图控件是一个跨平台视图，用于显示和批注地图。 它使用每个平台的本机地图控件，为用户提供快速且熟悉的地图体验。
ms.prod: xamarin
ms.assetid: 22C99029-0B16-43A6-BF58-26B48C4AED38
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/29/2019
ms.openlocfilehash: 3861200446ea9c0e368aa251f3e7ec3f992c7152
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "82517527"
---
# <a name="xamarinforms-map-control"></a>Xamarin Map Control

[![下载示例](~/media/shared/download.png)下载示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

[`Map`](xref:Xamarin.Forms.Maps.Map)控件是一个跨平台视图，用于显示和批注地图。 它使用每个平台的本机地图控件，为用户提供快速且熟悉的地图体验：

[![在 iOS 和 Android 上的地图控件的屏幕截图](map-images/map-default.png "地图控件")](map-images/map-default-large.png#lightbox "地图控件")

[`Map`](xref:Xamarin.Forms.Maps.Map)类定义以下属性，这些属性可控制地图外观和行为：

- [`IsShowingUser`](xref:Xamarin.Forms.Maps.Map.IsShowingUser)类型`bool`为的，指示映射是否显示用户的当前位置。
- [`ItemsSource`](xref:Xamarin.Forms.Maps.Map.ItemsSource)，类型`IEnumerable`为，它指定要显示的`IEnumerable`项的集合。
- [`ItemTemplate`](xref:Xamarin.Forms.Maps.Map.ItemTemplate)，类型[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)为，它指定要[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)应用于显示的项集合中的每个项的。
- `ItemTemplateSelector`，类型[`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector)为，它指定[`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector)将用于在运行时[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)为项选择。
- [`HasScrollEnabled`](xref:Xamarin.Forms.Maps.Map.HasScrollEnabled)类型`bool`的，确定是否允许该映射滚动。
- [`HasZoomEnabled`](xref:Xamarin.Forms.Maps.Map.HasZoomEnabled)类型`bool`为的，确定是否允许地图缩放。
- `MapElements`类型`IList<MapElement>`为的，表示地图上的元素列表，如多边形和折线。
- [`MapType`](xref:Xamarin.Forms.Maps.Map.MapType)类型[`MapType`](xref:Xamarin.Forms.Maps.Map.MapType)为的指示地图的显示样式。
- `MoveToLastRegionOnLayoutChange`，类型`bool`为，用于控制在发生布局更改时，所显示的地图区域是否将从其当前区域移动到以前设置的区域。
- [`Pins`](xref:Xamarin.Forms.Maps.Map.Pins)类型`IList<Pin>`为的，表示地图上的 pin 列表。
- [`VisibleRegion`](xref:Xamarin.Forms.Maps.Map.VisibleRegion)类型[`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan)为的返回当前显示的地图区域。

`MapElements`这些属性（ `Pins`、和`VisibleRegion`属性除外）由[`BindableProperty`](xref:Xamarin.Forms.BindableProperty)对象提供支持，这意味着它们可以是数据绑定的目标。

[`Map`](xref:Xamarin.Forms.Maps.Map)类还定义在点击`MapClicked`映射时触发的事件。 事件`MapClickedEventArgs`附带的对象具有名为的单个属性， `Position`类型[`Position`](xref:Xamarin.Forms.Maps.Position)为。 触发事件时， `Position`属性将设置为点击的映射位置。 有关[`Position`](xref:Xamarin.Forms.Maps.Position)结构的信息，请参阅[地图位置和距离](position-distance.md)。

有关[`ItemsSource`](xref:Xamarin.Forms.Maps.Map.ItemsSource)、 [`ItemTemplate`](xref:Xamarin.Forms.Maps.Map.ItemTemplate)和`ItemTemplateSelector`属性的信息，请参阅[显示 pin 集合](pins.md#display-a-pin-collection)。

## <a name="display-a-map"></a>显示地图

[`Map`](xref:Xamarin.Forms.Maps.Map)可以通过将其添加到布局或页面来显示：

```xaml
<ContentPage ...
             xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps">
    <maps:Map x:Name="map" />
</ContentPage>
```

> [!NOTE]
> 引用 Xamarin `xmlns` . Maps 控件需要额外的命名空间定义。 在前面的示例中`Xamarin.Forms.Maps` ，通过`maps`关键字引用命名空间。

等效 C# 代码如下：

```csharp
using Xamarin.Forms;
using Xamarin.Forms.Maps;

namespace WorkingWithMaps
{
    public class MapTypesPageCode : ContentPage
    {
        public MapTypesPageCode()
        {
            Map map = new Map();
            Content = map;
        }
    }
}
```

此示例将调用默认[`Map`](xref:Xamarin.Forms.Maps.Map)构造函数，该构造函数将地图居中：

[![在 iOS 和 Android 上具有默认位置的地图控件屏幕截图](map-images/map-default.png "具有默认位置的地图控件")](map-images/map-default-large.png#lightbox "具有默认位置的地图控件")

另外，还[`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan)可以将参数传递给[`Map`](xref:Xamarin.Forms.Maps.Map)构造函数，以便在加载时设置地图的中心点和缩放级别。 有关详细信息，请参阅[在地图上显示特定位置](#display-a-specific-location-on-a-map)。

## <a name="map-types"></a>映射类型

[`Map.MapType`](xref:Xamarin.Forms.Maps.Map.MapType)属性可以设置为[`MapType`](xref:Xamarin.Forms.Maps.MapType)枚举成员，以定义地图的显示样式。 `MapType` 枚举定义下列成员：

- `Street`指定将显示街道地图。
- `Satellite`指定将显示包含附属图像的地图。
- `Hybrid`指定将显示组合街道和附属数据的映射。

默认情况下， [`Map`](xref:Xamarin.Forms.Maps.Map)如果未定义该[`MapType`](xref:Xamarin.Forms.Maps.Map.MapType)属性，则将显示街道地图。 或者，可以`MapType`将属性设置为[`MapType`](xref:Xamarin.Forms.Maps.MapType)枚举成员之一：

```xaml
<maps:Map MapType="Satellite" />
```

等效 C# 代码如下：

```csharp
Map map = new Map
{
    MapType = MapType.Satellite
};
```

[`Map`](xref:Xamarin.Forms.Maps.Map)当[`MapType`](xref:Xamarin.Forms.Maps.Map.MapType)属性设置为`Street`时，以下屏幕截图显示：

[![IOS 和 Android 上带有街道地图类型的地图控件屏幕截图](map-images/maptype-street.png "带有街道 maptype 的地图控件")](map-images/maptype-street-large.png#lightbox "地图控件与街道地图类型")

[`Map`](xref:Xamarin.Forms.Maps.Map)当[`MapType`](xref:Xamarin.Forms.Maps.Map.MapType)属性设置为`Satellite`时，以下屏幕截图显示：

[![在 iOS 和 Android 上具有附属映射类型的地图控件屏幕截图](map-images/maptype-satellite.png "具有附属 maptype 的地图控件")](map-images/maptype-satellite-large.png#lightbox "具有附属映射类型的地图控件")

[`Map`](xref:Xamarin.Forms.Maps.Map)当[`MapType`](xref:Xamarin.Forms.Maps.Map.MapType)属性设置为`Hybrid`时，以下屏幕截图显示：

[![在 iOS 和 Android 上具有混合地图类型的地图控件屏幕截图](map-images/maptype-hybrid.png "混合 maptype 的地图控件")](map-images/maptype-hybrid-large.png#lightbox "混合地图类型的地图控件")

## <a name="display-a-specific-location-on-a-map"></a>显示地图上的特定位置

可以通过将[`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan)自变量传递给[`Map`](xref:Xamarin.Forms.Maps.Map)构造函数来设置映射时显示的地图区域：

```xaml
<maps:Map>
    <x:Arguments>
        <maps:MapSpan>
            <x:Arguments>
                <maps:Position>
                    <x:Arguments>
                        <x:Double>36.9628066</x:Double>
                        <x:Double>-122.0194722</x:Double>
                    </x:Arguments>
                </maps:Position>
                <x:Double>0.01</x:Double>
                <x:Double>0.01</x:Double>
            </x:Arguments>
        </maps:MapSpan>
    </x:Arguments>
</maps:Map>
```

等效 C# 代码如下：

```csharp
Position position = new Position(36.9628066, -122.0194722);
MapSpan mapSpan = new MapSpan(position, 0.01, 0.01);
Map map = new Map(mapSpan);
```

此示例将创建[`Map`](xref:Xamarin.Forms.Maps.Map)一个对象，该对象显示[`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan)对象指定的区域。 `MapSpan`对象以[`Position`](xref:Xamarin.Forms.Maps.Position)对象所表示的纬度和经度为中心，并跨0.01 纬度和0.01 经度度。 有关[`Position`](xref:Xamarin.Forms.Maps.Position)结构的信息，请参阅[地图位置和距离](position-distance.md)。 有关在 XAML 中传递自变量的信息，请参阅[在 xaml 中传递参数](~/xamarin-forms/xaml/passing-arguments.md)。

这样做的结果是：当显示地图时，它将位于特定位置，并跨越特定数量的纬度和经度度：

[![在 iOS 和 Android 上具有指定位置的地图控件的屏幕截图](map-images/map-region.png "具有指定位置的地图控件")](map-images/map-region-large.png#lightbox "具有指定位置的地图控件")

## <a name="create-a-mapspan-object"></a>创建 MapSpan 对象

有多种方法可以创建[`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan)对象。 常见的方法是向`MapSpan`构造函数提供所需的参数。 这些是由[`Position`](xref:Xamarin.Forms.Maps.Position)对象表示的纬度和经度，以及`double`表示所跨的纬度和经度度的值。 `MapSpan` 有关[`Position`](xref:Xamarin.Forms.Maps.Position)结构的信息，请参阅[地图位置和距离](position-distance.md)。

另外， [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan)类中有三种方法返回新`MapSpan`的对象：

1. [`ClampLatitude`](xref:Xamarin.Forms.Maps.MapSpan.ClampLatitude*)返回一个`MapSpan` ，它与`LongitudeDegrees`方法的类实例以及由其`north`和`south`参数定义的 radius 相同。
1. [`FromCenterAndRadius`](xref:Xamarin.Forms.Maps.MapSpan.FromCenterAndRadius*)`MapSpan`返回由其[`Position`](xref:Xamarin.Forms.Maps.Position)和[`Distance`](xref:Xamarin.Forms.Maps.Distance)参数定义的。
1. [`WithZoom`](xref:Xamarin.Forms.Maps.MapSpan.WithZoom*)返回`MapSpan`具有与方法的类实例相同的中心的，但其半径乘以其`double`参数。

有关[`Distance`](xref:Xamarin.Forms.Maps.Distance)结构的信息，请参阅[地图位置和距离](position-distance.md)。

[`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan)创建后，可以访问以下属性来检索有关它的数据：

- [`Center`](xref:Xamarin.Forms.Maps.MapSpan.Center)，它表示的[`Position`](xref:Xamarin.Forms.Maps.Position)地理中心中的`MapSpan`。
- [`LatitudeDegrees`](xref:Xamarin.Forms.Maps.MapSpan.LatitudeDegrees)，表示跨越的纬度度`MapSpan`。
- [`LongitudeDegrees`](xref:Xamarin.Forms.Maps.MapSpan.LongitudeDegrees)，表示跨越的经度的度数`MapSpan`。
- [`Radius`](xref:Xamarin.Forms.Maps.MapSpan.Radius)，表示`MapSpan`半径。

## <a name="move-the-map"></a>移动地图

可以[`Map.MoveToRegion`](xref:Xamarin.Forms.Maps.Map.MoveToRegion*)调用方法来更改地图的位置和缩放级别。 此方法接受一个[`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan)参数，该参数定义要显示的地图区域及其缩放级别。

下面的代码演示了在地图上移动所显示区域的示例：

```csharp
MapSpan mapSpan = MapSpan.FromCenterAndRadius(position, Distance.FromKilometers(0.444));
map.MoveToRegion(mapSpan);
```

## <a name="zoom-the-map"></a>缩放地图

可以更改的[`Map`](xref:Xamarin.Forms.Maps.Map)缩放级别，而无需更改其位置。 这可以通过使用映射 UI 来实现，也可以通过调用[`MoveToRegion`](xref:Xamarin.Forms.Maps.Map.MoveToRegion*)方法（使用将[`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan)当前位置作为[`Position`](xref:Xamarin.Forms.Maps.Position)参数的参数）以编程方式执行此操作：

```csharp
double zoomLevel = 0.5;
double latlongDegrees = 360 / (Math.Pow(2, zoomLevel));
if (map.VisibleRegion != null)
{
    map.MoveToRegion(new MapSpan(map.VisibleRegion.Center, latlongDegrees, latlongDegrees));
}
```

在此示例中， [`MoveToRegion`](xref:Xamarin.Forms.Maps.Map.MoveToRegion*)使用[`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan)参数调用方法，该自变量通过[`Map.VisibleRegion`](xref:Xamarin.Forms.Maps.Map.VisibleRegion)属性指定地图的当前位置，并将缩放级别指定为纬度和经度的度数。 总体结果是更改了地图的缩放级别，但其位置却不是。 在地图上实现缩放的另一种方法是使用[`MapSpan.WithZoom`](xref:Xamarin.Forms.Maps.MapSpan.WithZoom*)方法控制缩放系数。

> [!IMPORTANT]
> 无论是通过映射 UI 还是以编程方式缩放地图，都需要[`Map.HasZoomEnabled`](xref:Xamarin.Forms.Maps.Map.HasZoomEnabled)属性为。 `true` 有关此属性的详细信息，请参阅[禁用缩放](#disable-zoom)。

## <a name="customize-map-behavior"></a>自定义映射行为

可以通过设置其[`Map`](xref:Xamarin.Forms.Maps.Map)一些属性并通过处理`MapClicked`事件自定义的行为。

> [!NOTE]
> 可以通过创建地图自定义呈现器来实现其他映射行为自定义。 有关详细信息，请参阅[自定义 Xamarin。 Forms 地图](~/xamarin-forms/app-fundamentals/custom-renderer/map-pin.md)。

### <a name="disable-scroll"></a>禁用滚动

[`Map`](xref:Xamarin.Forms.Maps.Map)类定义类型`bool`的[`HasScrollEnabled`](xref:Xamarin.Forms.Maps.Map.HasScrollEnabled)属性。 默认情况下，此`true`属性为，表示允许滚动映射。 如果将此属性设置为`false`，则不会滚动映射。 下面的示例演示如何设置此属性：

```xaml
<maps:Map HasScrollEnabled="false" />
```

等效 C# 代码如下：

```csharp
Map map = new Map
{
    HasScrollEnabled = false
};
```

### <a name="disable-zoom"></a>禁用缩放

[`Map`](xref:Xamarin.Forms.Maps.Map)类定义类型`bool`的[`HasZoomEnabled`](xref:Xamarin.Forms.Maps.Map.HasZoomEnabled)属性。 默认情况下，此`true`属性为，表示可以对地图执行缩放。 如果将此属性设置为`false`，则无法缩放地图。 下面的示例演示如何设置此属性：

```xaml
<maps:Map HasZoomEnabled="false" />
```

等效 C# 代码如下：

```csharp
Map map = new Map
{
    HasZoomEnabled = false
};
```

### <a name="show-the-users-location"></a>显示用户的位置

[`Map`](xref:Xamarin.Forms.Maps.Map)类定义类型`bool`的[`IsShowingUser`](xref:Xamarin.Forms.Maps.Map.IsShowingUser)属性。 默认情况下，此`false`属性为，表示地图未显示用户的当前位置。 当此属性设置为`true`时，地图将显示用户的当前位置。 下面的示例演示如何设置此属性：

```xaml
<maps:Map IsShowingUser="true" />
```

等效 C# 代码如下：

```csharp
Map map = new Map
{
    IsShowingUser = true
};
```

> [!IMPORTANT]
> 在 iOS、Android 和通用 Windows 平台上，访问用户的位置时，需要授予应用程序的位置权限。 有关详细信息，请参阅[平台配置](setup.md#platform-configuration)。

### <a name="maintain-map-region-on-layout-change"></a>维护布局更改时的地图区域

[`Map`](xref:Xamarin.Forms.Maps.Map)类定义类型`bool`的`MoveToLastRegionOnLayoutChange`属性。 默认情况下，此`true`属性为，表示当发生布局更改时，所显示的地图区域将从其当前区域移动到以前设置的区域，如设备旋转。 如果将此属性设置为`false`，则在发生布局更改时，所显示的地图区域将保持居中。 下面的示例演示如何设置此属性：

```xaml
<maps:Map MoveToLastRegionOnLayoutChange="false" />
```

等效 C# 代码如下：

```csharp
Map map = new Map
{
    MoveToLastRegionOnLayoutChange = false
};
```

### <a name="map-clicks"></a>地图单击

[`Map`](xref:Xamarin.Forms.Maps.Map)类定义在点击`MapClicked`映射时触发的事件。 事件`MapClickedEventArgs`附带的对象具有名为的单个属性， `Position`类型[`Position`](xref:Xamarin.Forms.Maps.Position)为。 触发事件时， `Position`属性将设置为点击的映射位置。 有关[`Position`](xref:Xamarin.Forms.Maps.Position)结构的信息，请参阅[地图位置和距离](position-distance.md)。

下面的代码示例演示`MapClicked`事件的事件处理程序：

```csharp
void OnMapClicked(object sender, MapClickedEventArgs e)
{
    System.Diagnostics.Debug.WriteLine($"MapClick: {e.Position.Latitude}, {e.Position.Longitude}");
}
```

在此示例中， `OnMapClicked`事件处理程序输出表示螺纹地图位置的纬度和经度。 事件处理程序可以在事件中`MapClicked`注册，如下所示：

```xaml
<maps:Map MapClicked="OnMapClicked" />
```

等效 C# 代码如下：

```csharp
Map map = new Map();
map.MapClicked += OnMapClicked;
```

## <a name="related-links"></a>相关链接

- [地图示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)
- [地图位置和距离](position-distance.md)
- [自定义 Xamarin.Forms 地图](~/xamarin-forms/app-fundamentals/custom-renderer/map-pin.md)
- [在 XAML 中传递参数](~/xamarin-forms/xaml/passing-arguments.md)
