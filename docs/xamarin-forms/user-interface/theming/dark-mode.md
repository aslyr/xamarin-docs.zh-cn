---
title: 检测 Xamarin 中的暗模式。
description: 任何 Xamarin.Forms 应用程序中都可以使用资源字典、动态资源和平台知识的组合支持暗模式。
ms.prod: xamarin
ms.assetid: D10506DD-BAA0-437F-A4AD-882D16E7B60D
ms.technology: xamarin-forms
author: davidortinau
ms.author: daortin
ms.date: 03/13/2020
ms.openlocfilehash: 7fe1a98e6a497a5791f26df2fc96d41781b71ef6
ms.sourcegitcommit: b93754b220fca3d6e3d131341e3cfbe233d10f84
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/03/2020
ms.locfileid: "80628315"
---
# <a name="detect-dark-mode-in-xamarinforms-applications"></a>检测 Xamarin 中的暗模式。

Xamarin.Forms应用程序可以使用支持[主题](theming.md)的相同策略来响应操作系统主题更改。 主要区别在于如何触发主题的变化。 暗模式是指可在操作系统级别设置的更广泛的外观首选项集，并且哪些应用程序应立即尊重和响应。

在 Xamarin.Forms 应用程序中使用深色模式的最低要求是：

- iOS 13 或更高。
- Android 9 或更高（Android 9 支持您可以查询的外观模式）。
- Android 10 或更高（Android 10 通知应用模式更改）。
- UWP v10.0.10240.0 或更高。
- Xamarin.窗体（任何版本）。

支持外观模式（包括浅色和深色）的过程如下：

1. 定义整个应用程序在 中[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)共享的资源。
2. 为每个外观模式定义资源字典主题，该模式提供特定于要更改的每个样式的覆盖。
3. 在应用程序的**App.xaml**文件中设置默认外观模式主题。
4. 使用`DynamicResource`标记扩展对应用程序进行样式设置，您希望样式在外观模式更改时做出反应。
5. 添加代码以侦听操作系统主题更改，并加载应用程序的相应主题。

以下屏幕截图显示了主题页面，用于浅色和深色模式：

[![主题应用程序主页的屏幕截图，在 iOS 和 Android 上](theming-images/main-page-both-themes.png "主题应用程序的主页")](theming-images/main-page-both-themes-large.png#lightbox "主题应用程序的主页")
[![主题应用程序的详细信息页面的](theming-images/detail-page-both-themes.png "主题应用程序的详细信息页面")Android 屏幕截图](theming-images/detail-page-both-themes-large.png#lightbox "主题应用程序的详细信息页面")

## <a name="define-themes"></a>定义主题

请按照[主题指南](theming.md)了解有关创建黑暗和浅色主题的逐步详细信息。

您可以在平台代码的适当位置轻松为应用程序设置新主题。 要加载新主题，只需将应用程序的当前资源字典替换为您选择的主题资源字典：

```csharp
App.Current.Resources = new YourDarkTheme();
```

## <a name="detect-and-apply-theme"></a>检测和应用主题

使用[`RequestedTheme`](~/essentials/app-theme.md) [Xamarin.基本（](~/essentials/index.md)版本 1.4.0 或更高版本）可以检测当前运行的主题。 然后，您可以在新类或`App`类中创建帮助器方法来配置主题：

```csharp
public partial class App : Application
{
    public static void ApplyTheme()
    {
        if (AppInfo.RequestedTheme == AppTheme.Dark)
        {
            // Change to dark theme
            // e.g. App.Current.Resources = new YourDarkTheme();
        }
        else
        {
            // Change to light theme
            // e.g. App.Current.Resources = new YourLightTheme();
        }
    }
}
```

## <a name="react-to-appearance-mode-changes"></a>响应外观模式更改

设备上的外观模式可能会由于各种原因而改变，具体取决于用户配置首选项的方式，包括显式选择模式、时间或环境因素（如低光）。 您必须添加平台代码，以确保应用程序能够对这些更改做出反应，以下各节将更详细地讨论此更改。

### <a name="android"></a>Android

要在应用中支持深色模式，必须更新应用的主题，可在 中找到`Resources/values/styles.xml`的主题，以便从`DayNight`主题继承：

```xml
<style name="MainTheme.Base" parent="Theme.AppCompat.DayNight">
```

如果您已升级到 AndroidX[的材料组件](https://www.nuget.org/packages/Xamarin.Google.Android.Material/)（1.1.0-rc2） 或更新，您可以使用：

```xml
<style name="MainTheme.Base" parent="Theme.MaterialComponents.DayNight">
```

在应用程序的**MainActivity.cs**文件中，将`ConfigChanges.UiMode`该字段添加到`ConfigurationChanges``Activity`属性中的属性，以便通知应用 UI 模式更改：

```csharp
[Activity(
    // other settings
    ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation | ConfigChanges.UiMode)]
```

在同一**MainActivity.cs**文件中，重写`OnConfigurationChanged`方法：

```csharp
public override void OnConfigurationChanged(Configuration newConfig)
{
    base.OnConfigurationChanged(newConfig);
    App.ApplyTheme();
}
```

### <a name="ios"></a>iOS

在 iOS 上，在 UIViewController 上通知外观模式更改，在 Xamarin.窗体中，该控制器是`ContentPage`。 为了重写该方法，请在 iOS 项目中创建一个自定义呈现器，`PageRenderer.cs`称为 ：

```csharp
using System;
using UIKit;
using Xamarin.Forms;
using Xamarin.Forms.Platform.iOS;

[assembly: ExportRenderer(typeof(ContentPage), typeof(YourApp.iOS.Renderers.PageRenderer))]
namespace YourApp.iOS.Renderers
{
    public class PageRenderer : Xamarin.Forms.Platform.iOS.PageRenderer
    {
        protected override void OnElementChanged(VisualElementChangedEventArgs e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null || Element == null)
            {
                return;
            }

            try
            {
                App.ApplyTheme();
            }
            catch (Exception ex)
            {
                System.Diagnostics.Debug.WriteLine($"\t\t\tERROR: {ex.Message}");
            }
        }

        public override void TraitCollectionDidChange(UITraitCollection previousTraitCollection)
        {
            base.TraitCollectionDidChange(previousTraitCollection);

            App.ApplyTheme();
        }
    }
}
```

重写`TraitCollectionDidChange`该方法使您能够对`UserInterfaceStyle`更改执行操作。

### <a name="uwp"></a>UWP

在 UWP 上，将以下代码添加到应用程序的**MainPage.xaml.cs**文件中：

```csharp
public sealed partial class MainPage
{
    UISettings uiSettings;

    public MainPage()
    {
        this.InitializeComponent();

        LoadApplication(new YourApp.App());

        uiSettings = new UISettings();
        uiSettings.ColorValuesChanged += ColorValuesChanged;
    }

    private void ColorValuesChanged(UISettings sender, object args)
    {
        Xamarin.Essentials.MainThread.BeginInvokeOnMainThread(() =>
        {
            App.ApplyTheme();
        });
    }
}
```

## <a name="related-links"></a>相关链接

- ["示例"（示例）](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-theming/)
- [资源字典](~/xamarin-forms/xaml/resource-dictionaries.md)
- [Xamarin 中的动态样式。](~/xamarin-forms/user-interface/styles/xaml/dynamic.md)
- [使用 XAML 样式设置 Xamarin.Forms 应用的样式](~/xamarin-forms/user-interface/styles/xaml/index.md)
