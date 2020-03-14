---
title: 第 2 部分： 基本 XAML 语法
description: 本文介绍的属性元素的基本 XAML 语法功能和附加属性。
ms.prod: xamarin
ms.assetid: 4022F1DC-3802-4635-A553-688ABD3F0D5A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/25/2017
ms.openlocfilehash: f79a07a04eddeea1441f7938fdef210a37fb920a
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/13/2020
ms.locfileid: "79306571"
---
# <a name="part-2-essential-xaml-syntax"></a>第 2 部分： 基本 XAML 语法

[![下载示例](~/media/shared/download.png) 下载示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)

_XAML 主要用于实例化和初始化对象。但通常情况下，必须将属性设置为复杂对象，这些对象不能轻松地表示为 XML 字符串，有时必须在子类上设置一个类定义的属性。这两个需要需要属性元素和附加属性的基本 XAML 语法功能。_

## <a name="property-elements"></a>属性元素

在 XAML 中，类的属性通常设置为 XML 属性：

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large"
       TextColor="Aqua" />
```

但是，没有在 XAML 中设置属性的替代方法。 若要在 `TextColor`中尝试此替代方法，请先删除现有 `TextColor` 设置：

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large" />
```

通过将空元素分隔到开始标记和结束标记中，打开 `Label` 标记的空元素：

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large">

</Label>
```

这些标记内添加的类名和句点分隔的属性名称组成的开始和结束标记：

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large">
    <Label.TextColor>

    </Label.TextColor>
</Label>
```

将属性值设置为这些新的标记，像这样的内容：

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="Large">
    <Label.TextColor>
        Aqua
    </Label.TextColor>
</Label>
```

这两种指定 `TextColor` 属性的方法在功能上等效，但不要为同一属性使用两种方式，因为这会有效地设置属性两次，并且可能不明确。

使用此新语法，可以引入了一些便利的术语：

- `Label` 是*对象元素*。 它是 Xamarin.Forms 对象表示为一个 XML 元素。
- `Text`、`VerticalOptions`、`FontAttributes` 和 `FontSize` 是*属性特性*。 它们是 Xamarin.Forms 属性表示为 XML 属性。
- 在该最终代码段中，`TextColor` 已成为*属性元素*。 它是 Xamarin.Forms 属性，但它现在是一个 XML 元素。

定义的属性元素可能在第一次似乎是违反了 XML 语法，但它不是。 期间必须在 XML 中没有特殊含义。 对于 XML 解码器，`Label.TextColor` 只是一个普通的子元素。

但是，在 XAML 中，此语法是非常特殊的。 属性元素的规则之一是，`Label.TextColor` 标记中不会出现其他内容。 属性的值始终定义为属性元素的开始和结束标记之间的内容。

在多个属性，可以使用属性元素语法：

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center">
    <Label.FontAttributes>
        Bold
    </Label.FontAttributes>
    <Label.FontSize>
        Large
    </Label.FontSize>
    <Label.TextColor>
        Aqua
    </Label.TextColor>
</Label>
```

也可以使用属性元素语法的所有属性：

```xaml
<Label>
    <Label.Text>
        Hello, XAML!
    </Label.Text>
    <Label.FontAttributes>
        Bold
    </Label.FontAttributes>
    <Label.FontSize>
        Large
    </Label.FontSize>
    <Label.TextColor>
        Aqua
    </Label.TextColor>
    <Label.VerticalOptions>
        Center
    </Label.VerticalOptions>
</Label>
```

首先，属性元素语法可能看起来是不必要的过于繁琐替换好像某些内容相对来说非常简单，并在这些示例中这的确是这种情况。

但是，属性元素语法将变得重要太复杂，无法表示为一个简单的字符串属性的值时。 属性元素标记内可以实例化另一个对象，并设置其属性。 例如，可以使用属性设置将属性（例如 `VerticalOptions`）显式设置为 `LayoutOptions` 值：

```xaml
<Label>
    ...
    <Label.VerticalOptions>
        <LayoutOptions Alignment="Center" />
    </Label.VerticalOptions>
</Label>
```

另一个示例： `Grid` 有两个名为 `RowDefinitions` 和 `ColumnDefinitions`的属性。 这两个属性的类型为 `RowDefinitionCollection` 和 `ColumnDefinitionCollection`，它们是 `RowDefinition` 和 `ColumnDefinition` 对象的集合。 需要使用属性元素语法来设置这些集合。

下面是 `GridDemoPage` 类的 XAML 文件的开头，其中显示了 `RowDefinitions` 和 `ColumnDefinitions` 集合的属性元素标记：

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.GridDemoPage"
             Title="Grid Demo Page">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
            <RowDefinition Height="100" />
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="100" />
        </Grid.ColumnDefinitions>
        ...
    </Grid>
</ContentPage>
```

请注意，用于定义自动调整大小的单元格的像素宽度和高度，以及星型设置单元格的缩写的语法。

## <a name="attached-properties"></a>附加属性

您刚才看到 `Grid` 需要 `RowDefinitions` 的属性元素和 `ColumnDefinitions` 集合来定义行和列。 不过，程序员还必须有一种方法来指示 `Grid` 的每个子项所在的行和列。

在 `Grid` 的每个子级的标记中，使用以下属性指定该子元素的行和列：

- `Grid.Row`
- `Grid.Column`

这些属性的默认值为 0。 您还可以指示是否子跨越多个行或列具有这些属性：

- `Grid.RowSpan`
- `Grid.ColumnSpan`

这两个属性具有默认值为 1。

下面是完整的 GridDemoPage.xaml 文件：

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.GridDemoPage"
             Title="Grid Demo Page">

    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
            <RowDefinition Height="100" />
        </Grid.RowDefinitions>

        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="100" />
        </Grid.ColumnDefinitions>

        <Label Text="Autosized cell"
               Grid.Row="0" Grid.Column="0"
               TextColor="White"
               BackgroundColor="Blue" />

        <BoxView Color="Silver"
                 HeightRequest="0"
                 Grid.Row="0" Grid.Column="1" />

        <BoxView Color="Teal"
                 Grid.Row="1" Grid.Column="0" />

        <Label Text="Leftover space"
               Grid.Row="1" Grid.Column="1"
               TextColor="Purple"
               BackgroundColor="Aqua"
               HorizontalTextAlignment="Center"
               VerticalTextAlignment="Center" />

        <Label Text="Span two rows (or more if you want)"
               Grid.Row="0" Grid.Column="2" Grid.RowSpan="2"
               TextColor="Yellow"
               BackgroundColor="Blue"
               HorizontalTextAlignment="Center"
               VerticalTextAlignment="Center" />

        <Label Text="Span two columns"
               Grid.Row="2" Grid.Column="0" Grid.ColumnSpan="2"
               TextColor="Blue"
               BackgroundColor="Yellow"
               HorizontalTextAlignment="Center"
               VerticalTextAlignment="Center" />

        <Label Text="Fixed 100x100"
               Grid.Row="2" Grid.Column="2"
               TextColor="Aqua"
               BackgroundColor="Red"
               HorizontalTextAlignment="Center"
               VerticalTextAlignment="Center" />

    </Grid>
</ContentPage>
```

`Grid.Row` 和 `Grid.Column` 设置不是必需的，但通常包括在内，目的是为了清晰起见。

下面是如下所示：

[![网格布局](essential-xaml-syntax-images/griddemo.png)](essential-xaml-syntax-images/griddemo-large.png#lightbox)

从语法上来说，这些 `Grid.Row`、`Grid.Column`、`Grid.RowSpan`和 `Grid.ColumnSpan` 特性似乎是 `Grid`的静态字段或属性，但更有趣的是，`Grid` 不会定义命名为 `Row`、`Column`、`RowSpan`或 `ColumnSpan`的任何内容。

`Grid` 定义了四个名为 `RowProperty`、`ColumnProperty`、`RowSpanProperty`和 `ColumnSpanProperty`的可绑定属性。 这些是特殊类型的可绑定属性，称为*附加属性*。 它们由 `Grid` 类定义，但在 `Grid`的子级上设置。

如果希望在代码中使用这些附加属性，`Grid` 类将提供名为 `SetRow`、`GetColumn`等的静态方法。 但在 XAML 中，这些附加属性使用简单的属性名称设置为 `Grid` 的子级中的属性。

附加的属性的形式始终可识别在 XAML 文件中包含的类和一个句点分隔的属性名称的属性。 它们被称为*附加属性*，因为它们由一个类（在本例中为 `Grid`）定义，但附加到其他对象（在本例中为 `Grid`的子级）。 在布局过程中，`Grid` 可以询问这些附加属性的值，以了解每个子元素的位置。

`AbsoluteLayout` 类定义了两个名为 `LayoutBounds` 和 `LayoutFlags`的附加属性。 下面是使用 `AbsoluteLayout`的比例定位和大小调整功能实现的棋盘模式：

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.AbsoluteDemoPage"
             Title="Absolute Demo Page">

    <AbsoluteLayout BackgroundColor="#FF8080">
        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="0.33, 0, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="1, 0, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="0, 0.33, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="0.67, 0.33, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="0.33, 0.67, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="1, 0.67, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="0, 1, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

        <BoxView Color="#8080FF"
                 AbsoluteLayout.LayoutBounds="0.67, 1, 0.25, 0.25"
                 AbsoluteLayout.LayoutFlags="All" />

  </AbsoluteLayout>
</ContentPage>
```

和其内容如下：

[![绝对布局](essential-xaml-syntax-images/absolutedemo-large.png)](essential-xaml-syntax-images/absolutedemo-large.png#lightbox)

此类的某些内容，可能会质疑使用 XAML 的智慧。 当然，`LayoutBounds` 矩形的重复和规律性表示它在代码中可能更好地实现。

这的确是合法的关注点，并且没有与平衡使用代码和标记定义用户界面时没问题。 很容易地在 XAML 中定义一些视觉对象，然后使用代码隐藏文件的构造函数添加一些可能会更好地生成在循环中的多个视觉对象。

## <a name="content-properties"></a>内容属性

在前面的示例中，"`StackLayout`"、"`Grid`" 和 "`AbsoluteLayout`" 对象设置为 `ContentPage`的 "`Content`" 属性，并且这些布局的子节点实际上是 `Children` 集合中的项。 不过，这些 `Content` 和 `Children` 属性不在 XAML 文件中。

当然，您可以将 `Content` 和 `Children` 属性作为属性元素包括在内，如**XamlPlusCode**示例中所示：

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.XamlPlusCodePage"
             Title="XAML + Code Page">
    <ContentPage.Content>
        <StackLayout>
            <StackLayout.Children>
                <Slider VerticalOptions="CenterAndExpand"
                        ValueChanged="OnSliderValueChanged" />

                <Label x:Name="valueLabel"
                       Text="A simple Label"
                       FontSize="Large"
                       HorizontalOptions="Center"
                       VerticalOptions="CenterAndExpand" />

                <Button Text="Click Me!"
                      HorizontalOptions="Center"
                      VerticalOptions="CenterAndExpand"
                      Clicked="OnButtonClicked" />
            </StackLayout.Children>
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

真正的问题是：为什么 XAML 文件中*不*需要这些属性元素？

在 Xamarin 中定义的元素，允许在 XAML 中使用的窗体在类的 `ContentProperty` 特性中有一个标记。 如果在联机 Xamarin. Forms 文档中查找 `ContentPage` 类，将看到以下属性：

```csharp
[Xamarin.Forms.ContentProperty("Content")]
public class ContentPage : TemplatedPage
```

这意味着不需要 `Content` 属性元素标记。 在开始和结束 `ContentPage` 标记之间出现的任何 XML 内容都假设分配给 `Content` 属性。

 `StackLayout`、`Grid`、`AbsoluteLayout`和 `RelativeLayout` 均从 `Layout<View>`派生，并且如果你在 Xamarin. Forms 文档中查找 `Layout<T>`，你会看到另一个 `ContentProperty` 属性：

```csharp
[Xamarin.Forms.ContentProperty("Children")]
public abstract class Layout<T> : Layout ...
```

这允许将布局内容自动添加到 `Children` 集合，而无需显式 `Children` 属性元素标记。

其他类还具有 `ContentProperty` 特性定义。 例如，将 `Text``Label` 的 content 属性。 检查其他人的 API 文档。

## <a name="platform-differences-with-onplatform"></a>使用 OnPlatform 平台差异

在单页面应用程序中，通常会在页面上设置 "`Padding`" 属性，以避免覆盖 iOS 状态栏。 在代码中，可以使用 `Device.RuntimePlatform` 属性来实现此目的：

```csharp
if (Device.RuntimePlatform == Device.iOS)
{
    Padding = new Thickness(0, 20, 0, 0);
}
```

你还可以使用[`OnPlatform`](xref:Xamarin.Forms.OnPlatform`1)和[`On`](xref:Xamarin.Forms.On)类在 XAML 中执行类似操作。 首先，将 `Padding` 属性的属性元素包含在页面顶部附近：

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>

    </ContentPage.Padding>
    ...
</ContentPage>
```

在这些标记中，包含 `OnPlatform` 标记。 `OnPlatform` 是一个泛型类。 需要指定泛型类型参数，在本例中为 `Thickness`，这是 `Padding` 属性的类型。 幸运的是，有一个 XAML 特性专用于定义称为 `x:TypeArguments`的泛型参数。 这应与要设置的属性的类型匹配：

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">

        </OnPlatform>
    </ContentPage.Padding>
  ...
</ContentPage>
```

`OnPlatform` 具有一个名为 `Platforms` 的属性，该属性是 `On` 对象的 `IList`。 该属性使用属性元素标记：

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <OnPlatform.Platforms>

            </OnPlatform.Platforms>
        </OnPlatform>
    </ContentPage.Padding>
  ...
</ContentPage>
```

现在添加 `On` 元素。 对于每个属性，将 `Platform` 属性和 `Value` 属性设置为 `Thickness` 属性的标记：

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <OnPlatform.Platforms>
                <On Platform="iOS" Value="0, 20, 0, 0" />
                <On Platform="Android" Value="0, 0, 0, 0" />
                <On Platform="UWP" Value="0, 0, 0, 0" />
            </OnPlatform.Platforms>
        </OnPlatform>
    </ContentPage.Padding>
  ...
</ContentPage>
```

可以简化此标记。 `Platforms``OnPlatform` 的 content 属性，因此可以删除这些属性元素标记：

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS" Value="0, 20, 0, 0" />
            <On Platform="Android" Value="0, 0, 0, 0" />
            <On Platform="UWP" Value="0, 0, 0, 0" />
        </OnPlatform>
    </ContentPage.Padding>
  ...
</ContentPage>
```

`On` 的 `Platform` 属性的类型为 `IList<string>`，因此，如果值相同，则可以包含多个平台：

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS" Value="0, 20, 0, 0" />
            <On Platform="Android, UWP" Value="0, 0, 0, 0" />
        </OnPlatform>
    </ContentPage.Padding>
  ...
</ContentPage>
```

由于 Android 和 UWP 设置为 `Padding`的默认值，因此可以删除此标记：

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS" Value="0, 20, 0, 0" />
        </OnPlatform>
    </ContentPage.Padding>
  ...
</ContentPage>
```

这是在 XAML 中设置依赖于平台的 `Padding` 属性的标准方法。 如果 `Value` 设置无法由单个字符串表示，则可以为其定义属性元素：

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">

    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS">
                <On.Value>
                    0, 20, 0, 0
                </On.Value>
            </On>
        </OnPlatform>
    </ContentPage.Padding>
  ...
</ContentPage>
```

> [!NOTE]
> 还可以在 XAML 中使用 `OnPlatform` 标记扩展，根据每个平台自定义 UI 外观。 它提供与 `OnPlatform` 和 `On` 类相同的功能，但具有更简洁的表示形式。 有关详细信息，请参阅[OnPlatform 标记扩展](~/xamarin-forms/xaml/markup-extensions/consuming.md#onplatform)。

## <a name="summary"></a>摘要

使用属性元素和附加的属性，很多基本 XAML 语法已建立。 但是，有时您需要将属性设置为对象以间接方式，例如，从资源字典。 下一部分第3部分介绍了此方法[。XAML 标记扩展](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)。

## <a name="related-links"></a>相关链接

- [XamlSamples](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)
- [第1部分。与 XAML 入门](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
- [第3部分。XAML 标记扩展](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [第4部分。数据绑定基础知识](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
- [第5部分。从数据绑定到 MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
