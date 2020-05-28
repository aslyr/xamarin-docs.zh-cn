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
ms.openlocfilehash: c420fe65b020067169230dd06dbcd5ce65c036ab
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/28/2020
ms.locfileid: "84128617"
---
# <a name="swipeview-swipe-transition-mode-on-android"></a>Android 上的 SwipeView 滑动过渡模式

[![下载示例](~/media/shared/download.png) 下载示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

此 Android 平台特定的控制打开时使用的转换 `SwipeView` 。 它通过将 `SwipeView.SwipeTransitionMode` 可绑定的属性设置为枚举的值，在 XAML 中使用 `SwipeTransitionMode` ：

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core" >
    <StackLayout>
        <SwipeView android:SwipeView.SwipeTransitionMode="Drag">
            <SwipeView.LeftItems>
                <SwipeItems>
                    <SwipeItem Text="Delete"
                               IconImageSource="delete.png"
                               BackgroundColor="LightPink"
                               Invoked="OnDeleteSwipeItemInvoked" />
                </SwipeItems>
            </SwipeView.LeftItems>
            <!-- Content -->
        </SwipeView>
    </StackLayout>
</ContentPage>
```

此外，还可以使用 Fluent API 从 c # 使用该方法：

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

SwipeView swipeView = new Xamarin.Forms.SwipeView();
swipeView.On<Android>().SetSwipeTransitionMode(SwipeTransitionMode.Drag);
// ...
```

`SwipeView.On<Android>`方法指定此平台特定的仅在 Android 上运行。 `SwipeView.SetSwipeTransitionMode`命名空间中的方法 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 用于控制打开时使用的转换 `SwipeView` 。 `SwipeTransitionMode`枚举提供了两个可能的值：

- `Reveal`指示将在内容为 "重击" 的情况下显示滑动项 `SwipeView` ，并为属性的默认值 `SwipeView.SwipeTransitionMode` 。
- `Drag`指示当内容为重击时，将在视图中拖动滑动项 `SwipeView` 。

此外， `SwipeView.GetSwipeTransitionMode` 方法可用于返回 `SwipeTransitionMode` 应用于的 `SwipeView` 。

结果是将指定的 `SwipeTransitionMode` 值应用于 `SwipeView` ，后者控制打开时使用的转换 `SwipeView` ：

[![Android 上的 SwipeView SwipeTransitionModes 的屏幕截图](swipeview-swipetransitionmode-images/swipetransitionmode.png "Android 上的 SwipeTransitionModes")](swipeview-swipetransitionmode-images/swipetransitionmode-large.png#lightbox "Android 上的 SwipeTransitionModes")

## <a name="related-links"></a>相关链接

- [PlatformSpecifics （示例）](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [创建平台特定信息](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
