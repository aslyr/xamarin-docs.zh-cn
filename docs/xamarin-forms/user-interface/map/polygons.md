---
title: Xamarin，地图多边形、折线和圆圈
description: 本文介绍如何在 Xamarin. Forms 地图实例上创建多边形、折线和圆圈。
ms.prod: xamarin
ms.assetid: CDAF0B02-1AA8-4AD6-94A7-ABFC18006A2D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2020
ms.openlocfilehash: e1edbc4d7376023c9d3051b0518c8dc7368e63a7
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "82517347"
---
# <a name="xamarinforms-map-polygons-and-polylines"></a>Xamarin。窗体映射多边形和折线

[![下载示例](~/media/shared/download.png)下载示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

`Polygon`、 `Polyline`和`Circle`元素使您能够突出显示地图上的特定区域。 `Polygon`是一个完全封闭的形状，可以具有笔画和填充颜色。 `Polyline`是不完全包围某个区域的行。 `Circle`突出显示了地图的圆形区域：

[!["](polygons-images/polygon-polyline.png "地图上的多边形和折线")](polygons-images/polygon-polyline-large.png#lightbox "地图上的多边形和折线")
在 ios 和 android 上的[![地图圆屏幕截图](polygons-images/circle.png "地图上的圆圈")上的地图多边形和折线的屏幕截图"](polygons-images/circle-large.png#lightbox "地图上的圆圈")

`Polygon`、 `Polyline`和`Circle`类派生自`MapElement`类，该类公开以下可绑定属性：

- `StrokeColor`是确定`Color`线条颜色的对象。
- `StrokeWidth`确定线条`float`宽度的对象。

`Polygon`类定义了其他可绑定属性：

- `FillColor`是确定`Color`多边形背景色的对象。

此外， `Polygon`和`Polyline`类都定义一个`GeoPath`属性，该属性是指定形状点的[`Position`](xref:Xamarin.Forms.Maps.Position)对象的列表。

`Circle`类定义以下可绑定属性：

- `Center`是一个[`Position`](xref:Xamarin.Forms.Maps.Position)对象，用于定义纬度和经度中的圆的中心。
- `Radius`一个[`Distance`](xref:Xamarin.Forms.Maps.Distance)对象，它定义以米、公里或英里为单位的圆的半径。
- `FillColor`是一个`Color`属性，它确定圆周内的颜色。

> [!NOTE]
> 如果未`StrokeColor`指定属性，则笔划将默认为黑色。 如果未`FillColor`指定该属性，则填充将默认为透明。 因此，如果两个属性均未指定，则该形状将有一个不带填充的黑色轮廓。

## <a name="create-a-polygon"></a>创建多边形

通过`Polygon`实例化对象并将其添加到地图的`MapElements`集合，可以将该对象添加到地图中。 这可以通过以下操作在 XAML 中实现：

```xaml
<ContentPage ...
             xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps">
     <maps:Map>
         <maps:Map.MapElements>
             <maps:Polygon StrokeColor="#FF9900"
                           StrokeWidth="8"
                           FillColor="#88FF9900">
                 <maps:Polygon.Geopath>
                     <maps:Position>
                         <x:Arguments>
                             <x:Double>47.6368678</x:Double>
                             <x:Double>-122.137305</x:Double>
                         </x:Arguments>
                     </maps:Position>
                     ...
                 </maps:Polygon.Geopath>
             </maps:Polygon>
         </maps:Map.MapElements>
     </maps:Map>
</ContentPage>
```

等效 C# 代码如下：

```csharp
using Xamarin.Forms.Maps;
// ...
Map map = new Map
{
  // ...
};

// instantiate a polygon
Polygon polygon = new Polygon
{
    StrokeWidth = 8,
    StrokeColor = Color.FromHex("#1BA1E2"),
    FillColor = Color.FromHex("#881BA1E2"),
    Geopath =
    {
        new Position(47.6368678, -122.137305),
        new Position(47.6368894, -122.134655),
        new Position(47.6359424, -122.134655),
        new Position(47.6359496, -122.1325521),
        new Position(47.6424124, -122.1325199),
        new Position(47.642463,  -122.1338932),
        new Position(47.6406414, -122.1344833),
        new Position(47.6384943, -122.1361248),
        new Position(47.6372943, -122.1376912)
    }
};

// add the polygon to the map's MapElements collection
map.MapElements.Add(polygon);
```

指定`StrokeColor`了`StrokeWidth`和属性以自定义多边形的轮廓。 `FillColor`属性值与`StrokeColor`属性值相匹配，但指定了 alpha 值以使其透明，从而使基础地图可以通过形状可见。 `GeoPath`属性包含一个`Position`对象列表，这些对象定义多边形点的地理坐标。 将`Polygon`对象添加到的`MapElements`集合后，该对象将呈现在该映射上。 `Map`

> [!NOTE]
> `Polygon`是一个完全包围的形状。 如果第一个点和最后一个点不匹配，则将自动进行连接。

## <a name="create-a-polyline"></a>创建折线

通过`Polyline`实例化对象并将其添加到地图的`MapElements`集合，可以将该对象添加到地图中。 这可以通过以下操作在 XAML 中实现：

```xaml
<ContentPage ...
             xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps">
     <maps:Map>
         <maps:Map.MapElements>
             <maps:Polyline StrokeColor="Blue"
                            StrokeWidth="12">
                 <maps:Polyline.Geopath>
                     <maps:Position>
                         <x:Arguments>
                             <x:Double>47.6381401</x:Double>
                             <x:Double>-122.1317367</x:Double>
                         </x:Arguments>
                     </maps:Position>
                     ...
                 </maps:Polyline.Geopath>
             </maps:Polyline>
         </maps:Map.MapElements>
     </maps:Map>
</ContentPage>
```

```csharp
using Xamarin.Forms.Maps;
// ...
Map map = new Map
{
  // ...
};
// instantiate a polyline
Polyline polyline = new Polyline
{
    StrokeColor = Color.Blue,
    StrokeWidth = 12,
    Geopath =
    {
        new Position(47.6381401, -122.1317367),
        new Position(47.6381473, -122.1350841),
        new Position(47.6382847, -122.1353094),
        new Position(47.6384582, -122.1354703),
        new Position(47.6401136, -122.1360819),
        new Position(47.6403883, -122.1364681),
        new Position(47.6407426, -122.1377019),
        new Position(47.6412558, -122.1404056),
        new Position(47.6414148, -122.1418647),
        new Position(47.6414654, -122.1432702)
    }
};

// add the polyline to the map's MapElements collection
map.MapElements.Add(polyline);
```

指定`StrokeColor`了`StrokeWidth`和属性以自定义行。 `GeoPath`属性包含一个`Position`对象列表，这些对象定义折线点的地理坐标。 将`Polyline`对象添加到的`MapElements`集合后，该对象将呈现在该映射上。 `Map`

## <a name="create-a-circle"></a>创建圆形

通过`Circle`实例化对象并将其添加到地图的`MapElements`集合，可以将该对象添加到地图中。 这可以通过以下操作在 XAML 中实现：

```xaml
<ContentPage ...
             xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps">
      <maps:Map>
          <maps:Map.MapElements>
              <maps:Circle StrokeColor="#88FF0000"
                           StrokeWidth="8"
                           FillColor="#88FFC0CB">
                  <maps:Circle.Center>
                      <maps:Position>
                          <x:Arguments>
                              <x:Double>37.79752</x:Double>
                              <x:Double>-122.40183</x:Double>
                          </x:Arguments>
                      </maps:Position>
                  </maps:Circle.Center>
                  <maps:Circle.Radius>
                      <maps:Distance>
                          <x:Arguments>
                              <x:Double>250</x:Double>
                          </x:Arguments>
                      </maps:Distance>
                  </maps:Circle.Radius>
              </maps:Circle>             
          </maps:Map.MapElements>
          ...
      </maps:Map>
</ContentPage>
```

等效 C# 代码如下：

```csharp
using Xamarin.Forms.Maps;
// ...
Map map = new Map();

// Instantiate a Circle
Circle circle = new Circle
{
    Center = new Position(37.79752, -122.40183);,
    Radius = new Distance(250),
    StrokeColor = Color.FromHex("#88FF0000"),
    StrokeWidth = 8,
    FillColor = Color.FromHex("#88FFC0CB")
};

// Add the Circle to the map's MapElements collection
map.MapElements.Add(circle);
```

地图`Circle`上的位置取决于`Center`和`Radius`属性的值。 `Center`属性定义圆的中心（在纬度和经度中），而`Radius`属性定义圆的半径（以米为单位）。 指定`StrokeColor`了`StrokeWidth`和属性以自定义圆的轮廓。 `FillColor`属性值指定圆周内的颜色。 这两个颜色值都指定一个 alpha 通道，从而使基础地图可以通过圆圈可见。 将`Circle`对象添加到的`MapElements`集合后，该对象将呈现在映射上。 `Map`

> [!NOTE]
> `GeographyUtils`类具有`ToCircumferencePositions`一个扩展方法，该方法可`Circle`将对象（定义`Center`和`Radius`属性值）转换为构成圆周`Position`的纬度和经度坐标的对象的列表。

## <a name="related-links"></a>相关链接

- [地图示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)
