---
title: XAML 命名空间Xamarin.Forms
description: XAML 使用 xmlns XML 特性进行命名空间声明。 本文介绍 XAML 命名空间语法，并演示如何声明 XAML 命名空间来访问类型。
ms.prod: xamarin
ms.assetid: C03B5553-B199-4A19-9F0F-E5BCE1DB268F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/21/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 7f35342134767ccdadfab086bfa14f6b610b325d
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/18/2020
ms.locfileid: "84130372"
---
# <a name="xaml-namespaces-in-xamarinforms"></a>XAML 命名空间Xamarin.Forms

_XAML 使用 xmlns XML 特性进行命名空间声明。本文介绍 XAML 命名空间语法，并演示如何声明 XAML 命名空间来访问类型。_

## <a name="overview"></a>概述

XAML 文件的根元素中始终有两个 XAML 命名空间声明。 第一个定义默认命名空间，如以下 XAML 代码示例所示：

```xaml
xmlns="http://xamarin.com/schemas/2014/forms"
```

默认命名空间指定不带前缀的 XAML 文件中定义的元素引用 Xamarin.Forms 类，如 [`ContentPage`](xref:Xamarin.Forms.ContentPage) 。

第二个命名空间声明使用 `x` 前缀，如以下 XAML 代码示例所示：

```xaml
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
```

XAML 使用前缀来声明非默认命名空间，并在引用命名空间中的类型时使用前缀。 `x`命名空间声明指定在 xaml 中使用前缀定义的元素 `x` 用于 xaml 内部的元素和属性（特别是 2009 xaml 规范）。

下表概述了 `x` 支持的命名空间属性 Xamarin.Forms ：

|构造|说明|
|--- |--- |
|`x:Arguments`|指定非默认构造函数或工厂方法对象声明的构造函数参数。|
|`x:Class`|为 XAML 中定义的类指定命名空间和类名。 类名必须与代码隐藏文件的类名称相匹配。 请注意，此构造只能出现在 XAML 文件的根元素中。|
|`x:DataType`|指定 XAML 元素及其子对象将绑定到的对象的类型。|
|`x:FactoryMethod`|指定可用于初始化对象的工厂方法。|
|`x:FieldModifier`|为命名的 XAML 元素的生成字段指定访问级别。|
|`x:Key`|为中的每个资源指定唯一的用户定义的键 `ResourceDictionary` 。 键的值用于检索 XAML 资源，通常用作 `StaticResource` 标记扩展的参数。|
|`x:Name`|指定 XAML 元素的运行时对象名称。 设置 `x:Name` 与在代码中声明变量类似。|
|`x:TypeArguments`|指定泛型类型的构造函数的泛型类型参数。|

有关特性的详细信息 `x:DataType` ，请参阅[已编译的绑定](~/xamarin-forms/app-fundamentals/data-binding/compiled-bindings.md)。 有关属性的详细信息 `x:FieldModifier` ，请参阅[字段修饰符](~/xamarin-forms/xaml/field-modifiers.md)。 有关和特性的详细 `x:Arguments` 信息 `x:FactoryMethod` ，请参阅[在 XAML 中传递参数](~/xamarin-forms/xaml/passing-arguments.md)。 有关特性的详细信息 `x:TypeArguments` ，请参阅[XAML 中的泛型 Xamarin.Forms 和](generics.md)。

> [!NOTE]
> 除了上面列出的命名空间特性之外， Xamarin.Forms 还包括可通过命名空间前缀使用的标记扩展 `x` 。 有关详细信息，请参阅[使用 XAML 标记扩展](~/xamarin-forms/xaml/markup-extensions/consuming.md)。

在 XAML 中，命名空间声明从父元素继承到子元素。 因此，在 XAML 文件的根元素中定义命名空间时，该文件中的所有元素都将继承该命名空间声明。

## <a name="declaring-namespaces-for-types"></a>为类型声明命名空间

可以在 XAML 中引用类型，方法是声明具有前缀的 XAML 命名空间，命名空间声明指定公共语言运行时（CLR）命名空间名称，并选择性地包含程序集名称。 这是通过在命名空间声明中定义以下关键字的值来实现的：

- **clr-命名空间：** 或**使用：** –在包含要作为 XAML 元素公开的类型的程序集中声明的 clr 命名空间。 此关键字是必需的。
- **assembly =** –包含所引用的 CLR 命名空间的程序集。 此值是程序集的名称，没有文件扩展名。 程序集的路径应在包含将引用程序集的 XAML 文件的项目文件中建立为引用。 如果**clr 命名空间**的值与引用类型的应用程序代码在同一程序集中，则可以省略此关键字。

请注意，将 `clr-namespace` 或 `using` 标记与值分隔的字符是冒号，而 `assembly` 从其值中分隔标记的字符是等号。 要在两个标记之间使用的字符是一个分号。

下面的代码示例演示 XAML 命名空间声明：

```xaml
<ContentPage ... xmlns:local="clr-namespace:HelloWorld" ...>
  ...
</ContentPage>
```

也可以将其编写为：

```xaml
<ContentPage ... xmlns:local="using:HelloWorld" ...>
  ...
</ContentPage>
```

`local`前缀是一种约定，用于指示命名空间中的类型是应用程序的本地类型。 或者，如果类型在不同的程序集中，则还应在命名空间声明中定义程序集名称，如下面的 XAML 代码示例所示：

```xaml
<ContentPage ... xmlns:behaviors="clr-namespace:Behaviors;assembly=BehaviorsLibrary" ...>
  ...
</ContentPage>
```

然后，在从导入的命名空间声明类型的实例时指定命名空间前缀，如以下 XAML 代码示例所示：

```xaml
<ListView ...>
  <ListView.Behaviors>
    <behaviors:EventToCommandBehavior EventName="ItemSelected" ... />
  </ListView.Behaviors>
</ListView>
```

有关定义自定义命名空间架构的信息，请参阅[XAML 自定义命名空间架构](custom-namespace-schemas.md)。

## <a name="summary"></a>摘要

本文介绍了 XAML 命名空间语法，并演示了如何声明 XAML 命名空间来访问类型。 XAML 将 `xmlns` XML 特性用于命名空间声明，在 xaml 中，可以通过使用前缀声明 xaml 命名空间来引用类型。

## <a name="related-links"></a>相关链接

- [在 XAML 中传递参数](~/xamarin-forms/xaml/passing-arguments.md)
- [XAML 中的泛型Xamarin.Forms](generics.md)
