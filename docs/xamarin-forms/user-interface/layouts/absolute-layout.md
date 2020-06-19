---
title: Xamarin.FormsAbsoluteLayout
description: 本文介绍如何使用 Xamarin.Forms AbsoluteLayout 类创建完全像素的 ui。 此类根据子元素的大小和位置或绝对值来定位和调整其大小。
ms.prod: xamarin
ms.assetid: 01A5CCE0-AD45-4806-84FD-72C007005B38
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/25/2015
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 110b01d6482fbe3e23a772c90194b6bf40d54877
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/18/2020
ms.locfileid: "84570526"
---
# <a name="xamarinforms-absolutelayout"></a>Xamarin.FormsAbsoluteLayout

[![下载示例](~/media/shared/download.png) 下载示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)

[`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout)定位子元素并调整其大小，使其与自己的大小和位置或绝对值成正比。 可以使用比例值或静态值定位和调整子视图的大小，并且可以混合使用比例和静态值。

[![](absolute-layout-images/layouts-sml.png "Xamarin.Forms Layouts")](absolute-layout-images/layouts.png#lightbox "Xamarin.Forms Layouts")

本文介绍：

- **[目的](#purpose)** &ndash;的常见用途 `AbsoluteLayout` 。
- **[使用情况](#usage)** &ndash;如何使用 `AbsoluteLayout` 来实现所需的设计。
  - **[比例布局](#proportional-layouts)** &ndash;了解中的比例值如何工作 `AbsoluteLayout` 。
  - **[指定值](#specifying-values)** &ndash;了解如何指定比例值和绝对值。
  - **[比例值](#proportional-values)** &ndash;了解比例值的工作方式。
    - **[绝对值](#absolute-values)** &ndash;了解绝对值的工作方式。

## <a name="purpose"></a>目的

由于的定位模型 `AbsoluteLayout` ，布局使元素定位相对简单，使其与布局的任何一侧或中间部分齐平。 在按比例调整大小和位置之后，中的元素 `AbsoluteLayout` 可自动缩放到任何视图大小。 对于只有位置但不应进行缩放的项，可以混合绝对值和比例值。

`AbsoluteLayout`可用于任何需要在视图中定位的元素，在将元素对齐到边缘时特别有用。

## <a name="usage"></a>用法

### <a name="proportional-layouts"></a>比例布局

`AbsoluteLayout`具有唯一的定位点模型，在使用按比例定位时，元素定位点相对于其元素定位。 使用绝对定位时，定位点位于视图内（0，0）。 这有两个重要的后果：

- 元素不能使用比例值定位在屏幕上。
- 无论布局或设备的大小如何，元素都可以可靠地放置在布局的任何一侧或中心。

`AbsoluteLayout`像一样 `RelativeLayout` ，可以定位元素，使其重叠。

请注意，在下面的屏幕截图中，框的定位点为白点。 请注意，定位点和框在整个布局中移动时的关系：

![](absolute-layout-images/anchor-start.png "Anchor at Start")
![](absolute-layout-images/anchor-center.png "Anchor at Center")
![](absolute-layout-images/anchor-end.png "Anchor at End")

### <a name="specifying-values"></a>指定值

`AbsoluteLayout`使用四个值定位中的视图：

- **X** &ndash; 视图定位点的 x （水平）位置
- **Y** &ndash; 视图定位点的 y （垂直）位置
- **宽度** &ndash;视图的宽度
- **高度** &ndash;视图的高度

这些值中的每一个都可以设置为[成比例](#proportional-values)值或[绝对值](#absolute-values)。

值指定为界限和标志的组合。 `LayoutBounds`由 [`Rectangle`](xref:Xamarin.Forms.Rectangle) 四个值组成： `x` 、 `y` 、 `width` 和 `height` 。

### <a name="absolutelayoutflags"></a>AbsoluteLayoutFlags

[`AbsoluteLayoutFlags`](xref:Xamarin.Forms.AbsoluteLayoutFlags)指定如何解释值并具有以下预定义选项：

- **无** &ndash;将所有值解释为绝对值。 如果未指定布局标志，则这是默认值。
- **全部** &ndash;将所有值视为比例值。
- **WidthProportional** &ndash;将 `Width` 值解释为按比例和所有其他值作为绝对值。
- **HeightProportional** &ndash;仅将高度值解释为与所有其他值为绝对比例。
- **XProportional** &ndash;将 `X` 值视为比例，同时将所有其他值视为绝对值。
- **YProportional** &ndash;将 `Y` 值视为比例，同时将所有其他值视为绝对值。
- **PositionProportional** &ndash;将 `X` 和 `Y` 值解释为成比例，而大小值解释为绝对。
- **SizeProportional** &ndash;将 `Width` 和 `Height` 值解释为比例，而位置值是绝对的。

在 XAML 中，使用属性将边界和标志设置为布局中的视图定义的一部分 `AbsoluteLayout.LayoutBounds` 。 界限设置为以逗号分隔的值列表，按 `X` `Y` `Width` `Height` 顺序排列。 还可以使用属性在布局中的视图声明中指定标志 `AbsoluteLayout.LayoutFlags` 。 请注意，可以使用以逗号分隔的列表将标志组合到 XAML 中。 请考虑以下示例：

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="LayoutSamples.AbsoluteLayoutExploration"
Title="Absolute Layout Exploration">
    <ContentPage.Content>
        <AbsoluteLayout>
            <Label Text="I'm centered on iPhone 4 but no other device"
                AbsoluteLayout.LayoutBounds="115,150,100,100" LineBreakMode="WordWrap"  />
            <Label Text="I'm bottom center on every device."
                AbsoluteLayout.LayoutBounds=".5,1,.5,.1" AbsoluteLayout.LayoutFlags="All"
                LineBreakMode="WordWrap"  />
            <BoxView Color="Olive"  AbsoluteLayout.LayoutBounds="1,.5, 25, 100"
                AbsoluteLayout.LayoutFlags="PositionProportional" />
            <BoxView Color="Red" AbsoluteLayout.LayoutBounds="0,.5,25,100"
                AbsoluteLayout.LayoutFlags="PositionProportional" />
            <BoxView Color="Blue" AbsoluteLayout.LayoutBounds=".5,0,100,25"
                AbsoluteLayout.LayoutFlags="PositionProportional" />
        </AbsoluteLayout>
    </ContentPage.Content>
</ContentPage>
```

![](absolute-layout-images/exploration.png "AbsoluteLayout Examples")

注意以下事项：

- 使用绝对大小和位置值定位中心中的标签。 因此，它显示在 iPhone 4S 或更低的中心，但不会在大型设备上居中。
- 布局底部的文本使用比例大小和位置值定位。 它将始终显示在布局的底部中心，但其大小会随较大的布局大小增长。
- 三个彩色 `BoxView` 的分别使用比例位置和绝对大小定位在屏幕的上边缘、左边缘和右边缘。

下面实现了 c # 中的相同布局：

```csharp
public class AbsoluteLayoutExplorationCode : ContentPage
{
    public AbsoluteLayoutExplorationCode ()
    {
        Title = "Absolute Layout Exploration - Code";
        var layout = new AbsoluteLayout();

        var centerLabel = new Label {
        Text = "I'm centered on iPhone 4 but no other device.",
        LineBreakMode = LineBreakMode.WordWrap};

        AbsoluteLayout.SetLayoutBounds (centerLabel, new Rectangle (115, 159, 100, 100));
        // No need to set layout flags, absolute positioning is the default

        var bottomLabel = new Label { Text = "I'm bottom center on every device.", LineBreakMode = LineBreakMode.WordWrap };
        AbsoluteLayout.SetLayoutBounds (bottomLabel, new Rectangle (.5, 1, .5, .1));
        AbsoluteLayout.SetLayoutFlags (bottomLabel, AbsoluteLayoutFlags.All);

        var rightBox = new BoxView{ Color = Color.Olive };
        AbsoluteLayout.SetLayoutBounds (rightBox, new Rectangle (1, .5, 25, 100));
        AbsoluteLayout.SetLayoutFlags (rightBox, AbsoluteLayoutFlags.PositionProportional);

        var leftBox = new BoxView{ Color = Color.Red };
        AbsoluteLayout.SetLayoutBounds (leftBox, new Rectangle (0, .5, 25, 100));
        AbsoluteLayout.SetLayoutFlags (leftBox, AbsoluteLayoutFlags.PositionProportional);

        var topBox = new BoxView{ Color = Color.Blue };
        AbsoluteLayout.SetLayoutBounds (topBox, new Rectangle (.5, 0, 100, 25));
        AbsoluteLayout.SetLayoutFlags (topBox, AbsoluteLayoutFlags.PositionProportional);

        layout.Children.Add (bottomLabel);
        layout.Children.Add (centerLabel);
        layout.Children.Add (rightBox);
        layout.Children.Add (leftBox);
        layout.Children.Add (topBox);

        Content = layout;
    }
}
```

### <a name="proportional-values"></a>比例值

比例值定义了布局和视图之间的关系。 此关系将子视图的位置或刻度值定义为父布局的相应值的比例值。 这些值用 `double` 0 到1之间的值表示。

比例值用于在布局中定位和调整视图的大小。 因此，当视图的宽度设置为比例时，生成的宽度值将与 `AbsoluteLayout` 的宽度相乘。 例如，如果将 `AbsoluteLayout` width `500` 和 view 设置为的比例为0.5，则视图呈现的宽度将为250（500 x. 5）。

若要使用比例值，请 `LayoutBounds` 使用（x，y）比例和比例大小设置，然后将设置 `LayoutFlags` 为 `All` 。

在 XAML 中：

```xaml
<Label Text="I'm bottom center on every device."
  AbsoluteLayout.LayoutBounds=".5,1,.5,.1" AbsoluteLayout.LayoutFlags="All"  />
```

在 C# 中：

```csharp
var label = new Label {Text = "I'm bottom center on every device."};
AbsoluteLayout.SetLayoutBounds(label, new Rectangle(.5,1,.5,.1));
AbsoluteLayout.SetLayoutFlags(label, AbsoluteLayoutFlags.All);
```

### <a name="absolute-values"></a>绝对值

绝对值显式定义视图在布局中的定位位置。 与比例值不同的是，绝对值可以定位并调整不适合布局边界的视图的大小。

如果布局的大小未知，则使用绝对值进行定位可能会很危险。 使用绝对位置时，屏幕中心的一个元素会以任何其他大小进行偏移。 在支持的设备的各种屏幕尺寸上测试你的应用程序很重要。

若要使用绝对布局值，请 `LayoutBounds` 使用（x，y）坐标和显式大小设置，然后将设置 `LayoutFlags` 为 `None` 。

在 XAML 中：

```xaml
<Label Text="I'm centered on iPhone 4 but no other device."
  AbsoluteLayout.LayoutBounds="115,150,100,100"  />
```

在 C# 中：

```csharp
var label = new Label {Text = "I'm centered on iPhone 4 but no other device."};
AbsoluteLayout.SetLayoutBounds(label, new Rectangle(115,150,100,100));
```

## <a name="exploring-a-complex-layout"></a>探索复杂布局

每个布局都具有用于创建特定布局的优点和缺点。 在此系列布局文章中，已创建了一个使用三个不同布局实现的相同页面布局的示例应用。

请考虑以下 XAML：

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="TheBusinessTumble.AbsoluteLayoutPage"
Title="AbsoluteLayout">
    <ContentPage.ToolbarItems>
        <ToolbarItem Text="Save" />
    </ContentPage.ToolbarItems>
    <ContentPage.Content>
        <ScrollView>
            <AbsoluteLayout BackgroundColor="Maroon">
                <BoxView BackgroundColor="Gray" AbsoluteLayout.LayoutBounds="0
                    0,1,100" AbsoluteLayout.LayoutFlags="XProportional,YProportional,WidthProportional" />
                <Button BackgroundColor="Maroon"
                    AbsoluteLayout.LayoutBounds=".5,55,70,70" AbsoluteLayout.LayoutFlags="XProportional" BorderRadius="35" />
                <Button BackgroundColor="Red" AbsoluteLayout.LayoutBounds=".5
                    60,60,60" AbsoluteLayout.LayoutFlags="XProportional" BorderRadius="30" />
                <Label Text="User Name" FontAttributes="Bold" FontSize="26"
                    TextColor="Black" HorizontalTextAlignment="Center" AbsoluteLayout.LayoutBounds=".5,140,1,40" AbsoluteLayout.LayoutFlags="XProportional,WidthProportional" />
                <Entry Text="Bio + Hashtags" TextColor="White"
                    BackgroundColor="Maroon" AbsoluteLayout.LayoutBounds=".5,180,1,40" AbsoluteLayout.LayoutFlags="XProportional,WidthProportional" />
                <AbsoluteLayout BackgroundColor="White"
                        AbsoluteLayout.LayoutBounds="0, 220, 1, 50" AbsoluteLayout.LayoutFlags="XProportional,WidthProportional">
                    <AbsoluteLayout AbsoluteLayout.LayoutBounds="0,0,.5,1" AbsoluteLayout.LayoutFlags="WidthProportional,HeightProportional">
                        <Button BackgroundColor="Black" BorderRadius="20"
                            AbsoluteLayout.LayoutBounds="5,.5,40,40" AbsoluteLayout.LayoutFlags="YProportional" />
                        <Label Text="Accent Color" TextColor="Black"
                            AbsoluteLayout.LayoutBounds="50,.55,1,25" AbsoluteLayout.LayoutFlags="YProportional,WidthProportional" />
                    </AbsoluteLayout>
                    <AbsoluteLayout AbsoluteLayout.LayoutBounds="1,0,.5,1" AbsoluteLayout.LayoutFlags="WidthProportional,HeightProportional,XProportional">
                        <Button BackgroundColor="Maroon" BorderRadius="20"
                            AbsoluteLayout.LayoutBounds="5,.5,40,40" AbsoluteLayout.LayoutFlags="YProportional" />
                        <Label Text="Primary Color" TextColor="Black"
                            AbsoluteLayout.LayoutBounds="50,.55,1,25" AbsoluteLayout.LayoutFlags="YProportional,WidthProportional" />
                    </AbsoluteLayout>
                </AbsoluteLayout>
                <AbsoluteLayout AbsoluteLayout.LayoutBounds="0,270,1,50" AbsoluteLayout.LayoutFlags="WidthProportional" Padding="5,0,0,0">
                    <Label Text="Age:" TextColor="White"
                        AbsoluteLayout.LayoutBounds="0,25,.25,50" AbsoluteLayout.LayoutFlags="WidthProportional" />
                    <Entry Text="35" TextColor="White" BackgroundColor="Maroon"
                        AbsoluteLayout.LayoutBounds="1,10,.75,50" AbsoluteLayout.LayoutFlags="XProportional,WidthProportional" />
                </AbsoluteLayout>
                <AbsoluteLayout AbsoluteLayout.LayoutBounds="0,320,1,50" AbsoluteLayout.LayoutFlags="WidthProportional" Padding="5,0,0,0">
                    <Label Text="Interests:" TextColor="White"
                        AbsoluteLayout.LayoutBounds="0,25,.25,50" AbsoluteLayout.LayoutFlags="WidthProportional" />
                    <Entry Text="Xamarin.Forms" TextColor="White"
                        BackgroundColor="Maroon" AbsoluteLayout.LayoutBounds="1,10,.75,50" AbsoluteLayout.LayoutFlags="XProportional,WidthProportional" />
                </AbsoluteLayout>
                <AbsoluteLayout AbsoluteLayout.LayoutBounds="0,370,1,50" AbsoluteLayout.LayoutFlags="WidthProportional" Padding="5,0,0,0">
                    <Label Text="Ask me about:" TextColor="White"
                        AbsoluteLayout.LayoutBounds="0,25,.25,50" AbsoluteLayout.LayoutFlags="WidthProportional" />
                    <Entry Text="Xamarin, C#, .NET, Mono" TextColor="White"
                        BackgroundColor="Maroon" AbsoluteLayout.LayoutBounds="1,10,.75,50" AbsoluteLayout.LayoutFlags="XProportional,WidthProportional" />
                </AbsoluteLayout>
            </AbsoluteLayout>
        </ScrollView>
    </ContentPage.Content>
</ContentPage>
```

上面的代码将生成以下布局：

![](absolute-layout-images/abs.png "Complex AbsoluteLayout")

请注意 `AbsoluteLayout` ，s 是嵌套的，因为在某些情况下，嵌套布局比在同一布局中呈现所有元素更容易。

## <a name="related-links"></a>相关链接

- [创建移动应用 Xamarin.Forms ，第14章](https://developer.xamarin.com/r/xamarin-forms/book/chapter14.pdf)
- [AbsoluteLayout](xref:Xamarin.Forms.AbsoluteLayout)
- [布局（示例）](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)
- [BusinessTumble 示例（示例）](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-businesstumble)
