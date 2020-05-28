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
ms.openlocfilehash: aaf94536d41da47aec10fc655f9d053b753da5a2
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/28/2020
ms.locfileid: "84135962"
---
# <a name="masterdetailpage-shadow-on-ios"></a>IOS 上的 MasterDetailPage 阴影

[![下载示例](~/media/shared/download.png) 下载示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

此特定于平台的控件在显示母版页时是否对的详细信息页 [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) 应用了阴影。 它通过将 `MasterDetailPage.ApplyShadow` 可绑定的属性设置为来在 XAML 中使用 `true` ：

```xaml
<MasterDetailPage ...
                  xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
                  ios:MasterDetailPage.ApplyShadow="true">
    ...
</MasterDetailPage>
```

此外，还可以使用 Fluent API 从 c # 使用该方法：

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

public class iOSMasterDetailPageCS : MasterDetailPage
{
    public iOSMasterDetailPageCS(ICommand restore)
    {
        On<iOS>().SetApplyShadow(true);
        // ...
    }
}
```

`MasterDetailPage.On<iOS>`方法指定此平台特定的仅在 iOS 上运行。 `MasterDetailPage.SetApplyShadow`命名空间中的方法 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 用于控制在显示母版页时是否对的详细信息页 [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) 应用了阴影。 此外，该 `GetApplyShadow` 方法可用于确定是否将影子应用于的详细信息页 `MasterDetailPage` 。

结果是 [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) ，在显示母版页时，的详细信息页可以应用影子：

[![具有和不带阴影的 MasterDetailPage 的屏幕截图](masterdetailpage-shadow-images/shadow.png "带有和不带阴影的 MasterDetailPage")](masterdetailpage-shadow-images/shadow-large.png#lightbox "带有和不带阴影的 MasterDetailPage")

## <a name="related-links"></a>相关链接

- [PlatformSpecifics （示例）](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [创建平台特定信息](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
