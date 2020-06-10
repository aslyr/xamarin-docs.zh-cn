---
标题： "iOS 上的 NavigationPage Bar 分隔符" 说明： "平台说明允许使用仅在特定平台上可用的功能，而无需实现自定义呈现器或效果。 本文介绍如何使用特定于 iOS 平台的来隐藏位于 NavigationPage 上导航栏底部的分隔线和阴影。
ms-chap： xamarin assetid：5A45748A-6779-4441-82F2-415BD68473B9： xamarin 窗体作者： davidbritch： dabritch ms. 日期：10/24/2018 非 loc： [ Xamarin.Forms ， Xamarin.Essentials ]
---

# <a name="navigationpage-bar-separator-on-ios"></a>IOS 上的 NavigationPage Bar 分隔符

[![下载示例](~/media/shared/download.png) 下载示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

此 iOS 平台特定用于隐藏位于上导航栏底部的分隔线和阴影 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) 。 它通过将 [`NavigationPage.HideNavigationBarSeparator`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.HideNavigationBarSeparatorProperty) 可绑定的属性设置为来在 XAML 中使用 `false` ：

```xaml
<NavigationPage ...
                xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
                ios:NavigationPage.HideNavigationBarSeparator="true">

</NavigationPage>
```

此外，还可以使用 Fluent API 从 c # 使用该方法：

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;

public class iOSTitleViewNavigationPageCS : Xamarin.Forms.NavigationPage
{
    public iOSTitleViewNavigationPageCS()
    {
        On<iOS>().SetHideNavigationBarSeparator(true);
    }
}
```

`NavigationPage.On<iOS>`方法指定此平台特定的仅在 iOS 上运行。 [ `NavigationPage.SetHideNavigationBarSeparator` ] （X： Xamarin.Forms 。PlatformConfiguration. iOSSpecific. NavigationPage. SetHideNavigationBarSeparator （ Xamarin.Forms 。IPlatformElementConfiguration { Xamarin.Forms 。PlatformConfiguration、 Xamarin.Forms 。NavigationPage}，System.object）方法， [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 可用于控制是否隐藏导航栏分隔线。 此外，[ `NavigationPage.HideNavigationBarSeparator` ] （x： Xamarin.Forms 。PlatformConfiguration. iOSSpecific. NavigationPage. HideNavigationBarSeparator （ Xamarin.Forms 。IPlatformElementConfiguration { Xamarin.Forms 。PlatformConfiguration、 Xamarin.Forms 。NavigationPage}））方法可用于返回是否隐藏导航栏分隔符。

结果就是可以隐藏的导航栏分隔符 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) ：

![](navigation-bar-separator-images/navigationpage-hideseparatorbar.png "NavigationPage navigation bar hidden")

## <a name="related-links"></a>相关链接

- [PlatformSpecifics （示例）](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [创建平台特定信息](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
