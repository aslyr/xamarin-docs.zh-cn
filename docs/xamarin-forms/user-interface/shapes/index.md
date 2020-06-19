---
title: Xamarin.Forms形状
description: Xamarin.Forms形状是使您能够在屏幕上绘制形状的视图类型。
ms.prod: xamarin
ms.assetid: 4E749FE8-852C-46DA-BB1E-652936106357
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 6f8cb91a3699ce7e18e7f4d2e891fafa1ff552c2
ms.sourcegitcommit: 34fa3086c55b1e01838419c930f839c20662c362
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/18/2020
ms.locfileid: "84990765"
---
# <a name="xamarinforms-shapes"></a>Xamarin.Forms形状

![](~/media/shared/preview.png "This API is currently pre-release")

`Shape`是的一种类型 [`View`](xref:Xamarin.Forms.View) ，它使您能够在屏幕上绘制形状。 `Shape`对象可在布局类和大多数控件内使用，因为 `Shape` 该类派生自 `View` 类。

Xamarin.Forms可在 `Xamarin.Forms.Shapes` iOS、Android、macOS、通用 Windows 平台（UWP）和 Windows Presentation Foundation （WPF）上的命名空间中使用形状。

> [!IMPORTANT]
> Xamarin.Forms形状当前为实验性，只能通过设置标志来使用 `Shapes_Experimental` 。 有关详细信息，请参阅[实验标志](~/xamarin-forms/internals/experimental-flags.md)。

`Shape` 定义以下属性:

- `Aspect`类型为的，它 `Stretch` 描述形状如何填充其分配的空间。 此属性的默认值为 `Stretch.None`。
- `Fill`类型为的， `Color` 指示用于绘制形状内部的颜色。
- `Stroke`类型为的， `Color` 指示用于绘制形状边框的颜色。
- `StrokeDashArray`，类型为 `DoubleCollection` ，它表示值的集合，这些 `double` 值指示用于勾勒形状轮廓的虚线和间隙的模式。
- `StrokeDashOffset`类型为 `double` ，指定短划线模式内虚线开始处的距离。 此属性的默认值为0.0。
- `StrokeLineCap`，类型为 `PenLineCap` ，描述直线或线段的开头和结尾处的形状。 此属性的默认值为 `PenLineCap.Flat`。
- `StrokeLineJoin`类型为的， `PenLineJoin` 它指定在形状的顶点处使用的联接类型。 此属性的默认值为 `PenLineJoin.Miter`。
- `StrokeThickness`类型为的， `double` 指示形状轮廓的宽度。 此属性的默认值为1.0。

这些属性是由对象支持的 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) ，这意味着它们可以是数据绑定的目标和样式。

Xamarin.Forms定义派生自类的多个对象 `Shape` 。 其中包括 `Ellipse`、`Line`、`Path`、`Polygon`、`Polyline`、`Rectangle`。

## <a name="related-links"></a>相关链接

- [ShapeDemos （示例）](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
