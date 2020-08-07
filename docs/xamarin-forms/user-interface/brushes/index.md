---
title: Xamarin.Forms刷子
description: Xamarin.Forms画笔类是一个抽象类，它使用其输出绘制区域。
ms.prod: xamarin
ms.assetid: 44420FC2-304C-4175-8654-76769F79A813
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/28/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 4f1f56103b20eac84ce6106c0955acebf974cfe3
ms.sourcegitcommit: 08290d004d1a7e7ac579bf1f96abf8437921dc70
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/07/2020
ms.locfileid: "87919078"
---
# <a name="no-locxamarinforms-brushes"></a>Xamarin.Forms刷子

![预览 API](~/media/shared/preview.png "此 API 当前为预发布版本")

使用画笔，你可以使用不同的方法绘制某个区域，如控件的背景。 中的画笔支持在 Xamarin.Forms `Xamarin.Forms` IOS、Android、macOS、通用 WINDOWS 平台 (UWP) 上的命名空间中以及 WINDOWS PRESENTATION FOUNDATION (WPF) 中提供。

> [!IMPORTANT]
> 中的画笔支持 Xamarin.Forms 当前为试验性，只能通过设置标志来使用 `Brush_Experimental` 。 有关详细信息，请参阅[实验标志](~/xamarin-forms/internals/experimental-flags.md)。

`Brush`类是一个抽象类，它使用其输出绘制区域。 派生自的类 `Brush` 描述了不同的区域绘制方式。 以下列表描述了中可用的不同画笔类型 Xamarin.Forms ：

- `SolidColorBrush`，它使用纯色绘制区域。 有关详细信息，请参阅[ Xamarin.Forms 刷子：纯色](solidcolor.md)。
- `LinearGradientBrush`，它使用线性渐变绘制区域。 有关详细信息，请参阅[ Xamarin.Forms 画笔：线性渐变](lineargradient.md)。
- `RadialGradientBrush`使用径向渐变绘制区域。 有关详细信息，请参阅[ Xamarin.Forms 画笔：放射梯度](radialgradient.md)。

可以将这些画笔类型的实例分配给的 `Stroke` 和 `Fill` 属性，并将属性分配给的 `Shape` `Background` 属性 [`VisualElement`](xref:Xamarin.Forms.VisualElement) 。

> [!NOTE]
> `VisualElement.Background`属性允许在任何控件中将画笔用作背景。

`Brush`类还具有一个 `IsNullOrEmpty` 方法，该方法返回 `bool` 表示是否定义了画笔的。
