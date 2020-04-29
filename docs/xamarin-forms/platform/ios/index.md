---
title: Xamarin 中的 iOS 平台功能
description: 向 Xamarin 应用程序添加 iOS 特定功能。
ms.prod: xamarin
ms.assetid: 634AB62E-68C8-454C-838B-F1CC4E4E21BC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/05/2020
ms.openlocfilehash: 97b9b70a1df61beb7b064c99721b4277e2570b41
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "82517060"
---
# <a name="ios-platform-features-in-xamarinforms"></a>Xamarin 中的 iOS 平台功能

开发适用于 iOS 的 Xamarin 应用程序需要 Visual Studio。 "[支持的平台" 页](~/get-started/supported-platforms.md)包含有关先决条件的详细信息。

## <a name="platform-specifics"></a>平台特定内容

平台说明允许使用仅在特定平台上可用的功能，而无需实现自定义呈现器或效果。

以下特定于平台的功能适用于 Xamarin。 Forms 视图、页面和 iOS 上的布局：

- 对任何[`VisualElement`](xref:Xamarin.Forms.VisualElement)的模糊支持。 有关详细信息，请参阅[iOS 上的 VisualElement 模糊](visualelement-blur.md)。
- 禁用受支持[`VisualElement`](xref:Xamarin.Forms.VisualElement)的旧版颜色模式。 有关详细信息，请参阅[VisualElement 旧版彩色模式 On iOS](legacy-color-mode.md)。
- 在上启用投影[`VisualElement`](xref:Xamarin.Forms.VisualElement)。 有关详细信息，请参阅[iOS 上的 VisualElement 投影](visualelement-drop-shadow.md)。
- 使[`VisualElement`](xref:Xamarin.Forms.VisualElement)对象成为触控事件的第一个响应方。 有关详细信息，请参阅[VisualElement First 响应](visualelement-first-responder.md)者。

以下特定于平台的功能适用于适用于 iOS 的 Xamarin 视图：

- 设置[`Cell`](xref:Xamarin.Forms.Cell)背景色。 有关详细信息，请参阅[iOS 上的单元格背景色](cell-background-color.md)。
- 控制在中发生项目选择的[`DatePicker`](xref:Xamarin.Forms.DatePicker)时间。 有关详细信息，请参阅[iOS 上的 DatePicker 项选择](datepicker-selection.md)。
- [`Entry`](xref:Xamarin.Forms.Entry)通过调整字体大小确保输入文本适合于。 有关详细信息，请参阅[iOS 上的输入字体大小](entry-font-size.md)。
- 在中设置光标颜色[`Entry`](xref:Xamarin.Forms.Entry)。 有关详细信息，请参阅[iOS 上的入口游标颜色](entry-cursor-color.md)。
- 控制标题[`ListView`](xref:Xamarin.Forms.ListView)单元是否在滚动过程中浮动。 有关详细信息，请参阅[iOS 上的 ListView 组标头样式](listview-group-header-style.md)。
- 控制在更新[`ListView`](xref:Xamarin.Forms.ListView)项集合时是否禁用行动画。 有关详细信息，请参阅[在 iOS 上 ListView 行动画](listview-row-animations.md)。
- 在上设置分隔符样式[`ListView`](xref:Xamarin.Forms.ListView)。 有关详细信息，请参阅[iOS 上的 ListView 分隔符样式](listview-separator-style.md)。
- 控制在中发生项目选择的[`Picker`](xref:Xamarin.Forms.Picker)时间。 有关详细信息，请参阅[iOS 上的选取器项目选择](picker-selection.md)。
- 控制是否[`SearchBar`](xref:Xamarin.Forms.SearchBar)具有背景。 有关详细信息，请参阅[iOS 上的 SearchBar 样式](searchbar-style.md)。
- 通过点击[`Slider.Value`](xref:Xamarin.Forms.Slider.Value) [`Slider`](xref:Xamarin.Forms.Slider)栏上的某个位置，而不是通过拖动`Slider`滚动块，可以设置属性。 有关详细信息，请参阅[滚动条上](slider-thumb.md)的滚动条。
- 控制在打开时使用的转换`SwipeView`。 有关详细信息，请参阅[SwipeView 滑动过渡模式](swipeview-swipetransitionmode.md)。
- 控制在中发生项目选择的[`TimePicker`](xref:Xamarin.Forms.TimePicker)时间。 有关详细信息，请参阅[iOS 上的 TimePicker 项选择](timepicker-selection.md)。

以下特定于平台的功能适用于 iOS 上的 Xamarin 窗体页：

- 当泄露母版页时，控制的[`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage)详细信息页是否已应用阴影。 有关详细信息，请参阅[MasterDetailPage 影子](masterdetailpage-shadow.md)。
- 在上隐藏导航栏分隔符[`NavigationPage`](xref:Xamarin.Forms.NavigationPage)。 有关详细信息，请参阅[iOS 上的 NavigationPage Bar 分隔符](navigation-bar-separator.md)。
- 控制导航栏是否半透明。 有关详细信息，请参阅[导航栏半透明度 On iOS](navigation-bar-translucent.md)。
- 控制上的状态栏文本颜色是否[`NavigationPage`](xref:Xamarin.Forms.NavigationPage)经过调整以匹配导航栏的发光度。 有关详细信息，请参阅[NavigationPage Bar 文本 Color Mode On iOS](status-bar-text-color.md)。
- 控制页面标题在页面导航栏中是否显示为大标题。 有关详细信息，请参阅[iOS 上的大型页面标题](page-large-title.md)。
- 设置上主指示器的可见性[`Page`](xref:Xamarin.Forms.Page)。 有关详细信息，请参阅[iOS 上的主指示器可见性](page-home-indicator.md)。
- 在上设置状态栏可见性[`Page`](xref:Xamarin.Forms.Page)。 有关详细信息，请参阅[iOS 上的页面状态栏可见性](page-status-bar-visibility.md)。
- 确保页面内容位于屏幕上对于所有 iOS 设备都是安全的区域。 有关详细信息，请参阅[iOS 上的安全区域布局指南](page-safe-area-layout.md)。
- 设置模式页的呈现样式。 有关详细信息，请参阅[模式页面表示形式样式](page-presentation-style.md)。
- 在上设置选项卡栏的半透明度模式[`TabbedPage`](xref:Xamarin.Forms.TabbedPage)。 有关详细信息，请参阅[TabbedPage 半透明选项卡栏 On iOS](tabbedpage-translucent-tabbar.md)。

以下特定于平台的功能适用于适用于 iOS 的 Xamarin 布局布局：

- 控制是否[`ScrollView`](xref:Xamarin.Forms.ScrollView)处理触摸手势或将其传递给其内容。 有关详细信息，请参阅[ScrollView 内容涉及 iOS](scrollview-content-touches.md)。

IOS 上的 Xamarin [`Application`](xref:Xamarin.Forms.Application)类提供以下特定于平台的功能：

- 禁用命名字体大小的辅助功能缩放。 有关详细信息，请参阅[iOS 上命名字体大小的辅助功能缩放](named-font-size-scaling.md)。
- 启用要在主线程上执行的控件布局和呈现更新。 有关详细信息，请参阅[iOS 上的主线程控制更新](main-thread-updates-ui.md)。
- 启用滚动[`PanGestureRecognizer`](xref:Xamarin.Forms.PanGestureRecognizer)视图中的，以便使用滚动视图捕获和共享平移手势。 有关详细信息，请参阅[iOS 上的并发平移手势识别](application-pan-gesture.md)。

## <a name="ios-specific-formatting"></a>特定于 iOS 的格式

Xamarin 可以设置跨平台用户界面样式和颜色，但也可以使用 iOS 项目中的平台 Api 来设置 iOS 主题。

[阅读](formatting.md)有关使用 IOS 特定 api 设置用户界面格式的详细信息，如**Info.plist**配置和`UIAppearance` API。

![](images/status-white-sml.png "iOS Theming")

## <a name="other-ios-features"></a>其他 iOS 功能

使用[自定义呈现](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)器、 [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md)和[MessagingCenter](~/xamarin-forms/app-fundamentals/messaging-center.md)，可以将各种本机功能合并到适用于 iOS 的 Xamarin 应用程序中。
