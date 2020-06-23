---
title: Xamarin.Forms形状：线条
description: Xamarin.FormsLine 类可用于绘制线条。
ms.prod: xamarin
ms.assetid: 384F1A72-6D3B-4FD3-BC40-E00A73A463EC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/20/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: f97e6930f543f47383e2b8da09b36573bd3192d0
ms.sourcegitcommit: ef3d4a70e70927c4f231b763842c5355f1571d15
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2020
ms.locfileid: "85243745"
---
# <a name="xamarinforms-shapes-line"></a>Xamarin.Forms形状：线条

![](~/media/shared/preview.png "This API is currently pre-release")

[![下载示例](~/media/shared/download.png) 下载示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

`Line`类派生自 `Shape` 类，可用于绘制线条。 有关 `Line` 该类继承自类的属性的信息 `Shape` ，请参阅[ Xamarin.Forms 形状](index.md)。

`Line` 定义以下属性:

- `X1`类型为 double，指示直线起点的 x 坐标。 此属性的默认值为0.0。
- `Y1`类型为 double，指示直线起点的 y 坐标。 此属性的默认值为0.0。
- `X2`类型为 double，表示直线终点的 x 坐标。 此属性的默认值为0.0。
- `Y2`类型为 double，表示直线终点的 y 坐标。 此属性的默认值为0.0。

这些属性是由对象支持的 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) ，这意味着它们可以是数据绑定的目标和样式。

有关控制行结束绘制方式的信息，请参阅[控制线端](index.md#control-line-ends)。

## <a name="create-a-line"></a>创建行

若要绘制线条，请创建一个 `Line` 对象，并将其 `X1` 和属性设置 `Y1` 为其起点，并将其和 `X2` 属性设置 `Y` 为其端点。 此外，将其 `Stroke` 属性设置为， [`Color`](xref:Xamarin.Forms.Color) 因为没有笔触的线条是不可见的。

> [!NOTE]
> 设置 `Fill` 的属性 `Line` 不起作用，因为直线没有内部。

下面的 XAML 示例演示如何绘制线条：

```xaml
<Line X1="40"
      Y1="0"
      X2="0"
      Y2="120"
      Stroke="Red" />
```

在此示例中，将从（40，0）到（0120）绘制红色对角线：

![线](line-images/line.png "线")

由于 `X1` 、 `Y1` 、 `X2` 和 `Y2` 属性的默认值为0，因此可以使用最小语法绘制一些行：

```xaml
<Line Stroke="Red"
      X2="200" />
```

在此示例中，定义了200个与设备无关的单位的水平线。 由于其他属性默认为0，因此从（0，0）到（200，0）绘制线条。

下面的 XAML 示例演示如何绘制虚线：

```xaml
<Line X1="40"
      Y1="0"
      X2="0"
      Y2="120"
      Stroke="DarkBlue"
      StrokeDashArray="1,1"
      StrokeDashOffset="6" />
```

在此示例中，将从（40，0）到（0120）绘制深蓝色虚线对角线：

![虚线](line-images/dashed-line.png "虚线")

有关绘制虚线的详细信息，请参阅[绘制虚线形状](index.md#draw-dashed-shapes)。

## <a name="related-links"></a>相关链接

- [ShapeDemos （示例）](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Xamarin.Forms形状](index.md)
