---
title: Xamarin 中的泛型形式 XAML
description: Xamarin。窗体 XAML 通过将泛型约束指定为类型参数，为使用泛型 CLR 类型提供支持。
ms.prod: xamarin
ms.assetid: 97B73048-4F90-41AD-AB48-8EB804C4998B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/28/2020
ms.openlocfilehash: 9cda08a3bab0e25db2315c9795721e25d47d2429
ms.sourcegitcommit: 154a3e7aec775327565bb54eda1a610976af1d6f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/01/2020
ms.locfileid: "82624704"
---
# <a name="generics-in-xamarinforms-xaml"></a>Xamarin 中的泛型形式 XAML

[![下载示例](~/media/shared/download.png) 下载示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-generics/)

Xamarin。窗体 XAML 通过将泛型约束指定为类型参数，为使用泛型 CLR 类型提供支持。 此支持由`x:TypeArguments`指令提供，该指令将泛型的约束类型参数传递到泛型类型的构造函数。

> [!IMPORTANT]
> 不支持在 Xamarin. Forms XAML 中定义泛型类`x:TypeArguments`和指令。

类型参数指定为字符串，通常带有前缀，例如`sys:String`和。 `sys:Int32` 前缀是必需的，因为 CLR 泛型约束的典型类型来自未映射到默认 Xamarin 命名空间的库。 但是，XAML 2009 内置类型（例如`x:String`和`x:Int32`）也可以指定为类型参数，其中`x`是 xaml 2009 的 xaml 语言命名空间。 有关 XAML 2009 内置类型的详细信息，请参阅[xaml 2009 语言基元](/dotnet/desktop-wpf/xaml-services/types-for-primitives#xaml-2009-language-primitives)。

可以使用逗号分隔符指定多个类型参数。 此外，如果泛型约束使用泛型类型，则嵌套约束类型参数应包含在括号中。

> [!NOTE]
> `x:Type`标记扩展提供泛型类型的 CLR 类型引用，并在 c # 中具有与`typeof`运算符类似的函数。 有关详细信息，请参阅[x:Type 标记扩展](~/xamarin-forms/xaml/markup-extensions/consuming.md#type)。

## <a name="single-primitive-type-argument"></a>单个基元类型参数

可以使用`x:TypeArguments`指令将单个基元类型参数指定为带前缀的字符串参数：

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:scg="clr-namespace:System.Collections.Generic;assembly=netstandard"
             ...>
    <CollectionView>
        <CollectionView.ItemsSource>
            <scg:List x:TypeArguments="x:String">
                <x:String>Baboon</x:String>
                <x:String>Capuchin Monkey</x:String>
                <x:String>Blue Monkey</x:String>
                <x:String>Squirrel Monkey</x:String>
                <x:String>Golden Lion Tamarin</x:String>
                <x:String>Howler Monkey</x:String>
                <x:String>Japanese Macaque</x:String>
            </scg:List>
        </CollectionView.ItemsSource>
    </CollectionView>
</ContentPage>
```

在此示例中`System.Collections.Generic` ，定义为`scg` XAML 命名空间。 使用`CollectionView.ItemsSource` XAML 2009 内置`x:String`类型将`List<T>`属性设置为使用`string`类型参数实例化的。 `List<string>`集合已初始化为包含多`string`个项。

此外，也可以通过 CLR `List<T>` `String`类型实例化集合，但这等效于：

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:scg="clr-namespace:System.Collections.Generic;assembly=netstandard"
             xmlns:sys="clr-namespace:System;assembly=netstandard"
             ...>
    <CollectionView>
        <CollectionView.ItemsSource>
            <scg:List x:TypeArguments="sys:String">
                <sys:String>Baboon</sys:String>
                <sys:String>Capuchin Monkey</sys:String>
                <sys:String>Blue Monkey</sys:String>
                <sys:String>Squirrel Monkey</sys:String>
                <sys:String>Golden Lion Tamarin</sys:String>
                <sys:String>Howler Monkey</sys:String>
                <sys:String>Japanese Macaque</sys:String>
            </scg:List>
        </CollectionView.ItemsSource>
    </CollectionView>
</ContentPage>
```

## <a name="single-object-type-argument"></a>单个对象类型参数

可以使用`x:TypeArguments`指令将单个对象类型参数指定为带前缀的字符串参数：

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:models="clr-namespace:GenericsDemo.Models"
             xmlns:scg="clr-namespace:System.Collections.Generic;assembly=netstandard"
             ...>
    <CollectionView>
        <CollectionView.ItemsSource>
            <scg:List x:TypeArguments="models:Monkey">
                <models:Monkey Name="Baboon"
                               Location="Africa and Asia"
                               ImageUrl="https://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/Papio_anubis_%28Serengeti%2C_2009%29.jpg/200px-Papio_anubis_%28Serengeti%2C_2009%29.jpg" />
                <models:Monkey Name="Capuchin Monkey"
                               Location="Central and South America"
                               ImageUrl="https://upload.wikimedia.org/wikipedia/commons/thumb/4/40/Capuchin_Costa_Rica.jpg/200px-Capuchin_Costa_Rica.jpg" />
                <models:Monkey Name="Blue Monkey"
                               Location="Central and East Africa"
                               ImageUrl="https://upload.wikimedia.org/wikipedia/commons/thumb/8/83/BlueMonkey.jpg/220px-BlueMonkey.jpg" />
            </scg:List>
        </CollectionView.ItemsSource>
        <CollectionView.ItemTemplate>
            <DataTemplate>
                <Grid Padding="10">
                    <Grid.RowDefinitions>
                        <RowDefinition Height="Auto" />
                        <RowDefinition Height="Auto" />
                    </Grid.RowDefinitions>
                    <Grid.ColumnDefinitions>
                        <ColumnDefinition Width="Auto" />
                        <ColumnDefinition Width="Auto" />
                    </Grid.ColumnDefinitions>
                    <Image Grid.RowSpan="2"
                           Source="{Binding ImageUrl}"
                           Aspect="AspectFill"
                           HeightRequest="60"
                           WidthRequest="60" />
                    <Label Grid.Column="1"
                           Text="{Binding Name}"
                           FontAttributes="Bold" />
                    <Label Grid.Row="1"
                           Grid.Column="1"
                           Text="{Binding Location}"
                           FontAttributes="Italic"
                           VerticalOptions="End" />
                </Grid>
            </DataTemplate>
        </CollectionView.ItemTemplate>
    </CollectionView>
</ContentPage>
```

在此示例中`GenericsDemo.Models` ，将定义为`models` xaml 命名空间， `System.Collections.Generic`并将定义为`scg` xaml 命名空间。 `CollectionView.ItemsSource`属性设置为`List<T>`使用`Monkey`类型参数实例化的。 使用`List<Monkey>`多`Monkey`个项初始化集合，并[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)将定义每个`Monkey`对象的外观的设置为`ItemTemplate`的。 [`CollectionView`](xref:Xamarin.Forms.CollectionView)

## <a name="multiple-type-arguments"></a>多个类型参数

可以使用`x:TypeArguments`指令将多个类型自变量指定为带前缀的字符串参数（用逗号分隔）。 当泛型约束使用泛型类型时，嵌套约束类型参数包含在括号中：

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:models="clr-namespace:GenericsDemo.Models"
             xmlns:scg="clr-namespace:System.Collections.Generic;assembly=netstandard"
             ...>
    <CollectionView>
        <CollectionView.ItemsSource>
            <scg:List x:TypeArguments="scg:KeyValuePair(x:String,models:Monkey)">
                <scg:KeyValuePair x:TypeArguments="x:String,models:Monkey">
                    <x:Arguments>
                        <x:String>Baboon</x:String>
                        <models:Monkey Location="Africa and Asia"
                                       ImageUrl="https://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/Papio_anubis_%28Serengeti%2C_2009%29.jpg/200px-Papio_anubis_%28Serengeti%2C_2009%29.jpg" />
                    </x:Arguments>
                </scg:KeyValuePair>
                <scg:KeyValuePair x:TypeArguments="x:String,models:Monkey">
                    <x:Arguments>
                        <x:String>Capuchin Monkey</x:String>
                        <models:Monkey Location="Central and South America"
                                       ImageUrl="https://upload.wikimedia.org/wikipedia/commons/thumb/4/40/Capuchin_Costa_Rica.jpg/200px-Capuchin_Costa_Rica.jpg" />   
                    </x:Arguments>
                </scg:KeyValuePair>
                <scg:KeyValuePair x:TypeArguments="x:String,models:Monkey">
                    <x:Arguments>
                        <x:String>Blue Monkey</x:String>
                        <models:Monkey Location="Central and East Africa"
                                       ImageUrl="https://upload.wikimedia.org/wikipedia/commons/thumb/8/83/BlueMonkey.jpg/220px-BlueMonkey.jpg" />
                    </x:Arguments>
                </scg:KeyValuePair>
            </scg:List>
        </CollectionView.ItemsSource>
        <CollectionView.ItemTemplate>
            <DataTemplate>
                <Grid Padding="10">
                    <Grid.RowDefinitions>
                        <RowDefinition Height="Auto" />
                        <RowDefinition Height="Auto" />
                    </Grid.RowDefinitions>
                    <Grid.ColumnDefinitions>
                        <ColumnDefinition Width="Auto" />
                        <ColumnDefinition Width="Auto" />
                    </Grid.ColumnDefinitions>
                    <Image Grid.RowSpan="2"
                           Source="{Binding Value.ImageUrl}"
                           Aspect="AspectFill"
                           HeightRequest="60"
                           WidthRequest="60" />
                    <Label Grid.Column="1"
                           Text="{Binding Key}"
                           FontAttributes="Bold" />
                    <Label Grid.Row="1"
                           Grid.Column="1"
                           Text="{Binding Value.Location}"
                           FontAttributes="Italic"
                           VerticalOptions="End" />
                </Grid>
            </DataTemplate>
        </CollectionView.ItemTemplate>
    </CollectionView>
</ContentPage    
```

在此示例中`GenericsDemo.Models` ，将定义为`models` xaml 命名空间， `System.Collections.Generic`并将定义为`scg` xaml 命名空间。 该`CollectionView.ItemsSource`属性`List<T>`设置为一个实例化的，它使用`KeyValuePair<TKey, TValue>`约束和内部约束类型参数`string`和`Monkey`进行实例化。 使用`List<KeyValuePair<string,Monkey>>`非`KeyValuePair`默认`KeyValuePair`构造函数将集合初始化为多个项，并[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)将定义每`Monkey`个对象的外观的设置为`ItemTemplate`的。 [`CollectionView`](xref:Xamarin.Forms.CollectionView) 有关将参数传递给非默认构造函数的信息，请参阅[传递构造函数参数](~/xamarin-forms/xaml/passing-arguments.md#constructor_arguments)。

## <a name="related-links"></a>相关链接

- [XAML 中的泛型（示例）](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-generics/)
- [XAML 2009 语言基元](/dotnet/desktop-wpf/xaml-services/types-for-primitives#xaml-2009-language-primitives)
- [x:Type 标记扩展](~/xamarin-forms/xaml/markup-extensions/consuming.md#type)
- [传递构造函数参数](~/xamarin-forms/xaml/passing-arguments.md#constructor_arguments)
