---
title: 标题：“Xamarin.Forms 双屏平台帮助程序”说明：“本指南介绍了如何使用 Xamarin.Forms DualScreenHelper 类来优化 Surface Duo 和 Surface Neo 等双屏设备的应用体验。”
description: 'ms.prod: xamarin ms.assetid:5aa184c2-5611-427d-85c7-1c56486c3e1b ms.technology: xamarin-forms author: davidortinau ms.author: daortin ms.date:2020/02/08 no-loc: [Xamarin.Forms, Xamarin.Essentials]'
ms.prod: xamarin
ms.assetid: 5aa184c2-5611-427d-85c7-1c56486c3e1b
ms.technology: xamarin-forms
author: davidortinau
ms.author: daortin
ms.date: 02/08/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: d9daf5a24c0dcfd07d529955c411259f4c1359df
ms.sourcegitcommit: ea9269b5d9e3d68b61bb428560a10034117ee457
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/10/2020
ms.locfileid: "84138939"
---
# <a name="xamarinforms-dual-screen-platform-helpers"></a>Xamarin.Forms 双屏平台帮助程序

![](~/media/shared/preview.png "This API is currently pre-release")

[![下载示例](~/media/shared/download.png) 下载示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-dualscreendemos/)

`DualScreenHelper` 类可用于检测某设备是否支持画中画模式，然后使你能够在图片窗口中以图片的形式打开 `ContentPage`。 如果你在 Neo 设备上运行，则在撰写模式下，这将在 WonderBar 中打开页面。

## <a name="properties"></a>属性

- `HasCompactModeSupport` 可用于检查平台是否支持在 CompactMode 中打开 `ContentPage`。
- 如果平台支持，`OpenCompactMode` 将在 CompactMode 中打开 `ContentPage`。

## <a name="example"></a>示例

```csharp
async void OpenCompactWindowClicked(object sender, EventArgs e)
{
    if (!DualScreenHelper.HasCompactModeSupport())
    {
        await DisplayAlert("Unsupported", "This platform doesn't support this feature", "Ok");
        return;
    }

    ContentPage page = new ContentPage() { BackgroundColor = Color.Purple };

    var button = new Button()
    {
        Text = "Close this Window"
    };

    var layout = new StackLayout()
    {
        Children =
        {
            new Label(){ Text = "Welcome to your Compact Mode Window" },
            button
        },
        BackgroundColor = Color.Yellow,
        HorizontalOptions = LayoutOptions.Fill,
        VerticalOptions = LayoutOptions.Fill
    };

    page.Content = new ScrollView() { Content = layout };
    var args = await DualScreenHelper.OpenCompactMode(page);

    button.Command = new Command(async () =>
    {
        await args.CloseAsync();
    });
}
```
