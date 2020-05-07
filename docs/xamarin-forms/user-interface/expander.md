---
title: Xamarin 窗体扩展器
description: Xamarin 扩展器控件提供了一个可扩展容器来托管任何内容。 通过点击扩展器标头来显示或隐藏内容。
ms.prod: xamarin
ms.assetid: 381DCB55-522D-4414-B45B-E8DD70AA9985
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/15/2020
ms.openlocfilehash: b1e573a6070a637ef2fdfa65bb0fc1375522fc3c
ms.sourcegitcommit: 443ecd9146fe2a7bbb9b5ab6d33c835876efcf1f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/06/2020
ms.locfileid: "82852504"
---
# <a name="xamarinforms-expander"></a>Xamarin 窗体扩展器

[![下载示例](~/media/shared/download.png) 下载示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-expanderdemos/)

Xamarin `Expander`控件提供可扩展容器来托管任何内容。 控件具有标题和内容，并通过点击`Expander`标题显示或隐藏内容。 仅显示`Expander`标头时， `Expander`会*折叠*。 如果`Expander`内容可见， `Expander`则*展开*。

下面的屏幕截图显示`Expander`其折叠和展开状态，其中显示了指示标题和内容的红框：

![在 iOS 和 Android 上折叠和展开状态的扩展器的屏幕截图](expander-images/expander.png "IOS 和 Android 上的扩展器")

> [!IMPORTANT]
> `Expander`当前为试验性，只能通过设置`Expander_Experimental`标志来使用。 有关详细信息，请参阅[实验标志](~/xamarin-forms/internals/experimental-flags.md)。
>
> 此外， `Expander`控件完全在`Xamarin.Forms`命名空间中实现。 因此，它在 Xamarin 支持的所有平台上可用。

`Expander`控件定义以下属性：

- `CollapseAnimationEasing`，类型[`Easing`](xref:Xamarin.Forms.Easing)为，它表示折叠时要应用于`Expander`内容的缓动函数。
- `CollapseAnimationLength`，类型`uint`为，用于定义折叠时`Expander`动画的持续时间。 此属性的默认值为250毫秒。
- `Command`，类型`ICommand`为，在点击`Expander`标头时执行。
- `CommandParameter`，属于 `object` 类型，是传递给 `Command` 的参数。
- `Content`，类型[`View`](xref:Xamarin.Forms.View)为，用于定义在`Expander`展开时要显示的内容。
- `ContentTemplate`，类型[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)为，它是用于动态地膨胀内容的模板`Expander`。
- `ExpandAnimationEasing`，类型[`Easing`](xref:Xamarin.Forms.Easing)为，表示要在扩展期间应用于`Expander`内容的缓动函数。
- `ExpandAnimationLength`，类型`uint`为，用于定义在`Expander`展开时的动画持续时间。 此属性的默认值为250毫秒。
- `ForceUpdateSizeCommand`，类型`ICommand`为，它定义强制更新的`Expander`大小时执行的命令。 此属性使用`OneWayToSource`绑定模式。
- `Header`，类型[`View`](xref:Xamarin.Forms.View)为，用于定义标头内容。
- `IsExpanded`，类型`bool`为，确定`Expander`是否已展开。 此属性使用`TwoWay`绑定模式，其默认值为`false`。
- `Spacing`，类型`double`为，它表示标头与其内容之间的间距。 此属性的默认值为 0。
- `State`，类型`ExpanderState`为，表示的状态`Expander`。 此属性使用`OneWayToSource`绑定模式。

这些属性是由[`BindableProperty`](xref:Xamarin.Forms.BindableProperty)对象支持的，这意味着它们可以是数据绑定的目标和样式。

> [!NOTE]
> `Content`属性是`Expander`类的 content 属性，因此不需要从 XAML 显式设置。

`ExpanderState` 枚举定义下列成员：

- `Expanding`指示`Expander`正在进行扩展。
- `Expanded`指示`Expander`已展开。
- `Collapsing``Expander`指示折叠。
- `Collapsed`指示`Expander`已折叠。

该`Expander`控件还定义在`Tapped`点击`Expander`标头时触发的事件。 此外， `Expander`还包括一个`ForceUpdateSize`方法，可以调用该方法以编程方式`Expander`在运行时调整大小。

## <a name="create-an-expander"></a>创建扩展器

下面的示例演示如何`Expander`在 XAML 中实例化：

```xaml
<Expander>
    <Expander.Header>
        <Label Text="Baboon"
               FontAttributes="Bold"
               FontSize="Medium" />
    </Expander.Header>
    <Grid Padding="10">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="Auto" />
        </Grid.ColumnDefinitions>
        <Image Source="http://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/Papio_anubis_%28Serengeti%2C_2009%29.jpg/200px-Papio_anubis_%28Serengeti%2C_2009%29.jpg"
               Aspect="AspectFill"
               HeightRequest="120"
               WidthRequest="120" />
        <Label Grid.Column="1"
               Text="Baboons are African and Arabian Old World monkeys belonging to the genus Papio, part of the subfamily Cercopithecinae."
               FontAttributes="Italic" />
    </Grid>
</Expander>
```

在此示例中， `Expander`默认情况下折叠，并将[`Label`](xref:Xamarin.Forms.Label)显示为其标头。 点击标头会导致`Expander`展开以显示其内容，这是一个包含子[`Grid`](xref:Xamarin.Forms.Grid)控件的。 展开时`Expander` ，点击其标头会折叠`Expander`。

> [!IMPORTANT]
> 当隐式`Expander.Content`或显式设置属性时，将在`Expander`包含它的页被导航到时创建内容，即使`Expander`已折叠。 但是，可以`Expander.ContentTemplate`将属性设置为仅在第一次`Expander`展开时才会放大的内容。 有关详细信息，请参阅[按需创建扩展器内容](#create-expander-content-on-demand)。

或者， `Expander`可以在代码中创建：

```csharp
Expander expander = new Expander
{
    Header = new Label
    {
        Text = "Baboon",
        FontAttributes = FontAttributes.Bold,
        FontSize = Device.GetNamedSize(NamedSize.Medium, typeof(Label))
    }
};

Grid grid = new Grid
{
    Padding = new Thickness(10),
    ColumnDefinitions =
    {
        new ColumnDefinition { Width = GridLength.Auto },
        new ColumnDefinition { Width = GridLength.Auto }
    }
};

grid.Children.Add(new Image
{
    Source = "http://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/Papio_anubis_%28Serengeti%2C_2009%29.jpg/200px-Papio_anubis_%28Serengeti%2C_2009%29.jpg",
    Aspect = Aspect.AspectFill,
    HeightRequest = 120,
    WidthRequest = 120
});

grid.Children.Add(new Label
{
    Text = "Baboons are African and Arabian Old World monkeys belonging to the genus Papio, part of the subfamily Cercopithecinae.",
    FontAttributes = FontAttributes.Italic
}, 1, 0);

expander.Content = grid;
```

## <a name="create-expander-content-on-demand"></a>按需创建扩展器内容

`Expander`可以按需创建内容，以响应`Expander`展开。 这可以通过将`Expander.ContentTemplate`属性设置为[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)包含内容的来实现：

```xaml
<Expander>
    <Expander.Header>
        <Label Text="{Binding Name}"
               FontAttributes="Bold"
               FontSize="Medium" />
    </Expander.Header>
    <Expander.ContentTemplate>
        <DataTemplate>
            <Grid Padding="10">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="Auto" />
                    <ColumnDefinition Width="Auto" />
                </Grid.ColumnDefinitions>
                <Image Source="{Binding ImageUrl}"
                       Aspect="AspectFill"
                       HeightRequest="120"
                       WidthRequest="120" />
                <Label Grid.Column="1"
                       Text="{Binding Details}"
                       FontAttributes="Italic" />
            </Grid>
        </DataTemplate>
    </Expander.ContentTemplate>
</Expander>
```

在此示例中， `Expander`仅当首次`Expander`展开时才会放大内容。

此方法的优点是，当页面包含多个`Expander`对象时，仅当用户首`Expander`次展开时才会创建的内容。

## <a name="add-an-expansion-indicator"></a>添加扩展指示器

[`Image`](xref:Xamarin.Forms.Image)可以向`Expander`标头中添加，以提供展开状态的可视指示。 [`DataTrigger`](xref:Xamarin.Forms.DataTrigger)可以附加到`Image`，这会基于`Expander.IsExpanded`属性的值`Source`更改属性：

```xaml
<Expander>
    <Expander.Header>
        <Grid>
            <Label Text="{Binding Name}"
                   FontAttributes="Bold"
                   FontSize="Medium" />
            <Image Source="expand.png"
                   HorizontalOptions="End"
                   VerticalOptions="Start">
                <Image.Triggers>
                    <DataTrigger TargetType="Image"
                                 Binding="{Binding Source={RelativeSource AncestorType={x:Type Expander}}, Path=IsExpanded}"
                                 Value="True">
                        <Setter Property="Source"
                                Value="collapse.png" />
                    </DataTrigger>
                </Image.Triggers>
            </Image>
        </Grid>
    </Expander.Header>
    <Expander.ContentTemplate>
        <DataTemplate>
            <Grid Padding="10">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="Auto" />
                    <ColumnDefinition Width="Auto" />
                </Grid.ColumnDefinitions>
                <Image Source="{Binding ImageUrl}"
                       Aspect="AspectFill"
                       HeightRequest="120"
                       WidthRequest="120" />
                <Label Grid.Column="1"
                       Text="{Binding Details}"
                       FontAttributes="Italic" />
            </Grid>
        </DataTemplate>
    </Expander.ContentTemplate>
</Expander>
```

在此示例中， [`Image`](xref:Xamarin.Forms.Image)默认情况`expand`下显示图标：

![处于折叠状态、在 iOS 和 Android 上的扩展器图标的屏幕截图](expander-images/icon-expand.png "IOS 和 Android 上的 Expandd 图标")

当`IsExpanded`点击`true` `Expander`标头时，属性将变为，这将导致`collapse`显示图标：

![处于展开状态、在 iOS 和 Android 上的扩展器图标的屏幕截图](expander-images/icon-collapse.png "IOS 和 Android 上的 Expandd 图标")

有关触发器的详细信息，请参阅[Xamarin。窗体触发器](~/xamarin-forms/app-fundamentals/triggers.md)。

## <a name="define-the-space-between-header-and-content"></a>定义标题和内容之间的空间

默认情况下，中的内容`Expander`会直接显示在其标题的下方。 但是，可以通过将`Spacing`属性设置为一个`double`表示内容和标题的空白间距的值来更改此行为：

```xaml
<Expander Spacing="50"
          IsExpanded="true">
    <Expander.Header>
        <Label Text="Baboon"
               FontAttributes="Bold"
               FontSize="Medium" />
    </Expander.Header>
    <Grid Padding="10">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="Auto" />
        </Grid.ColumnDefinitions>
        <Image Source="http://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/Papio_anubis_%28Serengeti%2C_2009%29.jpg/200px-Papio_anubis_%28Serengeti%2C_2009%29.jpg"
               Aspect="AspectFill"
               HeightRequest="120"
               WidthRequest="120" />
        <Label Grid.Column="1"
               Text="Baboons are African and Arabian Old World monkeys belonging to the genus Papio, part of the subfamily Cercopithecinae."
               FontAttributes="Italic" />
    </Grid>
</Expander>
```

在此示例中， `Expander`内容显示在其标头下方50与设备无关的单位：

![在 iOS 和 Android 上具有间距集的扩展器的屏幕截图](expander-images/expander-spacing.png "在 iOS 和 Android 上设置了间距的扩展器")

## <a name="embed-an-expander-in-an-expander"></a>在扩展器中嵌入扩展器

的内容`Expander`可以设置为另一个`Expander`控件，以启用多个级别的扩展。 以下 XAML 显示了其`Expander`内容为另一个`Expander`对象的：

```xaml
<Expander Spacing="10">
    <Expander.Header>
        <Label Text="{Binding Name}"
               FontAttributes="Bold"
               FontSize="Medium" />
    </Expander.Header>
    <Expander Padding="10"
              Spacing="10">
        <Expander.Header>
            <Label Text="{Binding Location}"
                   FontSize="Medium" />
        </Expander.Header>
            <Expander.ContentTemplate>
                <DataTemplate>
                    <Grid>
                        <Grid.ColumnDefinitions>
                            <ColumnDefinition Width="Auto" />
                            <ColumnDefinition Width="Auto" />
                        </Grid.ColumnDefinitions>
                        <Image Source="{Binding ImageUrl}"
                               Aspect="AspectFill"
                               HeightRequest="120"
                               WidthRequest="120" />
                        <Label Grid.Column="1"
                               Text="{Binding Details}"
                               FontAttributes="Italic" />
                    </Grid>
                </DataTemplate>
            </Expander.ContentTemplate>
    </Expander>
</Expander>
```

在此示例中，点击根`Expander`标头将显示子项`Expander`的标头：

![IOS 和 Android 上嵌入的扩展器的屏幕截图](expander-images/embedded-expander1.png "IOS 和 Android 上的嵌入式扩展器")

点击子`Expander`标头会导致其内容被夸大并显示：

![IOS 和 Android 上嵌入的扩展器的屏幕截图](expander-images/embedded-expander2.png "IOS 和 Android 上的嵌入式扩展器")

## <a name="define-the-expand-and-collapse-animation"></a>定义展开和折叠动画

当可以通过将`Expander` `ExpandAnimationEasing`和`CollapseAnimationEasing`属性设置为包含在 Xamarin 窗体或自定义缓动函数中的任何缓动函数来定义展开或折叠时发生的动画。 默认情况下，展开和折叠动画发生在250毫秒上。 但是，可以通过将`ExpandAnimationLength`和`CollapseAnimationLength`属性设置为`uint`值来更改这些持续时间。

下面的 XAML 演示了一个示例，该示例定义了当`Expander`用户展开或折叠时发生的动画：

```xaml
<Expander ExpandAnimationEasing="{x:Static Easing.CubicIn}"
          ExpandAnimationLength="500"
          CollapseAnimationEasing="{x:Static Easing.CubicOut}"
          CollapseAnimationLength="500">
    <Expander.Header>
        <Label Text="{Binding Name}"
               FontAttributes="Bold"
               FontSize="Medium" />
    </Expander.Header>
    <Expander.ContentTemplate>
        <DataTemplate>
            <Grid Padding="10">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="Auto" />
                    <ColumnDefinition Width="Auto" />
                </Grid.ColumnDefinitions>
                <Image Source="{Binding ImageUrl}"
                       Aspect="AspectFill"
                       HeightRequest="120"
                       WidthRequest="120" />
                <Label Grid.Column="1"
                       Text="{Binding Details}"
                       FontAttributes="Italic" />
            </Grid>
        </DataTemplate>
    </Expander.ContentTemplate>
</Expander>
```

在此示例中， `CubicIn`缓动函数慢慢加快了500毫秒上的展开动画， `CubicOut`缓动函数通过500毫秒快速减速折叠动画。

有关缓动函数的详细信息，请参阅[Xamarin。窗体缓动函数](~/xamarin-forms/user-interface/animation/easing.md)。

## <a name="resize-an-expander-at-runtime"></a>在运行时调整扩展器的大小

使用`Expander` `ForceUpdateSize`方法可在运行时以编程方式调整大小。

`Expander`在给定的`expander`中，如果其内容[`Label`](xref:Xamarin.Forms.Label)包含`TapGestureRecognizer`附加到它的，则下面的代码示例演示如何调用`ForceUpdateSize`方法：

```csharp
void OnLabelTapped(object sender, EventArgs e)
{
    Label label = sender as Label;
    Expander expander = label.Parent.Parent.Parent as Expander;

    if (label.FontSize == Device.GetNamedSize(NamedSize.Default, label))
    {
        label.FontSize = Device.GetNamedSize(NamedSize.Large, label);
    }
    else
    {
        label.FontSize = Device.GetNamedSize(NamedSize.Default, label);
    }
    expander.ForceUpdateSize();
}
```

在此示例中， `FontSize`当点击[`Label`](xref:Xamarin.Forms.Label)时， `Label`会发生更改。 由于字体的大小变化，因此必须通过调用其`Expander` `ForceUpdateSize`方法来更新的大小。

## <a name="disable-an-expander"></a>禁用扩展器

应用程序可以输入一个状态，在该`Expander`状态下，展开不是有效的操作。 在这种情况下`Expander` ，可以通过将其`IsEnabled`属性设置为 false 来禁用。 这会阻止用户展开或折叠`Expander`。

## <a name="related-links"></a>相关链接

- [扩展器演示（示例）](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-expanderdemos/)
- [Xamarin。窗体缓动函数](~/xamarin-forms/user-interface/animation/easing.md)
- [Xamarin.Forms 触发器](~/xamarin-forms/app-fundamentals/triggers.md)
- [Xamarin，可绑定窗体布局](~/xamarin-forms/user-interface/layouts/bindable-layouts.md)
