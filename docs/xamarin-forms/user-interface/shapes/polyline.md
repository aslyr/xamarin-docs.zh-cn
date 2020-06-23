---
title: Xamarin.Forms形状：折线
description: Xamarin.Forms折线类可用于绘制一系列连接的直线。
ms.prod: xamarin
ms.assetid: 15D02690-AC03-457E-8815-8E4C17E4D642
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: b6c884caa7bfb301f949f353f3c6c3445704aa89
ms.sourcegitcommit: d86b7a18cf8b1ef28cd0fe1d311f1c58a65101a8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/19/2020
ms.locfileid: "85101352"
---
# <a name="xamarinforms-shapes-polyline"></a>Xamarin.Forms形状：折线

![](~/media/shared/preview.png "This API is currently pre-release")

[![下载示例](~/media/shared/download.png) 下载示例](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/ShapesDemos/)

`Polyline`类派生自 `Shape` 类，可用于绘制一系列连接的直线。 有关 `Polyline` 该类继承自类的属性的信息 `Shape` ，请参阅[ Xamarin.Forms 形状](index.md)。

`Polyline` 定义以下属性:

- `Points`，类型为 `PointCollection` ，它是 `Point` 描述折线顶点的结构的集合。
- `FillRule`，类型为 `FillRule` ，指定如何组合折线中的相交区域。 此属性的默认值为 `FillRule.EvenOdd`。

这些属性是由对象支持的 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) ，这意味着它们可以是数据绑定的目标和样式。

## <a name="create-a-polyline"></a>创建折线

下面的 XAML 示例演示如何绘制多边形：

```xaml
<Polyline Points="0,0 10,30, 15,0 18,60 23,30 35,30 40,0 43,60 48,30 100,30"
          Stroke="Red"
          HeightRequest="100"
          WidthRequest="500" />
```

## <a name="related-links"></a>相关链接

- [ShapeDemos （示例）](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/ShapesDemos/)
- [Xamarin.Forms形状](index.md)