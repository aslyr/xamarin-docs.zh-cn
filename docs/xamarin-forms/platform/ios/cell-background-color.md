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
ms.openlocfilehash: 90282262926fef663183be247e37d64dd1be9124
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/28/2020
ms.locfileid: "84138562"
---
# <a name="cell-background-color-on-ios"></a>IOS 上的单元格背景色

[![下载示例](~/media/shared/download.png) 下载示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

此 iOS 平台特定的设置实例的默认背景色 [`Cell`](xref:Xamarin.Forms.Cell) 。 它通过将 `Cell.DefaultBackgroundColor` 可绑定的属性设置为来在 XAML 中使用 [`Color`](xref:Xamarin.Forms.Color) ：

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        <ListView ItemsSource="{Binding GroupedEmployees}"
                  IsGroupingEnabled="true">
            <ListView.GroupHeaderTemplate>
                <DataTemplate>
                    <ViewCell ios:Cell.DefaultBackgroundColor="Teal">
                        <Label Margin="10,10"
                               Text="{Binding Key}"
                               FontAttributes="Bold" />
                    </ViewCell>
                </DataTemplate>
            </ListView.GroupHeaderTemplate>
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

var viewCell = new ViewCell { View = ... };
viewCell.On<iOS>().SetDefaultBackgroundColor(Color.Teal);
```

`ListView.On<iOS>`方法指定此平台特定的仅在 iOS 上运行。 `Cell.SetDefaultBackgroundColor`命名空间中的方法 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 将单元格背景色设置为指定的 [`Color`](xref:Xamarin.Forms.Color) 。 此外，该 `Cell.DefaultBackgroundColor` 方法可用于检索当前单元格背景色。

结果就是，可以将中的背景色 [`Cell`](xref:Xamarin.Forms.Cell) 设置为特定 [`Color`](xref:Xamarin.Forms.Color) ：

[![IOS 上的青色组标题单元的屏幕截图](cell-background-color-images/group-header-cell-color.png "带有蓝绿色组头单元格的 ListView")](cell-background-color-images/group-header-cell-color-large.png#lightbox "带有蓝绿色组头单元格的 ListView")

## <a name="related-links"></a>相关链接

- [PlatformSpecifics （示例）](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [创建平台特定信息](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
