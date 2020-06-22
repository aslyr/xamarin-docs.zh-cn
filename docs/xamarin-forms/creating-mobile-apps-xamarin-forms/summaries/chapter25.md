---
title: title:“摘要：第 25 章. 页面种类”说明："《使用 Xamarin.Forms 创建移动应用》- 摘要：“第 25 章：
description: '页面种类”ms.prod: xamarin ms.technology: xamarin-forms ms.assetid:D1D348F2-6A44-4781-ADCE-A0B7BB9AEF89 author: davidbritch ms.author: dabritch ms.date:2017/11/07 no-loc: [Xamarin.Forms, Xamarin.Essentials] “第 25 章：'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: D1D348F2-6A44-4781-ADCE-A0B7BB9AEF89
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: e66fb50b8d537ee0267457d5b0ab0f417813e676
ms.sourcegitcommit: ea9269b5d9e3d68b61bb428560a10034117ee457
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/10/2020
ms.locfileid: "84136612"
---
# <a name="summary-of-chapter-25-page-varieties"></a>页面种类”摘要 [![下载示例](~/media/shared/download.png) 下载示例](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25)

到目前为止，你已经了解了两个派生自 `Page` 的类，即 `ContentPage` 和 `NavigationPage`。

本章节将介绍另外两个类： [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage)：管理两个页面，即母版页和详细信息页

- [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)：管理通过选项卡访问的多个子页
- 这些页面类型提供了比[第 24 章：页面导航](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter24.md)中讨论的 `NavagationPage` 更复杂的导航选项。

母版页和详细信息页

## <a name="master-and-detail"></a>[`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) 定义了 `Page` 类型的两个属性：[`Master`](xref:Xamarin.Forms.MasterDetailPage.Master) 和 [`Detail`](xref:Xamarin.Forms.MasterDetailPage.Detail)。

通常会将这两个属性都设置为 `ContentPage`。 `MasterDetailPage` 在这两个页面之间显示和切换。 在这两个页面之间切换有两种基本方法：

拆分：母版页和详细信息页并排显示

- 弹出框：详细信息页覆盖或部分覆盖母版页
- 弹出框方法有几种变体（滑动、重叠和交换），但这些变体通常都是平台相关的。

可以将 `MasterDetailPage` 的 [`MasterDetailBehavior`](xref:Xamarin.Forms.MasterDetailPage.MasterBehavior) 属性设置为 [`MasterBehavior`](xref:Xamarin.Forms.MasterBehavior) 枚举的成员： 不过，此属性对电话没有影响。

- [`Default`](xref:Xamarin.Forms.MasterBehavior.Default)
- [`Split`](xref:Xamarin.Forms.MasterBehavior.Split)
- [`SplitOnLandscape`](xref:Xamarin.Forms.MasterBehavior.SplitOnLandscape)
- [`SplitOnPortrait`](xref:Xamarin.Forms.MasterBehavior.SplitOnPortrait)
- [`Popover`](xref:Xamarin.Forms.MasterBehavior.Popover)

电话始终有弹出框行为。 只有平板电脑和桌面窗口才能有拆分行为。 探索行为

### <a name="exploring-the-behaviors"></a>通过 [MasterDetailBehaviors](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/MasterDetailBehaviors) 示例可以试验不同设备上的默认行为。

此程序包含用于母版页和详细信息页的两个单独 `ContentPage` 衍生物（对两者都设置了 `Title` 属性），以及另一个派生自合并它们的 `MasterDetailPage` 的类。 详细信息页包含在 `NavigationPage` 中，因为 UWP 程序没有它就无法正常运行。 Windows 8.1 和 Windows Phone 8.1 平台要求，将位图设置为母版页的 `Icon` 属性。

重返校园

### <a name="back-to-school"></a>[SchoolAndDetail](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/SchoolAndDetail) 示例采用了一种稍微不同的方法来构造程序，以显示 [SchoolOfFineArt](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/SchoolOfFineArt) 库中的学生。

`Master` 和 `Detail` 属性是用 [SchoolAndDetailPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/SchoolAndDetail/SchoolAndDetail/SchoolAndDetail/SchoolAndDetailPage.xaml) 文件中的可视化树定义的，此文件派生自 `MasterDetailPage`。

借助这种安排，可以在母版页和详细信息页之间设置数据绑定。 此 XAML 文件还将 `MasterDetailPage` 的 [`IsPresented`](xref:Xamarin.Forms.MasterDetailPage.IsPresented) 属性设置为 `True`。

这样，母版页会在启动时显示；默认显示的是详细信息页。 当用户从母版页的 `ListView` 中选择项时，[SchoolAndDetailPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/SchoolAndDetail/SchoolAndDetail/SchoolAndDetail/SchoolAndDetailPage.xaml.cs) 文件将 `IsPresented` 设置为 `false`。 随后详细信息页显示： [![学校页和详细信息页的三倍屏幕截图](images/ch25fg09-small.png "MasterDetailPage 中的详细信息页")](images/ch25fg09-large.png#lightbox "MasterDetailPage 中的详细信息页")

你自己的用户界面

### <a name="your-own-user-interface"></a>尽管 Xamarin.Forms 提供了用于切换母版视图和详细信息视图的用户界面，但你也可以提供自己的用户界面。

为此，请执行以下操作： 将 [`IsGestureEnabled`](xref:Xamarin.Forms.MasterDetailPage.IsGestureEnabled) 属性设置为 `false`，以禁用轻扫

- 重写 [`ShouldShowToolbarButton`](xref:Xamarin.Forms.MasterDetailPage.ShouldShowToolbarButton) 方法，并返回 `false`，以隐藏 Windows 8.1 和 Windows Phone 8.1 上的工具栏按钮。
- 然后，必须提供在母版页和详细信息页之间切换的方法，如 [ColorsDetail](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/ColorsDetails) 示例所示。

[MasterDetailTaps](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/MasterDetailTaps) 示例展示了在母版页和详细信息页上使用 `TapGestureRecognizer` 的另一种方法。

TabbedPage

## <a name="tabbedpage"></a>[`TabbedPage`](xref:Xamarin.Forms.TabbedPage) 是可以使用选项卡切换的页面的集合。

它派生自 `MultiPage<Page>`，并且没有定义自己的公共属性或方法。 不过，[`MultiPage<T>`](xref:Xamarin.Forms.MultiPage`1) 确定定义了一个属性： `IList<T>` 类型的 [`Children`](xref:Xamarin.Forms.MultiPage`1.Children) 属性

- 使用页面对象填充此 `Children` 集合。

另一种定义 `TabbedPage` 子元素的方法很像 `ListView` 一样，即使用两个自动生成选项卡页的属性：

`IEnumerable` 类型的 [`ItemsSource`](xref:Xamarin.Forms.MultiPage`1.ItemsSource)

- `DataTemplate` 类型的 [`ItemTemplate`](xref:Xamarin.Forms.MultiPage`1.ItemTemplate)
- 不过，当集合包含多个项时，这种方法在 iOS 上并不适用。

`MultiPage<T>` 定义了另外两个属性，可便于跟踪当前查看的页面：

`T` 类型的 [`CurrentPage`](xref:Xamarin.Forms.MultiPage`1.CurrentPage)：是指页面

- `Object` 类型的 [`SelectedItem`](xref:Xamarin.Forms.MultiPage`1.SelectedItem)：是指 `ItemsSource` 集合中的对象
- `MultiPage<T>` 还定义了两个事件：

[`PagesChanged`](xref:Xamarin.Forms.MultiPage`1.PagesChanged)：在 `ItemsSource` 集合更改时触发

- [`CurrentPageChanged`](xref:Xamarin.Forms.MultiPage`1.CurrentPageChanged)：在查看的页面更改时触发
- 离散选项卡页

### <a name="discrete-tab-pages"></a>[DiscreteTabbedColors](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/DiscreteTabbedColors) 示例由三个选项卡页组成，它们以三种不同的方式显示颜色。

每个选项卡都是 `ContentPage` 衍生物，然后 `TabbedPage` 衍生物 [DiscreteTabbedColorsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/DiscreteTabbedColors/DiscreteTabbedColors/DiscreteTabbedColors/DiscreteTabbedColorsPage.xaml) 合并这三页。 对于 `TabbedPage` 中显示的每一页，必须使用 `Title` 属性来指定选项卡中的文本；Apple Store 还要求使用图标，因此为 iOS 设置了 `Icon` 属性：

[![离散选项卡颜色的三倍屏幕截图](images/ch25fg13-small.png "TabbedPage")](images/ch25fg13-large.png#lightbox "TabbedPage")

[StudentNotes](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/StudentNotes) 示例包含列出所有学生的主页。

点击学生后，便会转到 `TabbedPage` 衍生物 [`StudentNotesDataPage`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/StudentNotes/StudentNotes/StudentNotes/StudentNotesDataPage.xaml)，它将三个 `ContentPage` 对象合并到自己的可视化树中，其中一个对象允许为相应学生输入一些笔记。 使用 ItemTemplate

### <a name="using-an-itemtemplate"></a>[MultiTabbedColor](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/MultiTabbedColors) 示例使用 [Xamarin.FormsBook.Toolkit](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 库中的 [`NamedColor`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NamedColor.cs) 类。

[MultiTabbedColorsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/MultiTabbedColors/MultiTabbedColors/MultiTabbedColors/MultiTabbedColorsPage.xaml) 文件将 `TabbedPage` 的 `DataTemplate` 属性设置为以 `ContentPage` 开头的可视化树，其中包含对 `NamedColor` 属性的绑定（包括对 `Title` 属性的绑定）。 不过，这在 iOS 上是有问题的。

只有少数项可以显示，并且没有好办法向它们提供图标。 相关链接

## <a name="related-links"></a>[第 25 章全文 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch25-Apr2016.pdf)

- [第 25 章示例](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25)
- [母版页-详细信息页](~/xamarin-forms/app-fundamentals/navigation/master-detail-page.md)
- [选项卡页](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md)
- MasterDetailPage 中的详细信息页
