---
title: Xamarin.Essentials：磁力计
description: Xamarin.Essentials 中的 Magnetometer 类使你能够监视设备的磁力计传感器，此传感器指示设备相对于地球磁场的方向。
ms.assetid: 64DD0D41-03E2-40DD-9EC8-101CA0ED852B
author: jamesmontemagno
ms.author: jamont
ms.date: 11/04/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: b01bc1fd9c65186952635c5f472b1ac6beb0c9bd
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/18/2020
ms.locfileid: "84802279"
---
# <a name="xamarinessentials-magnetometer"></a>Xamarin.Essentials：磁力计

Magnetometer 类使你能够监视设备的磁力计传感器，此传感器指示设备相对于地球磁场的方向****。

## <a name="get-started"></a>入门

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-magnetometer"></a>使用 Magnetometer

在类中添加对 Xamarin.Essentials 的引用：

```csharp
using Xamarin.Essentials;
```

通过调用 `Start` 和 `Stop` 方法来使用 Magnetometer 功能以侦听磁力计的变化。 然后通过 `ReadingChanged` 事件反馈任何变化。 示例用法如下：

```csharp

public class MagnetometerTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.UI;

    public MagnetometerTest()
    {
        // Register for reading changes.
        Magnetometer.ReadingChanged += Magnetometer_ReadingChanged;
    }

    void Magnetometer_ReadingChanged(object sender, MagnetometerChangedEventArgs e)
    {
        var data = e.Reading;
        // Process MagneticField X, Y, and Z
        Console.WriteLine($"Reading: X: {data.MagneticField.X}, Y: {data.MagneticField.Y}, Z: {data.MagneticField.Z}");
    }

    public void ToggleMagnetometer()
    {
        try
        {
            if (Magnetometer.IsMonitoring)
              Magnetometer.Stop();
            else
              Magnetometer.Start(speed);
        }
        catch (FeatureNotSupportedException fnsEx)
        {
            // Feature not supported on device
        }
        catch (Exception ex)
        {
            // Other error has occurred.
        }
    }
}
```

所有数据将以 µT（微特斯拉）为单位返回。

[!include[](~/essentials/includes/sensor-speed.md)]

## <a name="api"></a>API

- [Magnetometer 源代码](https://github.com/xamarin/Essentials/tree/main/Xamarin.Essentials/Magnetometer)
- [Magnetometer API 文档](xref:Xamarin.Essentials.Magnetometer)
