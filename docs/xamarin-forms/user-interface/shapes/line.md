---
title: Xamarin.Forms形状：线条
description: Xamarin.FormsLine 类可用于绘制线条。
ms.prod: xamarin
ms.assetid: 384F1A72-6D3B-4FD3-BC40-E00A73A463EC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: c2255db884fbe51e253aaf0f01909e732df94850
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/18/2020
ms.locfileid: "84947294"
---
# <a name="xamarinforms-shapes-line"></a>Xamarin.Forms形状：线条

![](~/media/shared/preview.png "This API is currently pre-release")

[![下载示例](~/media/shared/download.png) 下载示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

`Line`类派生自 `Shape` 类，可用于绘制线条。 有关 `Line` 该类继承自类的属性的信息 `Shape` ，请参阅[ Xamarin.Forms 形状](index.md)。

`Line` 定义以下属性:

- `X1`类型为 double，指示直线起点的 x 坐标。 此属性的默认值为0.0。
- `X2`类型为 double，表示直线终点的 x 坐标。 此属性的默认值为0.0。
- `Y1`类型为 double，指示直线起点的 y 坐标。 此属性的默认值为0.0。
- `Y2`类型为 double，表示直线终点的 y 坐标。 此属性的默认值为0.0。

这些属性是由对象支持的 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) ，这意味着它们可以是数据绑定的目标和样式。

## <a name="create-a-line"></a>创建行

下面的 XAML 示例演示如何绘制线条：

```xaml
<Line X1="40"
      Y1="0"
      X2="0"
      Y2="120"
      Stroke="DarkBlue"
      StrokeThickness="4"
      HeightRequest="120"
      WidthRequest="120" />
```

> [!NOTE]
> 设置 `Fill` 的属性 `Line` 不起作用，因为直线没有内部。

## <a name="related-links"></a>相关链接

- [ShapeDemos （示例）](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapedemos/)
- [Xamarin.Forms形状](index.md)
