---
title: Xamarin.Forms形状：几何图形
description: Xamarin.Forms使用 geometry 类可以描述二维形状的几何。
ms.prod: xamarin
ms.assetid: 07DE3D66-1820-4642-BDDF-84146D40C99D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: c816da59dce291e6fbd354e2c6079fe24044e978
ms.sourcegitcommit: 34fa3086c55b1e01838419c930f839c20662c362
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/18/2020
ms.locfileid: "84990769"
---
# <a name="xamarinforms-shapes-geometries"></a>Xamarin.Forms形状：几何图形

![](~/media/shared/preview.png "This API is currently pre-release")

[![下载示例](~/media/shared/download.png) 下载示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

`Geometry`类以及从中派生的类使你可以描述二维形状的几何。 `Geometry`从两个或多个 geometry 对象创建的对象可以很简单，例如矩形和圆圈，或复合对象。 可以使用类来创建更复杂的几何图形 `PathGeometry` ，这使您能够描述弧线和曲线。

`Geometry`和 `Shape` 类看起来类似，因为它们都描述了二维形状，但有一个重要的差异。 类 `Geometry` 派生自 `BindableObject` 类，而 `Shape` 类派生自 `View` 类。 因此， `Shape` 对象可以呈现自身并参与布局系统，而对象则 `Geometry` 不能。 尽管 `Shape` 对象比对象更易于使用 `Geometry` ，但 `Geometry` 对象更通用。 `Shape`使用对象呈现2d 图形时， `Geometry` 可以使用对象来定义2d 图形的几何区域，并定义用于剪裁的区域。

所有几何图形的基类都是抽象类 `Geometry` 。 从此类派生的类可以分为三个类别：简单的几何图形、路径几何图形和复合几何图形。

## <a name="simple-geometries"></a>简单几何图形

简单的 geometry 类为 `EllipseGeometry` 、 `LineGeometry` 和 `RectangleGeometry` 。 它们用于创建基本几何形状，如圆、直线和矩形。 可以使用或通过将 geometry 对象组合在一起来创建这些相同的形状和更复杂的形状， `PathGeometry` 但这些类提供了一种更简单的方法来生成这些基本几何形状。

### <a name="ellipsegeometry"></a>System.windows.media.ellipsegeometry>

椭圆几何表示几何或椭圆或圆，并由中心点、x 轴半径和 y 轴半径定义。

`EllipseGeometry`类定义以下属性：

- `Center``Point`表示几何图形中心点的类型的。
- `RadiusX`，类型为 `double` ，表示几何图形的 x 轴半径值。 此属性的默认值为0.0。
- `RadiusY`，类型为 `double` ，表示几何图形的 y 轴半径值。 此属性的默认值为0.0。

这些属性是由对象支持的 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) ，这意味着它们可以是数据绑定的目标和样式。

下面的示例演示如何创建和呈现 `EllipseGeometry` ：

```xaml
<Path Fill="Blue"
      Stroke="Red"
      StrokeThickness="1">
  <Path.Data>
    <EllipseGeometry Center="50,50"
                     RadiusX="50"
                     RadiusY="50" />
  </Path.Data>
</Path>
```

在此示例中，的中心 `EllipseGeometry` 设置为（50，50），x 轴半径和 y 轴均设置为50。 这将创建一个直径为100的圆，其内部绘制为蓝色。

### <a name="linegeometry"></a>LineGeometry

直线几何表示线条的几何，并通过指定直线的起点和终点来定义。

`LimeGeometry`类定义以下属性：

- `StartPoint`，类型为 `Point` ，它表示线条的起点。
- `EndPoint`，类型为 `Point` ，它表示线条的终点。

这些属性是由对象支持的 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) ，这意味着它们可以是数据绑定的目标和样式。

下面的示例演示如何创建和呈现 `LineGeometry` ：

```xaml
<Path Stroke="Black"
      StrokeThickness="1">
  <Path.Data>
    <LineGeometry StartPoint="10,20"
                  EndPoint="100,130" />
  </Path.Data>
</Path>
```

在此示例中， `LineGeometry` 从（10，20）绘制到（100130）。

### <a name="rectanglegeometry"></a>RectangleGeometry

矩形几何图形表示一个矩形，并定义为 `Rect` 指定其相对位置及其高度和宽度的结构。

`RectangleGeometry`类定义以下属性：

- `Rect`，类型为 `FormsRect` ，它表示矩形的尺寸。

这些属性是由对象支持的 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) ，这意味着它们可以是数据绑定的目标和样式。

下面的示例演示如何创建和呈现 `RectangleGeometry` ：

```xaml
<Path Fill="Blue"
      Stroke="Black"
      StrokeThickness="1">
  <Path.Data>
    <RectangleGeometry Rect="50,50,25,25" />
  </Path.Data>
</Path>
```

矩形的位置和尺寸由 `Rect` 结构定义。 在此示例中，位置是（50，50），高度和宽度均为25，这将创建一个正方形。

## <a name="path-geometries"></a>路径几何图形

路径几何描述了可由弧形、曲线、椭圆、直线和矩形组成的复杂形状。

`PathGeometry`类定义以下属性：

- `Figures`，类型为 `PathFigureCollection` ，它表示 `PathFigure` 描述路径内容的对象的集合。
- `FillRule`，类型为 `FillRule` ，确定如何组合几何图形中包含的相交区域。 此属性的默认值为 `FillRule.EvenOdd`。

> [!NOTE]
> `Figures`属性是类的 `ContentProperty` `PathGeometry` ，因此不需要从 XAML 显式设置。

这些属性是由对象支持的 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) ，这意味着它们可以是数据绑定的目标和样式。

由 `PathGeometry` 对象的集合组成 `PathFigure` ，每个对象 `PathFigure` 描述几何图形中的一个形状。 每个 `PathFigure` 对象都由一个或多个 `PathSegment` 对象组成，其中每个对象都描述了一段形状。 有多种类型的段：

- `ArcSegment`，它在两个点之间创建一条椭圆弧。
- `BezierSegment`，用于在两个点之间创建一条三次方贝塞尔曲线。
- `LineSegment`，它在两个点之间创建一条直线。
- `PolyBezierSegment`，它创建一系列三次贝塞尔曲线。
- `PolyLineSegment`，它创建一系列线条。
- `PolyQuadraticBezierSegment`，它创建一系列二次贝塞尔曲线。
- `QuadraticBezierSegment`，它创建二次贝塞尔曲线。

内的段 `PathFigure` 合并为单个几何形状，其中每个段的终点都是下一段的起点。 的 `StartPoint` 属性 `PathFigure` 指定绘制第一段的起点。 每个后续段都从上一段的终点开始。 例如， `10,50` `10,150` 可以通过将 `StartPoint` 属性设置为 `10,50` 并创建 `LineSegment` 具有 `Point` 属性设置的 `10,150` 来定义中的垂直线：

```xaml
<Path Stroke="Black"
      StrokeThickness="1">
    <Path.Data>
        <PathGeometry>
            <PathGeometry.Figures>
                <PathFigureCollection>
                    <PathFigure StartPoint="10,50">
                        <PathFigure.Segments>
                            <PathSegmentCollection>
                                <LineSegment Point="10,150" />
                            </PathSegmentCollection>
                        </PathFigure.Segments>
                    </PathFigure>
                </PathFigureCollection>
            </PathGeometry.Figures>
        </PathGeometry>
    </Path.Data>
</Path>
```

可以通过组合使用 `PathSegment` 对象和使用中的多个对象，来创建更复杂的几何图形 `PathFigure` `PathGeometry` 。

## <a name="composite-geometries"></a>复合几何图形

复合几何图形对象可以使用创建 `GeometryGroup` 。 `GeometryGroup`类创建一个或多个对象的复合几何图形 `Geometry` 。 可以将任意数量的 `Geometry` 对象添加到 `GeometryGroup` 中。

下面的示例演示如何在中组合几何图形 `GeometryGroup` ：

```xaml
<Path Stroke="Green"
      StrokeThickness="2"
      Fill="Orange">
    <Path.Data>
        <GeometryGroup>
            <EllipseGeometry RadiusX="100"
                             RadiusY="100"
                             Center="150,150" />
            <EllipseGeometry RadiusX="100"
                             RadiusY="100"
                             Center="250,150" />
            <EllipseGeometry RadiusX="100"
                             RadiusY="100"
                             Center="150,250" />
            <EllipseGeometry RadiusX="100"
                             RadiusY="100"
                             Center="250,250" />
        </GeometryGroup>
    </Path.Data>
</Path>
```

在此示例中， `EllipseGeometry` 组合了具有相同 x 半径和 y 轴坐标但具有不同中心坐标的四个对象。 这会创建四个重叠圆圈，它们的内部填充了 `EvenOdd` 填充规则橙色。

## <a name="other-features"></a>其他功能

`GeometryHelper`类提供以下帮助器方法：

- `FlattenGeometry`
- `FlattenCubicBezier`
- `FlattenQuadraticBezier`
- `FlattenArc`

## <a name="related-links"></a>相关链接

- [ShapeDemos （示例）](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Xamarin.Forms形状](index.md)
