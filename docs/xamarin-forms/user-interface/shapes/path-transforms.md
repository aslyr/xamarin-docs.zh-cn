---
title: Xamarin.Forms形状：路径转换
description: Xamarin.Forms转换定义如何将路径对象从一个坐标空间转换到另一个坐标空间。
ms.prod: xamarin
ms.assetid: 07DE3D66-1820-4642-BDDF-84146D40C99D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 554a9dd0ca8be54c35d1891b60149bbbb66c3e7c
ms.sourcegitcommit: 91b4d2f93687fadec5c3f80aadc8f7298d911624
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85795004"
---
# <a name="xamarinforms-shapes-path-transforms"></a>Xamarin.Forms形状：路径转换

![](~/media/shared/preview.png "This API is currently pre-release")

[![下载示例](~/media/shared/download.png)下载示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

`Transform`定义如何将 `Path` 对象从一个坐标空间转换到另一个坐标空间。 这种映射通过转换进行描述 `Matrix` ，后者是包含三列值的三行的集合 `double` 。

在 2D x-y 平面中，使用3x3 矩阵进行变换。 仿射转换矩阵可以相乘，以形成任意数量的线性转换，如旋转和斜切，然后是转换。 下表显示了矩阵的结构 Xamarin.Forms ：

| | | |
|---------|---------|-----|
| M11     | M12     | 0.0 |
| M21     | M22     | 0.0 |
| OffsetX | OffsetY | 1.0 |

通过操作矩阵值，你可以旋转、缩放、倾斜和转换 `Path` 对象。 例如，如果将 `OffsetX` 值更改为100，则可以使用它将 `Path` 100 对象与设备无关的单位沿 x 轴移动。 如果将此 `M22` 值更改为3，则可以使用它将对象拉伸 `Path` 到其当前高度的三倍。 如果更改这两个值，则将 `Path` 沿 x 轴移动与设备无关的对象100，并将其高度拉伸3倍。

> [!NOTE]
> 仿射转换矩阵的最终列等于（0，0，1），因此只需指定前两列中的成员。 最后一行中的成员 `OffsetX` `OffsetY` 表示转换值。

虽然您可以直接使用 `Matrix` 结构来转换各个点，但 Xamarin.Forms 也提供了以下类，使您能够转换 `Path` 对象，而无需直接使用矩阵：

- `RotateTransform`，它 `Path` 通过指定的旋转 `Angle` 。
- `ScaleTransform`，它 `Path` 按指定 `ScaleX` 和量缩放对象 `ScaleY` 。
- `SkewTransform`，它 `Path` 按指定 `AngleX` 和量倾斜对象 `AngleY` 。
- `TranslateTransform`，它 `Path` 按指定 `X` 和数量移动对象 `Y` 。

Xamarin.Forms还提供了以下类来创建更复杂的转换：

- `TransformGroup`，它表示 `Transform` 由其他对象组成的复合 `Transform` 。
- `CompositeTransform`，它表示 `Transform` 由其他对象组成的复合 `Transform` 。
- `MatrixTransform`，它创建其他类未提供的自定义转换 `Transform` 。

所有这些类均派生自 `Transform` 类，该类定义 `Value` 类型的属性 `Matrix` 。 此属性将当前转换表示为 `Matrix` 对象。

若要将转换应用到 `Path` ，可以创建转换类并将其设置为属性的值 `Path.RenderTransform` 。

## <a name="rotation-transform"></a>旋转转换

旋转变换围绕 `Path` 二维 x-y 坐标系统中的指定点顺时针旋转对象。

`RotateTransform`派生自类的类 `Transform` 定义以下属性：

- `Angle`类型为的， `double` 表示顺时针旋转的角度（以度为单位）。 此属性的默认值为0.0。
- `CenterX`类型为的， `double` 表示旋转中心点的 x 坐标。 此属性的默认值为0.0。
- `CenterY`类型为的， `double` 表示旋转中心点的 y 坐标。 此属性的默认值为0.0。

这些属性是由对象支持的 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) ，这意味着它们可以是数据绑定的目标和样式。

`CenterX`和 `CenterY` 属性指定对象要旋转的点 `Path` 。 此中心点在转换对象的坐标空间中表示。 默认情况下，旋转应用于（0，0），这是对象的左上角 `Path` 。

下面的示例演示如何旋转 `Path` 对象：

```xaml
<Path Stroke="Black"
      Aspect="Uniform"
      HorizontalOptions="Center"
      HeightRequest="100"
      WidthRequest="100"
      Data="M13.908992,16.207977L32.000049,16.207977 32.000049,31.999985 13.908992,30.109983z">
    <Path.RenderTransform>
        <RotateTransform CenterX="0"
                         CenterY="0"
                         Angle="45" />
    </Path.RenderTransform>
</Path>
```

在此示例中， `Path` 对象围绕其左上角旋转了45度。

## <a name="scale-transform"></a>缩放转换

缩放转换可缩放 `Path` 2d x-y 坐标系统中的对象。

`ScaleTransform`派生自类的类 `Transform` 定义以下属性：

- `ScaleX`，类型为 `double` ，它表示 x 轴缩放比例。 此属性的默认值为1.0。
- `ScaleY`，类型为 `double` ，它表示 y 轴的缩放比例。 此属性的默认值为1.0。
- `CenterX`，类型为 `double` ，表示此变换中心点的 x 坐标。 此属性的默认值为0.0。
- `CenterY`，类型为 `double` ，表示此转换的中心点的 y 坐标。 此属性的默认值为0.0。

这些属性是由对象支持的 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) ，这意味着它们可以是数据绑定的目标和样式。

和的值 `ScaleX` 对 `ScaleY` 生成的缩放产生大的影响：

- 介于0和1之间的值将减小缩放对象的宽度和高度。
- 大于1的值将增加缩放对象的宽度和高度。
- 值1表示不缩放对象。
- 负值用于水平和垂直翻转刻度对象。
- 介于0和-1 之间的值将翻转刻度对象并减小其宽度和高度。
- 小于-1 的值将翻转对象并增加其宽度和高度。
- 值-1 翻转缩放对象，但不更改其水平或垂直大小。

`CenterX`和 `CenterY` 属性指定了对象的 `Path` 缩放点。 此中心点在转换对象的坐标空间中表示。 默认情况下，缩放将应用于（0，0），这是对象的左上角 `Path` 。 这 `Path` 会使对象移动并使其显得更大，因为当应用转换时，将更改对象所在的坐标空间 `Path` 。

下面的示例演示如何缩放 `Path` 对象：

```xaml
<Path Stroke="Black"
      Aspect="Uniform"
      HorizontalOptions="Center"
      HeightRequest="100"
      WidthRequest="100"
      Data="M13.908992,16.207977L32.000049,16.207977 32.000049,31.999985 13.908992,30.109983z">
    <Path.RenderTransform>
        <ScaleTransform CenterX="0"
                        CenterY="0"
                        ScaleX="2"
                        ScaleY="2" />
    </Path.RenderTransform>
</Path>
```

在此示例中，将 `Path` 对象缩放到大小的两倍。

## <a name="skew-transform"></a>倾斜变换

倾斜变换 `Path` 会在 2d x-y 坐标系统中倾斜对象，这对于在2d 对象中创建三维深度的错觉非常有用。

`SkewTransform`派生自类的类 `Transform` 定义以下属性：

- `AngleX`，类型为 `double` ，它表示 x 轴倾斜角度，该角度从 y 轴开始沿逆时针方向测量，以度为单位。 此属性的默认值为0.0。
- `AngleY`，类型为 `double` ，它表示 y 轴倾斜角度，该角度从 x 轴开始沿逆时针方向测量，以度为单位。 此属性的默认值为0.0。
- `CenterX`，类型为 `double` ，表示变换中心的 x 坐标。 此属性的默认值为0.0。
- `CenterY`，类型为 `double` ，表示变换中心的 y 坐标。 此属性的默认值为0.0。

这些属性是由对象支持的 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) ，这意味着它们可以是数据绑定的目标和样式。

若要预测斜切转换的效果，请考虑将 `AngleX` x 轴值与原始坐标系统相对倾斜。 因此，对于 `AngleX` 30，y 轴通过原点旋转30度，并将 x 的值从该原点倾斜30度。 同样，值为30时，将 `AngleY` 对象的 y 值 `Path` 从原点倾斜30度。

若要 `Path` 就地倾斜对象，请将 `CenterX` 和属性设置 `CenterY` 为对象的中心点。

下面的示例演示如何倾斜 `Path` 对象：

```xaml
<Path Stroke="Black"
      Aspect="Uniform"
      HorizontalOptions="Center"
      HeightRequest="100"
      WidthRequest="100"
      Data="M13.908992,16.207977L32.000049,16.207977 32.000049,31.999985 13.908992,30.109983z">
    <Path.RenderTransform>
        <SkewTransform CenterX="0"
                       CenterY="0"
                       AngleX="45"
                       AngleY="0" />
    </Path.RenderTransform>
</Path>
```

在此示例中， `Path` 从（0，0）的中心点向对象应用45度的水平倾斜。

## <a name="translate-transform"></a>转换转换

转换转换会移动 2D x-y 坐标系统中的对象。

`TranslateTransform`派生自类的类 `Transform` 定义以下属性：

- `X`，类型为 `double` ，表示沿 x 轴移动的距离。 此属性的默认值为0.0。
- `Y`，类型为 `double` ，表示沿 y 轴移动的距离。 此属性的默认值为0.0。

这些属性是由对象支持的 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) ，这意味着它们可以是数据绑定的目标和样式。

负值 `X` 将对象向左移动，正值将对象向右移动。 负值 `Y` 将对象上移，正值将对象向下移动。

下面的示例演示如何转换 `Path` 对象：

```xaml
<Path Stroke="Black"
      Aspect="Uniform"
      HorizontalOptions="Center"
      HeightRequest="100"
      WidthRequest="100"
      Data="M13.908992,16.207977L32.000049,16.207977 32.000049,31.999985 13.908992,30.109983z">
    <Path.RenderTransform>
        <TranslateTransform X="50"
                            Y="50" />
    </Path.RenderTransform>
</Path>
```

在此示例中， `Path` 对象被移动到右侧50个与设备无关的单位，并向下移动50个与设备无关的单位。

## <a name="apply-multiple-transforms"></a>应用多个转换

Xamarin.Forms具有两个支持将多个转换应用于 `Path` 对象的类。 这些是 `TransformGroup` 、和 `CompositeTransform` 。 `TransformGroup`按任何所需顺序执行转换，同时 `CompositeTransform` 按特定顺序执行转换。

### <a name="transform-groups"></a>转换组

转换组表示由多个对象组成的复合转换 `Transform` 。

`TransformGroup`派生自类的类 `Transform` 定义以下属性：

- `Children`，类型为 `TransformCollection` ，表示对象的集合 `Transform` 。

这些属性是由对象支持的 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) ，这意味着它们可以是数据绑定的目标和样式。

在使用类的复合转换中，转换顺序很重要 `TransformGroup` 。 例如，如果您首先旋转，然后缩放，然后再翻译，则会得到不同于第一次平移、旋转和缩放的结果。 其中一个原因是重要的是，旋转和缩放等转换是相对于坐标系统的原点执行的。 缩放以原点为中心的对象将产生不同的结果，以便缩放已移出原点的对象。 同样，旋转位于原点中心的对象会产生不同的结果，而不是旋转远离原点的对象。

下面的示例演示如何使用类执行复合转换 `TransformGroup` ：

```xaml
<Path Stroke="Black"
      Aspect="Uniform"
      HorizontalOptions="Center"
      HeightRequest="100"
      WidthRequest="100"
      Data="M13.908992,16.207977L32.000049,16.207977 32.000049,31.999985 13.908992,30.109983z">
    <Path.RenderTransform>
        <TransformGroup>
            <ScaleTransform ScaleY="2" />
            <RotateTransform Angle="45" />
        </TransformGroup>
    </Path.RenderTransform>
</Path>
```

在此示例中， `Path` 对象缩放为其大小的两倍，旋转45度。

## <a name="composite-transforms"></a>复合转换

复合转换向对象应用多个转换。

`CompositeTransform`派生自类的类 `Transform` 定义以下属性：

- `CenterX`，类型为 `double` ，表示此变换中心点的 x 坐标。 此属性的默认值为0.0。
- `CenterY`，类型为 `double` ，表示此转换的中心点的 y 坐标。 此属性的默认值为0.0。
- `ScaleX`，类型为 `double` ，它表示 x 轴缩放比例。 此属性的默认值为1.0。
- `ScaleY`，类型为 `double` ，它表示 y 轴的缩放比例。 此属性的默认值为1.0。
- `SkewX`，类型为 `double` ，它表示 x 轴倾斜角度，该角度从 y 轴开始沿逆时针方向测量，以度为单位。 此属性的默认值为0.0。
- `SkewY`，类型为 `double` ，它表示 y 轴倾斜角度，该角度从 x 轴开始沿逆时针方向测量，以度为单位。 此属性的默认值为0.0。
- `Rotation`类型为的， `double` 表示顺时针旋转的角度（以度为单位）。 此属性的默认值为0.0。
- `TranslateX`，类型为 `double` ，表示沿 x 轴移动的距离。 此属性的默认值为0.0。
- `TranslateY`，类型为 `double` ，表示沿 y 轴移动的距离。 此属性的默认值为0.0。

这些属性是由对象支持的 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) ，这意味着它们可以是数据绑定的目标和样式。

`CompositeTransform`按以下顺序应用转换：

1. Scale （ `ScaleX` 和 `ScaleY` ）。
1. 倾斜（ `SkewX` 和 `SkewY` ）。
1. 旋转（ `Rotation` ）。
1. 转换（ `TranslateX` 、 `TranslateY` ）。

如果要按不同的顺序将多个转换应用于对象，则应创建 `TransformGroup` 并按照预期顺序插入转换。

> [!IMPORTANT]
> `CompositeTransform`对于所有转换，均使用相同的中心点 `CenterX` 和 `CenterY` 。 如果要为每个转换指定不同的中心点，请使用 `TransformGroup` ，

下面的示例演示如何使用类执行复合转换 `CompositeTransform` ：

```xaml
<Path Stroke="Black"
      Aspect="Uniform"
      HorizontalOptions="Center"
      HeightRequest="100"
      WidthRequest="100"
      Data="M13.908992,16.207977L32.000049,16.207977 32.000049,31.999985 13.908992,30.109983z">
    <Path.RenderTransform>
        <CompositeTransform ScaleX="2"
                            ScaleY="2"
                            Rotation="45"
                            TranslateX="50"
                            TranslateY="50" />
    </Path.RenderTransform>
</Path>
```

在此示例中， `Path` 对象缩放为其大小的两倍，旋转45度，并由50个与设备无关的单位转换。

## <a name="custom-transforms"></a>自定义转换

矩阵转换使用仿射矩阵操作2D 平面中的对象或坐标系。 一个3x3 矩阵用于转换。 您可以将仿射矩阵转换相乘，以形成线性转换，如转换后的旋转和倾斜。

`MatrixTransform`派生自类的类 `Transform` 定义以下属性：

- `Matrix`类型为的， `Matrix` 表示定义转换的矩阵。

此属性是由对象支持的 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) ，这意味着它可以是数据绑定的目标和样式。

`MatrixTransform`类用于创建由 `RotateTransform` 、、 `ScaleTransform` `SkewTransform` 或 `TranslateTransform` 类不提供的自定义转换。

下面的示例演示如何 `Path` 使用转换对象 `MatrixTransform` ：

```xaml
<Path Stroke="Black"
      Aspect="Uniform"
      HorizontalOptions="Center"
      HeightRequest="100"
      WidthRequest="100"
      Data="M13.908992,16.207977L32.000049,16.207977 32.000049,31.999985 13.908992,30.109983z">
    <Path.RenderTransform>
        <MatrixTransform>
            <MatrixTransform.Matrix>
                <!-- M11 stretches, M12 skews -->
                <Matrix OffsetX="10"
                        OffsetY="100"
                        M11="3"
                        M12="2" />
            </MatrixTransform.Matrix>
        </MatrixTransform>
    </Path.RenderTransform>
</Path>
```

在此示例中， `Path` 对象在 X 和 Y 维中拉伸、倾斜和偏移。

此外，还可以使用内置的类型转换器以简化的形式编写 Xamarin.Forms ：

```xaml
<Path Stroke="Black"
      Aspect="Uniform"
      HorizontalOptions="Center"
      HeightRequest="100"
      WidthRequest="100"
      Data="M13.908992,16.207977L32.000049,16.207977 32.000049,31.999985 13.908992,30.109983z">
    <Path.RenderTransform>
        <MatrixTransform Matrix="3,2,0,1,10,100" />
    </Path.RenderTransform>
</Path>
```

在此示例中，将 `Matrix` 属性指定为由六个成员组成的以逗号分隔的字符串： `M11` 、 `M12` 、 `M21` 、、 `M22` `OffsetX` 和 `OffsetY` 。

## <a name="related-links"></a>相关链接

- [ShapeDemos （示例）](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Xamarin.Forms形状](index.md)
