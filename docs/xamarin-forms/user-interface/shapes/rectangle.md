---
title: Xamarin.Forms形状：矩形
description: Xamarin.FormsRectangle 类可用于绘制矩形。
ms.prod: xamarin
ms.assetid: 2DD663D3-DAEC-495C-AB6D-8A143FC97637
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: da9649a4abb2cb65930d98576eda81739b711886
ms.sourcegitcommit: d86b7a18cf8b1ef28cd0fe1d311f1c58a65101a8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/19/2020
ms.locfileid: "85101367"
---
# <a name="xamarinforms-shapes-rectangle"></a>Xamarin.Forms形状：矩形

![](~/media/shared/preview.png "This API is currently pre-release")

[![下载示例](~/media/shared/download.png) 下载示例](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/ShapesDemos/)

`Rectangle`类派生自 `Shape` 类，可用于绘制矩形。 有关 `Rectangle` 该类继承自类的属性的信息 `Shape` ，请参阅[ Xamarin.Forms 形状](index.md)。

`Rectangle` 定义以下属性:

- `RadiusX`，类型为 `double` ，它是用于圆角化矩形角的 x 轴半径。 此属性的默认值为0.0。
- `RadiusY`，类型为 `double` ，它是用于圆角化矩形角的 y 轴半径。 此属性的默认值为0.0。

这些属性是由对象支持的 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) ，这意味着它们可以是数据绑定的目标和样式。

`Rectangle`类将 `Aspect` 从类继承的属性设置 `Shape` 为 `Stretch.Fill` 。

## <a name="create-a-rectangle"></a>创建矩形

下面的 XAML 示例演示如何绘制带有圆角的实心矩形：

```xaml
<Rectangle Fill="DarkBlue"
           Stroke="Red"
           StrokeThickness="4"
           RadiusX="12"
           RadiusY="24"           
           WidthRequest="150"
           HeightRequest="50"
           HorizontalOptions="Start" />
```

## <a name="related-links"></a>相关链接

- [ShapeDemos （示例）](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/ShapesDemos/)
- [Xamarin.Forms形状](index.md)
