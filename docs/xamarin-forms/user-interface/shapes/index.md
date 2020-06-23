---
title: Xamarin.Forms形状
description: Xamarin.Forms形状是使您能够在屏幕上绘制形状的视图类型。
ms.prod: xamarin
ms.assetid: 4E749FE8-852C-46DA-BB1E-652936106357
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/22/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 13c79d7597325a3bf8dbabfa2983d55a92309c4b
ms.sourcegitcommit: dc49ba58510eeb52048a866e5d3daf5f1f68fbd2
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/22/2020
ms.locfileid: "85130877"
---
# <a name="xamarinforms-shapes"></a>Xamarin.Forms形状

![](~/media/shared/preview.png "This API is currently pre-release")

`Shape`是的一种类型 [`View`](xref:Xamarin.Forms.View) ，它使您能够在屏幕上绘制形状。 `Shape`对象可在布局类和大多数控件内使用，因为 `Shape` 该类派生自 `View` 类。

Xamarin.Forms可在 `Xamarin.Forms.Shapes` iOS、Android、macOS、通用 Windows 平台（UWP）和 Windows Presentation Foundation （WPF）上的命名空间中使用形状。

> [!IMPORTANT]
> Xamarin.Forms形状当前为实验性，只能通过设置标志来使用 `Shapes_Experimental` 。 有关详细信息，请参阅[实验标志](~/xamarin-forms/internals/experimental-flags.md)。

`Shape` 定义以下属性:

- `Aspect`类型为的，它 `Stretch` 描述形状如何填充其分配的空间。 此属性的默认值为 `Stretch.None`。
- `Fill`类型为的， [`Color`](xref:Xamarin.Forms.Color) 指示用于绘制形状内部的颜色。
- `Stroke`类型为的， [`Color`](xref:Xamarin.Forms.Color) 指示用于绘制形状边框的颜色。
- `StrokeDashArray`，类型为 `DoubleCollection` ，它表示值的集合，这些 `double` 值指示用于勾勒形状轮廓的虚线和间隙的模式。
- `StrokeDashOffset`类型为 `double` ，指定短划线模式内虚线开始处的距离。 此属性的默认值为0.0。
- `StrokeLineCap`，类型为 `PenLineCap` ，描述直线或线段的开头和结尾处的形状。 此属性的默认值为 `PenLineCap.Flat`。
- `StrokeLineJoin`类型为的， `PenLineJoin` 它指定在形状的顶点处使用的联接类型。 此属性的默认值为 `PenLineJoin.Miter`。
- `StrokeThickness`类型为的， `double` 指示形状轮廓的宽度。 此属性的默认值为1.0。

这些属性是由对象支持的 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) ，这意味着它们可以是数据绑定的目标和样式。

Xamarin.Forms定义派生自类的多个对象 `Shape` 。 它们包括 `Ellipse` 、、、 `Line` `Path` `Polygon` 、 `Polyline` 和 `Rectangle` 。

## <a name="paint-shapes"></a>绘制形状

[`Color`](xref:Xamarin.Forms.Color)对象用于绘制形状的 `Stroke` 和 `Fill` ：

```xaml
<Ellipse Fill="DarkBlue"
         Stroke="Red"
         StrokeThickness="4"
         WidthRequest="150"
         HeightRequest="50"
         HorizontalOptions="Start" />
```

在此示例中，指定了的笔划和填充 `Ellipse` ：

![绘制形状](images/ellipse.png "绘制形状")

> [!IMPORTANT]
> 如果没有 [`Color`](xref:Xamarin.Forms.Color) 为指定值 `Stroke` ，或者如果将设置 `StrokeThickness` 为0，则不会绘制形状周围的边框。

有关有效值的详细信息 [`Color`](xref:Xamarin.Forms.Color) ，请参阅[中 Xamarin.Forms 的颜色](~/xamarin-forms/user-interface/colors.md)。

## <a name="stretch-shapes"></a>Stretch 形状

`Shape`对象具有 `Aspect` 类型为的属性 `Stretch` 。 此属性确定如何 `Shape` 拉伸对象的内容以填充 `Shape` 对象的布局空间。 `Shape`对象的布局空间是布局系统分配的空间量 `Shape` Xamarin.Forms ，因为显式 `WidthRequest` 和设置， `HeightRequest` 或者因为其 `HorizontalOptions` 和设置而设置 `VerticalOptions` 。

`Stretch` 枚举定义下列成员：

- `None`，它指示内容保持其原始大小。 这是 `Shape.Aspect` 属性的默认值。
- `Fill`，它指示调整内容的大小以填充目标尺寸。 不保留纵横比。
- `Uniform`，它指示调整内容的大小以适合目标尺寸，同时保留纵横比。
- `UniformToFill`，指示在保持纵横比的同时调整内容的大小以填充目标尺寸。 如果目标矩形的纵横比不同于源矩形的纵横比，则对源内容进行剪裁以适合目标尺寸。

下面的 XAML 演示如何设置 `Aspect` 属性：

```xaml
<Path Aspect="Uniform"
      Stroke="Yellow"
      StrokeThickness="1"
      Fill="Red"
      BackgroundColor="LightGray"
      HorizontalOptions="Start"
      HeightRequest="100"
      WidthRequest="100">
    <Path.Data>
        <!-- Path data goes here -->
    </Path.Data>  
</Path>      
```

在此示例中， `Path` 对象绘制心形。 `Path`对象的 `WidthRequest` 和 `HeightRequest` 属性设置为100与设备无关的单位，并将其 `Aspect` 属性设置为 `Uniform` 。 因此，对象的内容会调整大小以适合目标尺寸，同时保持纵横比：

![Stretch 形状](images/aspect.png "Stretch 形状")

## <a name="related-links"></a>相关链接

- [ShapeDemos （示例）](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/ShapesDemos/)
- [颜色Xamarin.Forms](~/xamarin-forms/user-interface/colors.md)
