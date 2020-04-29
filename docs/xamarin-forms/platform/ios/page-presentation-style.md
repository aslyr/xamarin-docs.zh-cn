---
title: IOS 上的模式页呈现样式
description: 平台说明允许使用仅在特定平台上可用的功能，而无需实现自定义呈现器或效果。 本文介绍如何使用 iOS 平台特定的设置模式页面的呈现样式。
ms.prod: xamarin
ms.assetid: C791F7CF-330A-44BA-987A-4CFCCBB9278B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/02/2020
ms.openlocfilehash: 5078b280499929e0e2e3691539cf1927b4c79fe7
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "82517538"
---
# <a name="modal-page-presentation-style-on-ios"></a>IOS 上的模式页呈现样式

[![下载示例](~/media/shared/download.png)下载示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

此 iOS 平台特定用于设置模式页的呈现样式。 它通过将可`Page.ModalPresentationStyle`绑定的属性设置为一个`UIModalPresentationStyle`枚举值在 XAML 中使用：

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Page.ModalPresentationStyle="FormSheet">
    ...
</ContentPage>
```

此外，还可以使用 Fluent API 从 c # 使用该方法：

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

public class iOSModalFormSheetPageCS : ContentPage
{
    public iOSModalFormSheetPageCS()
    {
        On<iOS>().SetModalPresentationStyle(UIModalPresentationStyle.FormSheet);
        ...
    }
}
```

`Page.On<iOS>`方法指定此平台特定的仅在 iOS 上运行。 命名空间中的`Page.SetModalPresentationStyle`方法用于[`Page`](xref:Xamarin.Forms.Page)通过指定以下`UIModalPresentationStyle`枚举值之一，在上设置模式呈现样式： [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)

- `FullScreen`，它将模式呈现样式设置为包含整个屏幕。 默认情况下，使用此演示样式显示模式页面。
- `FormSheet`，它将模式呈现样式设置为居中并小于屏幕。
- `Automatic`，它将模式呈现样式设置为系统所选的默认样式。 对于大多数视图控制器， `UIKit`将其映射`UIModalPresentationStyle.PageSheet`到，但某些系统视图控制器可以将其映射到不同的样式。
- `OverFullScreen`，它将模式呈现样式设置为涵盖屏幕。
- `PageSheet`，它将模式呈现样式设置为涵盖基础内容。

此外， `GetModalPresentationStyle`方法可用于检索应用于的`UIModalPresentationStyle` [`Page`](xref:Xamarin.Forms.Page)枚举的当前值。

结果就是[`Page`](xref:Xamarin.Forms.Page)可以设置上的模式呈现样式：

[![](page-presentation-style-images/modal-presentation-style-small.png "Modal Presentation Styles")](page-presentation-style-images/modal-presentation-style-large.png#lightbox "Modal Presentation Styles")

> [!NOTE]
> 使用此特定于平台的的页面设置模式呈现样式必须使用模式导航。 有关详细信息，请参阅[Xamarin. Forms 模式页面](~/xamarin-forms/app-fundamentals/navigation/modal.md)。

## <a name="related-links"></a>相关链接

- [PlatformSpecifics （示例）](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [创建平台特定信息](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
