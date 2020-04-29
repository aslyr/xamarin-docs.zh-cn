---
title: Xamarin 单选按钮
description: Xamarin 单选按钮是一种允许用户从集中选择一个选项的按钮类型。 每个选项都由一个单选按钮表示，您只能在组中选择一个单选按钮。
ms.prod: xamarin
ms.assetid: E2AA40E0-69A5-41DF-BFC4-C151CA657451
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/13/2020
ms.openlocfilehash: 128ab4f6f00adaf86afc08eba37bcb81d3a04a90
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "82532999"
---
# <a name="xamarinforms-radiobutton"></a>Xamarin 单选按钮

[![下载示例](~/media/shared/download.png)下载示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-radiobuttondemos/)

Xamarin `RadioButton`是一种允许用户从集中选择一个选项的按钮。 每个选项都由一个单选按钮表示，您只能在组中选择一个单选按钮。 `RadioButton`类继承自[`Button`](xref:Xamarin.Forms.Button)类。

以下屏幕截图显示`RadioButton`在 IOS 和 Android 上处于清除状态和选中状态的对象：

![IOS 和 Android 上处于选中状态和已清除状态的单选按钮的屏幕截图](radiobutton-images/radiobutton-states.png "IOS 和 Android 上的单选按钮")

> [!IMPORTANT]
> `RadioButton`当前为试验性，只能通过设置`RadioButton_Experimental`标志来使用。 有关详细信息，请参阅[实验标志](~/xamarin-forms/internals/experimental-flags.md)。

`RadioButton`控件定义以下属性：

- `IsChecked`，类型`bool`为，用于定义是否`RadioButton`选择。 此属性使用`TwoWay`绑定，其默认值为`false`。
- `GroupName`，类型`string`为，用于定义指定哪些`RadioButton`控件互相排斥的名称。 此属性的默认值为`null`。

这些属性是由[`BindableProperty`](xref:Xamarin.Forms.BindableProperty)对象支持的，这意味着它们可以是数据绑定的目标和样式。

`RadioButton`控件定义通过用户`CheckedChanged`或编程操作更改`IsChecked`属性时触发的事件。 事件附带的`CheckedChangedEventArgs`对象具有名为的单个属性， `Value`类型`bool`为。 `CheckedChanged` 触发事件时， `Value`属性的值设置为`IsChecked`属性的新值。

此外， `RadioButton`类从[`Button`](xref:Xamarin.Forms.Button)类继承以下通常使用的属性：

- [`Command`](xref:Xamarin.Forms.Button.Command)，类型`ICommand`为，在选择时`RadioButton`执行。
- [`CommandParameter`](xref:Xamarin.Forms.Button.CommandParameter)，类型`object`为，它是传递到的`Command`参数。
- [`FontAttributes`](xref:Xamarin.Forms.Button.FontAttributes)，类型[`FontAttributes`](xref:Xamarin.Forms.FontAttributes)为，用于确定文本样式。
- [`FontFamily`](xref:Xamarin.Forms.Button.FontFamily)，类型`string`为，用于定义字体系列。
- [`FontSize`](xref:Xamarin.Forms.Button.FontSize)，类型`double`为，用于定义字体大小。
- [`Text`](xref:Xamarin.Forms.Button.Text)，类型`string`为，用于定义要显示的文本。
- [`TextColor`](xref:Xamarin.Forms.Button.TextColor)，类型[`Color`](xref:Xamarin.Forms.Color)为，用于定义显示文本的颜色。

有关[`Button`](xref:Xamarin.Forms.Button)控件的详细信息，请参阅[Xamarin Button](~/xamarin-forms/user-interface/button.md)。

## <a name="create-radiobuttons"></a>创建单选按钮

下面的示例演示如何在 XAML `RadioButton`中实例化对象：

```xaml
<StackLayout>
    <Label Text="What's your favorite animal?" />
    <RadioButton Text="Cat" />
    <RadioButton Text="Dog" />
    <RadioButton Text="Elephant" />
    <RadioButton Text="Monkey"
                 IsChecked="true" />
</StackLayout>
```

在此示例中`RadioButton` ，对象在相同的父容器内隐式分组。 此 XAML 将显示以下屏幕截图所示的外观：

![IOS 和 Android 上隐式分组的单选按钮的屏幕截图](radiobutton-images/radiobuttons.png "IOS 和 Android 上的隐式分组单选按钮")

或者， `RadioButton`可以在代码中创建对象：

```csharp
StackLayout stackLayout = new StackLayout
{
    Children =
    {
        new Label { Text = "What's your favorite animal?" },
        new RadioButton { Text = "Cat" },
        new RadioButton { Text = "Dog" },
        new RadioButton { Text = "Elephant" },
        new RadioButton { Text = "Monkey", IsChecked = true }
    }
};
```

## <a name="group-radiobuttons"></a>组单选按钮

单选按钮适用于组中，有两种分组单选按钮的方法：

- 将它们放在同一父容器中。 这称为隐式分组。
- 将每个`GroupName`单选按钮上的属性设置为相同的值。 这称为显式分组。

下面的 XAML 示例演示如何通过`RadioButton`设置对象的属性`GroupName`来对其进行显式分组：

```xaml
<Label Text="What's your favorite color?" />
<RadioButton Text="Red"
             TextColor="Red"
             GroupName="colors" />
<RadioButton Text="Green"
             TextColor="Green"
             GroupName="colors" />
<RadioButton Text="Blue"
             TextColor="Blue"
             GroupName="colors" />
<RadioButton Text="Other"
             GroupName="colors" />
```

在此示例中， `RadioButton`每个都相互排斥，因为它`GroupName`共享相同的值。 此 XAML 将显示以下屏幕截图所示的外观：

![IOS 和 Android 上的显式分组单选按钮的屏幕截图](radiobutton-images/grouped-radiobuttons.png "IOS 和 Android 上的显式分组单选按钮")

## <a name="respond-to-a-radiobutton-state-change"></a>响应单选按钮状态更改

单选按钮有两个状态：已选择或已清除。 选中单选按钮后，其`IsChecked`属性为。 `true` 当取消选中某个单选按钮时， `IsChecked`其属性`false`为。 某个单选按钮可以通过单击同一组中的另外一个单选按钮进行清除，但再次单击该按钮时将无法将其清除。 但是，您可以通过将单选按钮的`IsChecked`属性设置为来`false`以编程方式清除该单选按钮。

当`IsChecked`属性通过用户或编程操作更改时，将激发该`CheckedChanged`事件。 可注册此事件的事件处理程序以响应更改：

```xaml
<RadioButton Text="Red"
             TextColor="Red"
             GroupName="colors"
             CheckedChanged="OnColorsRadioButtonCheckedChanged" />
```

代码隐藏包含`CheckedChanged`事件的处理程序：

```csharp
void OnColorsRadioButtonCheckedChanged(object sender, CheckedChangedEventArgs e)
{
    // Perform required operation
}
```

`sender`自变量是此`RadioButton`事件的责任。 您可以使用它来访问`RadioButton`对象，或区分共享同一`RadioButton` `CheckedChanged`事件处理程序的多个对象。

或者，可以在代码中注册`CheckedChanged`事件的事件处理程序：

```csharp
RadioButton radioButton = new RadioButton { ... };
radioButton.CheckedChanged += (sender, e) =>
{
    // Perform required operation
};
```

> [!NOTE]
> 响应`RadioButton`状态更改的另一种方法是定义`ICommand` ，并将其分配给`RadioButton.Command`属性。 有关详细信息，请参阅[Button：使用命令界面](~/xamarin-forms/user-interface/button.md#using-the-command-interface)。

## <a name="radiobutton-visual-states"></a>单选按钮视觉状态

`RadioButton`具有一个`IsChecked` [`VisualState`](xref:Xamarin.Forms.VisualState) ，它可用于在选择时`RadioButton`启动视觉对象更改。

下面的 XAML 示例演示如何为`IsChecked`状态定义可视状态：

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <Style TargetType="RadioButton">
            <Setter Property="VisualStateManager.VisualStateGroups">
                <VisualStateGroupList>
                    <VisualStateGroup x:Name="CommonStates">
                        <VisualState x:Name="Normal">
                            <VisualState.Setters>
                                <Setter Property="TextColor"
                                        Value="Red" />
                                <Setter Property="Opacity"
                                        Value="0.5" />
                            </VisualState.Setters>
                        </VisualState>
                        <VisualState x:Name="IsChecked">
                            <VisualState.Setters>
                                <Setter Property="TextColor"
                                        Value="Green" />
                                <Setter Property="Opacity"
                                        Value="1" />
                            </VisualState.Setters>
                        </VisualState>
                    </VisualStateGroup>
                </VisualStateGroupList>
            </Setter>
        </Style>
    </ContentPage.Resources>
    <StackLayout>
        <Label Text="What's your favorite mode of transport?" />
        <RadioButton Text="Car"
                     CheckedChanged="OnRadioButtonCheckedChanged" />
        <RadioButton Text="Bike"
                     CheckedChanged="OnRadioButtonCheckedChanged" />
        <RadioButton Text="Train"
                     CheckedChanged="OnRadioButtonCheckedChanged" />
        <RadioButton Text="Walking"
                     CheckedChanged="OnRadioButtonCheckedChanged" />
    </StackLayout>
</ContentPage>
```

在此示例中，为[`Style`](xref:Xamarin.Forms.Style)隐`RadioButton`式目标对象。 `IsChecked` `RadioButton` `TextColor`指定在选择时，其属性将设置为绿色， `Opacity`值为1。 [`VisualState`](xref:Xamarin.Forms.VisualState) `Normal` `RadioButton` `TextColor`指定当处于已清除状态时，其属性将设置为红色， `Opacity`值为0.5。 `VisualState` 因此，整体效果是，在清除时`RadioButton` ，它是红色且部分透明的，并且在选中时，不使用透明度：

![在 iOS 和 Android 上按视觉状态设置的单选按钮外观的屏幕截图](radiobutton-images/ischecked-visualstate.png "IOS 和 Android 上的单选按钮视觉状态")

有关可视状态的详细信息，请参阅[Xamarin。窗体可视状态管理器](~/xamarin-forms/user-interface/visual-state-manager.md)。

## <a name="disable-a-radiobutton"></a>禁用单选按钮

有时，应用程序会进入处于选中`RadioButton`状态的状态，该状态是无效操作。 在这种情况下`RadioButton` ，可以通过将其`IsEnabled`属性设置为`false`来禁用。

## <a name="related-links"></a>相关链接

- [单选按钮演示（示例）](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-radiobuttondemos/)
- [Xamarin. Forms 按钮](~/xamarin-forms/user-interface/button.md)
- [Xamarin. Forms 视觉对象状态管理器](~/xamarin-forms/user-interface/visual-state-manager.md)
