---
title: ''
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: d3ae03ec6cbc3469422e6a2d57f186254e87f40c
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/28/2020
ms.locfileid: "84140009"
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
