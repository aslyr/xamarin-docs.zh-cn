---
标题： "iOS 上的 NavigationPage Bar 半透明度" 说明： "平台细节使你可以使用仅在特定平台上可用的功能，而无需实现自定义呈现器或效果。 本文介绍如何使用 iOS 平台特定的来更改 NavigationPage 中导航栏的透明度。
ms-chap： xamarin assetid： 1150941B-56DB-4781-BE36-A4C4F9F2C500 毫秒： xamarin 窗体作者： davidbritch 毫秒. 作者： dabritch 毫秒。日期：10/24/2018 非 loc： [ Xamarin.Forms ， Xamarin.Essentials ]
---

# <a name="navigationpage-bar-translucency-on-ios"></a>IOS 上的 NavigationPage Bar 半透明度

[![下载示例](~/media/shared/download.png) 下载示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

此 iOS 平台特定用于更改上导航栏的透明度 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) ，并通过将 [`NavigationPage.IsNavigationBarTranslucent`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.IsNavigationBarTranslucentProperty) 附加属性设置为值在 XAML 中使用 `boolean` ：

```xaml
<NavigationPage ...
                xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
                BackgroundColor="Blue"
                ios:NavigationPage.IsNavigationBarTranslucent="true">
  ...
</NavigationPage>
```

此外，还可以使用 Fluent API 从 c # 使用该方法：

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

(App.Current.MainPage as Xamarin.Forms.NavigationPage).BackgroundColor = Color.Blue;
(App.Current.MainPage as Xamarin.Forms.NavigationPage).On<iOS>().EnableTranslucentNavigationBar();
```

`NavigationPage.On<iOS>`方法指定此平台特定的仅在 iOS 上运行。 [ `NavigationPage.EnableTranslucentNavigationBar` ] （X： Xamarin.Forms 。PlatformConfiguration. iOSSpecific. NavigationPage. EnableTranslucentNavigationBar （ Xamarin.Forms 。IPlatformElementConfiguration { Xamarin.Forms 。PlatformConfiguration、 Xamarin.Forms 。NavigationPage}））方法，该方法 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 用于使导航栏半透明。 此外， [`NavigationPage`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage) 命名空间中的类 `Xamarin.Forms.PlatformConfiguration.iOSSpecific` 还具有 [ `DisableTranslucentNavigationBar` ] （x： Xamarin.Forms 。PlatformConfiguration. iOSSpecific. NavigationPage. DisableTranslucentNavigationBar （ Xamarin.Forms 。IPlatformElementConfiguration { Xamarin.Forms 。PlatformConfiguration、 Xamarin.Forms 。NavigationPage}）））方法，该方法将导航栏恢复为其默认状态和 [ `SetIsNavigationBarTranslucent` ] （x： Xamarin.Forms 。PlatformConfiguration. iOSSpecific. NavigationPage. SetIsNavigationBarTranslucent （ Xamarin.Forms 。IPlatformElementConfiguration { Xamarin.Forms 。PlatformConfiguration、 Xamarin.Forms 。NavigationPage}，System.object）方法，可通过调用 [ `IsNavigationBarTranslucent` ] （x：）来切换导航栏的透明度 Xamarin.Forms 。PlatformConfiguration. iOSSpecific. NavigationPage. IsNavigationBarTranslucent （ Xamarin.Forms 。IPlatformElementConfiguration { Xamarin.Forms 。PlatformConfiguration、 Xamarin.Forms 。NavigationPage}））方法：

```csharp
(App.Current.MainPage as Xamarin.Forms.NavigationPage)
  .On<iOS>()
  .SetIsNavigationBarTranslucent(!(App.Current.MainPage as Xamarin.Forms.NavigationPage).On<iOS>().IsNavigationBarTranslucent());
```

结果是可以更改导航栏的透明度：

![](navigation-bar-translucent-images/translucent-navigation-bar.png "Translucent Navigation Bar Platform-Specific")

## <a name="related-links"></a>相关链接

- [PlatformSpecifics （示例）](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [创建平台特定信息](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
