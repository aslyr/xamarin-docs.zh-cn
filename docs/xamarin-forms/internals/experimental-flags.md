---
title: Xamarin.Forms实验性标志
description: Xamarin.Forms试验性标志使工程团队能够更快地向用户交付新功能，同时还能在功能 Api 迁移到稳定版本之前对其进行更改。
ms.prod: xamarin
ms.assetid: AF4BDD27-89F6-48AE-A8CD-D7E4DDA2CCA2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/13/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 94fed78d7cf67ec5b6d783b1ced25a81266242d6
ms.sourcegitcommit: f7fe46c0236a7130b63a33d9d1670d5111582dd2
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/13/2020
ms.locfileid: "88186104"
---
# <a name="no-locxamarinforms-experimental-flags"></a>Xamarin.Forms实验性标志

Xamarin.Forms实现新功能时，有时会将其置于实验标志之后。 这使得工程团队能够更快地为你提供新功能，同时还能在功能 Api 迁移到稳定版本之前对其进行更改。 一旦将此功能移到稳定版本，就会删除该实验标志。

Xamarin.Forms包括以下实验标志：

- `Brush_Experimental`
- `CarouselView_Experimental`
- `DragAndDrop_Experimental`
- `Expander_Experimental`
- `Markup_Experimental`
- `MediaElement_Experimental`
- `RadioButton_Experimental`
- `Shapes_Experimental`
- `Shell_UWP_Experimental`
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
Xamarin.Forms.Forms.SetFlags(new string[] { "CarouselView_Experimental", "MediaElement_Experimental", "SwipeView_Experimental" });
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

## <a name="old-experimental-flags"></a>旧的实验性标志

下表列出了现已公开上市的功能的实验性标志，以及 Xamarin.Forms 在其中删除了实验标志的版本：

| Flag | Xamarin.Forms拆卸 |
| ---- | --------------------- |
| `AppTheme_Experimental` | 4.8 |
| `CollectionView_Experimental` | 4.3 |
| `FastRenderers_Experimental` | 4.0 |
| `IndicatorView_Experimental` | 4.7 |
| `Shell_Experimental` | 4.0  |
| `StateTriggers_Experimental` | 4.7 |
| `Visual_Experimental` | 3.6 |
