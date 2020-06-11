---
标题： "Android 上的 TabbedPage 工具栏位置和颜色" 说明： "平台细节使你可以使用仅在特定平台上可用的功能，而无需实现自定义呈现器或效果。 本文介绍如何使用 Android 平台特定的来设置 TabbedPage 上工具栏的位置和颜色。
ms-chap： xamarin assetid： A5C68D6A-9A5F-42EE-845D-1E5B0CB1544E： xamarin 窗体作者： davidbritch： dabritch ms. 日期：07/10/2018 非 loc： [ Xamarin.Forms ， Xamarin.Essentials ]
---

# <a name="tabbedpage-toolbar-placement-and-color-on-android"></a>Android 上的 TabbedPage 工具栏位置和颜色

[![下载示例](~/media/shared/download.png) 下载示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

> [!IMPORTANT]
> 在上设置工具栏颜色的平台细节 [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) 现在已过时，已由 [`SelectedTabColor`](xref:Xamarin.Forms.TabbedPage.SelectedTabColor) 和属性替换 [`UnselectedTabColor`](xref:Xamarin.Forms.TabbedPage.UnselectedTabColor) 。 有关详细信息，请参阅[创建 TabbedPage](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md#create-a-tabbedpage)。

这些特定于平台的用于设置上工具栏的位置和颜色 [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) 。 它们通过将 [`TabbedPage.ToolbarPlacement`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.ToolbarPlacementProperty) 附加属性设置为枚举的值，并将 [`ToolbarPlacement`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ToolbarPlacement) [`TabbedPage.BarItemColor`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.BarItemColorProperty) 和 [`TabbedPage.BarSelectedItemColor`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.BarSelectedItemColorProperty) 附加属性设置为 [`Color`](xref:Xamarin.Forms.Color) ，在 XAML 中使用。

```xaml
<TabbedPage ...
            xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
            android:TabbedPage.ToolbarPlacement="Bottom"
            android:TabbedPage.BarItemColor="Black"
            android:TabbedPage.BarSelectedItemColor="Red">
    ...
</TabbedPage>
```

或者，可以使用 Fluent API 从 c # 中使用：

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

On<Android>().SetToolbarPlacement(ToolbarPlacement.Bottom)
             .SetBarItemColor(Color.Black)
             .SetBarSelectedItemColor(Color.Red);
```

`TabbedPage.On<Android>`方法指定这些平台细节仅在 Android 上运行。 [ `TabbedPage.SetToolbarPlacement` ] （X： Xamarin.Forms 。PlatformConfiguration. AndroidSpecific. TabbedPage. SetToolbarPlacement （ Xamarin.Forms 。IPlatformElementConfiguration { Xamarin.Forms 。PlatformConfiguration Xamarin.Forms 。TabbedPage}， Xamarin.Forms 。PlatformConfiguration. AndroidSpecific. ToolbarPlacement））方法， [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) 用于设置上的工具栏位置 [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) ， [`ToolbarPlacement`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ToolbarPlacement) 枚举提供以下值：

- [`Default`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ToolbarPlacement.Default)-指示将工具栏放置在页面上的默认位置。 这是在手机上页面的顶部，而在其他设备上页面的底部惯例。
- [`Top`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ToolbarPlacement.Top)-指示将工具栏置于页面顶部。
- [`Bottom`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ToolbarPlacement.Bottom)-指示将工具栏置于页面的底部。

此外，[ `TabbedPage.SetBarItemColor` ] （x： Xamarin.Forms 。PlatformConfiguration. AndroidSpecific. TabbedPage. SetBarItemColor （ Xamarin.Forms 。IPlatformElementConfiguration { Xamarin.Forms 。PlatformConfiguration Xamarin.Forms 。TabbedPage}， Xamarin.Forms 。Color））和 [ `TabbedPage.SetBarSelectedItemColor` ] （x： Xamarin.Forms 。PlatformConfiguration. AndroidSpecific. TabbedPage. SetBarSelectedItemColor （ Xamarin.Forms 。IPlatformElementConfiguration { Xamarin.Forms 。PlatformConfiguration Xamarin.Forms 。TabbedPage}， Xamarin.Forms 。Color））方法用于分别设置工具栏项和所选工具栏项的颜色。

> [!NOTE]
> [ `GetToolbarPlacement` ] （X： Xamarin.Forms 。PlatformConfiguration. AndroidSpecific. TabbedPage. GetToolbarPlacement （ Xamarin.Forms 。IPlatformElementConfiguration { Xamarin.Forms 。PlatformConfiguration Xamarin.Forms 。TabbedPage}）），[ `GetBarItemColor` ] （x： Xamarin.Forms 。PlatformConfiguration. AndroidSpecific. TabbedPage. GetBarItemColor （ Xamarin.Forms 。IPlatformElementConfiguration { Xamarin.Forms 。PlatformConfiguration Xamarin.Forms 。TabbedPage}））和 [ `GetBarSelectedItemColor` ] （x： Xamarin.Forms 。PlatformConfiguration. AndroidSpecific. TabbedPage. GetBarSelectedItemColor （ Xamarin.Forms 。IPlatformElementConfiguration { Xamarin.Forms 。PlatformConfiguration Xamarin.Forms 。TabbedPage}））方法可用于检索工具栏的位置和颜色 [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) 。

结果是工具栏的位置、工具栏项的颜色以及所选工具栏项的颜色可以在上设置 [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) ：

![](tabbedpage-toolbar-placement-color-images/tabbedpage-toolbar-placement.png)

## <a name="related-links"></a>相关链接

- [PlatformSpecifics （示例）](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [创建平台特定信息](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific. AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
