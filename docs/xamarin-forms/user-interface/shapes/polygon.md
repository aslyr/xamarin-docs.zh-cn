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
ms.openlocfilehash: e6f8ad3afdcdb9137869dc57078ac94895f4183c
ms.sourcegitcommit: ef3d4a70e70927c4f231b763842c5355f1571d15
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2020
ms.locfileid: "85243787"
---
# <a name="xamarinforms-shapes-polygon"></a>Xamarin.Forms形状：多边形

![](~/media/shared/preview.png "This API is currently pre-release")

[![下载示例](~/media/shared/download.png) 下载示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

`Polygon`类派生自 `Shape` 类，可用于绘制多边形，这系列线条是形成闭合形状的线。 有关 `Polygon` 该类继承自类的属性的信息 `Shape` ，请参阅[ Xamarin.Forms 形状](index.md)。

`Polygon` 定义以下属性:

- `Points`，类型为 `PointCollection` ，它是 [`Point`](xref:Xamarin.Forms.Point) 描述多边形顶点的结构的集合。
- `FillRule`，类型为 `FillRule` ，它指定如何确定形状的内部填充。 此属性的默认值为 `FillRule.EvenOdd`。

这些属性是由对象支持的 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) ，这意味着它们可以是数据绑定的目标和样式。

`PointsCollection`类型是 `ObservableCollection` [`Point`](xref:Xamarin.Forms.Point) 对象的。 `Point`结构定义 `X` 类型的和属性， `Y` `double` 表示二维空间中的 x 坐标和 y 坐标对。 因此， `Points` 应将属性设置为 x 坐标和 y 坐标对的列表，它们描述多边形顶点点，并用单个逗号和/或一个或多个空格分隔。 例如，"40，10 70，80" 和 "40 10，70 80" 都是有效的。

`FillRule` 枚举定义下列成员：

- `EvenOdd`表示一个规则，该规则确定某个点是否位于多边形的填充区域。 它在任何方向上从点到无穷画一条射线，并对该射线相交的形状内的段数进行计数。 如果此数为奇数，则该点在内部。 如果此数值为偶数，则该点在外部。
- `Nonzero`表示一个规则，该规则确定某个点是否位于多边形的填充区域。 它在任何方向上从点到无穷画一条射线，然后检查一段形状与射线相交的位置。 从零计数开始，每次段从左到右交叉时，计数就会递增，并且每次从右到左对一段时间都进行递减。 计算交点后，如果结果为零，则点在多边形之外。 否则，在内部。

## <a name="create-a-polygon"></a>创建多边形

若要绘制多边形，请创建 `Polygon` 对象并将其 `Points` 属性设置为形状的顶点。 将自动绘制一条线，用于连接第一个点和最后一个点。 若要在多边形内绘制，请将其 `Fill` 属性设置为 [`Color`](xref:Xamarin.Forms.Color) 。 若要为多边形指定轮廓，请将其 `Stroke` 属性设置为 [`Color`](xref:Xamarin.Forms.Color) 。 `StrokeThickness`属性指定多边形轮廓的粗细。

下面的 XAML 示例演示如何绘制实心多边形：

```xaml
<Polygon Points="40,10 70,80 10,50"
         Fill="AliceBlue"
         Stroke="Green"
         StrokeThickness="5" />
```

在此示例中，绘制了表示三角形的实心多边形：

![填充多边形](polygon-images/filled.png "填充多边形")

下面的 XAML 示例演示如何绘制虚线多边形：

```xaml
<Polygon Points="40,10 70,80 10,50"
         Fill="AliceBlue"
         Stroke="Green"
         StrokeThickness="5"
         StrokeDashArray="1,1"
         StrokeDashOffset="6" />
```

在此示例中，多边形轮廓为虚线：

![虚线多边形](polygon-images/dashed.png "虚线多边形")

有关绘制虚线多边形的详细信息，请参阅[绘制虚线形状](index.md#draw-dashed-shapes)。

下面的 XAML 示例显示了使用默认填充规则的多边形：

```xaml
<Polygon Points="0 48, 0 144, 96 150, 100 0, 192 0, 192 96, 50 96, 48 192, 150 200 144 48"
         Fill="Blue"
         Stroke="Red"
         StrokeThickness="3" />
```

在此示例中，每个多边形的填充行为都是使用 `EvenOdd` 填充规则确定的。

![EvenOdd 多边形](polygon-images/evenodd.png "EvenOdd 多边形")

下面的 XAML 示例显示了一个使用 `Nonzero` 填充规则的多边形：

```xaml
<Polygon Points="0 48, 0 144, 96 150, 100 0, 192 0, 192 96, 50 96, 48 192, 150 200 144 48"
         Fill="Black"
         FillRule="Nonzero"
         Stroke="Yellow"
         StrokeThickness="3" />
```

![非零多边形](polygon-images/nonzero.png "非零多边形")

在此示例中，每个多边形的填充行为都是使用 `Nonzero` 填充规则确定的。

## <a name="related-links"></a>相关链接

- [ShapeDemos （示例）](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Xamarin.Forms形状](index.md)
