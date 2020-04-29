---
title: Xamarin. Forms 视觉对象状态管理器
description: 使用可视状态管理器根据代码中的可视状态集对 XAML 元素进行更改。
ms.prod: xamarin
ms.assetid: 17296F14-640D-484B-A24C-A4E9B7013E4F
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 02/21/2020
ms.openlocfilehash: c6930f3361394b04e90083594e2343b50dac64ab
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "82517515"
---
# <a name="xamarinforms-visual-state-manager"></a>Xamarin. Forms 视觉对象状态管理器

[![下载示例](~/media/shared/download.png)下载示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)

_使用可视状态管理器根据代码中的可视状态集对 XAML 元素进行更改。_

视觉状态管理器（VSM）提供了一种结构化的方法，可以从代码对用户界面进行可视更改。 在大多数情况下，应用程序的用户界面是在 XAML 中定义的，此 XAML 包含描述视觉状态管理器如何影响用户界面视觉对象的标记。

VSM 引入了_可视状态_的概念。 诸如这样的 Xamarin 窗体视图`Button`可以有多个不同的视觉外观，具体取决于&mdash;它是被禁用、按下还是有输入焦点。 这些是按钮的状态。

可视状态在_视觉状态组_中收集。 可视状态组中的所有可视状态都是互斥的。 可视状态和视觉状态组均由简单文本字符串标识。

Xamarin. Forms 视觉对象状态管理器定义了一个名为 "CommonStates" 的视觉状态组，其中包含以下可视状态：

- "Normal"
- Disabled
- 侧重于
- 选择

派生自[`VisualElement`](xref:Xamarin.Forms.VisualElement)的所有类都支持此视觉对象状态组，它是[`View`](xref:Xamarin.Forms.View)和[`Page`](xref:Xamarin.Forms.Page)的基类。

你还可以定义自己的视觉状态组和视觉状态，如本文所示。

> [!NOTE]
> Xamarin：熟悉[触发器](~/xamarin-forms/app-fundamentals/triggers.md)的窗体开发人员可以知道，触发器还可以根据视图属性中的更改或事件激发来更改用户界面中的视觉对象。 然而，使用触发器处理这些更改的各种组合可能会令人感到困惑。 从历史角度来看，可视状态管理器是在基于 Windows XAML 的环境中引入的，用于缓解由于视觉状态组合而产生的混淆。 使用 VSM，可视状态组中的可视状态始终是互斥的。 在任何时候，每个组中只有一个状态是当前状态。

## <a name="common-states"></a>常见状态

视觉状态管理器允许您在 XAML 文件中包含标记，如果视图正常、处于禁用状态或具有输入焦点，则可以更改视图的可视外观。 这些_状态称为公共状态_。

例如，假设您在页面上`Entry`有一个视图，并且您希望的视觉外观`Entry`按以下方式变化：

- 禁用`Entry`时`Entry` ，应会有粉红色的背景。
- `Entry`应正常使用酸橙色。
- 如果`Entry`有输入焦点，则应将其正常高度扩展到两倍。

可以将 VSM 标记附加到单个视图，也可以在样式中定义它（如果它应用于多个视图）。 接下来的两部分介绍了这些方法。

### <a name="vsm-markup-on-a-view"></a>视图上的 VSM 标记

若要将 VSM 标记附加`Entry`到视图，请首先`Entry`将分隔为开始标记和结束标记：

```xaml
<Entry FontSize="18">

</Entry>
```

由于其中一个状态将使用`FontSize`属性来使中的文本大小加倍，因此提供了一个显式字体大小。 `Entry`

接下来， `VisualStateManager.VisualStateGroups`在这些标记之间插入标记：

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>

    </VisualStateManager.VisualStateGroups>
</Entry>
```

[`VisualStateGroups`](xref:Xamarin.Forms.VisualStateManager.VisualStateGroupsProperty)是由[`VisualStateManager`](xref:Xamarin.Forms.VisualStateManager)类定义的附加可绑定属性。 （有关附加的可绑定属性的详细信息，请参阅[附加属性](~/xamarin-forms/xaml/attached-properties.md)一文。）这就是`VisualStateGroups`属性附加到`Entry`对象的方式。

`VisualStateGroups`属性的类型[`VisualStateGroupList`](xref:Xamarin.Forms.VisualStateGroupList)为，它是[`VisualStateGroup`](xref:Xamarin.Forms.VisualStateGroup)对象的集合。 在`VisualStateManager.VisualStateGroups`标记中，为想要包括`VisualStateGroup`的每个可视状态组插入一对标记：

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">

        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Entry>
```

请注意， `VisualStateGroup`标记具有一个`x:Name`指示组名称的特性。 `VisualStateGroup`类定义可改用`Name`的属性：

```xaml
<VisualStateGroup Name="CommonStates">
```

可以在同一元素`x:Name`中`Name`使用或，但不能同时使用两者。

`VisualStateGroup`类定义一个名为[`States`](xref:Xamarin.Forms.VisualStateGroup.States)的[`VisualState`](xref:Xamarin.Forms.VisualState)属性，该属性是一个对象的集合。 `States`是的_内容属性_ `VisualStateGroups` ，因此可以在`VisualState` `VisualStateGroup`标记之间直接包括标记。 （有关内容属性的介绍，请参见[基本的 XAML 语法](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md#content-properties)一文。）

下一步是为该组中的每个可视状态包含一对标记。 还可以使用`x:Name`或`Name`进行标识：

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="Normal">

            </VisualState>

            <VisualState x:Name="Focused">

            </VisualState>

            <VisualState x:Name="Disabled">

            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Entry>
```

`VisualState`定义名为[`Setters`](xref:Xamarin.Forms.VisualState.Setters)的[`Setter`](xref:Xamarin.Forms.Setter)属性，它是对象的集合。 它们是在[`Style`](xref:Xamarin.Forms.Style)对象`Setter`中使用的相同对象。

`Setters`_不_是的内容属性`VisualState`，因此需要包含`Setters`属性的属性元素标记：

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="Normal">
                <VisualState.Setters>

                </VisualState.Setters>
            </VisualState>

            <VisualState x:Name="Focused">
                <VisualState.Setters>

                </VisualState.Setters>
            </VisualState>

            <VisualState x:Name="Disabled">
                <VisualState.Setters>

                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Entry>
```

你现在可以在每对`Setter` `Setters`标记之间插入一个或多个对象。 定义前面所`Setter`述的视觉对象状态的对象如下：

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="Normal">
                <VisualState.Setters>
                    <Setter Property="BackgroundColor" Value="Lime" />
                </VisualState.Setters>
            </VisualState>

            <VisualState x:Name="Focused">
                <VisualState.Setters>
                    <Setter Property="FontSize" Value="36" />
                </VisualState.Setters>
            </VisualState>

            <VisualState x:Name="Disabled">
                <VisualState.Setters>
                    <Setter Property="BackgroundColor" Value="Pink" />
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Entry>
```

每`Setter`个标记指示特定属性在该状态为当前状态时的值。 `Setter`对象引用的任何属性都必须由可绑定的属性支持。

与此类似的标记是**[VsmDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)** 示例程序中 "**查看**" 页的基础。 此页包含三`Entry`个视图，但只有第二个视图附加了 VSM 标记：

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:VsmDemos"
             x:Class="VsmDemos.MainPage"
             Title="VSM Demos">

    <StackLayout>
        <StackLayout.Resources>
            <Style TargetType="Entry">
                <Setter Property="Margin" Value="20, 0" />
                <Setter Property="FontSize" Value="18" />
            </Style>

            <Style TargetType="Label">
                <Setter Property="Margin" Value="20, 30, 20, 0" />
                <Setter Property="FontSize" Value="Large" />
            </Style>
        </StackLayout.Resources>

        <Label Text="Normal Entry:" />
        <Entry />
        <Label Text="Entry with VSM: " />
        <Entry>
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup x:Name="CommonStates">
                    <VisualState x:Name="Normal">
                        <VisualState.Setters>
                            <Setter Property="BackgroundColor" Value="Lime" />
                        </VisualState.Setters>
                    </VisualState>
                    <VisualState x:Name="Focused">
                        <VisualState.Setters>
                            <Setter Property="FontSize" Value="36" />
                        </VisualState.Setters>
                    </VisualState>
                    <VisualState x:Name="Disabled">
                        <VisualState.Setters>
                            <Setter Property="BackgroundColor" Value="Pink" />
                        </VisualState.Setters>
                    </VisualState>
                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>

            <Entry.Triggers>
                <DataTrigger TargetType="Entry"
                             Binding="{Binding Source={x:Reference entry3},
                                               Path=Text.Length}"
                             Value="0">
                    <Setter Property="IsEnabled" Value="False" />
                </DataTrigger>
            </Entry.Triggers>
        </Entry>
        <Label Text="Entry to enable 2nd Entry:" />
        <Entry x:Name="entry3"
               Text=""
               Placeholder="Type something to enable 2nd Entry" />
    </StackLayout>
</ContentPage>
```

请注意，第`Entry`二个还`DataTrigger`在其`Trigger`集合中包含。 这会导致`Entry`在第三个`Entry`中键入内容之前禁用。 在 iOS、Android 和通用 Windows 平台（UWP）上运行时，会显示以下页面：

[![视图上的 VSM：已禁用](vsm-images/VsmOnViewDisabled.png "视图上的 VSM-已禁用")](vsm-images/VsmOnViewDisabled-Large.png#lightbox)

当前视觉状态为 "已禁用"，因此第二个`Entry`视觉对象的背景在 IOS 和 Android 屏幕上为粉红色。 的 UWP 实现`Entry`不允许在禁用时`Entry`设置背景色。

当你在第三`Entry`个文本中输入文本时`Entry` ，第二个开关将进入 "正常" 状态，并且背景现在为酸橙色：

[![视图上的 VSM：正常](vsm-images/VsmOnViewNormal.png "视图上的 VSM-正常")](vsm-images/VsmOnViewNormal-Large.png#lightbox)

当你触及第二`Entry`个时，它将获得输入焦点。 它将切换到 "重点" 状态，并扩展到其高度的两倍：

[![视图上的 VSM：聚焦](vsm-images/VsmOnViewFocused.png "基于视图的 VSM")](vsm-images/VsmOnViewFocused-Large.png#lightbox)

请注意， `Entry`在获得输入焦点时不会保留酸橙色背景。 当可视状态管理器在可视状态之间切换时，先前状态设置的属性将设置为 "未设置"。 请记住，可视状态是互斥的。 "正常" 状态并不意味着`Entry`已启用。 这意味着`Entry`已启用且没有输入焦点。

如果希望`Entry`在 "重点" 状态下有一个酸橙色背景，请将另一个`Setter`添加到该视觉对象状态：

```xaml
<VisualState x:Name="Focused">
    <VisualState.Setters>
        <Setter Property="FontSize" Value="36" />
        <Setter Property="BackgroundColor" Value="Lime" />
    </VisualState.Setters>
</VisualState>
```

为了使这些`Setter`对象正常工作， `VisualStateGroup`必须包含`VisualState`该组中所有状态的对象。 如果视觉对象状态不具有任何`Setter`对象，请将其包含为空标记：

```xaml
<VisualState x:Name="Normal" />
```

### <a name="visual-state-manager-markup-in-a-style"></a>样式中的可视状态管理器标记

通常需要在两个或更多视图中共享同一可视状态管理器标记。 在这种情况下，您需要将标记放在一个`Style`定义中。

下面是 "查看" `Style` " `Entry` **查看**" 页中的元素的现有隐式：

```xaml
<Style TargetType="Entry">
    <Setter Property="Margin" Value="20, 0" />
    <Setter Property="FontSize" Value="18" />
</Style>
```

为`Setter`附加的`VisualStateManager.VisualStateGroups`可绑定属性添加标记：

```xaml
<Style TargetType="Entry">
    <Setter Property="Margin" Value="20, 0" />
    <Setter Property="FontSize" Value="18" />
    <Setter Property="VisualStateManager.VisualStateGroups">

    </Setter>
</Style>
```

的内容属性`Setter`为`Value`，因此可以在这些标记中`Value`直接指定属性的值。 该属性的类型`VisualStateGroupList`为：

```xaml
<Style TargetType="Entry">
    <Setter Property="Margin" Value="20, 0" />
    <Setter Property="FontSize" Value="18" />
    <Setter Property="VisualStateManager.VisualStateGroups">
        <VisualStateGroupList>

        </VisualStateGroupList>
    </Setter>
</Style>
```

在这些标记中，可以包含一个或`VisualStateGroup`多个对象：

```xaml
<Style TargetType="Entry">
    <Setter Property="Margin" Value="20, 0" />
    <Setter Property="FontSize" Value="18" />
    <Setter Property="VisualStateManager.VisualStateGroups">
        <VisualStateGroupList>
            <VisualStateGroup x:Name="CommonStates">

            </VisualStateGroup>
        </VisualStateGroupList>
    </Setter>
</Style>
```

VSM 标记的剩余部分与之前相同。

下面是 "**样式**" 页中显示完整的 vsm 标记的 vsm：

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="VsmDemos.VsmInStylePage"
             Title="VSM in Style">
    <StackLayout>
        <StackLayout.Resources>
            <Style TargetType="Entry">
                <Setter Property="Margin" Value="20, 0" />
                <Setter Property="FontSize" Value="18" />
                <Setter Property="VisualStateManager.VisualStateGroups">
                    <VisualStateGroupList>
                        <VisualStateGroup x:Name="CommonStates">
                            <VisualState x:Name="Normal">
                                <VisualState.Setters>
                                    <Setter Property="BackgroundColor" Value="Lime" />
                                </VisualState.Setters>
                            </VisualState>
                            <VisualState x:Name="Focused">
                                <VisualState.Setters>
                                    <Setter Property="FontSize" Value="36" />
                                    <Setter Property="BackgroundColor" Value="Lime" />
                                </VisualState.Setters>
                            </VisualState>
                            <VisualState x:Name="Disabled">
                                <VisualState.Setters>
                                    <Setter Property="BackgroundColor" Value="Pink" />
                                </VisualState.Setters>
                            </VisualState>
                        </VisualStateGroup>
                    </VisualStateGroupList>
                </Setter>
            </Style>

            <Style TargetType="Label">
                <Setter Property="Margin" Value="20, 30, 20, 0" />
                <Setter Property="FontSize" Value="Large" />
            </Style>
        </StackLayout.Resources>

        <Label Text="Normal Entry:" />
        <Entry />
        <Label Text="Entry with VSM: " />
        <Entry>
            <Entry.Triggers>
                <DataTrigger TargetType="Entry"
                             Binding="{Binding Source={x:Reference entry3},
                                               Path=Text.Length}"
                             Value="0">
                    <Setter Property="IsEnabled" Value="False" />
                </DataTrigger>
            </Entry.Triggers>
        </Entry>
        <Label Text="Entry to enable 2nd Entry:" />
        <Entry x:Name="entry3"
               Text=""
               Placeholder="Type something to enable 2nd Entry" />
    </StackLayout>
</ContentPage>
```

现在，此`Entry`页上的所有视图都以相同的方式对其视觉对象状态做出响应。 另请注意，"重点" 状态现在包含另`Setter`一个，它还`Entry`提供了一个酸橙色背景，同时还提供了输入焦点：

[![VSM 样式](vsm-images/VsmInStyle.png "VSM 样式")](vsm-images/VsmInStyle-Large.png#lightbox)

## <a name="visual-states-in-xamarinforms"></a>Xamarin 中的可视状态

下表列出了在 Xamarin 中定义的视觉对象状态：

| 实例 | 状态 | 更多信息 |
| ----- | ------ | ---------------- |
| `Button` | `Pressed` | [按钮视觉状态](~/xamarin-forms/user-interface/button.md#button-visual-states) |
| `CheckBox` | `IsChecked` | [复选框可视状态](~/xamarin-forms/user-interface/checkbox.md#checkbox-visual-states) |
| `CarouselView` | `DefaultItem`, `CurrentItem`, `PreviousItem`, `NextItem` | [CarouselView 视觉状态](~/xamarin-forms/user-interface/carouselview/interaction.md#define-visual-states) |
| `ImageButton` | `Pressed` | [ImageButton 视觉状态](~/xamarin-forms/user-interface/imagebutton.md#imagebutton-visual-states) |
| `RadioButton` | `IsChecked` | [单选按钮视觉状态](~/xamarin-forms/user-interface/radiobutton.md#radiobutton-visual-states) |
| `VisualElement` | `Normal`, `Disabled`, `Focused`, `Selected` | [常见状态](#common-states) |

每个状态都可以通过名为`CommonStates`的视觉状态组进行访问。

此外，还`CollectionView`实现了`Selected`状态。 有关详细信息，请参阅[更改选定项的颜色](~/xamarin-forms/user-interface/collectionview/selection.md#change-selected-item-color)。

## <a name="set-state-on-multiple-elements"></a>设置多个元素的状态

在前面的示例中，可视状态附加到单个元素上并在其上操作。 但是，也可以创建附加到单个元素的可视状态，但这会为同一范围内的其他元素设置属性。 这样就无需在运行状态的每个元素上重复视觉状态。

[`Setter`](xref:Xamarin.Forms.Setter)类型具有一个`TargetName`类型`string`为的属性，该属性表示可视状态将操作的目标元素`Setter` 。 定义`TargetName`属性时，将在中`Setter` `Property` `TargetName`定义的元素的设置为`Value`：

```xaml
<Setter TargetName="label"
        Property="Label.TextColor"
        Value="Red" />
```

在此示例中， `Label`命名`label`的将其`TextColor`属性设置为`Red`。 设置`TargetName`属性时，必须在中`Property`指定属性的完整路径。 因此，若要在`TextColor`上设置属性`Label`， `Property`请将指定`Label.TextColor`为。

> [!NOTE]
> `Setter`对象引用的任何属性都必须由可绑定的属性支持。

**[VsmDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)** 示例中的 "**与 Setter TargetName 的 VSM** " 页面说明了如何在单个可视状态组中设置多个元素的状态。 XAML 文件包含`StackLayout`一个包含`Label`元素、 `Entry`和的： `Button`

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="VsmDemos.VsmSetterTargetNamePage"
             Title="VSM with Setter TargetName">
    <StackLayout Margin="10">
        <Label Text="What is the capital of France?" />
        <Entry x:Name="entry"
               Placeholder="Enter answer" />
        <Button Text="Reveal answer">
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup x:Name="CommonStates">
                    <VisualState x:Name="Normal" />
                    <VisualState x:Name="Pressed">
                        <VisualState.Setters>
                            <Setter Property="Scale"
                                    Value="0.8" />
                            <Setter TargetName="entry"
                                    Property="Entry.Text"
                                    Value="Paris" />
                        </VisualState.Setters>
                    </VisualState>
                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>
        </Button>
    </StackLayout>
</ContentPage>
```

VSM 标记已附加到`StackLayout`。 有两个互斥状态，分别名为 "常规" 和 "按下"，每种状态`VisualState`包含标记。

当`Button`未按下时，"正常" 状态处于活动状态，并且可以输入对问题的响应：

[![VSM Setter TargetName：正常状态](vsm-images/VsmSetterTargetNameNormal.png "VSM 资源库 targetname-正常")](vsm-images/VsmSetterTargetNameNormal-Large.png#lightbox)

按下时`Button` ，"按下" 状态变为活动状态：

[![VSM Setter TargetName：按下状态](vsm-images/VsmSetterTargetNamePressed.png "VSM 资源库 targetname-按下")](vsm-images/VsmSetterTargetNamePressed-Large.png#lightbox)

"按下" `VisualState`指定在按下`Button`时，其`Scale`属性将从默认值1更改为0.8。 此外，命名`Entry` `entry`的将其`Text`属性设置为巴黎。 因此，在按下时`Button` ，重新缩放会略小，并`Entry`显示巴黎。 然后，当释放`Button`时，它的默认值为1，并`Entry`显示以前输入的任何文本重新缩放。

> [!IMPORTANT]
> 属性路径当前在指定`Setter` `TargetName`属性的元素中不受支持。

## <a name="define-your-own-visual-states"></a>定义自己的视觉状态

派生自`VisualElement`的每个类都支持通用状态 "正常"、"重点" 和 "已禁用"。 此外， `CollectionView`类还支持 "选定" 状态。 在内部， [`VisualElement`](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Core/VisualElement.cs)类会在其启用或禁用，或者重点或失去焦点，并调用静态[`VisualStateManager.GoToState`](xref:Xamarin.Forms.VisualStateManager.GoToState(Xamarin.Forms.VisualElement,System.String))方法时进行检测：

```csharp
VisualStateManager.GoToState(this, "Focused");
```

这是你将在`VisualElement`类中找到的唯一视觉状态管理器代码。 由于`GoToState`每个对象都是根据每个派生自`VisualElement`的类调用的，因此，您可以将可视状态`VisualElement`管理器与任何对象结合使用来响应这些更改。

有趣的是，可视状态组 "CommonStates" 的名称未在中`VisualElement`显式引用。 组名不是可视状态管理器的 API 的一部分。 到目前为止，您可以将组的名称从 "CommonStates" 更改为其他任何程序，程序仍将运行。 组名称只是该组中状态的一般说明。 它被隐式理解，任何组中的可视状态都是互斥的：一个状态，并且在任何时候都只有一个状态是最新的。

如果要实现自己的可视状态，需要从代码调用`VisualStateManager.GoToState` 。 大多数情况下，你将从页面类的代码隐藏文件进行此调用。

**[VsmDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)** 示例中的 " **VSM 验证**" 页显示了如何在连接到输入验证时使用视觉状态管理器。 XAML `StackLayout`文件包含一个，其中包含两`Label`个元素`Entry` `Button`：和。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="VsmDemos.VsmValidationPage"
             Title="VSM Validation">
    <StackLayout x:Name="stackLayout"
                 Padding="10, 10">
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup Name="ValidityStates">
                    <VisualState Name="Valid">
                        <VisualState.Setters>
                            <Setter TargetName="helpLabel"
                                    Property="Label.TextColor"
                                    Value="Transparent" />
                            <Setter TargetName="entry"
                                    Property="Entry.BackgroundColor"
                                    Value="Lime" />
                        </VisualState.Setters>
                    </VisualState>
                    <VisualState Name="Invalid">
                        <VisualState.Setters>
                            <Setter TargetName="entry"
                                    Property="Entry.BackgroundColor"
                                    Value="Pink" />
                            <Setter TargetName="submitButton"
                                    Property="Button.IsEnabled"
                                    Value="False" />
                        </VisualState.Setters>
                    </VisualState>
                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>
        <Label Text="Enter a U.S. phone number:"
               FontSize="Large" />
        <Entry x:Name="entry"
               Placeholder="555-555-5555"
               FontSize="Large"
               Margin="30, 0, 0, 0"
               TextChanged="OnTextChanged" />
        <Label x:Name="helpLabel"
               Text="Phone number must be of the form 555-555-5555, and not begin with a 0 or 1" />
        <Button x:Name="submitButton"
                Text="Submit"
                FontSize="Large"
                Margin="0, 20"
                VerticalOptions="Center"
                HorizontalOptions="Center" />
    </StackLayout>
</ContentPage>
```

VSM 标记附加到`StackLayout` （名为`stackLayout`）。 有两个互斥状态，分别名为 "有效" 和 "无效"，其中每种状态`VisualState`都包含标记。

`Entry`如果不包含有效的电话号码，则当前状态为 "无效"，因此`Entry`具有粉红色背景，第二个`Label`为可见， `Button`禁用：

[![VSM 验证：无效状态](vsm-images/VsmValidationInvalid.png "VSM 验证-无效")](vsm-images/VsmValidationInvalid-Large.png#lightbox)

输入有效的电话号码时，当前状态将变为 "有效"。 `Entry`获取一个酸橙色背景，第二`Label`个消失， `Button`现已启用：

[![VSM 验证：有效状态](vsm-images/VsmValidationValid.png "VSM 验证-有效")](vsm-images/VsmValidationValid-Large.png#lightbox)

代码隐藏文件负责处理中的`TextChanged`事件。 `Entry` 处理程序使用正则表达式来确定输入字符串是否有效。 代码隐藏文件中名为`GoToState`的方法调用的静态`VisualStateManager.GoToState`方法： `stackLayout`

```csharp
public partial class VsmValidationPage : ContentPage
{
    public VsmValidationPage()
    {
        InitializeComponent();

        GoToState(false);
    }

    void OnTextChanged(object sender, TextChangedEventArgs args)
    {
        bool isValid = Regex.IsMatch(args.NewTextValue, @"^[2-9]\d{2}-\d{3}-\d{4}$");
        GoToState(isValid);
    }

    void GoToState(bool isValid)
    {
        string visualState = isValid ? "Valid" : "Invalid";
        VisualStateManager.GoToState(stackLayout, visualState);
    }
}
```

另请注意， `GoToState`从构造函数调用方法来初始化状态。 应始终具有当前状态。 但在代码中不存在任何引用视觉状态组的名称，但在 XAML 中将其作为 "ValidationStates" 引用是为了清楚起见。

请注意，代码隐藏文件只需考虑定义可视状态的页面上的对象，并为该对象调用`VisualStateManager.GoToState` 。 这是因为，这两种可视状态都在页面上定位多个对象。

您可能想知道：代码隐藏文件必须引用定义可视状态的页面上的对象，为什么代码隐藏文件无法直接访问此对象和其他对象？ 当然可以。 不过，使用 VSM 的优点是，你可以控制视觉对象如何完全在 XAML 中响应不同状态，这会将所有 UI 设计保存在一个位置。 这可以避免通过直接从代码隐藏访问视觉对象来设置视觉外观。

## <a name="visual-state-triggers"></a>可视状态触发器

可视状态支持状态触发器，这些触发器是一组专用的触发器，用于定义应用应用的[`VisualState`](xref:Xamarin.Forms.VisualState)条件。

状态触发器添加到的[`StateTriggers`](xref:Xamarin.Forms.VisualState.StateTriggers)集合中。 [`VisualState`](xref:Xamarin.Forms.VisualState) 此集合可以包含单个状态触发器或多个状态触发器。 当[`VisualState`](xref:Xamarin.Forms.VisualState)集合中的任何状态触发器处于活动状态时，将应用。

使用状态触发器控制可视状态时，Xamarin 使用以下优先规则来确定哪个触发器（及相应[`VisualState`](xref:Xamarin.Forms.VisualState)的）将处于活动状态：

1. 派生自[`StateTriggerBase`](xref:Xamarin.Forms.StateTriggerBase)的任何触发器。
1. 由于[`AdaptiveTrigger`](xref:Xamarin.Forms.AdaptiveTrigger)满足[`MinWindowWidth`](xref:Xamarin.Forms.AdaptiveTrigger.MinWindowWidth)条件，已激活。
1. 由于[`AdaptiveTrigger`](xref:Xamarin.Forms.AdaptiveTrigger)满足[`MinWindowHeight`](xref:Xamarin.Forms.AdaptiveTrigger.MinWindowHeight)条件，已激活。

如果多个触发器同时处于活动状态（例如，两个自定义触发器），则优先使用在标记中声明的第一个触发器。

有关状态触发器的详细信息，请参阅[状态触发器](~/xamarin-forms/app-fundamentals/triggers.md#state-triggers)。

<a name="adaptive-layout" />

## <a name="use-the-visual-state-manager-for-adaptive-layout"></a>使用视觉对象状态管理器进行自适应布局

通常，在手机上运行的 Xamarin 应用程序应用程序通常可以在纵向或横向纵横比上查看，并且可以调整桌面上运行的 Xamarin 程序的大小，以采用许多不同的大小和纵横比。 设计良好的应用程序可能会以不同的方式显示这些各种页面或窗口外形因素的内容。

此方法有时称为_自适应布局_。 因为自适应布局只涉及程序的视觉对象，所以它是可视状态管理器的理想应用。

一个简单的示例是应用程序，它显示了影响应用程序内容的小按钮集合。 在纵向模式下，可能会在页面顶部的水平行中显示这些按钮：

[![VSM 自适应布局：纵向](vsm-images/VsmAdaptiveLayoutPortrait.png "VSM 自适应布局-纵向")](vsm-images/VsmAdaptiveLayoutPortrait-Large.png#lightbox)

在横向模式下，按钮数组可以移到一边，并显示在列中：

[![VSM 自适应布局：横向](vsm-images/VsmAdaptiveLayoutLandscape.png "VSM 自适应布局-横向")](vsm-images/VsmAdaptiveLayoutLandscape-Large.png#lightbox)

从上到下，程序正在通用 Windows 平台、Android 和 iOS 上运行。

[VsmDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)示例中的 " **VSM 自适应布局**" 页面定义名为 "OrientationStates" 的组，其中包含两个名为 "纵向" 和 "横向" 的可视状态。 （更复杂的方法可能基于几个不同的页面或窗口宽度。）

VSM 标记出现在 XAML 文件中的四个位置。 指定`StackLayout` `mainStack`的包含了菜单和内容，这是一个`Image`元素。 这`StackLayout`应在纵向模式下具有垂直方向，在横向模式下为水平方向：

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="VsmDemos.VsmAdaptiveLayoutPage"
             Title="VSM Adaptive Layout">

    <StackLayout x:Name="mainStack">
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup Name="OrientationStates">
                <VisualState Name="Portrait">
                    <VisualState.Setters>
                        <Setter Property="Orientation" Value="Vertical" />
                    </VisualState.Setters>
                </VisualState>
                <VisualState Name="Landscape">
                    <VisualState.Setters>
                        <Setter Property="Orientation" Value="Horizontal" />
                    </VisualState.Setters>
                </VisualState>
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>

        <ScrollView x:Name="menuScroll">
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup Name="OrientationStates">
                    <VisualState Name="Portrait">
                        <VisualState.Setters>
                            <Setter Property="Orientation" Value="Horizontal" />
                        </VisualState.Setters>
                    </VisualState>
                    <VisualState Name="Landscape">
                        <VisualState.Setters>
                            <Setter Property="Orientation" Value="Vertical" />
                        </VisualState.Setters>
                    </VisualState>
                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>

            <StackLayout x:Name="menuStack">
                <VisualStateManager.VisualStateGroups>
                    <VisualStateGroup Name="OrientationStates">
                        <VisualState Name="Portrait">
                            <VisualState.Setters>
                                <Setter Property="Orientation" Value="Horizontal" />
                            </VisualState.Setters>
                        </VisualState>
                        <VisualState Name="Landscape">
                            <VisualState.Setters>
                                <Setter Property="Orientation" Value="Vertical" />
                            </VisualState.Setters>
                        </VisualState>
                    </VisualStateGroup>
                </VisualStateManager.VisualStateGroups>

                <StackLayout.Resources>
                    <Style TargetType="Button">
                        <Setter Property="VisualStateManager.VisualStateGroups">
                            <VisualStateGroupList>
                                <VisualStateGroup Name="OrientationStates">
                                    <VisualState Name="Portrait">
                                        <VisualState.Setters>
                                            <Setter Property="HorizontalOptions" Value="CenterAndExpand" />
                                            <Setter Property="Margin" Value="10, 5" />
                                        </VisualState.Setters>
                                    </VisualState>
                                    <VisualState Name="Landscape">
                                        <VisualState.Setters>
                                            <Setter Property="VerticalOptions" Value="CenterAndExpand" />
                                            <Setter Property="HorizontalOptions" Value="Center" />
                                            <Setter Property="Margin" Value="10" />
                                        </VisualState.Setters>
                                    </VisualState>
                                </VisualStateGroup>
                            </VisualStateGroupList>
                        </Setter>
                    </Style>
                </StackLayout.Resources>

                <Button Text="Banana"
                        Command="{Binding SelectedCommand}"
                        CommandParameter="Banana.jpg" />
                <Button Text="Face Palm"
                        Command="{Binding SelectedCommand}"
                        CommandParameter="FacePalm.jpg" />
                <Button Text="Monkey"
                        Command="{Binding SelectedCommand}"
                        CommandParameter="monkey.png" />
                <Button Text="Seated Monkey"
                        Command="{Binding SelectedCommand}"
                        CommandParameter="SeatedMonkey.jpg" />
            </StackLayout>
        </ScrollView>

        <Image x:Name="image"
               VerticalOptions="FillAndExpand"
               HorizontalOptions="FillAndExpand" />
    </StackLayout>
</ContentPage>
```

名为`ScrollView` `menuScroll`的内部， `StackLayout`命名`menuStack`为实现按钮菜单。 这些布局的方向相反`mainStack`。 菜单应在纵向模式下水平滚动，在横向模式下垂直。

VSM 标记的第四部分是按钮本身的隐式样式。 此标记将`VerticalOptions`设置`HorizontalOptions`、和`Margin`属性，这些属性特定于纵向和横向方向。

`BindingContext`代码隐藏文件设置的`menuStack`属性以实现`Button`命令，并且还将处理程序附加到页面的`SizeChanged`事件：

```csharp
public partial class VsmAdaptiveLayoutPage : ContentPage
{
    public VsmAdaptiveLayoutPage ()
    {
        InitializeComponent ();

        SizeChanged += (sender, args) =>
        {
            string visualState = Width > Height ? "Landscape" : "Portrait";
            VisualStateManager.GoToState(mainStack, visualState);
            VisualStateManager.GoToState(menuScroll, visualState);
            VisualStateManager.GoToState(menuStack, visualState);

            foreach (View child in menuStack.Children)
            {
                VisualStateManager.GoToState(child, visualState);
            }
        };

        SelectedCommand = new Command<string>((filename) =>
        {
            image.Source = ImageSource.FromResource("VsmDemos.Images." + filename);
        });

        menuStack.BindingContext = this;
    }

    public ICommand SelectedCommand { private set; get; }
}
```

`SizeChanged`处理程序将`VisualStateManager.GoToState`调用两个`StackLayout`和`ScrollView`两个元素，然后循环遍历的`menuStack`子级以调用`VisualStateManager.GoToState` `Button`元素。

这似乎是代码隐藏文件可以通过设置 XAML 文件中元素的属性来更直接地处理方向更改，但视觉对象状态管理器是一种结构化方法。 所有视觉对象都保存在 XAML 文件中，在此文件中，它们会更易于检查、维护和修改。

## <a name="visual-state-manager-with-xamarinuniversity"></a>具有 Xamarin 大学的视觉对象状态管理器

> [!VIDEO https://youtube.com/embed/qhUHbVP5mIQ]

**Xamarin 3.0 视觉对象状态管理器视频**

## <a name="related-links"></a>相关链接

- [VsmDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)
- [状态触发器](~/xamarin-forms/app-fundamentals/triggers.md#state-triggers)
