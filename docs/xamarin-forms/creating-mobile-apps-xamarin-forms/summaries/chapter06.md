---
title: “第 6 章： 按钮单击”摘要
description: 使用 Xamarin.Forms 创建移动应用：“第 6 章： 按钮单击”摘要
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: D4F9C429-A6CF-40FA-AC68-3F149307A5F9
author: davidbritch
ms.author: dabritch
ms.date: 07/18/2018
ms.openlocfilehash: 12c8cdc19f9e6765ca25ade97bcfdbffb7b60381
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/10/2020
ms.locfileid: "61334546"
---
# <a name="summary-of-chapter-6-button-clicks"></a>“第 6 章： 按钮单击”摘要

[![下载示例](~/media/shared/download.png) 下载示例](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06)

[`Button`](xref:Xamarin.Forms.Button) 是允许用户启动命令的视图。 `Button` 由文本（以及可选的图像，如[第 13 章：位图](chapter13.md)所示）标识。 因此，`Button` 定义了许多与 `Label` 相同的属性：

- [`Text`](xref:Xamarin.Forms.Button.Text)
- [`FontFamily`](xref:Xamarin.Forms.Button.FontFamily)
- [`FontSize`](xref:Xamarin.Forms.Button.FontSize)
- [`FontAttributes`](xref:Xamarin.Forms.Button.FontAttributes)
- [`TextColor`](xref:Xamarin.Forms.Button.TextColor)

`Button` 还定义了三个控制边框外观的属性，但这些属性及其相互独立性是否受支持是特定于平台的：

- `Color` 类型的 [`BorderColor`](xref:Xamarin.Forms.Button.BorderColor)
- `Double` 类型的 [`BorderWidth`](xref:Xamarin.Forms.Button.BorderWidth)
- `Double` 类型的 [`BorderRadius`](xref:Xamarin.Forms.Button.BorderRadius)

`Button` 还继承 `VisualElement` 和 `View` 的所有属性，包括 `BackgroundColor`、`HorizontalOptions` 和 `VerticalOptions`。

## <a name="processing-the-click"></a>处理单击

`Button` 类定义了在用户点击 `Button` 时触发的 [`Clicked`](xref:Xamarin.Forms.Button.Clicked) 事件。 `Click` 处理程序的类型为 `EventHandler`。 第一个参数是生成事件的 `Button` 对象；第二个参数是不提供其他任何信息的 `EventArgs` 对象。

[ButtonLogger  ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/ButtonLogger) 示例展示了简单的 `Clicked` 处理。

## <a name="sharing-button-clicks"></a>共享按钮单击

虽然多个 `Button` 视图可以共享同一个 `Clicked` 处理程序，但此处理程序通常需要确定哪一个 `Button` 对特定事件负责。 一种方法是，将各种 `Button` 对象存储为字段，并检查哪个对象正在此处理程序中触发事件。

[TwoButtons  ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/TwoButtons) 示例展示了这种方法。 此程序还展示了如何在按 `Button` 不再有效时将 `Button` 的 [`IsEnabled`](xref:Xamarin.Forms.VisualElement.IsEnabled) 属性设置为 `false`。 禁用的 `Button` 不生成 `Clicked` 事件。

## <a name="anonymous-event-handlers"></a>匿名事件处理程序

可以将 `Clicked` 处理程序定义为匿名 lambda 函数，如 [ButtonLambdas  ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/ButtonLambdas) 示例所示。 不过，必须有混乱而复杂的某反射代码，才能共享匿名处理程序。

## <a name="distinguishing-views-with-ids"></a>使用 ID 区分视图

还可以通过将 [`StyleId`](xref:Xamarin.Forms.Element.StyleId) 属性或 [`AutomationId`](xref:Xamarin.Forms.Element.AutomationId) 属性设置为 `string`，区分多个 `Button` 对象。 此属性由 `Element` 定义，但它没有用于 Xamarin.Forms。 它只被应用程序使用。

[SimplestKeypad  ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/SimplestKeypad) 示例对数字键盘上的所有 10 个数字键使用同一个事件处理程序，并使用 `StyleId` 属性区分它们：

[![最简单键盘的三倍屏幕截图](images/ch06fg04-small.png "计算器")](images/ch06fg04-large.png#lightbox "计算器")

## <a name="saving-transient-data"></a>保存临时数据

很多应用程序都需要在程序终止时保存数据，并在程序再次启动时重载相应数据。 [`Application`](xref:Xamarin.Forms.Application) 类定义了多个有助于程序保存和还原临时数据的成员：

- [`Properties`](xref:Xamarin.Forms.Application.Properties) 属性是包含 `string` 键和 `object` 项的字典。 字典内容在程序终止前自动保存到应用程序本地存储中，并在程序启动时重载。
- `Application` 类定义了三个由程序的标准 `App` 类重写的受保护虚方法：[`OnStart`](xref:Xamarin.Forms.Application.OnStart)、[`OnSleep`](xref:Xamarin.Forms.Application.OnSleep) 和 [`OnResume`](xref:Xamarin.Forms.Application.OnResume)。 这些是指应用程序生命周期  事件。
- [`SavePropertiesAsync`](xref:Xamarin.Forms.Application.SavePropertiesAsync) 方法保存字典内容。

没有必要调用 `SavePropertiesAsync`。 字典内容在程序终止前自动保存，并在程序启动前自动检索。 在程序测试期间，在程序发生故障时保存数据是很有用的。

还有用的是：

- [`Application.Current`](xref:Xamarin.Forms.Application.Current)：静态属性，返回可用于获取 `Properties` 字典的当前 `Application` 对象。

第一步是，标识页面上所有要在程序终止时暂留的变量。 如果知道这些变量发生变化的所有位置，只需在发生变化时将它们添加到 `Properties` 字典即可。 在页面的构造函数中，可以从 `Properties` 字典设置变量（如果键存在的话）。

较大程序可能需要处理应用程序生命周期事件。 最重要的是 `OnSleep` 方法。 此方法调用表明程序已离开前台。 用户也许按下了设备上的“主屏幕”  按钮，也许显示了所有应用程序，也许正在关机电话。 `OnSleep` 调用是程序终止前唯一收到的通知。 程序应利用此机会确保 `Properties` 字典是最新的。

`OnResume` 调用指明程序没有在上次调用 `OnSleep` 后终止，而是此时再次在前台运行。 程序可利用此机会刷新 Internet 连接（举个例子）。

`OnStart` 调用发生在程序启动期间。 没有必要等到此方法调用后才访问 `Properties` 字典，因为内容在 `App` 构造函数调用时就已还原。

[PersistentKeypad  ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/PersistentKeypad) 示例与 SimplestKeypad  极为相似，不同之处在于，程序使用 `OnSleep` 重写来保存当前键盘输入，并使用页面构造函数来还原相应数据。

> [!NOTE]
> Xamarin.Essentials [Preferences](~/essentials/preferences.md) 类提供了另一种保存程序设置的方法。

## <a name="related-links"></a>相关链接

- [第 6 章全文 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch06-Apr2016.pdf)
- [第 6 章示例](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06)
- [第 6 章 F# 示例](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/FS)
- [Xamarin.Forms 按钮](~/xamarin-forms/user-interface/button.md)
