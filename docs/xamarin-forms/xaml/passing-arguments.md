---
title: 在 XAML 中传递自变量
description: 本文演示如何使用可用于将参数传递到非默认构造函数，以调用工厂方法，并指定泛型参数的类型的 XAML 属性。
ms.prod: xamarin
ms.assetid: 8F3B267F-499E-4D79-9193-FCA99F199519
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/25/2016
ms.openlocfilehash: 80f332e45d6c46ad49543923e85cbb2eceadb378
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/13/2020
ms.locfileid: "79306559"
---
# <a name="passing-arguments-in-xaml"></a>在 XAML 中传递自变量

[![下载示例](~/media/shared/download.png) 下载示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-passingconstructorarguments)

_本文演示如何使用 XAML 属性，这些属性可用于将参数传递给非默认构造函数、调用工厂方法和指定泛型参数的类型。_

## <a name="overview"></a>概述

通常是使用构造函数需要参数，或通过调用静态创建方法的对象的实例化所必需的。 这可以通过使用 `x:Arguments` 和 `x:FactoryMethod` 属性在 XAML 中实现：

- `x:Arguments` 特性用于指定非默认构造函数或工厂方法对象声明的构造函数参数。 有关详细信息，请参阅[传递构造函数参数](#constructor_arguments)。
- `x:FactoryMethod` 特性用于指定可用于初始化对象的工厂方法。 有关详细信息，请参阅[调用工厂方法](#factory_methods)。

此外，`x:TypeArguments` 特性可用于指定泛型类型的构造函数的泛型类型参数。 有关详细信息，请参阅[指定泛型类型参数](#generic_type_arguments)。

<a name="constructor_arguments" />

## <a name="passing-constructor-arguments"></a>传递构造函数自变量

可以使用 `x:Arguments` 特性将参数传递给非默认构造函数。 表示自变量的类型的 XML 元素中，每个构造函数自变量必须进行分隔。 Xamarin.Forms 的基本类型支持以下元素：

- `x:Object`
- `x:Boolean`
- `x:Byte`
- `x:Int16`
- `x:Int32`
- `x:Int64`
- `x:Single`
- `x:Double`
- `x:Decimal`
- `x:Char`
- `x:String`
- `x:TimeSpan`
- `x:Array`
- `x:DateTime`

下面的代码示例演示如何将 `x:Arguments` 特性用于三个[`Color`](xref:Xamarin.Forms.Color)构造函数：

```xaml
<BoxView HeightRequest="150" WidthRequest="150" HorizontalOptions="Center">
  <BoxView.Color>
    <Color>
      <x:Arguments>
        <x:Double>0.9</x:Double>
      </x:Arguments>
    </Color>
  </BoxView.Color>
</BoxView>
<BoxView HeightRequest="150" WidthRequest="150" HorizontalOptions="Center">
  <BoxView.Color>
    <Color>
      <x:Arguments>
        <x:Double>0.25</x:Double>
        <x:Double>0.5</x:Double>
        <x:Double>0.75</x:Double>
      </x:Arguments>
    </Color>
  </BoxView.Color>
</BoxView>
<BoxView HeightRequest="150" WidthRequest="150" HorizontalOptions="Center">
  <BoxView.Color>
    <Color>
      <x:Arguments>
        <x:Double>0.8</x:Double>
        <x:Double>0.5</x:Double>
        <x:Double>0.2</x:Double>
        <x:Double>0.5</x:Double>
      </x:Arguments>
    </Color>
  </BoxView.Color>
</BoxView>
```

`x:Arguments` 标记内的元素数以及这些元素的类型必须与某个[`Color`](xref:Xamarin.Forms.Color)构造函数匹配。 带有单个参数的 `Color`[构造函数](xref:Xamarin.Forms.Color.%23ctor(System.Double))要求从0（黑色）到1（白色）的灰度值。 带有三个参数的 `Color`[构造函数](xref:Xamarin.Forms.Color.%23ctor(System.Double,System.Double,System.Double))需要红色、绿色和蓝色值，范围从0到1。 带有四个参数的 `Color`[构造函数](xref:Xamarin.Forms.Color.%23ctor(System.Double,System.Double,System.Double,System.Double))添加 alpha 通道作为第四个参数。

以下屏幕截图显示了用指定参数值调用每个[`Color`](xref:Xamarin.Forms.Color)构造函数的结果：

![BoxView 指定的 x:Arguments](passing-arguments-images/passing-arguments.png)

<a name="factory_methods" />

## <a name="calling-factory-methods"></a>调用工厂方法

可以在 XAML 中调用工厂方法，方法是使用 `x:FactoryMethod` 特性指定方法的名称，并使用 `x:Arguments` 特性指定其参数。 工厂方法是一个 `public static` 方法，该方法返回与定义方法的类或结构相同的类型的对象或值。

[`Color`](xref:Xamarin.Forms.Color)结构定义了许多工厂方法，下面的代码示例演示如何调用其中的三种方法：

```xaml
<BoxView HeightRequest="150" WidthRequest="150" HorizontalOptions="Center">
  <BoxView.Color>
    <Color x:FactoryMethod="FromRgba">
      <x:Arguments>
        <x:Int32>192</x:Int32>
        <x:Int32>75</x:Int32>
        <x:Int32>150</x:Int32>                        
        <x:Int32>128</x:Int32>
      </x:Arguments>
    </Color>
  </BoxView.Color>
</BoxView>
<BoxView HeightRequest="150" WidthRequest="150" HorizontalOptions="Center">
  <BoxView.Color>
    <Color x:FactoryMethod="FromHsla">
      <x:Arguments>
        <x:Double>0.23</x:Double>
        <x:Double>0.42</x:Double>
        <x:Double>0.69</x:Double>
        <x:Double>0.7</x:Double>
      </x:Arguments>
    </Color>
  </BoxView.Color>
</BoxView>
<BoxView HeightRequest="150" WidthRequest="150" HorizontalOptions="Center">
  <BoxView.Color>
    <Color x:FactoryMethod="FromHex">
      <x:Arguments>
        <x:String>#FF048B9A</x:String>
      </x:Arguments>
    </Color>
  </BoxView.Color>
</BoxView>
```

`x:Arguments` 标记内的元素数以及这些元素的类型必须与被调用的工厂方法的参数匹配。 [`FromRgba`](xref:Xamarin.Forms.Color.FromRgba(System.Int32,System.Int32,System.Int32,System.Int32)) factory 方法需要四个[`Int32`](https://docs.microsoft.com/dotnet/api/system.int32)参数，分别表示红色、绿色、蓝色和 alpha 值，范围介于0到255之间。 [`FromHsla`](xref:Xamarin.Forms.Color.FromHsla(System.Double,System.Double,System.Double,System.Double)) factory 方法需要四个[`Double`](https://docs.microsoft.com/dotnet/api/system.double)参数，分别表示色相、饱和度、亮度和 alpha 值，范围从0到1。 [`FromHex`](xref:Xamarin.Forms.Color.FromHex(System.String)) factory 方法需要一个表示十六进制（a） RGB 颜色的[`String`](https://docs.microsoft.com/dotnet/api/system.string) 。

以下屏幕截图显示了用指定参数值调用每个[`Color`](xref:Xamarin.Forms.Color)工厂方法的结果：

![BoxView 通过 x:FactoryMethod 和 x:Arguments 指定的颜色](passing-arguments-images/factory-methods.png)

<a name="generic_type_arguments" />

## <a name="specifying-a-generic-type-argument"></a>指定泛型类型参数

可以使用 `x:TypeArguments` 特性指定泛型类型的构造函数的泛型类型参数，如以下代码示例所示：

```xaml
<ContentPage ...>
  <StackLayout>
    <StackLayout.Margin>
      <OnPlatform x:TypeArguments="Thickness">
        <On Platform="iOS" Value="0,20,0,0" />
        <On Platform="Android" Value="5, 10" />
        <On Platform="UWP" Value="10" />
      </OnPlatform>
    </StackLayout.Margin>
  </StackLayout>
</ContentPage>
```

[`OnPlatform`](xref:Xamarin.Forms.OnPlatform`1)类是一个泛型类，必须使用与目标类型匹配的 `x:TypeArguments` 属性对其进行实例化。 在[`On`](xref:Xamarin.Forms.On)类中， [`Platform`](xref:Xamarin.Forms.On.Platform)属性可以接受单个 `string` 值或多个以逗号分隔的 `string` 值。 在此示例中， [`StackLayout.Margin`](xref:Xamarin.Forms.View.Margin)属性设置为特定于平台的[`Thickness`](xref:Xamarin.Forms.Thickness)。

## <a name="summary"></a>摘要

本文演示了使用的 XAML 特性，可用于将参数传递到非默认构造函数，以调用工厂方法，并指定泛型参数的类型。

## <a name="related-links"></a>相关链接

- [XAML 命名空间](~/xamarin-forms/xaml/namespaces.md)
- [传递构造函数参数（示例）](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-passingconstructorarguments)
- [调用工厂方法（示例）](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-callingfactorymethods)
