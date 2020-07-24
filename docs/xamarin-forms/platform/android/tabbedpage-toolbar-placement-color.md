---
title: Android 上的 TabbedPage 工具栏位置和颜色
description: 平台说明允许使用仅在特定平台上可用的功能，而无需实现自定义呈现器或效果。 本文介绍如何使用 Android 平台特定的来设置 TabbedPage 上工具栏的位置和颜色。
ms.prod: xamarin
ms.assetid: A5C68D6A-9A5F-42EE-845D-1E5B0CB1544E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
no-loc:
- ':::no-loc(Xamarin.Forms):::'
- ':::no-loc(Xamarin.Essentials):::'
ms.openlocfilehash: e32c516c4a0a8b12bd8a76478557905149890c6a
ms.sourcegitcommit: 952db1983c0bc373844c5fbe9d185e04a87d8fb4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/23/2020
ms.locfileid: "86997327"
---
# <a name="tabbedpage-toolbar-placement-and-color-on-android"></a>Android 上的 TabbedPage 工具栏位置和颜色

[![下载示例](~/media/shared/download.png) 下载示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

> [!IMPORTANT]
> 在上设置工具栏颜色的平台细节 [`TabbedPage`](xref::::no-loc(Xamarin.Forms):::.TabbedPage) 现在已过时，已由 [`SelectedTabColor`](xref::::no-loc(Xamarin.Forms):::.TabbedPage.SelectedTabColor) 和属性替换 [`UnselectedTabColor`](xref::::no-loc(Xamarin.Forms):::.TabbedPage.UnselectedTabColor) 。 有关详细信息，请参阅[创建 TabbedPage](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md#create-a-tabbedpage)。

这些特定于平台的用于设置上工具栏的位置和颜色 [`TabbedPage`](xref::::no-loc(Xamarin.Forms):::.TabbedPage) 。 它们通过将 [`TabbedPage.ToolbarPlacement`](xref::::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific.TabbedPage.ToolbarPlacementProperty) 附加属性设置为枚举的值，并将 [`ToolbarPlacement`](xref::::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific.ToolbarPlacement) [`TabbedPage.BarItemColor`](xref::::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific.TabbedPage.BarItemColorProperty) 和 [`TabbedPage.BarSelectedItemColor`](xref::::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific.TabbedPage.BarSelectedItemColorProperty) 附加属性设置为 [`Color`](xref::::no-loc(Xamarin.Forms):::.Color) ，在 XAML 中使用。

```xaml
<TabbedPage ...
            xmlns:android="clr-namespace::::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific;assembly=:::no-loc(Xamarin.Forms):::.Core"
            android:TabbedPage.ToolbarPlacement="Bottom"
            android:TabbedPage.BarItemColor="Black"
            android:TabbedPage.BarSelectedItemColor="Red">
    ...
</TabbedPage>
```

或者，可以使用 Fluent API 从 c # 中使用：

```csharp
using :::no-loc(Xamarin.Forms):::.PlatformConfiguration;
using :::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific;
...

On<Android>().SetToolbarPlacement(ToolbarPlacement.Bottom)
             .SetBarItemColor(Color.Black)
             .SetBarSelectedItemColor(Color.Red);
```

`TabbedPage.On<Android>`方法指定这些平台细节仅在 Android 上运行。 [ `TabbedPage.SetToolbarPlacement` ] （X： :::no-loc(Xamarin.Forms)::: 。PlatformConfiguration. AndroidSpecific. TabbedPage. SetToolbarPlacement （ :::no-loc(Xamarin.Forms)::: 。IPlatformElementConfiguration { :::no-loc(Xamarin.Forms)::: 。PlatformConfiguration :::no-loc(Xamarin.Forms)::: 。TabbedPage}， :::no-loc(Xamarin.Forms)::: 。PlatformConfiguration. AndroidSpecific. ToolbarPlacement））方法， [`:::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific`](xref::::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific) 用于设置上的工具栏位置 [`TabbedPage`](xref::::no-loc(Xamarin.Forms):::.TabbedPage) ， [`ToolbarPlacement`](xref::::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific.ToolbarPlacement) 枚举提供以下值：

- [`Default`](xref::::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific.ToolbarPlacement.Default)-指示将工具栏放置在页面上的默认位置。 这是在手机上页面的顶部，而在其他设备上页面的底部惯例。
- [`Top`](xref::::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific.ToolbarPlacement.Top)-指示将工具栏置于页面顶部。
- [`Bottom`](xref::::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific.ToolbarPlacement.Bottom)-指示将工具栏置于页面的底部。

此外，[ `TabbedPage.SetBarItemColor` ] （x： :::no-loc(Xamarin.Forms)::: 。PlatformConfiguration. AndroidSpecific. TabbedPage. SetBarItemColor （ :::no-loc(Xamarin.Forms)::: 。IPlatformElementConfiguration { :::no-loc(Xamarin.Forms)::: 。PlatformConfiguration :::no-loc(Xamarin.Forms)::: 。TabbedPage}， :::no-loc(Xamarin.Forms)::: 。Color））和 [ `TabbedPage.SetBarSelectedItemColor` ] （x： :::no-loc(Xamarin.Forms)::: 。PlatformConfiguration. AndroidSpecific. TabbedPage. SetBarSelectedItemColor （ :::no-loc(Xamarin.Forms)::: 。IPlatformElementConfiguration { :::no-loc(Xamarin.Forms)::: 。PlatformConfiguration :::no-loc(Xamarin.Forms)::: 。TabbedPage}， :::no-loc(Xamarin.Forms)::: 。Color））方法用于分别设置工具栏项和所选工具栏项的颜色。

> [!NOTE]
> [ `GetToolbarPlacement` ] （X： :::no-loc(Xamarin.Forms)::: 。PlatformConfiguration. AndroidSpecific. TabbedPage. GetToolbarPlacement （ :::no-loc(Xamarin.Forms)::: 。IPlatformElementConfiguration { :::no-loc(Xamarin.Forms)::: 。PlatformConfiguration :::no-loc(Xamarin.Forms)::: 。TabbedPage}）），[ `GetBarItemColor` ] （x： :::no-loc(Xamarin.Forms)::: 。PlatformConfiguration. AndroidSpecific. TabbedPage. GetBarItemColor （ :::no-loc(Xamarin.Forms)::: 。IPlatformElementConfiguration { :::no-loc(Xamarin.Forms)::: 。PlatformConfiguration :::no-loc(Xamarin.Forms)::: 。TabbedPage}））和 [ `GetBarSelectedItemColor` ] （x： :::no-loc(Xamarin.Forms)::: 。PlatformConfiguration. AndroidSpecific. TabbedPage. GetBarSelectedItemColor （ :::no-loc(Xamarin.Forms)::: 。IPlatformElementConfiguration { :::no-loc(Xamarin.Forms)::: 。PlatformConfiguration :::no-loc(Xamarin.Forms)::: 。TabbedPage}））方法可用于检索工具栏的位置和颜色 [`TabbedPage`](xref::::no-loc(Xamarin.Forms):::.TabbedPage) 。

结果是工具栏的位置、工具栏项的颜色以及所选工具栏项的颜色可以在上设置 [`TabbedPage`](xref::::no-loc(Xamarin.Forms):::.TabbedPage) ：

![TabbedPage 工具栏配置](tabbedpage-toolbar-placement-color-images/tabbedpage-toolbar-placement.png)

## <a name="related-links"></a>相关链接

- [PlatformSpecifics （示例）](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [创建平台特定信息](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific API](xref::::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific. AppCompat API](xref::::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific.AppCompat)
