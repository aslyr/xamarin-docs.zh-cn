---
title： "样式简介 Xamarin.Forms " 说明： "样式允许自定义视觉对象的外观。 样式是针对特定类型定义的，包含该类型的可用属性的值。 "
ms-chap： xamarin assetid：3FF899C0-6CFB-4C1D-837D-9E9E10181967： xamarin 窗体作者： davidbritch： dabritch ms. 日期：04/27/2016 非 loc： [ Xamarin.Forms ， Xamarin.Essentials ]
---

# <a name="introduction-to-xamarinforms-styles"></a>样式简介 Xamarin.Forms

_样式允许自定义视觉对象的外观。样式是针对特定类型定义的，其中包含可用于该类型的属性的值。_

Xamarin.Forms应用程序通常包含多个具有相同外观的控件。 例如，应用程序可能有多个 [`Label`](xref:Xamarin.Forms.Label) 实例，这些实例具有相同的字体选项和布局选项，如下面的 XAML 代码示例所示：

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    x:Class="Styles.NoStylesPage"
    Title="No Styles"
    IconImageSource="xaml.png">
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <Label Text="These labels"
                   HorizontalOptions="Center"
                   VerticalOptions="CenterAndExpand"
                   FontSize="Large" />
            <Label Text="are not"
                   HorizontalOptions="Center"
                   VerticalOptions="CenterAndExpand"
                   FontSize="Large" />
            <Label Text="using styles"
                   HorizontalOptions="Center"
                   VerticalOptions="CenterAndExpand"
                   FontSize="Large" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

以下代码示例演示了在 C# 中创建的等效页：

```csharp
public class NoStylesPageCS : ContentPage
{
    public NoStylesPageCS ()
    {
        Title = "No Styles";
        IconImageSource = "csharp.png";
        Padding = new Thickness (0, 20, 0, 0);

        Content = new StackLayout {
            Children = {
                new Label {
                    Text = "These labels",
                    HorizontalOptions = LayoutOptions.Center,
                    VerticalOptions = LayoutOptions.CenterAndExpand,
                    FontSize = Device.GetNamedSize (NamedSize.Large, typeof(Label))
                },
                new Label {
                    Text = "are not",
                    HorizontalOptions = LayoutOptions.Center,
                    VerticalOptions = LayoutOptions.CenterAndExpand,
                    FontSize = Device.GetNamedSize (NamedSize.Large, typeof(Label))
                },
                new Label {
                    Text = "using styles",
                    HorizontalOptions = LayoutOptions.Center,
                    VerticalOptions = LayoutOptions.CenterAndExpand,
                    FontSize = Device.GetNamedSize (NamedSize.Large, typeof(Label))
                }
            }
        };
    }
}
```

每个 [`Label`](xref:Xamarin.Forms.Label) 实例都具有相同的属性值，用于控制所显示的文本的外观 `Label` 。 这会导致如以下屏幕截图中所示的外观：

[![不带样式的标签外观](introduction-images/no-styles.png)](introduction-images/no-styles-large.png#lightbox)

设置各个控件的外观可能是重复性的并且容易出错。 相反，可以创建一个样式来定义外观，然后将其应用于所需的控件。

## <a name="create-a-style"></a>创建样式

[`Style`](xref:Xamarin.Forms.Style) 类将属性值的集合分组到一个对象中，然后可以将该对象应用于多个视觉对象元素实例。 这有助于减少重复的标记，并允许更轻松地更改应用程序的外观。

虽然样式主要设计用于基于 XAML 的应用程序，但也可在 c # 中创建：

- [`Style`](xref:Xamarin.Forms.Style)通常会在为 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) [`Resources`](xref:Xamarin.Forms.VisualElement.Resources) 控件、页或应用程序集合分配的中定义在 XAML 中创建的实例 [`Resources`](xref:Xamarin.Forms.Application.Resources) 。
- [`Style`](xref:Xamarin.Forms.Style)用 c # 创建的实例通常在页的类中定义，或在可全局访问的类中定义。

选择在何处定义 [`Style`](xref:Xamarin.Forms.Style) 会影响其应用范围：

- [`Style`](xref:Xamarin.Forms.Style)在控件级别定义的实例只能应用于控件及其子级。
- [`Style`](xref:Xamarin.Forms.Style)在页面级别定义的实例只能应用于页面及其子页面。
- 在应用程序级别定义的 [`Style`](xref:Xamarin.Forms.Style) 实例可以应用于整个应用程序。

每个 [`Style`](xref:Xamarin.Forms.Style) 实例都包含一个或多个 [`Setter`](xref:Xamarin.Forms.Setter) 对象的集合，其中每个 `Setter` 都具有 [`Property`](xref:Xamarin.Forms.Setter.Property) 和 [`Value`](xref:Xamarin.Forms.Setter.Value)。 `Property` 是应用样式的元素的可绑定属性的名称，而 `Value` 是应用于属性的值。

每个 [`Style`](xref:Xamarin.Forms.Style) 实例可以为*显式*或*隐式*：

- *显式* [`Style`](xref:Xamarin.Forms.Style) 实例是通过指定 [`TargetType`](xref:Xamarin.Forms.Style.TargetType) 和 `x:Key` 值，并将目标元素的 [`Style`](xref:Xamarin.Forms.NavigableElement.Style) 属性设置为 `x:Key` 引用来定义的。 有关*显式*样式的详细信息，请参阅[显式样式](~/xamarin-forms/user-interface/styles/explicit.md)。
- *隐式* [`Style`](xref:Xamarin.Forms.Style) 实例是通过仅指定指定的来定义的 [`TargetType`](xref:Xamarin.Forms.Style.TargetType) 。 `Style`然后，实例将自动应用于该类型的所有元素。 请注意，的子类 `TargetType` 不会自动 `Style` 应用。 有关*隐式*样式的详细信息，请参阅[隐式样式](~/xamarin-forms/user-interface/styles/implicit.md)。

创建 [`Style`](xref:Xamarin.Forms.Style) 时，始终需要 [`TargetType`](xref:Xamarin.Forms.Style.TargetType) 属性。 下面的代码示例演示了在 XAML 中创建的*显式*样式（注意 `x:Key` ）：

```xaml
<Style x:Key="labelStyle" TargetType="Label">
    <Setter Property="HorizontalOptions" Value="Center" />
    <Setter Property="VerticalOptions" Value="CenterAndExpand" />
    <Setter Property="FontSize" Value="Large" />
</Style>
```

若要应用 `Style` ，目标对象必须是 [`VisualElement`](xref:Xamarin.Forms.VisualElement) 与的属性值相匹配的， [`TargetType`](xref:Xamarin.Forms.Style.TargetType) `Style` 如下面的 XAML 代码示例所示：

```xaml
<Label Text="Demonstrating an explicit style" Style="{StaticResource labelStyle}" />
```

视图层次结构中较低的样式优先于定义较高的样式。 例如，在 [`Style`](xref:Xamarin.Forms.Style) [`Label.TextColor`](xref:Xamarin.Forms.Label.TextColor) 应用程序级别设置为的设置将被设置为的 `Red` 页级别样式重写 `Label.TextColor` `Green` 。 同样，控件级样式将重写页面级别样式。 此外，如果 `Label.TextColor` 直接在控件属性上设置，则这将优先于任何样式。

本节中的文章演示并说明如何创建和应用*显式*样式和*隐式*样式，如何创建全局样式，样式继承，如何在运行时响应样式更改，以及如何使用中包含的内置样式 Xamarin.Forms 。

> [!NOTE]
> **什么是 StyleId？**
>
> 在 Xamarin.Forms 2.2 之前， [`StyleId`](xref:Xamarin.Forms.Element.StyleId) 属性用于识别应用程序中的单个元素，用于标识 UI 测试中的标识和主题引擎（如 Pixate）。 但是， Xamarin.Forms 2.2 引入了 [`AutomationId`](xref:Xamarin.Forms.Element.AutomationId) 属性，该属性取代了 [`StyleId`](xref:Xamarin.Forms.Element.StyleId) 属性。

## <a name="related-links"></a>相关链接

- [XAML 标记扩展](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [样式](xref:Xamarin.Forms.Style)
- [](xref:Xamarin.Forms.Setter)Setter
