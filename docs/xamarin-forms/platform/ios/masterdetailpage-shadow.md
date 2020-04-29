---
title: IOS 上的 MasterDetailPage 阴影
description: 平台说明允许使用仅在特定平台上可用的功能，而无需实现自定义呈现器或效果。 本文介绍如何使用特定于 iOS 平台的，该平台在显示母版页时，控制是否已将 MasterDetailPage 的详细信息页应用于它。
ms.prod: xamarin
ms.assetid: FB907EA2-C00A-43A5-84B1-A43584C867A5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/05/2020
ms.openlocfilehash: eaa82b5accae73e4c6e8eb8d3fbcc873fc111be9
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "82532957"
---
# <a name="masterdetailpage-shadow-on-ios"></a>IOS 上的 MasterDetailPage 阴影

[![下载示例](~/media/shared/download.png)下载示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

此特定于平台的控件在显示母版页时是否[`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage)对的详细信息页应用了阴影。 它通过将可`MasterDetailPage.ApplyShadow`绑定的属性设置为来`true`在 XAML 中使用：

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

`MasterDetailPage.On<iOS>`方法指定此平台特定的仅在 iOS 上运行。 命名空间中的`MasterDetailPage.SetApplyShadow`方法用于控制在显示母版页时是否对的[`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage)详细信息页应用了阴影。 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 此外，该`GetApplyShadow`方法可用于确定是否将影子应用于的详细信息页`MasterDetailPage`。

结果是，在显示母版页时，的[`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage)详细信息页可以应用影子：

[![具有和不带阴影的 MasterDetailPage 的屏幕截图](masterdetailpage-shadow-images/shadow.png "带有和不带阴影的 MasterDetailPage")](masterdetailpage-shadow-images/shadow-large.png#lightbox "带有和不带阴影的 MasterDetailPage")

## <a name="related-links"></a>相关链接

- [PlatformSpecifics （示例）](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [创建平台特定信息](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
