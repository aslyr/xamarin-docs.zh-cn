---
标题： "Android 上的 TabbedPage 页面过渡动画" 说明： "平台说明允许使用仅在特定平台上可用的功能，而无需实现自定义呈现器或效果。 本文介绍如何使用 Android 平台特定的，以便在 TabbedPage 中的页面间导航时禁用过渡动画。
ms-chap： xamarin assetid： 2DB4EA6D-9CED-4137-BAB2-B20A457B1CA3 毫秒： xamarin 窗体作者： davidbritch 毫秒. 作者： dabritch 毫秒。日期：07/10/2018 非 loc： [ Xamarin.Forms ， Xamarin.Essentials ]
---

# <a name="tabbedpage-page-transition-animations-on-android"></a>Android 上的 TabbedPage 页面过渡动画

[![下载示例](~/media/shared/download.png) 下载示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

此 Android 平台特定用于在中通过编程方式或使用选项卡栏在页面间导航时禁用转换动画 [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) 。 它通过将 `TabbedPage.IsSmoothScrollEnabled` 可绑定的属性设置为来在 XAML 中使用 `false` ：

```xaml
<TabbedPage ...
            xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
            android:TabbedPage.IsSmoothScrollEnabled="false">
    ...
</TabbedPage>
```

此外，还可以使用 Fluent API 从 c # 使用该方法：

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

On<Android>().SetIsSmoothScrollEnabled(false);
```

`TabbedPage.On<Android>`方法指定此平台特定的仅在 Android 上运行。 `TabbedPage.SetIsSmoothScrollEnabled`命名空间中的方法 [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) 用于控制在中的页面之间导航时是否显示转换动画 [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) 。 此外， `TabbedPage` 命名空间中的类 `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` 还具有以下方法：

- `IsSmoothScrollEnabled`，用于检索在中的页面之间导航时是否显示转换动画 `TabbedPage` 。
- `EnableSmoothScroll`，用于在中的页面之间导航时启用过渡动画 `TabbedPage` 。
- `DisableSmoothScroll`，用于在中的页面之间导航时禁用转换动画 `TabbedPage` 。

## <a name="related-links"></a>相关链接

- [PlatformSpecifics （示例）](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [创建平台特定信息](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific. AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
