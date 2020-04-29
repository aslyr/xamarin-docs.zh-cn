---
title: Xamarin.Forms 复选框
description: Xamarin 复选框是一种可以选中或空的按钮类型。 选中复选框后，它将被视为已启用。 如果复选框为空，则将其视为关闭。
ms.prod: xamarin
ms.assetid: B8B9268B-BCB8-42B9-B08C-C0F22C137238
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/11/2019
ms.openlocfilehash: 10b7c4c3478545863ef49a23ef0f1be777e7eda9
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "82517118"
---
# <a name="xamarinforms-checkbox"></a>Xamarin.Forms 复选框

[![下载示例](~/media/shared/download.png)下载示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-checkboxdemos/)

Xamarin `CheckBox`是可以选中或空的按钮类型。 选中复选框后，它将被视为已启用。 如果复选框为空，则将其视为关闭。

`CheckBox`定义一个`bool`名为`IsChecked`的属性，指示是否`CheckBox`选中。 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)对象也支持此属性，这意味着它可以进行样式设计，并且是数据绑定的目标。

> [!NOTE]
> 可`IsChecked`绑定的属性的默认绑定模式为[`BindingMode.TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay)。

`CheckBox`定义一个`CheckedChanged`事件，该事件在`IsChecked`属性更改时通过用户操作或应用程序设置`IsChecked`属性时引发。 事件附带的`CheckedChangedEventArgs`对象具有名为的单个属性， `Value`类型`bool`为。 `CheckedChanged` 触发事件时， `Value`属性的值设置为`IsChecked`属性的新值。

## <a name="create-a-checkbox"></a>创建复选框

下面的示例演示如何`CheckBox`在 XAML 中实例化：

```xaml
<CheckBox />
```

此 XAML 将显示以下屏幕截图所示的外观：

![IOS 和 Android 上的空复选框的屏幕截图](checkbox-images/checkbox-empty.png "空复选框")

默认情况下， `CheckBox`为空。 `CheckBox`可以通过用户操作来检查，也可以通过将`IsChecked`属性设置为`true`来进行检查：

```xaml
<CheckBox IsChecked="true" />
```

此 XAML 将显示以下屏幕截图所示的外观：

![IOS 和 Android 上选中的复选框的屏幕截图](checkbox-images/checkbox-checked.png "选中的复选框")

或者， `CheckBox`可以在代码中创建：

```csharp
CheckBox checkBox = new CheckBox { IsChecked = true };
```

## <a name="respond-to-a-checkbox-changing-state"></a>响应更改状态的复选框

当`IsChecked`属性更改时，无论是通过用户操作，还是在应用程序`IsChecked`设置属性时`CheckedChanged` ，都将激发该事件。 可注册此事件的事件处理程序以响应更改：

```xaml
<CheckBox CheckedChanged="OnCheckBoxCheckedChanged" />
```

代码隐藏文件包含`CheckedChanged`事件的处理程序：

```csharp
void OnCheckBoxCheckedChanged(object sender, CheckedChangedEventArgs e)
{
    // Perform required operation after examining e.Value
}
```

`sender`自变量是此`CheckBox`事件的责任。 您可以使用它来访问`CheckBox`对象，或区分共享同一`CheckBox` `CheckedChanged`事件处理程序的多个对象。

或者，可以在代码中注册`CheckedChanged`事件的事件处理程序：

```csharp
CheckBox checkBox = new CheckBox { ... };
checkBox.CheckedChanged += (sender, e) =>
{
    // Perform required operation after examining e.Value
};
```

## <a name="data-bind-a-checkbox"></a>数据绑定复选框

可以`CheckedChanged`通过使用数据绑定和触发器来响应`CheckBox`选中或空的事件处理程序：

```xaml
<CheckBox x:Name="checkBox" />
<Label Text="Lorem ipsum dolor sit amet, elit rutrum, enim hendrerit augue vitae praesent sed non, lorem aenean quis praesent pede.">
    <Label.Triggers>
        <DataTrigger TargetType="Label"
                     Binding="{Binding Source={x:Reference checkBox}, Path=IsChecked}"
                     Value="true">
            <Setter Property="FontAttributes"
                    Value="Italic, Bold" />
            <Setter Property="FontSize"
                    Value="Large" />
        </DataTrigger>
    </Label.Triggers>
</Label>
```

在此示例中， [`Label`](xref:Xamarin.Forms.Label)在数据触发器中使用绑定表达式来监视的`IsChecked`属性。 `CheckBox` 当此属性为`true`时， `FontAttributes`此`FontSize`属性的和`Label`属性将为。 `IsChecked`当属性`false`返回到时，的`FontAttributes` `FontSize` `Label`和属性将重置为它们的初始状态。

在下面的屏幕截图中，iOS 屏幕快照[`Label`](xref:Xamarin.Forms.Label)在`CheckBox`为空时显示格式设置，而 Android 屏幕截图`Label`在选中时`CheckBox`显示格式设置：

[![IOS 和 Android 上的数据绑定复选框的屏幕截图](checkbox-images/checkbox-databinding.png ""数据绑定" 复选框")](checkbox-images/checkbox-databinding-large.png#lightbox ""数据绑定" 复选框")

有关触发器的详细信息，请参阅[Xamarin。窗体触发器](~/xamarin-forms/app-fundamentals/triggers.md)。

## <a name="disable-a-checkbox"></a>禁用复选框

有时，应用程序会进入处于选中`CheckBox`状态的状态，该状态是无效操作。 在这种情况下`CheckBox` ，可以通过将其`IsEnabled`属性设置为`false`来禁用。

## <a name="checkbox-appearance"></a>CheckBox 外观

`CheckBox`除了从[`View`](xref:Xamarin.Forms.View)类继承的属性之外， `CheckBox`还定义了一个`Color`属性，该属性将其颜色设置为[`Color`](xref:Xamarin.Forms.Color)：

```xaml
<CheckBox Color="Red" />
```

下面的屏幕截图显示一系列已`CheckBox`选中的对象，其中每个`Color`对象的属性设置为[`Color`](xref:Xamarin.Forms.Color)不同的：

![IOS 和 Android 上的彩色复选框的屏幕截图](checkbox-images/checkbox-colors.png "彩色复选框")

## <a name="checkbox-visual-states"></a>复选框可视状态

`CheckBox`包含一个`IsChecked` [`VisualState`](xref:Xamarin.Forms.VisualState) ，它可用于在检查时启动对的`CheckBox`视觉对象更改。

下面的 XAML 示例演示如何为`IsChecked`状态定义可视状态：

```xaml
<CheckBox ...>
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="Normal">
                <VisualState.Setters>
                    <Setter Property="Color"
                            Value="Red" />
                </VisualState.Setters>
            </VisualState>

            <VisualState x:Name="IsChecked">
                <VisualState.Setters>
                    <Setter Property="Color"
                            Value="Green" />
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</CheckBox>
```

在此示例中， `IsChecked` [`VisualState`](xref:Xamarin.Forms.VisualState)指定在选中时`CheckBox` ，其`Color`属性将设置为绿色。 指定在`CheckBox`处于正常状态时，其`Color`属性将设置为红色。 `Normal` `VisualState` 因此，整体效果是在为空`CheckBox`时为红色，选中时为绿色。

有关可视状态的详细信息，请参阅[Xamarin。窗体可视状态管理器](~/xamarin-forms/user-interface/visual-state-manager.md)。

## <a name="related-links"></a>相关链接

- [复选框演示（示例）](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-checkboxdemos/)
- [Xamarin.Forms 触发器](~/xamarin-forms/app-fundamentals/triggers.md)
- [Xamarin. Forms 视觉对象状态管理器](~/xamarin-forms/user-interface/visual-state-manager.md)
