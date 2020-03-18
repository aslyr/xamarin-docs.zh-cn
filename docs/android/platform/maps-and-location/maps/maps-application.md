---
title: 启动地图应用程序
description: 如何从 Xamarin.Android 应用启动内置地图应用程序。
ms.prod: xamarin
ms.assetid: 929EACB8-8950-50E1-093C-43FB5F1F1CD5
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/25/2018
ms.openlocfilehash: 7b74f564f2b6e9613874a774258a7e999002e61a
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/10/2020
ms.locfileid: "73027081"
---
# <a name="launching-the-maps-application"></a>启动地图应用程序

在 Xamarin.Android 中使用地图的最简单方法是利用如下所示的内置地图应用程序：

[![内置 Google 地图应用的屏幕截图示例](maps-application-images/01-mapsapplication.png)](maps-application-images/01-mapsapplication.png#lightbox)

在使用地图应用程序时，地图不是应用程序的一部分。 相反，应用程序将启动地图应用程序并从外部加载地图。 下一节将介绍如何使用 Xamarin.Android 启动类似上面的地图。

## <a name="creating-the-intent"></a>创建意向

使用地图应用程序非常简单，只需要使用适当的 URI 创建意向，将操作设置为 ActionView 然后调用 StartActivity 方法。 例如，以下代码启动地图应用程序，以给定纬度和经度为中心：

```csharp
var geoUri = Android.Net.Uri.Parse ("geo:42.374260,-71.120824");
var mapIntent = new Intent (Intent.ActionView, geoUri);
StartActivity (mapIntent);
```

使用该代码，即可启动上一个屏幕截图中显示的地图。 除了指定纬度和经度，地图的 URI 方案还支持几个其他选项。

## <a name="geo-uri-scheme"></a>地理 URI 方案

上面的代码使用了地理方案来创建 URI。 此 URI 方案支持多种格式，如下所示：

- `geo:latitude,longitude` &ndash; 打开以纬度/经度为中心的地图应用程序。 

- `geo:latitude,longitude?z=zoom` &ndash; 打开地图应用程序，以纬度/经度为中心并缩放到指定级别。 缩放级别的范围是从 1 到 23：1 显示整个地球，23 是最近的缩放级别。

- `geo:0,0?q=my+street+address` &ndash; 打开地图应用程序，找到街道地址的位置。 

- `geo:0,0?q=business+near+city` &ndash; 打开地图应用程序并显示带注释的搜索结果。 

执行查询的 URI 版本（即街道地址或搜索词）使用 Google 的地理编码器服务来检索随后显示在地图上的位置。 例如，URI `geo:0,0?q=coop+Cambridge` 结果显示在下面的地图中：

[![显示带搜索词的 Google 地图的屏幕截图示例](maps-application-images/02-mapsearch.png)](maps-application-images/02-mapsearch.png#lightbox)

有关地理 URI 方案的更多信息，请参阅[在地图上显示位置](https://developer.android.com/guide/components/intents-common.html#Maps)。

## <a name="street-view"></a>街道视图

除了地理方案外，Android 还支持从意向加载街道视图。 从 Xamarin.Android 启动的街道视图应用程序的示例如下所示：

[![街道视图屏幕截图示例](maps-application-images/03-streetview.png)](maps-application-images/03-streetview.png#lightbox)

若要启动街道视图，只需使用 `google.streetview` URI 方案，如以下代码所示：

```csharp
var streetViewUri = Android.Net.Uri.Parse (
       "google.streetview:cbll=42.374260,-71.120824&cbp=1,90,,0,1.0&mz=20");  
var streetViewIntent = new Intent (Intent.ActionView, streetViewUri);  
StartActivity (streetViewIntent);
```

上面使用的 google.streetview URI 方案采用以下形式：

```csharp
google.streetview:cbll=lat,lng&cbp=1,yaw,,pitch,zoom&mz=mapZoom
```

如你所见，支持多个参数，如下所示：

- `lat` &ndash; 要在街道视图中显示的位置的纬度。

- `lng` &ndash; 要在街道视图中显示的位置的经度。

- `pitch` &ndash; 街道视图全景的角度，从中心测量，以度为单位，其中 90 度为垂直向下，-90 度为垂直向上。

- `yaw` &ndash; 街道视图全景的视觉中心，以顺时针方向从北测量，以度为单位。

- `zoom` &ndash; 街道视图全景的缩放倍数，其中 1.0 = 正常缩放，2.0 = 放大 2 倍，3.0 = 放大 4 倍，依此类推。

- `mz` &ndash; 从街道视图转到地图应用程序时将使用的地图缩放级别。

使用内置地图应用程序或街道视图是快速添加地图支持的简便方法。 但是，借助 Android 的地图 API 可以更好地控制地图体验。
