---
title: 第 24 章摘要。 页导航
description: 使用 Xamarin.Forms 创建移动应用：第 24 章摘要。 页导航
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: DDCDB49C-6008-4F72-B095-463EE21D7C23
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: fd8e4fc77917fcba9bc61e59ced714ac1cd6fbe9
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/13/2020
ms.locfileid: "77130834"
---
# <a name="summary-of-chapter-24-page-navigation"></a>第 24 章摘要。 页导航

[![下载示例](~/media/shared/download.png) 下载示例](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24)

许多应用程序由多个页面组成，用户会在这些页面间导航。 应用程序始终具有一个主要页面  或主页  页面，用户可以从其中导航到其他页面（这些页面在堆栈中进行维护，以便可向回导航）。 [第 25 章：  页类型](chapter25.md)中介绍了其他导航选项。

## <a name="modal-pages-and-modeless-pages"></a>模式页面和无模式页面

`VisualElement` 定义一个类型为 [`INavigation`](xref:Xamarin.Forms.INavigation) 的 [`Navigation`](xref:Xamarin.Forms.NavigableElement.Navigation) 属性，其中包含以下两个用于导航到新页面的方法：

- [`PushAsync`](xref:Xamarin.Forms.INavigation.PushAsync(Xamarin.Forms.Page))
- [`PushModalAsync`](xref:Xamarin.Forms.INavigation.PushModalAsync(Xamarin.Forms.Page))

这两个方法都接受 `Page` 实例作为参数，并返回一个 `Task` 对象。 以下两个方法会导航回上一页：

- [`PopAsync`](xref:Xamarin.Forms.INavigation.PopAsync)
- [`PopModalAsync`](xref:Xamarin.Forms.INavigation.PopModalAsync)

如果用户界面具有自己的“返回”  按钮（如 Android 和 Windows 手机），则应用程序无需调用这些方法。

尽管这些方法可从任何 `VisualElement` 获取，但通常会通过当前 `Page` 实例的 `Navigation` 属性调用它们。

当用户在返回到上一页之前需要在页面上提供某些信息时，应用程序通常使用模式页面。 不是模式页面的页面有时称为无模式或分层  。 页面本身中的任何内容都不会将它区分为模式或无模式；而是通过用于导航到它的方法来进行控制。 若要跨所有平台正常工作，模式页面必须提供自己的用户界面，以便导航回到上一页。

通过 [ModelessAndModal  ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ModelessAndModal) 示例可以探索无模式页面与模式页面之间的差异。 使用页面导航的任何应用程序都必须将其主页传递到 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) 构造函数（通常在程序的 `App` 类中）。 一个好处是，不再需要在适用于 iOS 的页面上设置 `Padding`。

你会发现对于无模式页面，会显示页面的 [`Title`](xref:Xamarin.Forms.Page.Title) 属性。 iOS、Android 和 Windows 平板电脑和台式机平台全都提供了一个用户界面元素，用于导航回到上一页。 当然，Android 和 Windows Phone 设备具有标准的“返回”  按钮用于返回。

对于模式页面，不显示页面 `Title`，且不提供任何用户界面元素以返回到上一页。 尽管可以使用 Android 和 Windows Phone 标准“返回”  按钮返回到上一页，但其他平台上的模式页面必须提供自己的机制以用于返回。

### <a name="animated-page-transitions"></a>对页面过渡效果进行动画处理

如果希望页面过渡包含动画，则会提供各种导航方法的备用版本，其中第二个参数设置为 `true`：

- [PushAsync](xref:Xamarin.Forms.INavigation.PushAsync(Xamarin.Forms.Page,System.Boolean))
- [PushModalAsync](xref:Xamarin.Forms.INavigation.PushModalAsync(Xamarin.Forms.Page,System.Boolean))
- [PopAsync](xref:Xamarin.Forms.INavigation.PopAsync(System.Boolean))
- [PopModalAsync](xref:Xamarin.Forms.INavigation.PopModalAsync(System.Boolean))

不过在默认情况下，标准页面导航方法包含动画，因此这些方法仅对在启动时导航到特定页面（如本章结尾所讨论）或是在提供自己的入口动画时（如 [第 22 章：  动画](chapter22.md)中所讨论）十分有用。

### <a name="visual-and-functional-variations"></a>视觉对象和功能变体

`NavigationPage` 包含两个属性，在 `App` 方法中实例化类时可以设置它们：

- [`BarBackgroundColor`](xref:Xamarin.Forms.NavigationPage.BarBackgroundColor)
- [`BarTextColor`](xref:Xamarin.Forms.NavigationPage.BarTextColor)

`NavigationPage` 还包括四个附加的可绑定属性，这些属性会影响在其上设置它们的特定页面：

- [`SetHasBackButton`](xref:Xamarin.Forms.NavigationPage.SetHasBackButton(Xamarin.Forms.Page,System.Boolean)) 和 [`GetHasBackButton`](xref:Xamarin.Forms.NavigationPage.GetHasBackButton(Xamarin.Forms.Page))
- [`SetHasNavigationBar`](xref:Xamarin.Forms.NavigationPage.SetHasNavigationBar(Xamarin.Forms.BindableObject,System.Boolean)) 和 [`GetHasNavigationBar`](xref:Xamarin.Forms.NavigationPage.GetHasNavigationBar(Xamarin.Forms.BindableObject))
- [`SetBackButtonTitle`](xref:Xamarin.Forms.NavigationPage.SetBackButtonTitle(Xamarin.Forms.BindableObject,System.String)) 和 [`GetBackButtonTitle`](xref:Xamarin.Forms.NavigationPage.GetBackButtonTitle(Xamarin.Forms.BindableObject)) 仅适用于 iOS
- [`SetTitleIcon`](xref:Xamarin.Forms.NavigationPage.SetTitleIcon(Xamarin.Forms.BindableObject,Xamarin.Forms.FileImageSource)) 和 [`GetTitleIcon`](xref:Xamarin.Forms.NavigationPage.GetTitleIcon(Xamarin.Forms.BindableObject)) 仅适用于 iOS 和 Android

### <a name="exploring-the-mechanics"></a>探索机制

页面导航方法全都是异步的，应与 `await` 一起使用。 完成并不表示页面导航已完成，而只是表示检查页面导航堆栈是安全的。

当一个页面导航到另一个页面时，第一个页面通常会调用其 [`OnDisappearing`](xref:Xamarin.Forms.Page.OnDisappearing) 方法，第二个页面调用其 [`OnAppearing`](xref:Xamarin.Forms.Page.OnAppearing) 方法。 同样，当一个页面返回到另一个页面时，第一个页面会调用其 `OnDisappearing` 方法，第二个页面通常调用其 `OnAppearing` 方法。 这些调用的顺序（以及调用导航的异步方法的完成）与平台相关。 在前面两个语句中使用“通常”一词是由于 Android 模式页面导航，在其中不会进行这些方法调用。

此外，对 `OnAppearing` 和 `OnDisappearing` 方法的调用不一定表示页面导航。

`INavigation` 接口包含两个集合属性，通过它们可以检查导航堆栈：

- 用于无模式堆栈的类型为 `IReadOnlyList<Page>` 的 [`NavigationStack`](xref:Xamarin.Forms.INavigation.NavigationStack)
- 用于模式堆栈的类型为 `IReadOnlyList<Page>` 的 [`ModalStack`](xref:Xamarin.Forms.INavigation.ModalStack)

最安全的方法是通过 `NavigationPage` 的 `Navigation` 属性（应是 `App` 类的 [`MainPage`](xref:Xamarin.Forms.Application.MainPage) 属性）访问这些堆栈。 只有在异步页面导航方法完成之后，才能安全地检查这些堆栈。 如果当前页面是模式页面，则 `NavigationPage` 的 [`CurrentPage`](xref:Xamarin.Forms.NavigationPage.CurrentPage) 属性并不指示当前页面，而是指示最后一个无模式页面。

通过 [SinglePageNavigation  ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/SinglePageNavigation) 示例可以探索页面导航和堆栈以及法律类型的页面导航：

- 无模式页面可以导航到另一个无模式页面或模式页面
- 模式页面只能导航到另一个模式页面

### <a name="enforcing-modality"></a>强制执行模态

当需要从用户处获取一些信息时，应用程序会使用模式页面。 在提供信息之前，应禁止用户返回到上一页。 在 iOS 上，可以轻松地提供“返回”  按钮，并且仅在用户完成了对页面的操作时才启用它。 但对于 Android 和 Windows Phone 设备，应用程序应重写 [`OnBackButtonPressed`](xref:Xamarin.Forms.Page.OnBackButtonPressed) 方法，并返回 `true`（如果程序自己处理了“返回”  按钮本身），如 [ModalEnforcement  ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ModalEnforcement) 示例中所示。

[MvvmEnforcement  ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/MvvmEnforcement) 示例演示这如何在 MVVM 方案中正常运行。

## <a name="navigation-variations"></a>导航变体

如果可能会多次导航到特定模式页面，则它应保留信息，以便用户可以编辑信息，而不是再次键入所有信息。 可以通过保留模式页面的特定实例来处理这种情况，但更好的方法（特别是在 iOS 上）是在视图模型中保留信息。

### <a name="making-a-navigation-menu"></a>创建导航菜单

[ViewGalleryType  ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ViewGalleryType) 示例演示如何使用 `TableView` 列出菜单项。 每个项都与特定页面的一个 `Type` 对象关联。 选择该项后，程序会实例化页面并导航到该页面。

[![视图库类型的三重屏幕截图](images/ch24fg21-small.png "TableView 列表菜单项")](images/ch24fg21-large.png#lightbox "TableView 列表菜单项")

[**ViewGalleryInst**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ViewGalleryInst) 示例示例有点不同，因为菜单包含每个页面的实例（而不是类型）。 这有助于保留每个页面中的信息，但所有页面都必须在程序启动时进行实例化。

### <a name="manipulating-the-navigation-stack"></a>操作导航堆栈

[StackManipulation  ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/StackManipulation) 演示 `INavigation` 所定义的多个函数，通过这些函数可以采用结构化方法操作导航堆栈：

- [`RemovePage`](xref:Xamarin.Forms.INavigation.RemovePage(Xamarin.Forms.Page))
- [`InsertPageBefore`](xref:Xamarin.Forms.INavigation.InsertPageBefore(Xamarin.Forms.Page,Xamarin.Forms.Page))
- [`PopToRootAsync`](xref:Xamarin.Forms.INavigation.PopToRootAsync) 和 [`PopToRootAsync`](xref:Xamarin.Forms.INavigation.PopToRootAsync(System.Boolean)) 及可选动画

### <a name="dynamic-page-generation"></a>动态页面生成

[BuildAPage  ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/BuildAPage) 示例演示如何在运行时基于用户输入构造页面。

## <a name="patterns-of-data-transfer"></a>数据传输模式

通常需要在页面之间共享数据 &mdash; 用于将数据传输到导航页面，并且使页面可以将数据返回给调用它的页面。 可通过几种方法实现此目的。

### <a name="constructor-arguments"></a>构造函数参数

导航到新页面时，可以使用允许页面初始化自己的构造函数参数实例化页面类。 [SchoolAndStudents  ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/SchoolAndStudents) 示例演示了这一点。 导航页面还可以让导航到它的页面设置其 `BindingContext`。

### <a name="properties-and-method-calls"></a>属性和方法调用

其余数据传输示例探讨了当一个页面导航到另一个页面并返回时在页面之间传递信息的问题。 在这些讨论中，home  页面导航到 info  页面，并须将初始化的信息传输到 info  页面。 info  页面从用户处获取其他信息，并将信息传输到 home  页面。

在实例化 info  页面后，home  页面便可轻松访问该页面中的公共方法和属性。 info  页面也可以访问 home  页面中的公共方法和属性，但是为此选择一个好时机可能十分麻烦。 [DateTransfer1  ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer1) 示例在其 `OnDisappearing` 重写中执行此操作。 一个缺点是 info  页面需要知道 home  页面的类型。

### <a name="messagingcenter"></a>MessagingCenter

Xamarin.Forms [`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) 类提供了另一种方法，用于使两个页面相互通信。 消息由文本字符串标识，可以附带任何对象。

希望接收来自特定类型的消息的程序必须使用 [`MessagingCenter.Subscribe`](xref:Xamarin.Forms.MessagingCenter.Subscribe*) 订阅这些消息，并指定回调函数。 稍后，它可以通过调用 [`MessagingCenter.Unsubscribe`](xref:Xamarin.Forms.MessagingCenter.Unsubscribe*) 来取消订阅。 回调函数接收从指定类型（具有通过 [`Send`](xref:Xamarin.Forms.MessagingCenter.Send*) 方法发送的指定名称）发送的任何消息。

[DateTransfer2  ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer2) 程序演示了如何使用消息中心传输数据，但是同样，这要求 info  页面知道 home  页面的类型。

### <a name="events"></a>事件

事件是一种历史悠久的方法，使一个类可以在不知道另一个类的类型的情况下将信息发送给该类。 在 [DateTransfer3  ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer3) 示例中，info  类定义一个在信息准备就绪时激发的事件。 但是，没有方便的位置可供 home  页面分离事件处理程序。

### <a name="the-app-class-intermediary"></a>应用类中介

[DateTransfer4  ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer4) 示例演示如何通过 home  页面和 info  页面访问 `App` 类中定义的属性。 这是一个很好的解决方案，但下一个部分会介绍更好的解决方案。

### <a name="switching-to-a-viewmodel"></a>切换到 ViewModel

将 ViewModel 用于信息会使 home  页面和 info  页面可以共享信息类的实例。 [DateTransfer5  ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer5) 示例对此进行了演示。

### <a name="saving-and-restoring-page-state"></a>保存和还原页面状态

如果在 info  页面处于活动状态时程序进入睡眠状态，则在应用程序必须保存信息时，`App` 类中介或 ViewModel 方法是理想的方法。 [DateTransfer6  ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer6) 示例演示了这一点。

## <a name="saving-and-restoring-the-navigation-stack"></a>保存和还原导航堆栈

在一般情况下，进入睡眠状态的多页面程序应在还原时导航到同一个页面。 这意味着此类程序应保存导航堆栈的内容。 此部分演示如何在旨在实现此用途的类中自动执行此过程。 此类还会调用各个页面，使它们可以保存和还原其页面状态。

[Xamarin.FormsBook.Toolkit  ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 库定义一个名为 [`IPersistantPage`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/IPersistentPage.cs) 的接口，类可以实现该接口以保存和还原 `Properties` 字典中的项。

Xamarin.FormsBook.Toolkit  库中的 [`MultiPageRestorableApp`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MultiPageRestorableApp.cs) 类派生自 `Application`。 随后可以从 `MultiPageRestorableApp` 派生 `App` 类并执行一些保养工作。

[StackRestoreDemo  ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/StackRestoreDemo) 演示了 `MultiPageRestorableApp` 的使用。

### <a name="something-like-a-real-life-app"></a>类似于真实应用的内容

[NoteTaker  ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/NoteTaker) 示例还使用 `MultiPageRestorableApp`，并允许输入和编辑保存在 `Properties` 字典中的注释。

## <a name="related-links"></a>相关链接

- [第 24 章全文 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch24-Apr2016.pdf)
- [第 24 章示例](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24)
- [分层导航](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md)
- [模式页](~/xamarin-forms/app-fundamentals/navigation/modal.md)
