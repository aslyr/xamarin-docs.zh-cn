---
标题： " Xamarin.Forms description：" 中的设备样式在 Xamarin.Forms Device 类中包含六个动态样式（称为设备样式）。 本文介绍如何在应用程序中使用设备样式 Xamarin.Forms 。
ms-chap： xamarin assetid：7FF19ED1-0822-4238-9435-AD970317A2F8： xamarin 窗体作者： davidbritch： dabritch ms. 日期：02/17/2016 非 loc： [ Xamarin.Forms ， Xamarin.Essentials ]
---

# <a name="device-styles-in-xamarinforms"></a>设备样式Xamarin.Forms

[![下载示例](~/media/shared/download.png) 下载示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-dynamicstyles)

_Xamarin 在 Device 类中包含六种动态样式，称为设备样式。_

*设备*样式为：

- [`BodyStyle`](xref:Xamarin.Forms.Device.Styles.BodyStyle)
- [`CaptionStyle`](xref:Xamarin.Forms.Device.Styles.CaptionStyle)
- [`ListItemDetailTextStyle`](xref:Xamarin.Forms.Device.Styles.ListItemDetailTextStyle)
- [`ListItemTextStyle`](xref:Xamarin.Forms.Device.Styles.ListItemTextStyle)
- [`SubtitleStyle`](xref:Xamarin.Forms.Device.Styles.SubtitleStyle)
- [`TitleStyle`](xref:Xamarin.Forms.Device.Styles.TitleStyle)

所有六个样式只能应用于 [`Label`](xref:Xamarin.Forms.Label) 实例。 例如， `Label` 显示段落正文的可能会将其 [`Style`](xref:Xamarin.Forms.NavigableElement.Style) 属性设置为 [`BodyStyle`](xref:Xamarin.Forms.Device.Styles.BodyStyle) 。

下面的代码示例演示如何在 XAML 页中使用*设备*样式：

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.DeviceStylesPage" Title="Device" IconImageSource="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="myBodyStyle" TargetType="Label"
              BaseResourceKey="BodyStyle">
                <Setter Property="TextColor" Value="Accent" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <Label Text="Title style"
              Style="{DynamicResource TitleStyle}" />
            <Label Text="Subtitle text style"
              Style="{DynamicResource SubtitleStyle}" />
            <Label Text="Body style"
              Style="{DynamicResource BodyStyle}" />
            <Label Text="Caption style"
              Style="{DynamicResource CaptionStyle}" />
            <Label Text="List item detail text style"
              Style="{DynamicResource ListItemDetailTextStyle}" />
            <Label Text="List item text style"
              Style="{DynamicResource ListItemTextStyle}" />
            <Label Text="No style" />
            <Label Text="My body style"
              Style="{StaticResource myBodyStyle}" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

使用标记扩展将设备样式绑定到 `DynamicResource` 。 通过更改文本大小的**辅助功能**设置，可在 iOS 中查看样式的动态特性。 *设备*样式的外观在每个平台上都是不同的，如以下屏幕截图所示：

![](device-images/device-styles.png "Device Styles on Each Platform")

*Device*通过将 [`BaseResourceKey`](xref:Xamarin.Forms.Style.BaseResourceKey) 属性设置为设备样式的键名称，也可以从派生设备样式。 在上面的代码示例中， `myBodyStyle` 从继承， [`BodyStyle`](xref:Xamarin.Forms.Device.Styles.BodyStyle) 并设置了重音文本颜色。 有关动态样式继承的详细信息，请参阅[动态样式继承](~/xamarin-forms/user-interface/styles/xaml/dynamic.md#dynamic-style-inheritance)。

下面的代码示例演示了 c # 中的等效页：

```csharp
public class DeviceStylesPageCS : ContentPage
{
    public DeviceStylesPageCS ()
    {
        var myBodyStyle = new Style (typeof(Label)) {
            BaseResourceKey = Device.Styles.BodyStyleKey,
            Setters = {
                new Setter {
                    Property = Label.TextColorProperty,
                    Value = Color.Accent
                }
            }
        };

        Title = "Device";
        IconImageSource = "csharp.png";
        Padding = new Thickness (0, 20, 0, 0);

        Content = new StackLayout {
            Children = {
                new Label { Text = "Title style", Style = Device.Styles.TitleStyle },
                new Label { Text = "Subtitle style", Style = Device.Styles.SubtitleStyle },
                new Label { Text = "Body style", Style = Device.Styles.BodyStyle },
                new Label { Text = "Caption style", Style = Device.Styles.CaptionStyle },
                new Label { Text = "List item detail text style",
                  Style = Device.Styles.ListItemDetailTextStyle },
                new Label { Text = "List item text style", Style = Device.Styles.ListItemTextStyle },
                new Label { Text = "No style" },
                new Label { Text = "My body style", Style = myBodyStyle }
            }
        };
    }
}
```

[`Style`](xref:Xamarin.Forms.NavigableElement.Style)每个实例的属性 [`Label`](xref:Xamarin.Forms.Label) 都设置为类中的相应属性 [`Devices.Styles`](xref:Xamarin.Forms.Device.Styles) 。

## <a name="accessibility"></a>可访问性

*设备*样式遵循辅助功能首选项，因此在每个平台上更改辅助功能首选项时，字体大小将会改变。 因此，为了支持可访问的文本，请确保将*设备*样式用作应用程序内任何文本样式的基础。

以下屏幕截图演示了每个平台上的设备样式，可访问的最小字体大小：

[![](device-images/minimum-size.png "Accessible Small Device Styles on Each Platform")](device-images/minimum-size-large.png#lightbox "Accessible Small Device Styles on Each Platform")

以下屏幕截图演示了每个平台上具有最大可访问字体大小的设备样式：

![](device-images/maximum-size.png "Accessible Large Device Styles on Each Platform")

## <a name="related-links"></a>相关链接

- [文本样式](~/xamarin-forms/user-interface/text/styles.md)
- [XAML 标记扩展](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [动态样式（示例）](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-dynamicstyles)
- [使用样式（示例）](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithstyles)
- [设备样式](xref:Xamarin.Forms.Device.Styles)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
- [样式](xref:Xamarin.Forms.Style)
- [](xref:Xamarin.Forms.Setter)Setter
