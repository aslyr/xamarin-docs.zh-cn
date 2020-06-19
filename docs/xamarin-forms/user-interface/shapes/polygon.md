---
title: Xamarin.Forms形状：多边形
description: Xamarin.Forms多边形类可用于绘制多边形，多边形是形成闭合形状的连接的一系列线条。
ms.prod: xamarin
ms.assetid: D6539F60-A5AC-46EF-86EB-E9F508EB1FA8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 455221b71c612b7987f876e8e9b82e33e5d91129
ms.sourcegitcommit: 34fa3086c55b1e01838419c930f839c20662c362
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/18/2020
ms.locfileid: "84990869"
---
# <a name="xamarinforms-shapes-polygon"></a>Xamarin.Forms形状：多边形

![](~/media/shared/preview.png "This API is currently pre-release")

[![下载示例](~/media/shared/download.png) 下载示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

`Polygon`类派生自 `Shape` 类，可用于绘制多边形，这系列线条是形成闭合形状的线。 有关 `Polygon` 该类继承自类的属性的信息 `Shape` ，请参阅[ Xamarin.Forms 形状](index.md)。

`Polygon` 定义以下属性:

- `Points`，类型为 `PointCollection` ，它是 `Point` 描述多边形顶点的结构的集合。
- `FillRule`，类型为 `FillRule` ，它指定如何确定形状的内部填充。 此属性的默认值为 `FillRule.EvenOdd`。

这些属性是由对象支持的 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) ，这意味着它们可以是数据绑定的目标和样式。

## <a name="create-a-polygon"></a>创建多边形

下面的 XAML 示例演示如何绘制多边形：

```xaml
<Polygon Points="40,10 70,80 10,50"
         Fill="AliceBlue"
         Stroke="Green"
         StrokeThickness="5"
         HeightRequest="100"
         WidthRequest="200" />
```

## <a name="related-links"></a>相关链接

- [ShapeDemos （示例）](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Xamarin.Forms形状](index.md)
