---
title: Xamarin.Essentials 单位转换器
description: Xamarin.Essentials 中的 UnitConverters 类提供了几种单位转换器，以在开发人员使用 Xamarin.Essentials 时为其提供帮助。
ms.assetid: ''
author: ''
ms.custom: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: deff6997ff921e6048ccb2497a0747c770501a04
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/28/2020
ms.locfileid: "84137132"
---
# <a name="xamarinessentials-unit-converters"></a>Xamarin.Essentials：单位转换器

UnitConverters 类提供了几种单位转换器，以在开发人员使用 Xamarin.Essentials 时为其提供帮助。

## <a name="get-started"></a>入门

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-unit-converters"></a>使用单位转换器

在类中添加对 Xamarin.Essentials 的引用：

```csharp
using Xamarin.Essentials;
```

通过使用 Xamarin.Essentials 中的静态 `UnitConverters` 类，可以使用所有单位转换器。 例如，可以轻松地将华氏度转换为摄氏度。

```csharp
var celsius = UnitConverters.FahrenheitToCelsius(32.0);
```

以下是可用转换的列表：

- FahrenheitToCelsius
- CelsiusToFahrenheit
- CelsiusToKelvin
- KelvinToCelsius
- MilesToMeters
- MilesToKilometers
- KilometersToMiles
- MetersToInternationalFeet
- InternationalFeetToMeters
- DegreesToRadians
- RadiansToDegrees
- DegreesPerSecondToRadiansPerSecond
- RadiansPerSecondToDegreesPerSecond
- DegreesPerSecondToHertz
- RadiansPerSecondToHertz
- HertzToDegreesPerSecond
- HertzToRadiansPerSecond
- KilopascalsToHectopascals
- HectopascalsToKilopascals
- KilopascalsToPascals
- HectopascalsToPascals
- AtmospheresToPascals
- PascalsToAtmospheres
- CoordinatesToMiles
- CoordinatesToKilometers
- KilogramsToPounds
- PoundsToKilograms
- StonesToPounds
- PoundsToStones

## <a name="api"></a>API

- [单位转换器源代码](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Types/UnitConverters.shared.cs)
- [单位转换器 API 文档](xref:Xamarin.Essentials.UnitConverters)

## <a name="related-video"></a>相关视频

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Unit-Conversion-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
