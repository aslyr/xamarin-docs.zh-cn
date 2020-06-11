---
title： "页面工具栏在 Windows 上的位置" 说明： "平台细节使你可以使用仅在特定平台上可用的功能，而无需实现自定义呈现器或效果。 本文介绍如何使用 Windows 平台特定的来更改页面上工具栏的位置。
ms-chap： xamarin assetid：99F29E95-0C36-4A3B-BDE8-7E9F119E844E： xamarin 窗体作者： davidbritch： dabritch ms. 日期：10/24/2018 非 loc： [ Xamarin.Forms ， Xamarin.Essentials ]
---

# <a name="page-toolbar-placement-on-windows"></a>Windows 上的页面工具栏布局

[![下载示例](~/media/shared/download.png) 下载示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

此通用 Windows 平台平台特定用于更改工具栏在上的位置 [`Page`](xref:Xamarin.Forms.Page) ，并通过将 [`Page.ToolbarPlacement`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.Page.ToolbarPlacementProperty) 附加属性设置为枚举的值来在 XAML 中使用 [`ToolbarPlacement`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement) ：

```xaml
<TabbedPage ...
            xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core"
            windows:Page.ToolbarPlacement="Bottom">
  ...
</TabbedPage>
```

此外，还可以使用 Fluent API 从 c # 使用该方法：

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

page.On<Windows>().SetToolbarPlacement(ToolbarPlacement.Bottom);
```

`Page.On<Windows>`方法指定此平台特定的仅在 Windows 上运行。 [ `Page.SetToolbarPlacement` ] （X： Xamarin.Forms 。PlatformConfiguration. WindowsSpecific. SetToolbarPlacement （ Xamarin.Forms 。IPlatformElementConfiguration { Xamarin.Forms 。PlatformConfiguration、 Xamarin.Forms 。Page}， Xamarin.Forms 。PlatformConfiguration. WindowsSpecific. ToolbarPlacement））方法， [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) 用于设置工具栏位置， [`ToolbarPlacement`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement) 枚举提供三个值： [`Default`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement.Default) 、 [`Top`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement.Top) 和 [`Bottom`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement.Bottom) 。

结果是将指定的工具栏位置应用到 [`Page`](xref:Xamarin.Forms.Page) 实例：

[![](page-toolbar-placement-images/toolbar-placement.png "Toolbar Placement Platform-Specific")](page-toolbar-placement-images/toolbar-placement-large.png#lightbox "Toolbar Placement Platform-Specific")

## <a name="related-links"></a>相关链接

- [PlatformSpecifics （示例）](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [创建平台特定信息](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [WindowsSpecific API](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)
