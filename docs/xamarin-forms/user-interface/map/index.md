---
title: Xamarin. Forms 地图
description: 地图控件显示一个地图，并需要 Xamarin. Maps NuGet 包。
ms.prod: xamarin
ms.assetid: B669B5EE-D24C-4C69-93E1-2CA5CC9108B5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/29/2019
ms.openlocfilehash: ffd8f7cc31707d09bb3442c180a867d31afcef0f
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "82517498"
---
# <a name="xamarinforms-map"></a>Xamarin. Forms 地图

## <a name="initialization-and-configuration"></a>[初始化和配置](setup.md)

若要在应用程序中使用地图功能，需要使用[Xamarin](https://www.nuget.org/packages/Xamarin.Forms.Maps/) NuGet NuGet 包。 此外，访问用户的位置时，需要向应用程序授予 location 权限。

## <a name="map-control"></a>[Map Control](map.md)

[`Map`](xref:Xamarin.Forms.Maps.Map)控件是一个跨平台视图，用于显示和批注地图。 它使用每个平台的本机地图控件，为用户提供快速且熟悉的地图体验。

## <a name="position-and-distance"></a>[位置和距离](position-distance.md)

通常[`Position`](xref:Xamarin.Forms.Maps.Position)在定位地图及其引脚时使用结构，以及在定位地图时[`Distance`](xref:Xamarin.Forms.Maps.Distance)可以选择使用的结构。

## <a name="pins"></a>[根](pins.md)

[`Map`](xref:Xamarin.Forms.Maps.Map)控件允许用[`Pin`](xref:Xamarin.Forms.Maps.Pin)对象标记位置。 `Pin`是在点击时打开信息窗口的地图标记。

## <a name="polygons-polylines-and-circles"></a>[多边形、折线和圆圈](polygons.md)

`Polygon`、 `Polyline`和`Circle`元素使您能够突出显示地图上的特定区域。 `Polygon`是一个完全封闭的形状，可以具有笔画和填充颜色。 `Polyline`是不完全包围某个区域的行。 `Circle`突出显示地图的圆形区域。

## <a name="geocoding"></a>[地理编码](geocoder.md)

类[`Geocoder`](xref:Xamarin.Forms.Maps.Geocoder)在存储在对象中[`Position`](xref:Xamarin.Forms.Maps.Position)的字符串地址、纬度和经度坐标之间转换。

## <a name="launch-the-native-map-app"></a>[启动本机映射应用](native-map-app.md)

可以通过 Xamarin `Launcher`类从 xamarin 应用程序中启动每个平台上的本机映射应用。
