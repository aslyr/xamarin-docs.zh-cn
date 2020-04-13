---
title: Xamarin.窗体标签
description: 本文介绍如何使用 Xamarin.Forms 标签类在应用程序中显示单行和多行文本。
ms.prod: xamarin
ms.assetid: 02E6C553-5670-49A0-8EE9-5153ED21EA91
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/09/2020
ms.openlocfilehash: bed6b8a774ecb352427f16b78d10e50821f92701
ms.sourcegitcommit: 765b69ed451a0f48625ea597c3f39de95f3ae693
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/09/2020
ms.locfileid: "80987590"
---
# <a name="xamarinforms-label"></a>Xamarin.窗体标签

[![下载示例](~/media/shared/download.png)下载示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)

_在 Xamarin.窗体中显示文本_

该[`Label`](xref:Xamarin.Forms.Label)视图用于显示单行和多行文本。 标签可以具有文本装饰、彩色文本和使用自定义字体（族、大小和选项）。

## <a name="text-decorations"></a>文本装饰

通过将属性设置为一个或多个[`Label`](xref:Xamarin.Forms.Label)`Label.TextDecorations``TextDecorations`枚举成员，可以将下划线和击线文本修饰应用于实例：

- `None`
- `Underline`
- `Strikethrough`

以下 XAML 示例演示了`Label.TextDecorations`设置属性：

```xaml
<Label Text="This is underlined text." TextDecorations="Underline"  />
<Label Text="This is text with strikethrough." TextDecorations="Strikethrough" />
<Label Text="This is underlined text with strikethrough." TextDecorations="Underline, Strikethrough" />
```

等效 C# 代码如下：

```csharp
var underlineLabel = new Label { Text = "This is underlined text.", TextDecorations = TextDecorations.Underline };
var strikethroughLabel = new Label { Text = "This is text with strikethrough.", TextDecorations = TextDecorations.Strikethrough };
var bothLabel = new Label { Text = "This is underlined text with strikethrough.", TextDecorations = TextDecorations.Underline | TextDecorations.Strikethrough };
```

以下屏幕截图显示了应用于`TextDecorations`[`Label`](xref:Xamarin.Forms.Label)实例的枚举成员：

![带有文本装饰的标签](label-images/label-textdecorations.png)

> [!NOTE]
> 文本修饰也可以应用于[`Span`](xref:Xamarin.Forms.Span)实例。 有关该类的详细信息，`Span`请参阅[格式化文本](#Formatted_Text)。

## <a name="character-spacing"></a>字符间距

字符间距可以通过将[`Label`](xref:Xamarin.Forms.Label)`Label.CharacterSpacing`属性设置为`double`值应用于实例：

```xaml
<Label Text="Character spaced text"
       CharacterSpacing="10" />
```

等效 C# 代码如下：

```csharp
Label label = new Label { Text = "Character spaced text", CharacterSpacing = 10 };
```

结果是，文本[`Label`](xref:Xamarin.Forms.Label)中显示的字符与间隔`CharacterSpacing`设备无关的单位。

## <a name="new-lines"></a>新行

从 XAML 将文本[`Label`](xref:Xamarin.Forms.Label)强制到新行有两种主要技术：

1. 使用单码换行符，即"&#10;"。
1. 使用*属性元素*语法指定文本。

以下代码显示了这两种技术的示例：

```xaml
<!-- Unicode line feed character -->
<Label Text="First line &#10; Second line" />

<!-- Property element syntax -->
<Label>
    <Label.Text>
        First line
        Second line
    </Label.Text>
</Label>
```

在 C# 中，文本可以强制到具有"\n"字符的新行上：

```csharp
Label label = new Label { Text = "First line\nSecond line" };
```

## <a name="colors"></a>颜色

标签可以通过可[`TextColor`](xref:Xamarin.Forms.Label.TextColor)绑定属性设置为使用自定义文本颜色。

为确保每个平台上的颜色可用，需要特别注意。 由于每个平台的文本和背景颜色都有不同的默认值，因此您需要小心选择适用于每个平台的默认值。

以下 XAML 示例设置 的文本`Label`颜色：

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="TextSample.LabelPage"
             Title="Label Demo">
    <StackLayout Padding="5,10">
      <Label TextColor="#77d065" FontSize = "20" Text="This is a green label." />
    </StackLayout>
</ContentPage>
```

等效 C# 代码如下：

```csharp
public partial class LabelPage : ContentPage
{
    public LabelPage ()
    {
        InitializeComponent ();

        var layout = new StackLayout { Padding = new Thickness(5,10) };
        var label = new Label { Text="This is a green label.", TextColor = Color.FromHex("#77d065"), FontSize = 20 };
        layout.Children.Add(label);
        this.Content = layout;
    }
}
```

以下屏幕截图显示了设置`TextColor`属性的结果：

![标签文本颜色示例](label-images/textcolor.png)

有关颜色的详细信息，请参阅[颜色](~/xamarin-forms/user-interface/colors.md)。

## <a name="fonts"></a>字体

有关在 上指定字体的详细信息`Label`，请参阅[字体](~/xamarin-forms/user-interface/text/fonts.md)。

<a name="Truncation_and_Wrapping" />

## <a name="truncation-and-wrapping"></a>截断和包装

可以设置标签以处理无法以多种方式之一（由`LineBreakMode`属性公开）在一行上拟合的文本。 [`LineBreakMode`](xref:Xamarin.Forms.LineBreakMode)是具有以下值的枚举：

- **头截**&ndash;断截断文本的头部，显示结尾。
- **字符包装**&ndash;在字符边界处将文本包装到新行上。
- **中间截图**&ndash;显示文本的开头和结尾，中间以省略号替换。
- **NoWrap**&ndash;不会换行文本，只显示一行可以容纳的文本。
- **尾截条**&ndash;显示文本的开头，截断末尾。
- **WordWrap**&ndash;在单词边界处包装文本。

## <a name="display-a-specific-number-of-lines"></a>显示特定数量的行

通过将`Label.MaxLines`属性设置为`int`值，可以指定[`Label`](xref:Xamarin.Forms.Label)由 显示的行数：

- 当`MaxLines`为 -1（其默认值）时，`Label`属性的值仅[`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode)显示一行（可能截断）或包含所有文本的所有行。
- 当`MaxLines`为 0`Label`时，不显示。
- 当`MaxLines`为 1 时，结果与将[`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode)[`NoWrap`](xref:Xamarin.Forms.LineBreakMode)属性设置为 时[`HeadTruncation`](xref:Xamarin.Forms.LineBreakMode)相同[`MiddleTruncation`](xref:Xamarin.Forms.LineBreakMode)，[`TailTruncation`](xref:Xamarin.Forms.LineBreakMode)或 。 但是，如果`Label`适用，将尊重[`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode)财产在放置椭圆时的价值。
- 当`MaxLines`大于 1 时，`Label`将显示最多为指定数量的行数，同时尊重[`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode)属性相对于椭圆线放置的值（如果适用）。 但是，`MaxLines`如果[`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode)属性设置为[`NoWrap`](xref:Xamarin.Forms.LineBreakMode)，则将属性设置为 大于 1 的值不起作用。

下面的 XAML 示例演示了`MaxLines`在 上[`Label`](xref:Xamarin.Forms.Label)设置 属性：

```xaml
<Label Text="Lorem ipsum dolor sit amet, consectetur adipiscing elit. In facilisis nulla eu felis fringilla vulputate. Nullam porta eleifend lacinia. Donec at iaculis tellus."
       LineBreakMode="WordWrap"
       MaxLines="2" />
```

等效 C# 代码如下：

```csharp
var label =
{
  Text = "Lorem ipsum dolor sit amet, consectetur adipiscing elit. In facilisis nulla eu felis fringilla vulputate. Nullam porta eleifend lacinia. Donec at iaculis tellus.", LineBreakMode = LineBreakMode.WordWrap,
  MaxLines = 2
};
```

以下屏幕截图显示将`MaxLines`属性设置为 2 的结果，当文本足够长到超过 2 行时：

![标签最大值线示例](label-images/label-maxlines.png)

## <a name="display-html"></a>显示 HTML

类[`Label`](xref:Xamarin.Forms.Label)具有一个`TextType`属性，该属性确定`Label`实例应显示纯文本还是 HTML 文本。 此属性应设置为`TextType`枚举的成员之一：

- `Text`表示 将显示`Label`纯文本，并且是属性的`Label.TextType`默认值。
- `Html`表示 将显示`Label`HTML 文本。

因此，[`Label`](xref:Xamarin.Forms.Label)实例可以通过将`Label.TextType`属性设置为 和 将`Html``Label.Text`属性设置为 HTML 字符串来显示 HTML：

```csharp
Label label = new Label
{
    Text = "This is <strong style=\"color:red\">HTML</strong> text.",
    TextType = TextType.Html
};
```

在上面的示例中，必须使用`\`符号转义 HTML 中的双引号字符。

在 XAML 中，由于额外转义 和`<``>`符号，HTML 字符串可能会变得不可读：

```xaml
<Label Text="This is &lt;strong style=&quot;color:red&quot;&gt;HTML&lt;/strong&gt; text."
       TextType="Html"  />
```

或者，为了获得更高的可读性，可以在`CDATA`一节中内联 HTML：

```xaml
<Label TextType="Html">
    <![CDATA[
    This is <strong style="color:red">HTML</strong> text.
    ]]>
</Label>
```

在此示例中，该`Label.Text`属性设置为`CDATA`节中内联的 HTML 字符串。 这之所以有效，`Text`是因为属性是`ContentProperty`类`Label`的 。

以下屏幕截图显示[`Label`](xref:Xamarin.Forms.Label)显示 HTML：

![在 iOS 和 Android 上显示 HTML 的标签屏幕截图](label-images/label-html.png)

> [!IMPORTANT]
> 在 中[`Label`](xref:Xamarin.Forms.Label)显示 HTML 仅限于基础平台支持的 HTML 标记。

<a name="Formatted_Text" />

## <a name="formatted-text"></a>带格式文本

标签公开允许[`FormattedText`](xref:Xamarin.Forms.Label.FormattedText)在同一视图中显示具有多种字体和颜色的文本的属性。

属性`FormattedText`的类型为[`FormattedString`](xref:Xamarin.Forms.FormattedString)，它包括一个或多个[`Span`](xref:Xamarin.Forms.Span)实例，通过 属性[`Spans`](xref:Xamarin.Forms.FormattedString.Spans)设置。 以下`Span`属性可用于设置可视外观：

- [`BackgroundColor`](xref:Xamarin.Forms.Span.BackgroundColor)• 跨距背景的颜色。
- `CharacterSpacing`，属于 `double` 类型，是 `Span` 文本字符之间的间距。
- [`Font`](xref:Xamarin.Forms.Span.Font)• 范围中文本的字体。
- [`FontAttributes`](xref:Xamarin.Forms.Span.FontAttributes)• 范围中文本的字体属性。
- [`FontFamily`](xref:Xamarin.Forms.Span.FontFamily)• 范围中文本的字体所属的字体系列。
- [`FontSize`](xref:Xamarin.Forms.Span.FontSize)• 范围中文本的字体大小。
- [`ForegroundColor`](xref:Xamarin.Forms.Span.ForegroundColor)• 范围中文本的颜色。 此属性已过时，已替换为 该`TextColor`属性。
- [`LineHeight`](xref:Xamarin.Forms.Span.LineHeight)- 应用于跨度的默认线高度的乘数。 有关详细信息，请参阅[线高度](#line-height)。
- [`Style`](xref:Xamarin.Forms.Span.Style)• 要应用于范围的样式。
- [`Text`](xref:Xamarin.Forms.Span.Text)• 范围的文本。
- [`TextColor`](xref:Xamarin.Forms.Span.TextColor)• 范围中文本的颜色。
- `TextDecorations`- 要应用于范围中文本的装饰。 有关详细信息，请参阅[文本装饰](#text-decorations)。

和[`BackgroundColor`](xref:Xamarin.Forms.Span.BackgroundColor)[`Text`](xref:Xamarin.Forms.Span.Text)可[`Text`](xref:Xamarin.Forms.Span.Text)绑定属性具有[`OneWay`](xref:Xamarin.Forms.BindingMode)默认绑定模式。 有关此绑定模式的详细信息，请参阅[绑定模式](~/xamarin-forms/app-fundamentals/data-binding/binding-mode.md)指南中的[默认绑定模式](~/xamarin-forms/app-fundamentals/data-binding/binding-mode.md#the-default-binding-mode)。

此外，该[`GestureRecognizers`](xref:Xamarin.Forms.GestureElement.GestureRecognizers)属性可用于定义将响应 上手势的手势识别器的集合[`Span`](xref:Xamarin.Forms.Span)。

> [!NOTE]
> 无法在 中[`Span`](xref:Xamarin.Forms.Span)显示 HTML。

下面的 XAML 示例演示`FormattedText`了由三[`Span`](xref:Xamarin.Forms.Span)个实例组成的属性：

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="TextSample.LabelPage"
             Title="Label Demo - XAML">
    <StackLayout Padding="5,10">
        ...
        <Label LineBreakMode="WordWrap">
            <Label.FormattedText>
                <FormattedString>
                    <Span Text="Red Bold, " TextColor="Red" FontAttributes="Bold" />
                    <Span Text="default, " Style="{DynamicResource BodyStyle}">
                        <Span.GestureRecognizers>
                            <TapGestureRecognizer Command="{Binding TapCommand}" />
                        </Span.GestureRecognizers>
                    </Span>
                    <Span Text="italic small." FontAttributes="Italic" FontSize="Small" />
                </FormattedString>
            </Label.FormattedText>
        </Label>
    </StackLayout>
</ContentPage>
```

等效 C# 代码如下：

```csharp
public class LabelPageCode : ContentPage
{
    public LabelPageCode ()
    {
        var layout = new StackLayout{ Padding = new Thickness (5, 10) };
        ...
        var formattedString = new FormattedString ();
        formattedString.Spans.Add (new Span{ Text = "Red bold, ", ForegroundColor = Color.Red, FontAttributes = FontAttributes.Bold });

        var span = new Span { Text = "default, " };
        span.GestureRecognizers.Add(new TapGestureRecognizer { Command = new Command(async () => await DisplayAlert("Tapped", "This is a tapped Span.", "OK")) });
        formattedString.Spans.Add(span);
        formattedString.Spans.Add (new Span { Text = "italic small.", FontAttributes = FontAttributes.Italic, FontSize =  Device.GetNamedSize(NamedSize.Small, typeof(Label)) });

        layout.Children.Add (new Label { FormattedText = formattedString });
        this.Content = layout;
    }
}
```

> [!IMPORTANT]
> 可通过[`Text`](xref:Xamarin.Forms.Span.Text)数据绑定设置`Span`属性。 有关详细信息，请参阅[数据绑定](~/xamarin-forms/app-fundamentals/data-binding/index.md)。

请注意，还可以[`Span`](xref:Xamarin.Forms.Span)响应添加到 span[`GestureRecognizers`](xref:Xamarin.Forms.GestureElement.GestureRecognizers)集合的任何手势。 例如，在上述[`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer)代码示例中，已添加到第`Span`二个代码示例中。 因此，当点击`Span`时，`TapGestureRecognizer`将通过执行`ICommand`[`Command`](xref:Xamarin.Forms.TapGestureRecognizer.Command)属性定义的将响应。 有关手势识别器的详细信息，请参阅[Xamarin.窗体手势](~/xamarin-forms/app-fundamentals/gestures/index.md)。

以下屏幕截图显示了将`FormattedString`属性设置为三个`Span`实例的结果：

![标签格式化文本示例](label-images/formattedtext.png)

## <a name="line-height"></a>行高

可以通过[`Label`](xref:Xamarin.Forms.Label)设置[`Span`](xref:Xamarin.Forms.Span)[`Label.LineHeight`](xref:Xamarin.Forms.Label.LineHeight)属性或[`Span.LineHeight`](xref:Xamarin.Forms.Span.LineHeight)`double`值来自定义 和 的垂直高度。 在 iOS 和 Android 上，这些值是原始行高度的乘数，在通用 Windows 平台`Label.LineHeight`（UWP） 上，属性值是标签字体大小的乘数。

> [!NOTE]
>
> - 在 iOS[`Label.LineHeight`](xref:Xamarin.Forms.Label.LineHeight)上[`Span.LineHeight`](xref:Xamarin.Forms.Span.LineHeight)，和 属性更改适合单行的文本的行高度，以及环绕到多行的文本。
> - 在 Android[`Label.LineHeight`](xref:Xamarin.Forms.Label.LineHeight)上[`Span.LineHeight`](xref:Xamarin.Forms.Span.LineHeight)， 和 属性仅更改环绕到多行的文本的行高。
> - 在 UWP[`Label.LineHeight`](xref:Xamarin.Forms.Label.LineHeight)上，该属性更改环绕到多行的文本的行高度，[`Span.LineHeight`](xref:Xamarin.Forms.Span.LineHeight)并且该属性不起作用。

下面的 XAML 示例演示了[`LineHeight`](xref:Xamarin.Forms.Label.LineHeight)在 上[`Label`](xref:Xamarin.Forms.Label)设置 属性：

```xaml
<Label Text="Lorem ipsum dolor sit amet, consectetur adipiscing elit. In facilisis nulla eu felis fringilla vulputate. Nullam porta eleifend lacinia. Donec at iaculis tellus."
       LineBreakMode="WordWrap"
       LineHeight="1.8" />
```

等效 C# 代码如下：

```csharp
var label =
{
  Text = "Lorem ipsum dolor sit amet, consectetur adipiscing elit. In facilisis nulla eu felis fringilla vulputate. Nullam porta eleifend lacinia. Donec at iaculis tellus.", LineBreakMode = LineBreakMode.WordWrap,
  LineHeight = 1.8
};
```

以下屏幕截图显示了将[`Label.LineHeight`](xref:Xamarin.Forms.Label.LineHeight)属性设置为 1.8 的结果：

![标签线高度示例](label-images/label-lineheight.png)

下面的 XAML 示例演示了[`LineHeight`](xref:Xamarin.Forms.Span.LineHeight)在 上[`Span`](xref:Xamarin.Forms.Span)设置 属性：

```xaml
<Label LineBreakMode="WordWrap">
    <Label.FormattedText>
        <FormattedString>
            <Span Text="Lorem ipsum dolor sit amet, consectetur adipiscing elit. In a tincidunt sem. Phasellus mollis sit amet turpis in rutrum. Sed aliquam ac urna id scelerisque. "
                  LineHeight="1.8"/>
            <Span Text="Nullam feugiat sodales elit, et maximus nibh vulputate id."
                  LineHeight="1.8" />
        </FormattedString>
    </Label.FormattedText>
</Label>
```

等效 C# 代码如下：

```csharp
var formattedString = new FormattedString();
formattedString.Spans.Add(new Span
{
  Text = "Lorem ipsum dolor sit amet, consectetur adipiscing elit. In a tincidunt sem. Phasellus mollis sit amet turpis in rutrum. Sed aliquam ac urna id scelerisque. ",
  LineHeight = 1.8
});
formattedString.Spans.Add(new Span
{
  Text = "Nullam feugiat sodales elit, et maximus nibh vulputate id.",
  LineHeight = 1.8
});
var label = new Label
{
  FormattedText = formattedString,
  LineBreakMode = LineBreakMode.WordWrap
};
```

以下屏幕截图显示了将[`Span.LineHeight`](xref:Xamarin.Forms.Span.LineHeight)属性设置为 1.8 的结果：

![跨线高度示例](label-images/span-lineheight.png)

## <a name="padding"></a>填充

填充表示元素与其子元素之间的空间，用于将元素与其自己的内容分开。 可以通过将`Label.Padding`属性设置为[`Thickness`](xref:Xamarin.Forms.Thickness)值应用于[`Label`](xref:Xamarin.Forms.Label)实例：

```xaml
<Label Padding="10">
    <Label.FormattedText>
        <FormattedString>
            <Span Text="Lorem ipsum" />
            <Span Text="dolor sit amet." />
        </FormattedString>
    </Label.FormattedText>
</Label>
```

等效 C# 代码如下：

```csharp
FormattedString formattedString = new FormattedString();
formattedString.Spans.Add(new Span
{
  Text = "Lorem ipsum"
});
formattedString.Spans.Add(new Span
{
  Text = "dolor sit amet."
});
Label label = new Label
{
    FormattedText = formattedString,
    Padding = new Thickness(20)
};
```

> [!IMPORTANT]
> 在 iOS 上[`Label`](xref:Xamarin.Forms.Label)，当创建设置属性`Padding`的 时，将应用填充，以后可以更新填充值。 但是，当创建`Label`未设置`Padding`该属性的 时，尝试以后设置该属性将不起作用。
>
> 在 Android 和通用 Windows`Padding`平台上，可以在创建 或更高`Label`版本时指定属性值。

有关填充的详细信息，请参阅[边距和填充](~/xamarin-forms/user-interface/layouts/margin-and-padding.md)。

## <a name="hyperlinks"></a>超链接

通过以下方法，[`Label`](xref:Xamarin.Forms.Label)显示[`Span`](xref:Xamarin.Forms.Span)和实例的文本可以转换为超链接：

1. 设置`TextColor`或`TextDecoration`[`Label`](xref:Xamarin.Forms.Label)[`Span`](xref:Xamarin.Forms.Span)的 和 属性。
1. 将[`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer)添加到[`GestureRecognizers`](xref:Xamarin.Forms.GestureElement.GestureRecognizers)[`Label`](xref:Xamarin.Forms.Label)的 集合[`Span`](xref:Xamarin.Forms.Span)中，[`Command`](xref:Xamarin.Forms.TapGestureRecognizer.Command)的属性绑定到`ICommand`，[`CommandParameter`](xref:Xamarin.Forms.TapGestureRecognizer.CommandParameter)其属性包含要打开的 URL。
1. `ICommand`定义将由 执行 的[`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer)。
1. 编写将由 执行的代码`ICommand`。

以下代码示例取自[超链接演示](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-hyperlinks/)示例，显示了其内容从多个[`Label`](xref:Xamarin.Forms.Label)[`Span`](xref:Xamarin.Forms.Span)实例设置的 。

```xaml
<Label>
    <Label.FormattedText>
        <FormattedString>
            <Span Text="Alternatively, click " />
            <Span Text="here"
                  TextColor="Blue"
                  TextDecorations="Underline">
                <Span.GestureRecognizers>
                    <TapGestureRecognizer Command="{Binding TapCommand}"
                                          CommandParameter="https://docs.microsoft.com/xamarin/" />
                </Span.GestureRecognizers>
            </Span>
            <Span Text=" to view Xamarin documentation." />
        </FormattedString>
    </Label.FormattedText>
</Label>
```

在此示例中，第一个和第三[`Span`](xref:Xamarin.Forms.Span)个实例包括文本，而第二`Span`个实例表示可评估的超链接。 它的文本颜色设置为蓝色，并且具有下划线文本修饰。 这将创建超链接的外观，如以下屏幕截图所示：

[![超链接](label-images/hyperlinks-small.png "超链接")](label-images/hyperlinks-large.png#lightbox)

当点击超链接时，[`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer)将通过执行其`ICommand`[`Command`](xref:Xamarin.Forms.TapGestureRecognizer.Command)属性定义的来响应。 此外，[`CommandParameter`](xref:Xamarin.Forms.TapGestureRecognizer.CommandParameter)属性指定的 URL 将作为参数传递给 。 `ICommand`

XAML 页面的代码后面包含实现`TapCommand`：

```csharp
public partial class MainPage : ContentPage
{
    // Launcher.OpenAsync is provided by Xamarin.Essentials.
    public ICommand TapCommand => new Command<string>(async (url) => await Launcher.OpenAsync(url));

    public MainPage()
    {
        InitializeComponent();
        BindingContext = this;
    }
}
```

`TapCommand`执行`Launcher.OpenAsync`方法，将[`TapGestureRecognizer.CommandParameter`](xref:Xamarin.Forms.TapGestureRecognizer.CommandParameter)属性值作为参数传递。 该方法`Launcher.OpenAsync`由 Xamarin.Essentials 提供，并在 Web 浏览器中打开 URL。 因此，总体效果是，当在页面上点击超链接时，将显示一个 Web 浏览器，并将与超链接关联的 URL 导航到。

### <a name="creating-a-reusable-hyperlink-class"></a>创建可重用的超链接类

创建超链接的上一种方法要求每次应用程序中需要超链接时编写重复代码。 但是，可以对[`Label`](xref:Xamarin.Forms.Label)[`Span`](xref:Xamarin.Forms.Span)和 类进行子类化以创建`HyperlinkLabel`和`HyperlinkSpan`类，并添加手势识别器和文本格式代码。

以下代码示例取自[超链接演示](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-hyperlinks/)示例，显示了一个`HyperlinkSpan`类：

```csharp
public class HyperlinkSpan : Span
{
    public static readonly BindableProperty UrlProperty =
        BindableProperty.Create(nameof(Url), typeof(string), typeof(HyperlinkSpan), null);

    public string Url
    {
        get { return (string)GetValue(UrlProperty); }
        set { SetValue(UrlProperty, value); }
    }

    public HyperlinkSpan()
    {
        TextDecorations = TextDecorations.Underline;
        TextColor = Color.Blue;
        GestureRecognizers.Add(new TapGestureRecognizer
        {
            // Launcher.OpenAsync is provided by Xamarin.Essentials.
            Command = new Command(async () => await Launcher.OpenAsync(Url))
        });
    }
}
```

类`HyperlinkSpan`定义属性`Url`和关联的[`BindableProperty`](xref:Xamarin.Forms.BindableProperty)，构造函数设置超链接外观，并在点击超链接时将响应的[`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer)。 `HyperlinkSpan`当被点击时，`TapGestureRecognizer`将通过执行`Launcher.OpenAsync`方法来响应在 Web 浏览器中打开`Url`由属性指定的 URL。

可以通过`HyperlinkSpan`将类的实例添加到 XAML 来使用该类：

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:HyperlinkDemo"
             x:Class="HyperlinkDemo.MainPage">
    <StackLayout>
        ...
        <Label>
            <Label.FormattedText>
                <FormattedString>
                    <Span Text="Alternatively, click " />
                    <local:HyperlinkSpan Text="here"
                                         Url="https://docs.microsoft.com/appcenter/" />
                    <Span Text=" to view AppCenter documentation." />
                </FormattedString>
            </Label.FormattedText>
        </Label>
    </StackLayout>
</ContentPage>
```

## <a name="styling-labels"></a>样式标签

前面的各节按实例[`Label`](xref:Xamarin.Forms.Label)介绍[`Span`](xref:Xamarin.Forms.Span)设置和属性。 但是，属性集可以分组到一个样式中，该样式始终应用于一个或多个视图。 这可以提高代码的可读性，并使设计更改更易于实现。 有关详细信息，请参阅[样式](~/xamarin-forms/user-interface/text/styles.md)。

## <a name="related-links"></a>相关链接

- [文本（示例）](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)
- [超链接（示例）](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-hyperlinks)
- [使用 Xamarin.窗体创建移动应用程序，第 3 章](https://developer.xamarin.com/r/xamarin-forms/book/chapter03.pdf)
- [标签 API](xref:Xamarin.Forms.Label)
- [跨 API](xref:Xamarin.Forms.Span)
