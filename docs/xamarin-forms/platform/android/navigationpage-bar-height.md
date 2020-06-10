---
标题： "Android 上的 NavigationPage Bar Height" 说明： "平台说明允许使用仅在特定平台上可用的功能，而无需实现自定义呈现器或效果。 本文介绍如何使用 Android 平台特定的来设置 NavigationPage 上导航栏的高度。
ms-chap： xamarin assetid： C8A73B64-FE70-408A-A72E-8AF147F0C52C： xamarin 窗体作者： davidbritch： dabritch ms. 日期：07/10/2018 非 loc： [ Xamarin.Forms ， Xamarin.Essentials ]
---

# <a name="navigationpage-bar-height-on-android"></a>Android 上的 NavigationPage 栏高度

[![下载示例](~/media/shared/download.png) 下载示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

此 Android 平台特定的设置上导航栏的高度 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) 。 它通过将 [`NavigationPage.BarHeight`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.NavigationPage.BarHeightProperty) 可绑定的属性设置为整数值，在 XAML 中使用：

```xaml
<NavigationPage ...
                xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat;assembly=Xamarin.Forms.Core"
                android:NavigationPage.BarHeight="450">
    ...
</NavigationPage>
```

此外，还可以使用 Fluent API 从 c # 使用该方法：

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat;
...

public class AndroidNavigationPageCS : Xamarin.Forms.NavigationPage
{
    public AndroidNavigationPageCS()
    {
        On<Android>().SetBarHeight(450);
    }
}
```

`NavigationPage.On<Android>`方法指定此平台特定的仅在应用程序兼容 Android 上运行。 [ `NavigationPage.SetBarHeight` ] （X： Xamarin.Forms 。PlatformConfiguration. AndroidSpecific. AppCompat. SetBarHeight （ Xamarin.Forms 。IPlatformElementConfiguration { Xamarin.Forms 。PlatformConfiguration Xamarin.Forms 。NavigationPage}，System.object）方法， [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat) 用于在上设置导航栏的高度（& e） [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) 。 此外，[ `NavigationPage.GetBarHeight` ] （x： Xamarin.Forms 。PlatformConfiguration. AndroidSpecific. AppCompat. GetBarHeight （ Xamarin.Forms 。IPlatformElementConfiguration { Xamarin.Forms 。PlatformConfiguration Xamarin.Forms 。NavigationPage}））方法可用于返回中导航栏的高度 `NavigationPage` 。

结果就是可以设置上导航栏的高度 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) ：

![](navigationpage-bar-height-images/navigationpage-barheight.png "NavigationPage navigation bar height")

## <a name="related-links"></a>相关链接

- [PlatformSpecifics （示例）](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [创建平台特定信息](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific. AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
