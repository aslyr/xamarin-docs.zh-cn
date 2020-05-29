---
title: Xamarin.Forms实验性标志
description: Xamarin.Forms试验性标志使工程团队能够更快地向用户交付新功能，同时还能在功能 Api 迁移到稳定版本之前对其进行更改。
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: b548323330ccdce6fb01e83c7e8ab7c2d5307125
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/28/2020
ms.locfileid: "84139056"
---
# <a name="xamarinforms-experimental-flags"></a>Xamarin.Forms实验性标志

Xamarin.Forms实现新功能时，有时会将其置于实验标志之后。 这使得工程团队能够更快地为你提供新功能，同时还能在功能 Api 迁移到稳定版本之前对其进行更改。 一旦将此功能移到稳定版本，就会删除该实验标志。

Xamarin.Forms包括以下实验标志：

- `AppTheme_Experimental`
- `CarouselView_Experimental`
- `Expander_Experimental`
- `IndicatorView_Experimental`
- `Markup_Experimental`
- `MediaElement_Experimental`
- `RadioButton_Experimental`
- `Shell_UWP_Experimental`
- `StateTriggers_Experimental`
- `SwipeView_Experimental`

使用实验标志后面的功能要求你在应用程序中启用标志或标志。 启用实验性标志的方法有两种：

- 启用平台项目中的实验标志或标志。
- 在类中启用实验标志或标志 `App` 。

> [!WARNING]
> 如果不启用标志，使用实验标志后面的功能将导致应用程序引发异常，指示必须启用哪个标志。

## <a name="enable-flags-in-platform-projects"></a>在平台项目中启用标志

`Xamarin.Forms.Forms.SetFlags`方法可用于在平台项目中启用实验标志：

```csharp
Xamarin.Forms.Forms.SetFlags("CarouselView_Experimental");
```

`SetFlags`方法应在 `AppDelegate` IOS、Android 类中的类以及 `MainActivity` `App` UWP 上的类中调用。

> [!IMPORTANT]
> 必须先在平台项目中启用实验标志，然后才能 `Forms.Init` 调用方法。

`Xamarin.Forms.Forms.SetFlags`方法接受一个 `string` 数组参数，这使得可以在一个方法调用中启用多个实验标志：

```csharp
Xamarin.Forms.Forms.SetFlags(new string[] { "CarouselView_Experimental", "IndicatorView_Experimental", "SwipeView_Experimental" });
```

> [!WARNING]
> 不要多次调用 `SetFlags` 方法，因为后续调用将覆盖以前调用的结果。

## <a name="enable-flags-in-your-app-class"></a>在应用类中启用标志

`Device.SetFlags`方法可用于在 `App` 共享代码项目的类中启用实验标志：

```csharp
Device.SetFlags(new string[]{ "MediaElement_Experimental" });
```

`Device.SetFlags`方法接受参数，这样就可以 `IReadOnlyList<string>` 在单个方法调用中启用多个实验标志：

```csharp
Device.SetFlags(new string[]{ "CarouselView_Experimental", "MediaElement_Experimental", "SwipeView_Experimental" });
```

> [!WARNING]
> 不要多次调用 `SetFlags` 方法，因为后续调用将覆盖以前调用的结果。
