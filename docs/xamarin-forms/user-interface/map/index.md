---
title: Xamarin.Forms将
description: 地图控件显示一个映射，并需要 Xamarin.Forms 。映射 NuGet 包。
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 2461ffa8168207e6a57fae005f752be48772a34a
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/28/2020
ms.locfileid: "84139823"
---
# <a name="xamarinforms-map"></a>Xamarin.Forms将

## <a name="initialization-and-configuration"></a>[初始化和配置](setup.md)

[ Xamarin.Forms 。](https://www.nuget.org/packages/Xamarin.Forms.Maps/)在应用程序中使用地图功能需要 maps NuGet 包。 此外，访问用户的位置时，需要向应用程序授予 location 权限。

## <a name="map-control"></a>[Map Control](map.md)

[`Map`](xref:Xamarin.Forms.Maps.Map)控件是一个跨平台视图，用于显示和批注地图。 它使用每个平台的本机地图控件，为用户提供快速且熟悉的地图体验。

## <a name="position-and-distance"></a>[位置和距离](position-distance.md)

[`Position`](xref:Xamarin.Forms.Maps.Position)通常在定位地图及其引脚时使用结构，以及在 [`Distance`](xref:Xamarin.Forms.Maps.Distance) 定位地图时可以选择使用的结构。

## <a name="pins"></a>[固定](pins.md)

[`Map`](xref:Xamarin.Forms.Maps.Map)控件允许用对象标记位置 [`Pin`](xref:Xamarin.Forms.Maps.Pin) 。 `Pin`是在点击时打开信息窗口的地图标记。

## <a name="polygons-polylines-and-circles"></a>[多边形、折线和圆形](polygons.md)

`Polygon`、 `Polyline` 和 `Circle` 元素使您能够突出显示地图上的特定区域。 `Polygon`是一个完全封闭的形状，可以具有笔画和填充颜色。 `Polyline`是不完全包围某个区域的行。 `Circle`突出显示地图的圆形区域。

## <a name="geocoding"></a>[地理编码](geocoder.md)

[`Geocoder`](xref:Xamarin.Forms.Maps.Geocoder)类在存储在对象中的字符串地址、纬度和经度坐标之间转换 [`Position`](xref:Xamarin.Forms.Maps.Position) 。

## <a name="launch-the-native-map-app"></a>[启动本机映射应用](native-map-app.md)

可以 Xamarin.Forms 通过类从应用程序启动每个平台上的本机映射应用 Xamarin.Essentials `Launcher` 。
