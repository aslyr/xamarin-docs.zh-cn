---
title: ''
description: 本文介绍如何 Xamarin.Forms 通过允许从文本字符串以外的源设置元素特性来使用 xaml 标记扩展来扩展 XAML 的强大功能和灵活性。
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 568cffc335f28b1a47f3278ad061d851ebef84b6
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/28/2020
ms.locfileid: "84130385"
---
# <a name="xaml-markup-extensions"></a>XAML 标记扩展

[![下载示例](~/media/shared/download.png) 下载示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-markupextensions)

XAML 标记扩展允许从文本字符串以外的源设置元素特性，从而帮助扩展 XAML 的强大功能和灵活性。

例如，通常会将 `Color` 的属性设置为，如下所示 `BoxView` ：

```xaml
<BoxView Color="Blue" />
```

或者，您可以将其设置为十六进制的 RGB 颜色值：

```xaml
<BoxView Color="#FF0080" />
```

在这两种情况下，都将设置为属性的文本字符串 `Color` 转换为 `Color` 类的值 [`ColorTypeConverter`](xref:Xamarin.Forms.ColorTypeConverter) 。

您可能更愿意改为 `Color` 从存储在资源字典中的值，或从您已创建的类的静态属性的值或从页上的另一个元素的类型的属性或通过 `Color` 单独的色调、饱和度和发光度值构造的值设置属性。

所有这些选项都可以使用 XAML 标记扩展。 但不要让短语 "标记扩展" 心有余悸： XAML 标记扩展*不*是 XML 扩展。 即使对于 XAML 标记扩展，XAML 也始终是合法的 XML。

标记扩展实际上只是表达元素特性的另一种方法。 XAML 标记扩展通常由括在大括号中的属性设置标识：

```xaml
<BoxView Color="{StaticResource themeColor}" />
```

大括号中的任何属性设置*始终*为 XAML 标记扩展。 不过，正如您将看到的，也可以在不使用大括号的情况下引用 XAML 标记扩展。

本文分为两部分：

## <a name="consuming-xaml-markup-extensions"></a>[使用 XAML 标记扩展](consuming.md)  

使用在中定义的 XAML 标记扩展 Xamarin.Forms 。

## <a name="creating-xaml-markup-extensions"></a>[创建 XAML 标记扩展](creating.md)

编写您自己的自定义 XAML 标记扩展。

## <a name="related-links"></a>相关链接

- [标记扩展（示例）](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-markupextensions)
- [书籍中的 XAML 标记扩展章节 Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md)
- [资源字典](~/xamarin-forms/xaml/resource-dictionaries.md)
- [动态样式](~/xamarin-forms/user-interface/styles/dynamic.md)
- [数据绑定](~/xamarin-forms/app-fundamentals/data-binding/index.md)
