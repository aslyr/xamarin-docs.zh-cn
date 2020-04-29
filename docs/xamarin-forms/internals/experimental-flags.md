---
title: Xamarin. Forms 试验性标志
description: Xamarin. 窗体试验性标志使工程团队能够更快地向用户交付新功能，同时仍然能够在功能 Api 迁移到稳定版本之前对其进行更改。
ms.prod: xamarin
ms.assetid: AF4BDD27-89F6-48AE-A8CD-D7E4DDA2CCA2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/14/2020
ms.openlocfilehash: cca377a7a88599bc34fd66695ad303162e6be200
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "82516550"
---
# <a name="xamarinforms-experimental-flags"></a>Xamarin. Forms 试验性标志

当实现新的 Xamarin 窗体功能时，有时会将其置于实验标志之后。 这使得工程团队能够更快地为你提供新功能，同时还能在功能 Api 迁移到稳定版本之前对其进行更改。 一旦将此功能移到稳定版本，就会删除该实验标志。

Xamarin 包含以下实验标志：

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
- 在`App`类中启用实验标志或标志。

> [!WARNING]
> 如果不启用标志，使用实验标志后面的功能将导致应用程序引发异常，指示必须启用哪个标志。

## <a name="enable-flags-in-platform-projects"></a>在平台项目中启用标志

`Xamarin.Forms.Forms.SetFlags`方法可用于在平台项目中启用实验标志：

```csharp
Xamarin.Forms.Forms.SetFlags("CarouselView_Experimental");
```

方法`SetFlags`应在 IOS、Android `AppDelegate` `MainActivity`类中的类以及 UWP 上的`App`类中调用。

> [!IMPORTANT]
> 必须先在平台项目中启用实验标志，然后才能`Forms.Init`调用方法。

`Xamarin.Forms.Forms.SetFlags`方法接受一个`string`数组参数，这使得可以在一个方法调用中启用多个实验标志：

```csharp
Xamarin.Forms.Forms.SetFlags(new string[] { "CarouselView_Experimental", "IndicatorView_Experimental", "SwipeView_Experimental" });
```

> [!WARNING]
> 不要多次调用`SetFlags`方法，因为后续调用将覆盖以前调用的结果。

## <a name="enable-flags-in-your-app-class"></a>在应用类中启用标志

`Device.SetFlags`方法可用于在共享代码项目的`App`类中启用实验标志：

```csharp
Device.SetFlags(new string[]{ "MediaElement_Experimental" });
```

`Device.SetFlags`方法接受`IReadOnlyList<string>`参数，这样就可以在单个方法调用中启用多个实验标志：

```csharp
Device.SetFlags(new string[]{ "CarouselView_Experimental", "MediaElement_Experimental", "SwipeView_Experimental" });
```

> [!WARNING]
> 不要多次调用`SetFlags`方法，因为后续调用将覆盖以前调用的结果。
