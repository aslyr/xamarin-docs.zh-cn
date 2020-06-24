---
title: Xamarin.Essentials：设备显示信息
description: 本文档介绍 Xamarin.Essentials 中的 DeviceDisplay 类，此类提供运行应用程序的设备的屏幕指标。
ms.assetid: 2821C908-C613-490D-8E8C-1BD3269FCEEA
author: jamesmontemagno
ms.custom: video
ms.author: jamont
ms.date: 11/04/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 867e6bd1828d4158f70226dbaad678f9d6037bb0
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/18/2020
ms.locfileid: "84802395"
---
# <a name="xamarinessentials-device-display-information"></a>Xamarin.Essentials：设备显示信息

DeviceDisplay 类提供有关运行应用程序的设备屏幕指标的信息，并可以请求在应用程序运行时防止屏幕进入睡眠状态。

## <a name="get-started"></a>入门

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-devicedisplay"></a>使用 DeviceDisplay

在类中添加对 Xamarin.Essentials 的引用：

```csharp
using Xamarin.Essentials;
```

## <a name="main-display-info"></a>主显示器信息

除了基本的设备信息外，DeviceDisplay 类还包含有关设备的屏幕和方向信息。

```csharp
// Get Metrics
var mainDisplayInfo = DeviceDisplay.MainDisplayInfo;

// Orientation (Landscape, Portrait, Square, Unknown)
var orientation = mainDisplayInfo.Orientation;

// Rotation (0, 90, 180, 270)
var rotation = mainDisplayInfo.Rotation;

// Width (in pixels)
var width = mainDisplayInfo.Width;

// Height (in pixels)
var height = mainDisplayInfo.Height;

// Screen density
var density = mainDisplayInfo.Density;
```

DeviceDisplay 类还会公开可以订阅的一个事件，每当任何屏幕指标更改时就会触发此事件：

```csharp
public class DisplayInfoTest
{
    public DisplayInfoTest()
    {
        // Subscribe to changes of screen metrics
        DeviceDisplay.MainDisplayInfoChanged += OnMainDisplayInfoChanged;
    }

    void OnMainDisplayInfoChanged(object sender, DisplayInfoChangedEventArgs  e)
    {
        // Process changes
        var displayInfo = e.DisplayInfo;
    }
}
```

DeviceDisplay 类公开调用了 `KeepScreenOn` 的 `bool` 属性，可将其设置为防止关闭或锁定设备的显示器。

```csharp
public class KeepScreenOnTest
{
    public void ToggleScreenLock()
    {
        DeviceDisplay.KeepScreenOn = !DeviceDisplay.KeepScreenOn;
    }
}
```

## <a name="platform-differences"></a>平台差异

# <a name="android"></a>[Android](#tab/android)

没有差异。

# <a name="ios"></a>[iOS](#tab/ios)

- 必须在 UI 线程上访问 `DeviceDisplay`，否则将引发异常。 可以使用 [`MainThread.BeginInvokeOnMainThread`](~/essentials/main-thread.md) 方法在 UI 线程上运行该代码。

# <a name="uwp"></a>[UWP](#tab/uwp)

没有差异。

--------------

## <a name="api"></a>API

- [DeviceDisplay 源代码](https://github.com/xamarin/Essentials/tree/main/Xamarin.Essentials/DeviceDisplay)
- [DeviceDisplay API 文档](xref:Xamarin.Essentials.DeviceDisplay)

## <a name="related-video"></a>相关视频

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Device-Display-Information-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
