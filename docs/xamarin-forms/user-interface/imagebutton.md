---
title: Xamarin.Forms ImageButton
description: ImageButton 显示图像和响应点击或单击，将定向的应用程序来执行特定任务。
ms.prod: xamarin
ms.assetid: B5906AB6-3F79-4FCB-8C78-1F0AF18AB39E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/04/2019
ms.openlocfilehash: 7c6647a0299b5ece3caaaa1d322ec1a0efac3557
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/13/2020
ms.locfileid: "79305713"
---
# <a name="xamarinforms-imagebutton"></a>Xamarin.Forms ImageButton

[![下载示例](~/media/shared/download.png) 下载示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)

_ImageButton 会显示一个图像，并对指示应用程序的点击或单击进行响应，以执行特定任务。_

`ImageButton` 视图将[`Button`](xref:Xamarin.Forms.Button)视图和[`Image`](xref:Xamarin.Forms.Image)视图组合在一起，以创建其内容为图像的按钮。 用户按下了手指或用鼠标单击 `ImageButton`，以指示应用程序执行特定任务。 但是，与 `Button` 视图不同，`ImageButton` 视图没有文本和文本外观的概念。

> [!NOTE]
> 虽然[`Button`](xref:Xamarin.Forms.Button)视图定义了一个[`Image`](xref:Xamarin.Forms.Button.Image)属性，该属性允许您在 `Button`上显示图像，但当在 `Button` 文本旁显示一个小图标时，将使用此属性。

本指南中的代码示例摘自[FormsGallery 示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)。

## <a name="setting-the-image-source"></a>设置图像源

`ImageButton` 定义一个 `Source` 属性，该属性应设置为要在按钮中显示的图像，图像源是文件、URI、资源或流。 有关从不同源加载图像的详细信息，请参阅[Xamarin 中的图像](images.md)。

下面的示例演示如何在 XAML 中实例化 `ImageButton`：

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="FormsGallery.XamlExamples.ImageButtonDemoPage"
             Title="ImageButton Demo">
    <StackLayout>
        <Label Text="ImageButton"
               FontSize="50"
               FontAttributes="Bold"
               HorizontalOptions="Center" />

       <ImageButton Source="XamarinLogo.png"
                    HorizontalOptions="Center"
                    VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

`Source` 属性指定显示在 `ImageButton`中的图像。 在此示例中将它设置为本地文件将加载从每个平台项目，从而导致下面的屏幕截图：

[![基本 ImageButton](imagebutton-images/BasicImageButton.png "基本 ImageButton")](imagebutton-images/BasicImageButton-Large.png#lightbox "基本 ImageButton")

默认情况下，`ImageButton` 是矩形，但您可以通过使用 `CornerRadius` 属性为其指定圆角。 有关 `ImageButton` 外观的详细信息，请参阅[ImageButton 外观](#imagebutton-appearance)。

> [!NOTE]
> 虽然 `ImageButton` 可以加载动态 GIF，但它只显示 GIF 的第一帧。

下面的示例演示如何创建在功能上等效于上一个 XAML 示例，但完全在页C#:

```csharp
public class ImageButtonDemoPage : ContentPage
{
    public ImageButtonDemoPage()
    {
        Label header = new Label
        {
            Text = "ImageButton",
            FontSize = 50,
            FontAttributes = FontAttributes.Bold,
            HorizontalOptions = LayoutOptions.Center
        };

        ImageButton imageButton = new ImageButton
        {
            Source = "XamarinLogo.png",
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.CenterAndExpand
        };

        // Build the page.
        Title = "ImageButton Demo";
        Content = new StackLayout
        {
            Children = { header, imageButton }
        };
    }
}
```

## <a name="handling-imagebutton-clicks"></a>处理 ImageButton 单击

`ImageButton` 定义一个 `Clicked` 事件，当用户使用手指或鼠标指针点击 `ImageButton` 时，将触发该事件。 当从 `ImageButton`的表面释放手指或鼠标按钮时，将触发事件。 `ImageButton` 必须将其 `IsEnabled` 属性设置为 "`true`" 以响应点击。

下面的示例演示如何在 XAML 中实例化 `ImageButton` 并处理其 `Clicked` 事件：

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="FormsGallery.XamlExamples.ImageButtonDemoPage"
             Title="ImageButton Demo">
    <StackLayout>
        <Label Text="ImageButton"
               FontSize="50"
               FontAttributes="Bold"
               HorizontalOptions="Center" />

       <ImageButton Source="XamarinLogo.png"
                    HorizontalOptions="Center"
                    VerticalOptions="CenterAndExpand"
                    Clicked="OnImageButtonClicked" />

        <Label x:Name="label"
               Text="0 ImageButton clicks"
               FontSize="Large"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

`Clicked` 事件设置为一个名为 `OnImageButtonClicked` 的事件处理程序，该处理程序位于代码隐藏文件中：

```csharp
public partial class ImageButtonDemoPage : ContentPage
{
    int clickTotal;

    public ImageButtonDemoPage()
    {
        InitializeComponent();
    }

    void OnImageButtonClicked(object sender, EventArgs e)
    {
        clickTotal += 1;
        label.Text = $"{clickTotal} ImageButton click{(clickTotal == 1 ? "" : "s")}";
    }
}
```

点击 `ImageButton` 时，`OnImageButtonClicked` 方法执行。 `sender` 参数是负责此事件的 `ImageButton`。 您可以使用此访问 `ImageButton` 对象，或区分共享同一 `Clicked` 事件的多个 `ImageButton` 对象。

此特定 `Clicked` 处理程序会递增计数器，并在[`Label`](xref:Xamarin.Forms.Label)中显示计数器值：

[![基本 ImageButton 单击](imagebutton-images/ImageButton.png "基本 ImageButton 单击")](imagebutton-images/ImageButton-Large.png#lightbox "基本 ImageButton 单击")

下面的示例演示如何创建在功能上等效于上一个 XAML 示例，但完全在页C#:

```csharp
public class ImageButtonDemoPage : ContentPage
{
    Label label;
    int clickTotal = 0;

    public ImageButtonDemoPage()
    {
        Label header = new Label
        {
            Text = "ImageButton",
            FontSize = 50,
            FontAttributes = FontAttributes.Bold,
            HorizontalOptions = LayoutOptions.Center
        };

        ImageButton imageButton = new ImageButton
        {
            Source = "XamarinLogo.png",
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.CenterAndExpand
        };
        imageButton.Clicked += OnImageButtonClicked;

        label = new Label
        {
            Text = "0 ImageButton clicks",
            FontSize = Device.GetNamedSize(NamedSize.Large, typeof(Label)),
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.CenterAndExpand
        };

        // Build the page.
        Title = "ImageButton Demo";
        Content = new StackLayout
        {
            Children =
            {
                header,
                imageButton,
                label
            }
        };
    }

    void OnImageButtonClicked(object sender, EventArgs e)
    {
        clickTotal += 1;
        label.Text = $"{clickTotal} ImageButton click{(clickTotal == 1 ? "" : "s")}";
    }
}
```

## <a name="disabling-the-imagebutton"></a>禁用 ImageButton

有时，应用程序处于特定状态，其中特定 `ImageButton` 单击是无效操作。 在这些情况下，应通过将 `ImageButton` 的 `IsEnabled` 属性设置为 `false`来禁用。

## <a name="using-the-command-interface"></a>使用命令界面

应用程序可以响应 `ImageButton` 点击，而不处理 `Clicked` 事件。 `ImageButton` 实现了一种称为_命令或命令_性接口_commanding_的替代通知机制。 这包括两个属性：

- [`ICommand`](xref:System.Windows.Input.ICommand)类型的 `Command`， [`System.Windows.Input`](xref:System.Windows.Input)命名空间中定义了一个接口。
- [`Object`](xref:System.Object)类型的 `CommandParameter` 属性。

尤其是在实现模型-视图-视图模型 (MVVM) 体系结构时，此方法非常适合与数据绑定和。

有关使用命令界面的详细信息，请参阅在[按钮](button.md)指南中[使用命令界面](button.md#using-the-command-interface)。

## <a name="pressing-and-releasing-the-imagebutton"></a>按下和释放 ImageButton

除了 `Clicked` 事件，`ImageButton` 还定义 `Pressed` 和 `Released` 事件。 当鼠标按 `ImageButton`上，或在鼠标指针位于 `ImageButton`上并按下鼠标按钮时，将发生 `Pressed` 事件。 当鼠标右键或鼠标松开时，会发生 `Released` 事件。 通常情况下，`Clicked` 事件同时与 `Released` 事件同时触发，但是，如果手指或鼠标指针在被释放之前离开 `ImageButton` 表面，则 `Clicked` 事件可能不会发生。

有关这些事件的详细信息，请参阅[按下并松开](button.md#pressing-and-releasing-the-button)[按钮](button.md)指南中的按钮。

## <a name="imagebutton-appearance"></a>ImageButton 外观

除了 `ImageButton` 继承自[`View`](xref:Xamarin.Forms.View)类的属性以外，`ImageButton` 还定义了多个影响其外观的属性：

- `Aspect` 是调整图像大小以适应显示区域的方式。
- `BorderColor` 是围绕 `ImageButton`的区域的颜色。
- `BorderWidth` 是边框的宽度。
- `CornerRadius` 是 `ImageButton`的角半径。

`Aspect` 属性可以设置为[`Aspect`](xref:Xamarin.Forms.Aspect)枚举的成员之一：

- [`Fill`](xref:Xamarin.Forms.Aspect.Fill) -将图像拉伸到完全完全填充 `ImageButton`。 这可能会导致被扭曲图像。
- [`AspectFill`](xref:Xamarin.Forms.Aspect.AspectFill)剪切图像，使其填充 `ImageButton`，同时保留纵横比。
- [`AspectFit`](xref:Xamarin.Forms.Aspect.AspectFit) -letterboxes 图像（如有必要），使整个图像适合 `ImageButton`，并根据图像的宽度或高度，将空白区域添加到顶部/底部或边缘。 这是[`Aspect`](xref:Xamarin.Forms.Aspect)枚举的默认值。

> [!NOTE]
> `ImageButton` 类还具有控制 `ImageButton`布局行为的[`Margin`](xref:Xamarin.Forms.View.Margin)和 `Padding` 属性。 有关详细信息，请参阅[边距和填充](~/xamarin-forms/user-interface/layouts/margin-and-padding.md)。

## <a name="imagebutton-visual-states"></a>ImageButton 可视状态

`ImageButton` 具有 `Pressed` [`VisualState`](xref:Xamarin.Forms.VisualState) ，可用于在用户按下时启动对 `ImageButton` 的视觉更改（前提是已启用）。

下面的 XAML 示例演示如何为 `Pressed` 状态定义可视状态：

```xaml
<ImageButton Source="XamarinLogo.png"
             ...>
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="Normal">
                <VisualState.Setters>
                    <Setter Property="Scale"
                            Value="1" />
                </VisualState.Setters>
            </VisualState>

            <VisualState x:Name="Pressed">
                <VisualState.Setters>
                    <Setter Property="Scale"
                            Value="0.8" />
                </VisualState.Setters>
            </VisualState>

        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</ImageButton>
```

`Pressed` [`VisualState`](xref:Xamarin.Forms.VisualState)指定在按下 `ImageButton` 时，其[`Scale`](xref:Xamarin.Forms.VisualElement.Scale)属性将从其默认值1更改为0.8。 `Normal` `VisualState` 指定当 `ImageButton` 处于正常状态时，其 `Scale` 属性将设置为1。 因此，整体效果是，在按下 `ImageButton` 时，重新缩放会略小一些，释放 `ImageButton` 时，它将被重新缩放为其默认大小。

有关可视状态的详细信息，请参阅[Xamarin。窗体可视状态管理器](~/xamarin-forms/user-interface/visual-state-manager.md)。

## <a name="related-links"></a>相关链接

- [FormsGallery 示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)
