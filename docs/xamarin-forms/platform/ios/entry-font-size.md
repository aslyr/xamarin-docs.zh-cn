---
title: IOS 上的输入字体大小
description: 平台说明允许使用仅在特定平台上可用的功能，而无需实现自定义呈现器或效果。 本文介绍如何使用特定于 iOS 平台的来缩放条目的字体大小。
ms.prod: xamarin
ms.assetid: E8881D4E-902B-4397-A43E-916B2885EC87
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: a0c30d32d53397392a4be2ca984ac5f60975ca78
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/22/2020
ms.locfileid: "86936313"
---
# <a name="entry-font-size-on-ios"></a>IOS 上的输入字体大小

[![下载示例](~/media/shared/download.png)下载示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

此 iOS 平台特定用于缩放的字体大小 [`Entry`](xref:Xamarin.Forms.Entry) ，以确保输入的文本适合控件。 它通过将 [`Entry.AdjustsFontSizeToFitWidth`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.AdjustsFontSizeToFitWidthProperty) 附加属性设置为值在 XAML 中使用 `boolean` ：

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
    <StackLayout Margin="20">
        <Entry x:Name="entry"
               Placeholder="Enter text here to see the font size change"
               FontSize="22"
               ios:Entry.AdjustsFontSizeToFitWidth="true" />
        ...
    </StackLayout>
</ContentPage>
```

此外，还可以使用 Fluent API 从 c # 使用该方法：

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

entry.On<iOS>().EnableAdjustsFontSizeToFitWidth();
```

`Entry.On<iOS>`方法指定此平台特定的仅在 iOS 上运行。 [ `Entry.EnableAdjustsFontSizeToFitWidth` ] （X： Xamarin.Forms 。PlatformConfiguration. iOSSpecific. EnableAdjustsFontSizeToFitWidth （ Xamarin.Forms 。IPlatformElementConfiguration { Xamarin.Forms 。PlatformConfiguration、 Xamarin.Forms 。Entry}））方法，该方法 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 用于缩放输入文本文本的字号，以确保它适合于 [`Entry`](xref:Xamarin.Forms.Entry) 。 此外， [`Entry`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry) 命名空间中的类 `Xamarin.Forms.PlatformConfiguration.iOSSpecific` 还具有 [ `DisableAdjustsFontSizeToFitWidth` ] （x： Xamarin.Forms 。PlatformConfiguration. iOSSpecific. DisableAdjustsFontSizeToFitWidth （ Xamarin.Forms 。IPlatformElementConfiguration { Xamarin.Forms 。PlatformConfiguration、 Xamarin.Forms 。Entry}））方法，该方法可禁用特定于平台的和 [ `SetAdjustsFontSizeToFitWidth` ] （x： Xamarin.Forms 。PlatformConfiguration. iOSSpecific. SetAdjustsFontSizeToFitWidth （ Xamarin.Forms 。IPlatformElementConfiguration { Xamarin.Forms 。PlatformConfiguration、 Xamarin.Forms 。Entry}，System.object）方法，该方法可用于通过调用 [ `AdjustsFontSizeToFitWidth` ] （x：）来切换字体大小缩放。 Xamarin.FormsPlatformConfiguration. iOSSpecific. AdjustsFontSizeToFitWidth （ Xamarin.Forms 。IPlatformElementConfiguration { Xamarin.Forms 。PlatformConfiguration、 Xamarin.Forms 。Entry}））方法：

```csharp
entry.On<iOS>().SetAdjustsFontSizeToFitWidth(!entry.On<iOS>().AdjustsFontSizeToFitWidth());
```

结果就是缩放的字体大小， [`Entry`](xref:Xamarin.Forms.Entry) 以确保输入的文本适合控件：

![调整特定于平台的入口字体大小](entry-font-size-images/entry-font-size.png)

## <a name="related-links"></a>相关链接

- [PlatformSpecifics （示例）](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [创建平台特定信息](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
