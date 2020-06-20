---
title: Xamarin.Forms形状：椭圆形
description: Xamarin.Forms可以使用椭圆形类来绘制椭圆和圆圈。
ms.prod: xamarin
ms.assetid: 5BF81E25-12E5-49F0-A40C-0CF4C5D63B9B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 142deccbcd29548e2d72b7144a01322f9909d98f
ms.sourcegitcommit: d86b7a18cf8b1ef28cd0fe1d311f1c58a65101a8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/19/2020
ms.locfileid: "85101278"
---
# <a name="xamarinforms-shapes-ellipse"></a>Xamarin.Forms形状：椭圆形

![](~/media/shared/preview.png "This API is currently pre-release")

[![下载示例](~/media/shared/download.png) 下载示例](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/ShapesDemos/)

`Ellipse`类派生自 `Shape` 类，可用于绘制椭圆和圆。 有关 `Ellipse` 该类继承自类的属性的信息 `Shape` ，请参阅[ Xamarin.Forms 形状](index.md)。

`Ellipse`类将 `Aspect` 从类继承的属性设置 `Shape` 为 `Stretch.Fill` 。

## <a name="create-an-ellipse"></a>创建一个椭圆形

下面的 XAML 示例演示如何绘制实心椭圆：

```xaml
<Ellipse Fill="Red"
         WidthRequest="150"
         HeightRequest="50"
         HorizontalOptions="Start" />
```

下面的 XAML 示例演示如何绘制圆形：

```xaml
<Ellipse Stroke="Red"
         StrokeThickness="4"
         WidthRequest="150"
         HeightRequest="150"
         HorizontalOptions="Start" />
```

## <a name="related-links"></a>相关链接

- [ShapeDemos （示例）](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/ShapesDemos/)
- [Xamarin.Forms形状](index.md)
