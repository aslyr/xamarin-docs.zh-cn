---
title: ''
description: 本文探讨了用于在应用程序中显示 SkiaSharp 图形的转换 Xamarin.Forms ，并通过示例代码对此进行了演示。
ms.prod: ''
ms.technology: ''
ms.assetid: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: e20ea5d1d3f813b04a927601fbe1180ff39ed176
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/28/2020
ms.locfileid: "84140187"
---
# <a name="skiasharp-transforms"></a>SkiaSharp 转换

[![下载示例](~/media/shared/download.png) 下载示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_了解用于显示 SkiaSharp 图形的转换_

SkiaSharp 支持作为对象方法实现的传统图形转换 [`SKCanvas`](xref:SkiaSharp.SKCanvas) 。 从数学上来说，转换会改变 `SKCanvas` 图形对象呈现时在绘图函数中指定的坐标和大小。 转换通常非常适合用于绘制重复的图形或动画。 在 &mdash; &mdash; 不使用转换的情况下，不能使用某些技术（如旋转位图或文本）。

SkiaSharp 转换支持以下操作：

- *转换*为从一个位置到另一个位置的移位坐标
- *缩放*以增大或减小坐标和大小
- *旋转*以围绕某个点旋转坐标
- *倾斜*以水平或垂直变换坐标，使矩形成为平行四边形

这些转换称为*仿射*转换。 仿射转换始终会保留平行线，而永远不会使坐标或大小变为无限大。 正方形永远不会转换为平行四边形以外的任何内容，一个圆圈永远不会转换为椭圆以外的任何内容。

SkiaSharp 还支持基于标准的 3 x 3 变换矩阵的非仿射转换（也称为*projective*或*透视*转换）。 非仿射转换允许将正方形转换为任意凸四边形，这是一个四面，其所有内部角度小于180度。 非仿射转换可能会导致坐标或大小变为无限大，但对于三维效果至关重要。

## <a name="differences-between-skiasharp-and-xamarinforms-transforms"></a>SkiaSharp 与转换之间的差异 Xamarin.Forms

Xamarin.Forms还支持类似于 SkiaSharp 中的转换。 Xamarin.Forms [`VisualElement`](xref:Xamarin.Forms.VisualElement) 类定义下列转换属性：

- [`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX)与[`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY)
- [`Scale`](xref:Xamarin.Forms.VisualElement.Scale)
- [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation)、 [`RotationX`](xref:Xamarin.Forms.VisualElement.RotationX) 和[`RotationY`](xref:Xamarin.Forms.VisualElement.RotationY)

`RotationX`和 `RotationY` 属性是创建准三维效果的透视转换。

SkiaSharp 转换和转换之间有几个重要的区别 Xamarin.Forms ：

第一个区别是，在 `SKCanvas` 将 Xamarin.Forms 转换应用于单个派生对象时，SkiaSharp 转换将应用到整个对象 `VisualElement` 。 （可以将转换应用于 Xamarin.Forms `SKCanvasView` 对象本身，因为 `SKCanvasView` 派生自 `VisualElement` ，但在此之后 `SKCanvasView` ，SkiaSkarp 转换适用。）

SkiaSharp 变换相对于的左上角， `SKCanvas` 而 Xamarin.Forms 变换相对 `VisualElement` 于其所应用到的的左上角。 当应用缩放和旋转转换时，这一区别非常重要，因为这些转换始终是相对于特定点的。

相当大的差别在于，SKiaSharp 转换是*方法*，而 Xamarin.Forms 转换是*属性*。 这是与语法区别不同的语义差异： SkiaSharp 转换在 Xamarin.Forms 转换设置状态时执行操作。 SkiaSharp 转换适用于随后绘制的图形对象，但不适用于在应用转换之前绘制的图形对象。 与此相反， Xamarin.Forms 转换会在属性设置后立即应用于以前呈现的元素。 当调用方法时，SkiaSharp 转换是累积的;Xamarin.Forms当属性设置为另一个值时，将替换转换。

此部分中的所有示例程序都显示在[**SkiaSharpFormsDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)程序的 " **SkiaSharp 转换**" 部分中。 可在解决方案的[**转换**](https://github.com/xamarin/xamarin-forms-samples/tree/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms)文件夹中找到源代码。

## <a name="the-translate-transform"></a>[翻译转换](translate.md)

了解如何使用转换转换来转换 SkiaSharp 图形。

## <a name="the-scale-transform"></a>[缩放转换](scale.md)

发现用于将对象缩放到各种大小的 SkiaSharp 缩放变换。

## <a name="the-rotate-transform"></a>[旋转转换](rotate.md)

浏览 SkiaSharp 旋转变换可能产生的效果和动画。

## <a name="the-skew-transform"></a>[倾斜转换](skew.md)

了解倾斜变换如何创建倾斜的图形对象。

## <a name="matrix-transforms"></a>[矩阵转换](matrix.md)

利用多样的转换矩阵深入了解 SkiaSharp 转换。

## <a name="touch-manipulations"></a>[触摸操作](touch.md)

使用矩阵变换实现触控操作，以便进行拖动、缩放和旋转。

## <a name="non-affine-transforms"></a>[非仿射转换](non-affine.md)

超越非仿射转换效果的 oridinary。

## <a name="3d-rotation"></a>[3D 旋转](3d-rotation.md)

使用非仿射转换在三维空间中旋转2D 对象。

## <a name="related-links"></a>相关链接

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos （示例）](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
