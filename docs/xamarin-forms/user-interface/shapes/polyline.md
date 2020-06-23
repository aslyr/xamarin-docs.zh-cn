---
title: Xamarin.Forms形状：折线
description: Xamarin.Forms折线类可用于绘制一系列连接的直线。
ms.prod: xamarin
ms.assetid: 15D02690-AC03-457E-8815-8E4C17E4D642
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/21/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: fee7dd2a2e5b713b3a82fc2e1227b21caddbceaa
ms.sourcegitcommit: ef3d4a70e70927c4f231b763842c5355f1571d15
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2020
ms.locfileid: "85243781"
---
# <a name="xamarinforms-shapes-polyline"></a>Xamarin.Forms形状：折线

![](~/media/shared/preview.png "This API is currently pre-release")

[![下载示例](~/media/shared/download.png) 下载示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

`Polyline`类派生自 `Shape` 类，可用于绘制一系列连接的直线。 折线类似于多边形，只不过折线中的最后一个点未连接到第一个点。 有关 `Polyline` 该类继承自类的属性的信息 `Shape` ，请参阅[ Xamarin.Forms 形状](index.md)。

`Polyline` 定义以下属性:

- `Points`，类型为 `PointCollection` ，它是 `Point` 描述折线顶点的结构的集合。
- `FillRule`，类型为 `FillRule` ，指定如何组合折线中的相交区域。 此属性的默认值为 `FillRule.EvenOdd`。

这些属性是由对象支持的 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) ，这意味着它们可以是数据绑定的目标和样式。

`PointsCollection`类型是 `ObservableCollection` [`Point`](xref:Xamarin.Forms.Point) 对象的。 `Point`结构定义 `X` 类型的和属性， `Y` `double` 表示二维空间中的 x 坐标和 y 坐标对。 因此， `Points` 应将属性设置为描述折线顶点点的 x 坐标和 y 坐标对的列表，并用单个逗号和/或一个或多个空格分隔。 例如，"40，10 70，80" 和 "40 10，70 80" 都是有效的。

`FillRule` 枚举定义下列成员：

- `EvenOdd`表示一个规则，该规则确定某个点是否位于折线的填充区域。 它在任何方向上从点到无穷画一条射线，并对该射线相交的形状内的段数进行计数。 如果此数为奇数，则该点在内部。 如果此数值为偶数，则该点在外部。
- `Nonzero`表示一个规则，该规则确定某个点是否位于折线的填充区域。 它在任何方向上从点到无穷画一条射线，然后检查一段形状与射线相交的位置。 从零计数开始，每次段从左到右交叉时，计数就会递增，并且每次从右到左对一段时间都进行递减。 计算交点后，如果结果为零，则点在折线外。 否则，在内部。

## <a name="create-a-polyline"></a>创建折线

若要绘制折线，请创建 `Polyline` 对象并将其 `Points` 属性设置为形状的顶点。 若要为折线指定轮廓，请将其 `Stroke` 属性设置为 [`Color`](xref:Xamarin.Forms.Color) 。 `StrokeThickness`属性指定折线轮廓的粗细。

> [!IMPORTANT]
> 如果将的 `Fill` 属性设置 `Polyline` 为，则 [`Color`](xref:Xamarin.Forms.Color) 会绘制折线的内部空间，即使起点和终点不相交也是如此。

下面的 XAML 示例演示如何绘制折线：

```xaml
<Polyline Points="0,0 10,30, 15,0 18,60 23,30 35,30 40,0 43,60 48,30 100,30"
          Stroke="Red" />
```

在此示例中，绘制了红色折线：

![折线](polyline-images/stroke.png "折线")

下面的 XAML 示例演示如何绘制虚线折线：

```xaml
<Polyline Points="0,0 10,30, 15,0 18,60 23,30 35,30 40,0 43,60 48,30 100,30"
          Stroke="Red"
          StrokeThickness="2"
          StrokeDashArray="1,1"
          StrokeDashOffset="6" />
```

在此示例中，折线为虚线：

![虚线折线](polyline-images/dashed.png "虚线折线")

有关绘制虚线折线的详细信息，请参阅[绘制虚线形状](index.md#draw-dashed-shapes)。

下面的 XAML 示例显示了使用默认填充规则的折线：

```xaml
<Polyline Points="0 48, 0 144, 96 150, 100 0, 192 0, 192 96, 50 96, 48 192, 150 200 144 48"
          Fill="Blue"
          Stroke="Red"
          StrokeThickness="3" />
```

在此示例中，使用填充规则确定折线的填充行为 `EvenOdd` 。

![EvenOdd 折线](polyline-images/evenodd.png "EvenOdd polyine")

下面的 XAML 示例显示了一个使用 `Nonzero` 填充规则的折线：

```xaml
<Polyline Points="0 48, 0 144, 96 150, 100 0, 192 0, 192 96, 50 96, 48 192, 150 200 144 48"
          Fill="Black"
          FillRule="Nonzero"
          Stroke="Yellow"
          StrokeThickness="3" />
```

![非零折线](polyline-images/nonzero.png "非零折线")

在此示例中，使用填充规则确定折线的填充行为 `Nonzero` 。

## <a name="related-links"></a>相关链接

- [ShapeDemos （示例）](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Xamarin.Forms形状](index.md)
