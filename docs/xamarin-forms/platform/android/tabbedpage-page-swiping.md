---
标题： "Android on Android" 说明： "平台细节使你可以使用仅在特定平台上可用的功能，而无需实现自定义呈现器或效果。 本文介绍如何使用 Android 平台特定的，以便在 TabbedPage 中的页面之间使用水平手指手势进行轻扫。
ms-chap： xamarin assetid： D1C09CCB-7246-41A4-8BD2-FA6FABCF1C72： xamarin 窗体作者： davidbritch： dabritch ms. 日期：07/10/2018 非 loc： [ Xamarin.Forms ， Xamarin.Essentials ]
---

# <a name="tabbedpage-page-swiping-on-android"></a>Android 上的 TabbedPage 页刷

[![下载示例](~/media/shared/download.png) 下载示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

此 Android 平台特定用于通过中的页面之间的水平手指手势启用轻扫 [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) 。 它通过将 [`TabbedPage.IsSwipePagingEnabled`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.IsSwipePagingEnabledProperty) 附加属性设置为值在 XAML 中使用 `boolean` ：

```xaml
<TabbedPage ...
            xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
            android:TabbedPage.OffscreenPageLimit="2"
            android:TabbedPage.IsSwipePagingEnabled="true">
    ...
</TabbedPage>
```

此外，还可以使用 Fluent API 从 c # 使用该方法：

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

On<Android>().SetOffscreenPageLimit(2)
             .SetIsSwipePagingEnabled(true);
```

`TabbedPage.On<Android>`方法指定此平台特定的仅在 Android 上运行。 [ `TabbedPage.SetIsSwipePagingEnabled` ] （X： Xamarin.Forms 。PlatformConfiguration. AndroidSpecific. TabbedPage. SetIsSwipePagingEnabled （ Xamarin.Forms 。Msds-bindableobject，System.object）方法， [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) 用于在中的页之间启用轻扫 [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) 。 此外， `TabbedPage` 命名空间中的类 `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` 还具有 [ `EnableSwipePaging` ] （x： Xamarin.Forms 。PlatformConfiguration. AndroidSpecific. TabbedPage. EnableSwipePaging （ Xamarin.Forms 。IPlatformElementConfiguration { Xamarin.Forms 。PlatformConfiguration Xamarin.Forms 。TabbedPage}）））方法，该方法可启用此特定于平台的和 [ `DisableSwipePaging` ] （x： Xamarin.Forms 。PlatformConfiguration. AndroidSpecific. TabbedPage. DisableSwipePaging （ Xamarin.Forms 。IPlatformElementConfiguration { Xamarin.Forms 。PlatformConfiguration Xamarin.Forms 。TabbedPage}）））方法来禁用特定于平台的。 [`TabbedPage.OffscreenPageLimit`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.OffscreenPageLimitProperty)附加属性和 [ `SetOffscreenPageLimit` ] （x： Xamarin.Forms 。PlatformConfiguration. AndroidSpecific. TabbedPage. SetOffscreenPageLimit （ Xamarin.Forms 。Msds-bindableobject，System.object）方法，用于设置在当前页面的任意一侧应保留在空闲状态的页数。

结果是启用了通过显示的页面进行的轻扫分页 [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) ：

![](tabbedpage-page-swiping-images/tabbedpage-swipe.png)

## <a name="related-links"></a>相关链接

- [PlatformSpecifics （示例）](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [创建平台特定信息](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific. AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
