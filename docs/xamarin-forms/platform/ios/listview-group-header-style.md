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
ms.openlocfilehash: 46e8dec3d5644defdeb8a2265a73815adfde92d8
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/28/2020
ms.locfileid: "84136027"
---
# <a name="listview-group-header-style-on-ios"></a>IOS 上的 ListView 组标头样式

[![下载示例](~/media/shared/download.png) 下载示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

此 iOS 平台特定控制 [`ListView`](xref:Xamarin.Forms.ListView) 在滚动过程中标题单元是否浮动。 它通过将 `ListView.GroupHeaderStyle` 可绑定的属性设置为枚举的值，在 XAML 中使用 `GroupHeaderStyle` ：

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        <ListView ... ios:ListView.GroupHeaderStyle="Grouped">
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

此外，还可以使用 Fluent API 从 c # 使用该方法：

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

listView.On<iOS>().SetGroupHeaderStyle(GroupHeaderStyle.Grouped);
```

`ListView.On<iOS>`方法指定此平台特定的仅在 iOS 上运行。 `ListView.SetGroupHeaderStyle`命名空间中的方法 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 用于控制 [`ListView`](xref:Xamarin.Forms.ListView) 标题单元是否在滚动过程中浮动。 `GroupHeaderStyle`枚举提供了两个可能的值：

- `Plain`–指示在滚动时标题单元浮动 [`ListView`](xref:Xamarin.Forms.ListView) （默认值）。
- `Grouped`–指示在滚动时，标头单元格不浮动 [`ListView`](xref:Xamarin.Forms.ListView) 。

此外， `ListView.GetGroupHeaderStyle` 方法可用于返回 `GroupHeaderStyle` 应用于的 [`ListView`](xref:Xamarin.Forms.ListView) 。

结果是，将指定的 `GroupHeaderStyle` 值应用于 [`ListView`](xref:Xamarin.Forms.ListView) ，后者控制在滚动过程中标题单元格是否浮动：

[![IOS 上的浮动和非浮动 ListView 标题单元的屏幕截图](listview-group-header-style-images/group-header-styles.png "包含浮动和非浮动标题单元格的 ListView")](listview-group-header-style-images/group-header-styles-large.png#lightbox "包含浮动和非浮动标题单元格的 ListView")

## <a name="related-links"></a>相关链接

- [PlatformSpecifics （示例）](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [创建平台特定信息](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
