---
title: Xamarin. Forms 试验性标志
description: Xamarin. 窗体试验性标志使工程团队能够更快地向用户交付新功能，同时仍然能够在功能 Api 迁移到稳定版本之前对其进行更改。
ms.prod: xamarin
ms.assetid: AF4BDD27-89F6-48AE-A8CD-D7E4DDA2CCA2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/20/2020
ms.openlocfilehash: cebb996da992058616f9cf96ef3212c9ce27022a
ms.sourcegitcommit: 6c60914b380ff679bbffd7790edd4d5e18005d0a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/21/2020
ms.locfileid: "80112599"
---
# <a name="xamarinforms-experimental-flags"></a>Xamarin. Forms 试验性标志

当实现新的 Xamarin 窗体功能时，有时会将其置于实验标志之后。 这使得工程团队能够更快地为你提供新功能，同时还能在功能 Api 迁移到稳定版本之前对其进行更改。 一旦将此功能移到稳定版本，就会删除该实验标志。

Xamarin 包含以下实验标志：

- `CarouselView_Experimental`
- `IndicatorView_Experimental`
- `Markup_Experimental`
- `MediaElement_Experimental`
- `Shell_UWP_Experimental`
- `StateTriggers_Experimental`
- `SwipeView_Experimental`

使用实验标志后面的功能要求你在应用程序中启用标志或标志。 启用实验性标志的方法有两种：

- 启用平台项目中的实验标志或标志。
- 启用 `App` 类中的实验标志或标志。

> [!WARNING]
> 如果不启用标志，使用实验标志后面的功能将导致应用程序引发异常，指示必须启用哪个标志。

## <a name="enable-flags-in-platform-projects"></a>在平台项目中启用标志

`Xamarin.Forms.Forms.SetFlags` 方法可用于在平台项目中启用实验标志：

```csharp
Xamarin.Forms.Forms.SetFlags("CarouselView_Experimental");
```

`SetFlags` 方法应 `AppDelegate` 在 iOS、Android 上的 `MainActivity` 类以及 UWP 上的 `App` 类中调用。

> [!IMPORTANT]
> 必须先在平台项目中启用实验标志，然后才能调用 `Forms.Init` 方法。

`Xamarin.Forms.Forms.SetFlags` 方法接受 `string` 数组参数，这样就可以在一个方法调用中启用多个实验标志：

```csharp
Xamarin.Forms.Forms.SetFlags(new string[] { "CarouselView_Experimental", "IndicatorView_Experimental", "SwipeView_Experimental" });
```

> [!WARNING]
> 请勿多次调用 `SetFlags` 方法，因为后续调用将覆盖先前调用的结果。

## <a name="enable-flags-in-your-app-class"></a>在应用类中启用标志

`Device.SetFlags` 方法可用于在共享代码项目的 `App` 类中启用实验标志：

```csharp
Device.SetFlags(new string[]{ "MediaElement_Experimental" });
```

`Device.SetFlags` 方法接受 `IReadOnlyList<string>` 参数，这样就可以在一个方法调用中启用多个实验标志：

```csharp
Device.SetFlags(new string[]{ "CarouselView_Experimental", "MediaElement_Experimental", "SwipeView_Experimental" });
```

> [!WARNING]
> 请勿多次调用 `SetFlags` 方法，因为后续调用将覆盖先前调用的结果。
