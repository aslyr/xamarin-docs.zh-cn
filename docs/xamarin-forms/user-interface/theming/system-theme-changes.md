---
标题： "响应应用程序中的系统主题更改 Xamarin.Forms " 说明： " Xamarin.Forms 应用程序可以通过使用 OnAppTheme 类型和 DynamicResource 标记扩展来响应操作系统主题更改。"
assetid： D10506DD-BAA0-437F-A4AD-882D16E7B60D： xamarin： xamarin ms. 技术： xamarin 窗体作者： davidbritch： dabritch ms. 日期：04/22/2020： [ Xamarin.Forms ， Xamarin.Essentials ]
---

# <a name="respond-to-system-theme-changes-in-xamarinforms-applications"></a>响应应用程序中的系统主题更改 Xamarin.Forms

[![下载示例](~/media/shared/download.png) 下载示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-systemthemesdemo/)

设备通常包括浅色和深色主题，它们分别引用可在操作系统级别设置的各种外观首选项。 应用程序应遵守这些系统主题，并在系统主题发生更改时立即做出响应。

根据设备配置的不同原因，系统主题可能会发生变化。 这包括用户显式更改的系统主题，它因一天中的某个时间而变化，并因环境因素（如低亮度）而变化。

Xamarin.Forms应用程序可以通过使用 `AppThemeColor` 类、 `OnAppTheme<T>` 类和标记扩展来定义资源来响应系统主题更改 `OnAppTheme` 。 然后，应使用标记扩展来使用这些资源 `DynamicResource` 。

> [!IMPORTANT]
> 对系统主题更改的响应当前为试验性，只能通过设置标志来使用 `AppTheme_Experimental` 。 有关详细信息，请参阅[实验标志](~/xamarin-forms/internals/experimental-flags.md)。

Xamarin.Forms为了响应系统主题更改，必须满足以下要求：

- Xamarin.Forms4.6 或更高版本。
- iOS 13 或更高版本。
- Android 10 （API 29）或更高版本。
- UWP 版本14393或更高版本。

以下屏幕截图显示了主题页面，适用于 iOS 和 Android 上的轻型和深色系统主题：

[![IOS 和 Android](system-theme-changes-images/main-page-both-themes.png "主题应用的主页")](system-theme-changes-images/main-page-both-themes-large.png#lightbox "主题应用的主页") 
 上主题应用的主页的屏幕截图[ ![IOS 和 Android 上的主题应用的详细信息页的屏幕截图](system-theme-changes-images/detail-page-both-themes.png "主题应用的详细信息页")](system-theme-changes-images/detail-page-both-themes-large.png#lightbox "主题应用的详细信息页")

## <a name="define-and-consume-theme-resources"></a>定义和使用主题资源

适用于浅色和深色主题的资源可通过 `AppThemeColor` 类、 `OnAppTheme<T>` 类和标记扩展进行定义 `OnAppTheme` 。 对于每种方法，将根据当前系统主题的值自动应用这些资源。 此外，如果在应用程序运行时系统主题发生更改，则会自动更新使用这些资源的对象。

### <a name="appthemecolor"></a>AppThemeColor

`AppThemeColor`类用于定义 [`Color`](xref:Xamarin.Forms.Color) 轻薄系统主题的资源。 `AppThemeColor`应在中定义资源 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) ：

```xaml
<Application ...>
    <Application.Resources>
        <AppThemeColor x:Key="PageBackgroundColor"
                       Light="White"
                       Dark="Black" />
        <AppThemeColor x:Key="NavigationBarColor"
                       Light="WhiteSmoke"
                       Dark="Teal" />
        <AppThemeColor x:Key="PrimaryColor"
                       Light="WhiteSmoke"
                       Dark="Teal" />
        <AppThemeColor x:Key="SecondaryColor"
                       Light="Black"
                       Dark="White" />
        <AppThemeColor x:Key="PrimaryTextColor"
                       Light="Black"
                       Dark="White" />
        <AppThemeColor x:Key="SecondaryTextColor"
                       Light="White"
                       Dark="White" />
        <AppThemeColor x:Key="TertiaryTextColor"
                       Light="Gray"
                       Dark="WhiteSmoke" />
        <AppThemeColor x:Key="TransparentColor"
                       Light="Transparent"
                       Dark="Transparent" />
    </Application.Resources>
</Application>
```

每个 `AppThemeColor` 资源都必须具有一个 `x:Key` 属性，该属性在中为其提供一个描述性密钥 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 。 和属性的值 `Light` `Dark` 应为 [`Color`](xref:Xamarin.Forms.Color) 对象。 此外， `Default` 可以将属性设置为 `Color` ，以供使用对象默认使用。

`AppThemeColor`资源可以内联使用：

```xaml
<Label Text="This monkey reacts appropriately to ridiculous assertions and actions"
       TextColor="{DynamicResource PrimaryTextColor}" />
```

或者， `AppThemeColor` 可通过隐式或显式对象使用资源 [`Style`](xref:Xamarin.Forms.Style) ：

```xaml
<Style TargetType="NavigationPage">
    <Setter Property="BarBackgroundColor"
            Value="{DynamicResource NavigationBarColor}" />
    <Setter Property="BarTextColor"
            Value="{DynamicResource SecondaryColor}" />
</Style>
```

> [!IMPORTANT]
> `AppThemeColor`应使用标记扩展来使用资源 `DynamicResource` 。 这可确保在系统主题发生更改时，使用对象的外观会更新。

### <a name="onappthemelttgt"></a>OnAppTheme &lt; T&gt;

`OnAppTheme<T>`类用于为光速和深色系统主题定义任意类型的资源。 `OnAppTheme<T>`应在中定义资源 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) ，并将 `T` 参数指定为特性的值 `x:TypeArguments` ：

```xaml
<Application ...>
    <Application.Resources>
        <OnAppTheme x:Key="ImageLogo"
                    x:TypeArguments="FileImageSource"
                    Light="lightlogo.png"
                    Dark="darklogo.png" />
    </Application.Resources>
</Application>
```

每个 `OnAppTheme<T>` 资源都必须具有一个 `x:Key` 属性，该属性在中为其提供一个描述性密钥 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 。 和属性的值 `Light` `Dark` 应为定义为特性的类型的对象 `x:TypeArguments` 。 此外， `Default` 可以将属性设置为 `T` 默认情况下使用的对象类型的对象。

`OnAppTheme<T>`资源可以内联使用：

```xaml
<Image Source="{DynamicResource ImageLogo}"
       Aspect="AspectFit"
       HeightRequest="200" /
```

或者， `OnAppTheme<T>` 可通过隐式或显式对象使用资源 [`Style`](xref:Xamarin.Forms.Style) ：

```xaml
<Style x:Key="imageLogoStyle"
       TargetType="Image">
    <Setter Property="Source"
            Value="{DynamicResource ImageLogo}" />
    <Setter Property="Aspect"
            Value="AspectFit" />
</Style>
```

> [!IMPORTANT]
> `OnAppTheme<T>`应使用标记扩展来使用资源 `DynamicResource` 。 这可确保在系统主题发生更改时，使用对象的外观会更新。

### <a name="onapptheme-markup-extension"></a>OnAppTheme 标记扩展

`OnAppTheme`标记扩展使你能够根据当前系统主题指定要使用的资源（如图像或颜色）。 它提供与类相同的功能 `OnAppTheme<T>` ，但具有更简洁的表示形式：

```xaml
<ContentPage ...>
    <StackLayout Margin="20">
        <Label Text="This text is green in light mode, and red in dark mode."
               TextColor="{OnAppTheme Light=Green, Dark=Red}" />
        <Image Source="{OnAppTheme Light=lightlogo.png, Dark=darklogo.png}" />
    </StackLayout>
</ContentPage>
```

在此示例中， [`Label`](xref:Xamarin.Forms.Label) 当设备使用光源主题时，第一个的文本颜色设置为绿色; 当设备使用其深主题时，将设置为红色。 同样， [`Image`](xref:Xamarin.Forms.Image) 根据当前系统主题显示不同的图像文件。

有关标记扩展的详细信息 `OnAppTheme` ，请参阅[OnAppTheme 标记扩展](~/xamarin-forms/xaml/markup-extensions/consuming.md#onapptheme-markup-extension)。

## <a name="detect-the-current-system-theme"></a>检测当前系统主题

可以通过获取属性的值来检测当前系统主题 `Application.RequestedTheme` ：

```csharp
OSAppTheme currentTheme = Application.Current.RequestedTheme;
```

`RequestedTheme`属性返回 `OSAppTheme` 枚举成员。 `OSAppTheme` 枚举定义下列成员：

- `Unspecified`，它指示设备使用未指定的主题。
- `Light`，它指示设备使用的是光源主题。
- `Dark`，它指示设备使用的是深色主题。

## <a name="react-to-theme-changes"></a>主题更改的响应

设备上的系统主题可能会因多种原因而发生变化，这取决于设备的配置方式。 Xamarin.Forms当系统主题通过处理事件进行更改时，可以通知应用 `Application.RequestedThemeChanged` ：

```csharp
Application.Current.RequestedThemeChanged += (s, a) =>
{
    // Respond to the theme change
};
```

`AppThemeChangedEventArgs`事件附带的对象 `RequestedThemeChanged` 具有名为的单个属性 `RequestedTheme` ，类型为 `OSAppTheme` 。 可以检查此属性以检测请求的系统主题。

## <a name="related-links"></a>相关链接

- [SystemThemes （示例）](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-systemthemesdemo/)
- [OnAppTheme 标记扩展](~/xamarin-forms/xaml/markup-extensions/consuming.md#onapptheme-markup-extension)
- [资源字典](~/xamarin-forms/xaml/resource-dictionaries.md)
- [中的动态样式Xamarin.Forms](~/xamarin-forms/user-interface/styles/xaml/dynamic.md)
- [使用 XAML 样式设置 Xamarin.Forms 应用的样式](~/xamarin-forms/user-interface/styles/xaml/index.md)
