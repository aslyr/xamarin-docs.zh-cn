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
ms.openlocfilehash: 0db20620870340386ccd0cedf7f98cb2975527ba
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/28/2020
ms.locfileid: "84128026"
---
# <a name="large-page-titles-on-ios"></a>IOS 上的大型页面标题

[![下载示例](~/media/shared/download.png) 下载示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

此 iOS 平台特定用于在的导航栏上将页面标题显示为大标题 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) ，适用于使用 iOS 11 或更高版本的设备。 当用户开始滚动内容时，较大的标题将左对齐并使用较大的字体，并转换为标准标题，以便有效地使用屏幕房地产。 但在横向方向，标题将返回到导航栏的中心，以优化内容布局。 它通过将 `NavigationPage.PrefersLargeTitles` 附加属性设置为值在 XAML 中使用 `boolean` ：

```xaml
<NavigationPage xmlns="http://xamarin.com/schemas/2014/forms"
                xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
                ...
                ios:NavigationPage.PrefersLargeTitles="true">
  ...
</NavigationPage>
```

或者，可以使用 Fluent API 从 c # 使用它：

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

var navigationPage = new Xamarin.Forms.NavigationPage(new iOSLargeTitlePageCS());
navigationPage.On<iOS>().SetPrefersLargeTitles(true);
```

`NavigationPage.On<iOS>`方法指定此平台特定的仅在 iOS 上运行。 `NavigationPage.SetPrefersLargeTitle`命名空间中的方法 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 控制是否启用大型标题。

如果在上启用了大标题 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) ，则导航堆栈中的所有页面都将显示大标题。 通过将 `Page.LargeTitleDisplay` 附加属性设置为枚举的值，可以在页面上覆盖此行为 `LargeTitleDisplayMode` ：

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             Title="Large Title"
             ios:Page.LargeTitleDisplay="Never">
  ...
</ContentPage>
```

或者，可以使用 Fluent API 从 c # 重写页面行为：

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

public class iOSLargeTitlePageCS : ContentPage
{
    public iOSLargeTitlePageCS(ICommand restore)
    {
        On<iOS>().SetLargeTitleDisplay(LargeTitleDisplayMode.Never);
        ...
    }
    ...
}
```

`Page.On<iOS>`方法指定此平台特定的仅在 iOS 上运行。 `Page.SetLargeTitleDisplay`命名空间中的方法 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 控制中的大型标题行为 [`Page`](xref:Xamarin.Forms.Page) ， `LargeTitleDisplayMode` 枚举提供三个可能的值：

- `Always`–强制导航栏和字体大小使用大格式。
- `Automatic`–使用与导航堆栈中的上一项相同的样式（大或小）。
- `Never`–强制使用常规的小格式导航栏。

此外，该 `SetLargeTitleDisplay` 方法可用于通过调用返回当前的方法来切换枚举值 `LargeTitleDisplay` `LargeTitleDisplayMode` ：

```csharp
switch (On<iOS>().LargeTitleDisplay())
{
    case LargeTitleDisplayMode.Always:
        On<iOS>().SetLargeTitleDisplay(LargeTitleDisplayMode.Automatic);
        break;
    case LargeTitleDisplayMode.Automatic:
        On<iOS>().SetLargeTitleDisplay(LargeTitleDisplayMode.Never);
        break;
    case LargeTitleDisplayMode.Never:
        On<iOS>().SetLargeTitleDisplay(LargeTitleDisplayMode.Always);
        break;
}
```

因此，指定的 `LargeTitleDisplayMode` 将应用于 [`Page`](xref:Xamarin.Forms.Page) ，后者控制大标题行为：

![](page-large-title-images/large-title.png "Blur Effect Platform-Specific")

## <a name="related-links"></a>相关链接

- [PlatformSpecifics （示例）](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [创建平台特定信息](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
