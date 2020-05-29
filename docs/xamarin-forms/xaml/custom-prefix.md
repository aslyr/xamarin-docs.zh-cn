---
title: XAML 命名空间中建议的前缀Xamarin.Forms
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 71ae523f40f3f7529c12f853778404e224fbae30
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/28/2020
ms.locfileid: "84138133"
---
# <a name="xaml-namespace-recommended-prefixes-in-xamarinforms"></a>XAML 命名空间中建议的前缀Xamarin.Forms

`XmlnsPrefixAttribute`控件作者可以使用类来指定建议用于与 xaml 命名空间关联的前缀，以供 xaml 使用。 当支持对象树序列化到 XAML 时，或与具有 XAML 编辑功能的设计环境进行交互时，前缀非常有用。 例如：

- XAML 文本编辑器可以将 `XmlnsPrefixAttribute` 用作初始 XAML 命名空间映射的提示 `xmlns` 。
- 将 `XmlnsPrefixAttribute` 对象拖出工具箱和可视化设计图面时，xaml 设计环境可以使用将映射添加到 xaml。

建议使用构造函数在程序集级别定义命名空间前缀 `XmlnsPrefixAttribute` ，该构造函数采用两个参数：一个指定 XAML 命名空间的标识符的字符串，以及一个指定建议前缀的字符串：

```csharp
[assembly: XmlnsPrefix("http://xamarin.com/schemas/2014/forms", "xf")]
```

前缀应使用短字符串，因为前缀通常应用于来自 XAML 命名空间的所有序列化元素。 因此，前缀字符串长度可能会对序列化的 XAML 输出的大小产生明显影响。

> [!NOTE]
> `XmlnsPrefixAttribute`可以将多个应用程序应用于一个程序集。 例如，如果你有一个定义多个 XAML 命名空间的类型的程序集，则可以为每个 XAML 命名空间定义不同的前缀值。

## <a name="related-links"></a>相关链接

- [XAML 自定义命名空间架构](custom-namespace-schemas.md)
- [XAML 命名空间Xamarin.Forms](namespaces.md)
