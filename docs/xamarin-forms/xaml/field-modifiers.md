---
title: XAML 字段修饰符Xamarin.Forms
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
ms.openlocfilehash: db00f522b71a8993ef0f7f6cf5070813ce07396a
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/28/2020
ms.locfileid: "84138120"
---
# <a name="xaml-field-modifiers-in-xamarinforms"></a>XAML 字段修饰符Xamarin.Forms

`x:FieldModifier`Namespace 特性指定为命名 XAML 元素生成的字段的访问级别。 特性的有效值为：

- `private`–指定 XAML 元素的生成字段只能在声明它的类的主体内访问。
- `public`–指定 XAML 元素的生成字段没有访问限制。
- `protected`–指定 XAML 元素的生成字段可在其类和派生类实例内访问。
- `internal`–指定只能在同一程序集内的类型内访问 XAML 元素的生成字段。
- `notpublic`–指定只能在同一程序集内的类型内访问 XAML 元素的生成字段。

默认情况下，如果未设置该属性的值，则该元素的生成字段将为 `private` 。

> [!NOTE]
> 特性的值可以使用任何大小写，因为它将被转换为小写 Xamarin.Forms 。

若要处理某个属性，必须满足以下条件 `x:FieldModifier` ：

- 顶级 XAML 元素必须是有效的 `x:Class` 。
- 当前 XAML 元素具有指定的 `x:Name` 。

以下 XAML 显示了设置属性的示例：

```xaml
<Label x:Name="privateLabel" />
<Label x:Name="internalLabel" x:FieldModifier="internal" />
<Label x:Name="publicLabel" x:FieldModifier="public" />
```

> [!IMPORTANT]
> `x:FieldModifier`特性不能用于指定 XAML 类的访问级别。
