---
title: Android 上的 TabbedPage 页刷
description: 平台说明允许使用仅在特定平台上可用的功能，而无需实现自定义呈现器或效果。 本文介绍如何使用 Android 平台特定的，该平台允许在 TabbedPage 的页面之间通过水平手指进行轻扫。
ms.prod: xamarin
ms.assetid: D1C09CCB-7246-41A4-8BD2-FA6FABCF1C72
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
no-loc:
- ':::no-loc(Xamarin.Forms):::'
- ':::no-loc(Xamarin.Essentials):::'
ms.openlocfilehash: 0043d90e631c19a55b766a877a9cd30316f14650
ms.sourcegitcommit: 952db1983c0bc373844c5fbe9d185e04a87d8fb4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/23/2020
ms.locfileid: "86997370"
---
# <a name="tabbedpage-page-swiping-on-android"></a>Android 上的 TabbedPage 页刷

[![下载示例](~/media/shared/download.png) 下载示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

此 Android 平台特定用于通过中的页面之间的水平手指手势启用轻扫 [`TabbedPage`](xref::::no-loc(Xamarin.Forms):::.TabbedPage) 。 它通过将 [`TabbedPage.IsSwipePagingEnabled`](xref::::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific.TabbedPage.IsSwipePagingEnabledProperty) 附加属性设置为值在 XAML 中使用 `boolean` ：

```xaml
<TabbedPage ...
            xmlns:android="clr-namespace::::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific;assembly=:::no-loc(Xamarin.Forms):::.Core"
            android:TabbedPage.OffscreenPageLimit="2"
            android:TabbedPage.IsSwipePagingEnabled="true">
    ...
</TabbedPage>
```

此外，还可以使用 Fluent API 从 c # 使用该方法：

```csharp
using :::no-loc(Xamarin.Forms):::.PlatformConfiguration;
using :::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific;
...

On<Android>().SetOffscreenPageLimit(2)
             .SetIsSwipePagingEnabled(true);
```

`TabbedPage.On<Android>`方法指定此平台特定的仅在 Android 上运行。 [ `TabbedPage.SetIsSwipePagingEnabled` ] （X： :::no-loc(Xamarin.Forms)::: 。PlatformConfiguration. AndroidSpecific. TabbedPage. SetIsSwipePagingEnabled （ :::no-loc(Xamarin.Forms)::: 。Msds-bindableobject，System.object）方法， [`:::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific`](xref::::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific) 用于在中的页之间启用轻扫 [`TabbedPage`](xref::::no-loc(Xamarin.Forms):::.TabbedPage) 。 此外， `TabbedPage` 命名空间中的类 `:::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific` 还具有 [ `EnableSwipePaging` ] （x： :::no-loc(Xamarin.Forms)::: 。PlatformConfiguration. AndroidSpecific. TabbedPage. EnableSwipePaging （ :::no-loc(Xamarin.Forms)::: 。IPlatformElementConfiguration { :::no-loc(Xamarin.Forms)::: 。PlatformConfiguration :::no-loc(Xamarin.Forms)::: 。TabbedPage}）））方法，该方法可启用此特定于平台的和 [ `DisableSwipePaging` ] （x： :::no-loc(Xamarin.Forms)::: 。PlatformConfiguration. AndroidSpecific. TabbedPage. DisableSwipePaging （ :::no-loc(Xamarin.Forms)::: 。IPlatformElementConfiguration { :::no-loc(Xamarin.Forms)::: 。PlatformConfiguration :::no-loc(Xamarin.Forms)::: 。TabbedPage}）））方法来禁用特定于平台的。 [`TabbedPage.OffscreenPageLimit`](xref::::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific.TabbedPage.OffscreenPageLimitProperty)附加属性和 [ `SetOffscreenPageLimit` ] （x： :::no-loc(Xamarin.Forms)::: 。PlatformConfiguration. AndroidSpecific. TabbedPage. SetOffscreenPageLimit （ :::no-loc(Xamarin.Forms)::: 。Msds-bindableobject，System.object）方法，用于设置在当前页面的任意一侧应保留在空闲状态的页数。

结果是启用了通过显示的页面进行的轻扫分页 [`TabbedPage`](xref::::no-loc(Xamarin.Forms):::.TabbedPage) ：

![通过 TabbedPage 进行分页](tabbedpage-page-swiping-images/tabbedpage-swipe.png)

## <a name="related-links"></a>相关链接

- [PlatformSpecifics （示例）](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [创建平台特定信息](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific API](xref::::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific. AppCompat API](xref::::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific.AppCompat)
