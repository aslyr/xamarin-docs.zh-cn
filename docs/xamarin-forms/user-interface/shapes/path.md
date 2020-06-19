---
title: Xamarin.Forms形状：路径
description: Xamarin.FormsPath 类可用于绘制曲线和复杂形状。
ms.prod: xamarin
ms.assetid: B29486F4-9A5E-4588-ABDF-7EB1E69B9AE6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: e558c510fa71f4d8f33b70b7dcb7e9a9334c6a84
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/18/2020
ms.locfileid: "84947292"
---
# <a name="xamarinforms-shapes-path"></a>Xamarin.Forms形状：路径

![](~/media/shared/preview.png "This API is currently pre-release")

[![下载示例](~/media/shared/download.png) 下载示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

`Path`类派生自 `Shape` 类，可用于绘制曲线和复杂形状。 这些曲线和形状通常使用对象进行描述 `Geometry` 。 有关 `Path` 该类继承自类的属性的信息 `Shape` ，请参阅[ Xamarin.Forms 形状](index.md)。

`Path` 定义以下属性:

- `Data`，类型为 `Geometry` ，它指定要绘制的形状。
- `RenderTransform`，类型为 `Transform` ，表示在绘制路径之前应用于该路径的几何图形的转换。

这些属性是由对象支持的 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) ，这意味着它们可以是数据绑定的目标和样式。

有关转换的详细信息，请参阅[ Xamarin.Forms 路径转换](path-transforms.md)。

## <a name="create-a-path"></a>创建路径

下面的 XAML 示例演示如何使用特殊的缩写语法绘制多边形：

```xaml
<Path Data="M 10,50 L 200,70"
      Stroke="Black"
      StrokeThickness="1"
      Aspect="Uniform"
      HorizontalOptions="Start"
      HeightRequest="100"
      WidthRequest="100" />
```

`Data`字符串以 "moveto" 命令开头，该命令由指示 `M` ，这将为路径建立起点。 路径数据参数区分大小写。 资本 `M` 表示开始点的绝对位置。 小写 `m` 会指示相对坐标。 `L`line 命令，它创建从起点到指定终点的直线。

## <a name="path-geometry"></a>路径几何图形

可以使用对象描述曲线和形状 `Geometry` ，这些对象用于设置 `Path` 对象的 `Data` 属性。

有多种 `Geometry` 可供选择的对象。 `EllipseGeometry`、 `LineGeometry` 和 `RectangleGeometry` 类描述相对简单的形状。 若要创建更复杂的形状或创建曲线，请使用 `PathGeometry` 。

`PathGeometry`对象由一个或多个 `PathFigure` 对象组成。 每个 `PathFigure` 对象都表示不同的形状。 每个 `PathFigure` 对象都由一个或多个 `PathSegment` 对象组成，每个对象表示形状的连接部分。 段类型包括以下内容： `LineSegment` 、 `BezierSegment` 和 `ArcSegment` 。

在下面的示例中， `Path` 用于绘制三角形：

```xaml
<Path Stroke="Black"
      StrokeThickness="1"
      Aspect="Uniform"
      HorizontalOptions="Start"
      HeightRequest="100"
      WidthRequest="100">
    <Path.Data>
        <PathGeometry>
            <PathGeometry.Figures>
                <PathFigureCollection>
                    <PathFigure IsClosed="True"
                                StartPoint="10,100">
                        <PathFigure.Segments>
                            <PathSegmentCollection>
                                <LineSegment Point="100,100" />
                                <LineSegment Point="100,50" />
                            </PathSegmentCollection>
                        </PathFigure.Segments>
                    </PathFigure>
                </PathFigureCollection>
            </PathGeometry.Figures>
        </PathGeometry>
    </Path.Data>
</Path>
```

有关几何图形的详细信息，请参阅[ Xamarin.Forms 几何图形](geometries.md)。

## <a name="related-links"></a>相关链接

- [ShapeDemos （示例）](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapedemos/)
- [Xamarin.Forms形状](index.md)
- [Xamarin.Forms形状几何图形](geometries.md)
- [Xamarin.Forms路径转换](path-transforms.md)
