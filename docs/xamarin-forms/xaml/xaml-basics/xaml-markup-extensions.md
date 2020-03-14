---
title: 第 3 部分： XAML 标记扩展
description: XAML 标记扩展构成允许将属性设置为对象或从其他源间接引用的值的 XAML 中的重要功能。
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F4A37564-B18B-42FF-B841-9A1949895AB6
author: davidbritch
ms.author: dabritch
ms.date: 03/27/2018
ms.openlocfilehash: 89e2026ff16a9614234d6ee4bfa4df620cf58b56
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/13/2020
ms.locfileid: "79305899"
---
# <a name="part-3-xaml-markup-extensions"></a>第 3 部分： XAML 标记扩展

[![下载示例](~/media/shared/download.png) 下载示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)

_XAML 标记扩展在 XAML 中构成了一项重要功能，该功能允许将属性设置为间接从其他源引用的对象或值。XAML 标记扩展对于共享对象和引用在整个应用程序中使用的常量尤其重要，但它们在数据绑定中找到了最大的实用程序。_

## <a name="xaml-markup-extensions"></a>XAML 标记扩展

一般情况下，使用 XAML 将对象的属性设置为显式值，如字符串、 数字、 枚举成员或转换为在后台值的字符串。

有时，但是，属性必须改为引用其他，某一位置定义的值，或可能由代码在运行时需要很少的处理。 为此，可以使用 XAML*标记扩展*。

这些 XAML 标记扩展不是扩展的 XML。 XAML 是完全合法的 XML。 它们被称为 "扩展"，因为它们由实现 `IMarkupExtension`的类中的代码支持。 您可以编写自己的自定义标记扩展。

在许多情况下，XAML 标记扩展是 XAML 文件中立即认出因为它们将显示由大括号分隔的属性设置为: {和}，但有时标记扩展将出现在标记为传统的元素。

## <a name="shared-resources"></a>共享的资源

某些 XAML 页属性设置为相同的值包含多个视图。 例如，这些 `Button` 对象的许多属性设置是相同的：

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.SharedResourcesPage"
             Title="Shared Resources Page">

    <StackLayout>
        <Button Text="Do this!"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                BorderWidth="3"
                Rotation="-15"
                TextColor="Red"
                FontSize="24" />

        <Button Text="Do that!"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                BorderWidth="3"
                Rotation="-15"
                TextColor="Red"
                FontSize="24" />

        <Button Text="Do the other thing!"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                BorderWidth="3"
                Rotation="-15"
                TextColor="Red"
                FontSize="24" />

    </StackLayout>
</ContentPage>
```

如果其中一个属性需要更改，可能想要进行的更改只需一次而不是三次。 如果这是代码，您将可能使用常量和静态只读对象来帮助保持一致且轻松地修改此类值。

在 XAML 中，最常见的解决方案是将此类值或对象存储在*资源字典*中。 `VisualElement` 类定义 `ResourceDictionary`类型的名为 `Resources` 的属性，该属性是一个包含类型 `string` 的键的字典和类型 `object`的值。 可以将对象放入此字典，然后从标记中，所有在 XAML 中引用它们。

若要在页面上使用资源字典，请包含一对 `Resources` 属性元素标记。 它是最方便的方式将这些页面的顶部：

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.SharedResourcesPage"
             Title="Shared Resources Page">

    <ContentPage.Resources>

    </ContentPage.Resources>
    ...
</ContentPage>
```

还需要显式包含 `ResourceDictionary` 标记：

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.SharedResourcesPage"
             Title="Shared Resources Page">

    <ContentPage.Resources>
        <ResourceDictionary>

        </ResourceDictionary>
    </ContentPage.Resources>
    ...
</ContentPage>
```

现在可以对资源字典添加对象和各种类型的值。 这些类型必须是可实例化。 它们不能为抽象类，例如。 这些类型还必须具有公共无参数构造函数。 每一项都需要使用 `x:Key` 特性指定的字典键。 例如：

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.SharedResourcesPage"
             Title="Shared Resources Page">

    <ContentPage.Resources>
        <ResourceDictionary>
            <LayoutOptions x:Key="horzOptions"
                           Alignment="Center" />

            <LayoutOptions x:Key="vertOptions"
                           Alignment="Center"
                           Expands="True" />
        </ResourceDictionary>
    </ContentPage.Resources>
    ...
</ContentPage>
```

这两个项是结构类型 `LayoutOptions`的值，每个项都有一个唯一键和一个或两个属性集。 在代码和标记中，使用 `LayoutOptions`的静态字段更为常见，但更方便的方法是设置属性。

现在，必须将这些按钮的 `HorizontalOptions` 和 `VerticalOptions` 属性设置为这些资源，并通过 `StaticResource` XAML 标记扩展完成此操作：

```xaml
<Button Text="Do this!"
        HorizontalOptions="{StaticResource horzOptions}"
        VerticalOptions="{StaticResource vertOptions}"
        BorderWidth="3"
        Rotation="-15"
        TextColor="Red"
        FontSize="24" />
```

`StaticResource` 标记扩展始终用大括号分隔，并包含字典键。

该名称 `StaticResource` 区分其与 `DynamicResource`，它也支持。 `DynamicResource` 适用于与运行时可能更改的值相关联的字典键，而 `StaticResource` 只在构造页上的元素时从字典访问元素。

对于 `BorderWidth` 属性，必须在字典中存储 double。 XAML 方便地定义常见数据类型（如 `x:Double` 和 `x:Int32`）的标记：

```xaml
<ContentPage.Resources>
    <ResourceDictionary>
        <LayoutOptions x:Key="horzOptions"
                       Alignment="Center" />

        <LayoutOptions x:Key="vertOptions"
                       Alignment="Center"
                       Expands="True" />

        <x:Double x:Key="borderWidth">
            3
        </x:Double>
    </ResourceDictionary>
</ContentPage.Resources>
```

不需要将其放在三个行。 此旋转角度此字典条目仅占用一行：

```xaml
<ContentPage.Resources>
    <ResourceDictionary>
        <LayoutOptions x:Key="horzOptions"
                       Alignment="Center" />

        <LayoutOptions x:Key="vertOptions"
                       Alignment="Center"
                       Expands="True" />

         <x:Double x:Key="borderWidth">
            3
         </x:Double>

        <x:Double x:Key="rotationAngle">-15</x:Double>
    </ResourceDictionary>
</ContentPage.Resources>
```

可以通过与 `LayoutOptions` 值相同的方式来引用这两个资源：

```xaml
<Button Text="Do this!"
        HorizontalOptions="{StaticResource horzOptions}"
        VerticalOptions="{StaticResource vertOptions}"
        BorderWidth="{StaticResource borderWidth}"
        Rotation="{StaticResource rotationAngle}"
        TextColor="Red"
        FontSize="24" />
```

对于类型 `Color`的资源，可以使用在直接分配这些类型的属性时使用的相同字符串表示形式。 创建资源时，将调用类型转换器。 下面是 `Color`类型的资源：

```xaml
<Color x:Key="textColor">Red</Color>
```

通常情况下，程序会将 `FontSize` 属性设置为 `NamedSize` 枚举的成员（如 `Large`）。 `FontSizeConverter` 类在幕后工作，使用 `Device.GetNamedSized` 方法将其转换为与平台相关的值。 但是，在定义字体大小资源时，使用数字值会更有意义，这里显示为 `x:Double` 类型：

```xaml
<x:Double x:Key="fontSize">24</x:Double>
```

现在，除 `Text` 以外的所有属性均由资源设置定义：

```xaml
<Button Text="Do this!"
        HorizontalOptions="{StaticResource horzOptions}"
        VerticalOptions="{StaticResource vertOptions}"
        BorderWidth="{StaticResource borderWidth}"
        Rotation="{StaticResource rotationAngle}"
        TextColor="{StaticResource textColor}"
        FontSize="{StaticResource fontSize}" />
```

还可以使用资源字典中的 `OnPlatform` 为平台定义不同的值。 下面是 `OnPlatform` 对象如何作为不同文本颜色的资源字典的一部分：

```xaml
<OnPlatform x:Key="textColor"
            x:TypeArguments="Color">
    <On Platform="iOS" Value="Red" />
    <On Platform="Android" Value="Aqua" />
    <On Platform="UWP" Value="#80FF80" />
</OnPlatform>
```

请注意，`OnPlatform` 获取 `x:Key` 属性，因为它是字典中的对象和 `x:TypeArguments` 特性，因为它是泛型类。 初始化对象时，`iOS`、`Android`和 `UWP` 特性会转换为 `Color` 值。

下面是最终完成 XAML 文件具有访问共享的六个值的三个按钮：

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.SharedResourcesPage"
             Title="Shared Resources Page">

    <ContentPage.Resources>
        <ResourceDictionary>
            <LayoutOptions x:Key="horzOptions"
                           Alignment="Center" />

            <LayoutOptions x:Key="vertOptions"
                           Alignment="Center"
                           Expands="True" />

            <x:Double x:Key="borderWidth">3</x:Double>

            <x:Double x:Key="rotationAngle">-15</x:Double>

            <OnPlatform x:Key="textColor"
                        x:TypeArguments="Color">
                <On Platform="iOS" Value="Red" />
                <On Platform="Android" Value="Aqua" />
                <On Platform="UWP" Value="#80FF80" />
            </OnPlatform>

            <x:Double x:Key="fontSize">24</x:Double>
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout>
        <Button Text="Do this!"
                HorizontalOptions="{StaticResource horzOptions}"
                VerticalOptions="{StaticResource vertOptions}"
                BorderWidth="{StaticResource borderWidth}"
                Rotation="{StaticResource rotationAngle}"
                TextColor="{StaticResource textColor}"
                FontSize="{StaticResource fontSize}" />

        <Button Text="Do that!"
                HorizontalOptions="{StaticResource horzOptions}"
                VerticalOptions="{StaticResource vertOptions}"
                BorderWidth="{StaticResource borderWidth}"
                Rotation="{StaticResource rotationAngle}"
                TextColor="{StaticResource textColor}"
                FontSize="{StaticResource fontSize}" />

        <Button Text="Do the other thing!"
                HorizontalOptions="{StaticResource horzOptions}"
                VerticalOptions="{StaticResource vertOptions}"
                BorderWidth="{StaticResource borderWidth}"
                Rotation="{StaticResource rotationAngle}"
                TextColor="{StaticResource textColor}"
                FontSize="{StaticResource fontSize}" />

    </StackLayout>
</ContentPage>
```

屏幕截图验证一致样式和依赖于平台的样式：

[![的样式控件](xaml-markup-extensions-images/sharedresources.png)](xaml-markup-extensions-images/sharedresources-large.png#lightbox)

尽管最常见的是在页面顶部定义 `Resources` 集合，但请记住，`Resources` 属性由 `VisualElement`定义，你可以在页面上的其他元素上有 `Resources` 的集合。 例如，尝试在本示例中将一个添加到 `StackLayout`：

```xaml
<StackLayout>
    <StackLayout.Resources>
        <ResourceDictionary>
            <Color x:Key="textColor">Blue</Color>
        </ResourceDictionary>
    </StackLayout.Resources>
    ...
</StackLayout>
```

你会发现这些按钮的文本颜色现在为蓝色。 基本上，每当 XAML 分析器遇到 `StaticResource` 标记扩展时，它都会在可视化树中向上搜索，并使用它遇到的第一个包含该键的 `ResourceDictionary`。

资源字典中存储的最常见的对象类型之一是 Xamarin `Style`，它定义属性设置的集合。 文章[样式](~/xamarin-forms/user-interface/styles/index.md)中讨论了样式。

有时，开发人员在 XAML 中的新用户可以将视觉元素（如 `Label` 或 `Button` 放在 `ResourceDictionary`中。 虽然肯定可能，但它没有太大意义。 `ResourceDictionary` 的目的是共享对象。 不能共享一个可视元素。 同一个实例不能出现两次在单个页面上。

## <a name="the-xstatic-markup-extension"></a>X:static 标记扩展

尽管其名称相似，但 `x:Static` 和 `StaticResource` 差别很大。 `StaticResource` 从资源字典中返回一个对象，而 `x:Static` 访问以下项之一：

- 公共静态字段
- 公共静态属性
- 公共常量字段
- 枚举成员。

`StaticResource` 标记扩展由定义资源字典的 XAML 实现支持，而 `x:Static` 是 XAML 的内部组成部分，因为 `x` 前缀会显示。

下面的几个示例演示了 `x:Static` 如何显式引用静态字段和枚举成员：

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="{x:Static LayoutOptions.Start}"
       HorizontalTextAlignment="{x:Static TextAlignment.Center}"
       TextColor="{x:Static Color.Aqua}" />
```

到目前为止，这并不令人印象非常深刻。 但 `x:Static` 标记扩展还可以从自己的代码中引用静态字段或属性。 例如，下面是一个 `AppConstants` 类，其中包含一些静态字段，你可能想要在整个应用程序的多个页上使用这些字段：

```csharp
using System;
using Xamarin.Forms;

namespace XamlSamples
{
    static class AppConstants
    {
        public static readonly Thickness PagePadding;

        public static readonly Font TitleFont;

        public static readonly Color BackgroundColor = Color.Aqua;

        public static readonly Color ForegroundColor = Color.Brown;

        static AppConstants()
        {
            switch (Device.RuntimePlatform)
            {
                case Device.iOS:
                    PagePadding = new Thickness(5, 20, 5, 0);
                    TitleFont = Font.SystemFontOfSize(35, FontAttributes.Bold);
                    break;

                case Device.Android:
                    PagePadding = new Thickness(5, 0, 5, 0);
                    TitleFont = Font.SystemFontOfSize(40, FontAttributes.Bold);
                    break;

                case Device.UWP:
                    PagePadding = new Thickness(5, 0, 5, 0);
                    TitleFont = Font.SystemFontOfSize(50, FontAttributes.Bold);
                    break;
            }
        }
    }
}
```

若要引用的 XAML 文件中此类的静态字段，将需要设法指出此文件的所在位置在 XAML 文件中。 使用 XML 命名空间声明来执行此操作。

回想一下作为标准 Xamarin.Forms XAML 模板的一部分创建的 XAML 文件包含两个 XML 命名空间声明： 一个用于访问 Xamarin.Forms 类和另一个用于引用标记和属性到 XAML 内部函数：

```csharp
xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
```

你将需要额外的 XML 命名空间声明，若要访问其他类。 每个其他的 XML 命名空间声明定义一个新的前缀。 若要访问共享应用程序的本地类 .NET Standard 库，如 `AppConstants`，XAML 程序员通常使用前缀 `local`。 命名空间声明必须指示 CLR （公共语言运行时）命名空间名称，也称为 .NET 命名空间名称，这是显示在C# `namespace` 定义中或 `using` 指令中的名称：

```csharp
xmlns:local="clr-namespace:XamlSamples"
```

此外可以在.NET Standard 库引用任何程序集中定义.NET 命名空间的 XML 命名空间的声明。 例如，下面是标准 .NET `System` 命名空间的 `sys` 前缀，位于**netstandard**程序集中。 由于这是另一个程序集，因此还必须指定程序集名称，在本例中为**netstandard**：

```csharp
xmlns:sys="clr-namespace:System;assembly=netstandard"
```

请注意，关键字 `clr-namespace` 后跟一个冒号和 .NET 命名空间名称，后面跟有分号、关键字 `assembly`、等号和程序集名称。

是的，`clr-namespace` 后面有一个冒号，但 `assembly`后跟有等号。 语法是特意定义的：大多数 XML 命名空间声明都引用了一个 URI，该 URI 开始使用 URI 方案名称（如 `http`），该 URI 后面始终跟有一个冒号。 此字符串的 `clr-namespace` 部分旨在模拟该约定。

这两个命名空间声明都包含在**StaticConstantsPage**示例中。 请注意，`BoxView` 尺寸设置为 `Math.PI` 和 `Math.E`，但比例为100：

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples"
             xmlns:sys="clr-namespace:System;assembly=netstandard"
             x:Class="XamlSamples.StaticConstantsPage"
             Title="Static Constants Page"
             Padding="{x:Static local:AppConstants.PagePadding}">

    <StackLayout>
       <Label Text="Hello, XAML!"
              TextColor="{x:Static local:AppConstants.BackgroundColor}"
              BackgroundColor="{x:Static local:AppConstants.ForegroundColor}"
              Font="{x:Static local:AppConstants.TitleFont}"
              HorizontalOptions="Center" />

      <BoxView WidthRequest="{x:Static sys:Math.PI}"
               HeightRequest="{x:Static sys:Math.E}"
               Color="{x:Static local:AppConstants.ForegroundColor}"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand"
               Scale="100" />
    </StackLayout>
</ContentPage>
```

结果 `BoxView` 相对于屏幕的大小与平台相关：

[使用 x:Static 标记扩展 ![控件](xaml-markup-extensions-images/staticconstants.png)](xaml-markup-extensions-images/staticconstants-large.png#lightbox)

## <a name="other-standard-markup-extensions"></a>其他标准标记扩展

多个标记扩展是固有的 XAML 和在 Xamarin.Forms XAML 文件中受支持。 其中一些不常使用，但在需要时至关重要：

- 如果默认情况下，如果属性具有非 `null` 值，但你想要将其设置为 `null`，请将其设置为 `{x:Null}` 标记扩展。
- 如果属性的类型为 `Type`，则可以使用标记扩展 `{x:Type someClass}`将其分配给 `Type` 对象。
- 您可以使用 `x:Array` 标记扩展在 XAML 中定义数组。 此标记扩展具有一个名为 `Type` 的必需属性，该属性指示数组中元素的类型。
- 第4部分将讨论 `Binding` 标记扩展[。数据绑定基础知识](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)。
- [相关绑定](~/xamarin-forms/app-fundamentals/data-binding/relative-bindings.md)中讨论了 `RelativeSource` 标记扩展。

## <a name="the-constraintexpression-markup-extension"></a>ConstraintExpression 标记扩展

标记扩展可以有属性，但它们不将设置等的 XML 属性。 在标记扩展中，属性设置以逗号分隔，并在大括号内显示没有引号。

这可以使用名为 `ConstraintExpression`的 Xamarin. Forms 标记扩展进行说明，该扩展与 `RelativeLayout` 类一起使用。 作为常量，或相对于父级或其他命名的视图，可以指定的位置或子视图的大小。 使用 `ConstraintExpression` 的语法，您可以使用另一个视图的属性和 `Constant`的 `Factor` 时间设置视图的位置或大小。 比这更复杂的任何内容需要代码。

下面是一个示例：

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.RelativeLayoutPage"
             Title="RelativeLayout Page">

    <RelativeLayout>

        <!-- Upper left -->
        <BoxView Color="Red"
                 RelativeLayout.XConstraint=
                     "{ConstraintExpression Type=Constant,
                                            Constant=0}"
                 RelativeLayout.YConstraint=
                     "{ConstraintExpression Type=Constant,
                                            Constant=0}" />
        <!-- Upper right -->
        <BoxView Color="Green"
                 RelativeLayout.XConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Width,
                                            Factor=1,
                                            Constant=-40}"
                 RelativeLayout.YConstraint=
                     "{ConstraintExpression Type=Constant,
                                            Constant=0}" />
        <!-- Lower left -->
        <BoxView Color="Blue"
                 RelativeLayout.XConstraint=
                     "{ConstraintExpression Type=Constant,
                                            Constant=0}"
                 RelativeLayout.YConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Height,
                                            Factor=1,
                                            Constant=-40}" />
        <!-- Lower right -->
        <BoxView Color="Yellow"
                 RelativeLayout.XConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Width,
                                            Factor=1,
                                            Constant=-40}"
                 RelativeLayout.YConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Height,
                                            Factor=1,
                                            Constant=-40}" />

        <!-- Centered and 1/3 width and height of parent -->
        <BoxView x:Name="oneThird"
                 Color="Red"
                 RelativeLayout.XConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Width,
                                            Factor=0.33}"
                 RelativeLayout.YConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Height,
                                            Factor=0.33}"
                 RelativeLayout.WidthConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Width,
                                            Factor=0.33}"
                 RelativeLayout.HeightConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Height,
                                            Factor=0.33}"  />

        <!-- 1/3 width and height of previous -->
        <BoxView Color="Blue"
                 RelativeLayout.XConstraint=
                     "{ConstraintExpression Type=RelativeToView,
                                            ElementName=oneThird,
                                            Property=X}"
                 RelativeLayout.YConstraint=
                     "{ConstraintExpression Type=RelativeToView,
                                            ElementName=oneThird,
                                            Property=Y}"
                 RelativeLayout.WidthConstraint=
                     "{ConstraintExpression Type=RelativeToView,
                                            ElementName=oneThird,
                                            Property=Width,
                                            Factor=0.33}"
                 RelativeLayout.HeightConstraint=
                     "{ConstraintExpression Type=RelativeToView,
                                            ElementName=oneThird,
                                            Property=Height,
                                            Factor=0.33}"  />
    </RelativeLayout>
</ContentPage>
```

可能需要与此示例的最重要经验是标记扩展的语法： 没有引号必须出现在标记扩展的大括号内。 键入时标记扩展在 XAML 文件中，是自然地想要将属性的值括在引号。 忍不住 ！

下面是运行的程序：

[使用约束 ![相对布局](xaml-markup-extensions-images/relativelayout.png)](xaml-markup-extensions-images/relativelayout-large.png#lightbox)

## <a name="summary"></a>摘要

如下所示的 XAML 标记扩展提供 XAML 文件的重要支持。 但可能会 `Binding`最有价值的 XAML 标记扩展，这将在本系列的下一部分的第[4 部分中介绍。数据绑定基础知识](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)。

## <a name="related-links"></a>相关链接

- [XamlSamples](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)
- [第1部分。与 XAML 入门](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
- [第2部分。基本 XAML 语法](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
- [第4部分。数据绑定基础知识](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
- [第5部分。从数据绑定到 MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
