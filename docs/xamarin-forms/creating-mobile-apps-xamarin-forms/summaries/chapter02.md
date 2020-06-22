---
title: title:“摘要：第 2 章. 应用剖析”说明："《使用 Xamarin.Forms 创建移动应用》- 摘要：摘要：第 2 章.
description: '应用剖析”ms.prod: xamarin ms.technology: xamarin-forms ms.assetid:8764EB7D-8331-4CF7-9BE1-26D0DEE9E0BB author: davidbritch ms.author: dabritch ms.date:2018/07/17 no-loc: [Xamarin.Forms, Xamarin.Essentials] 摘要：第 2 章.'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 8764EB7D-8331-4CF7-9BE1-26D0DEE9E0BB
author: davidbritch
ms.author: dabritch
ms.date: 07/17/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 78da3ed91acea0c056074d712d368de70b251392
ms.sourcegitcommit: ea9269b5d9e3d68b61bb428560a10034117ee457
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/10/2020
ms.locfileid: "84136911"
---
# <a name="summary-of-chapter-2-anatomy-of-an-app"></a>应用剖析 [![下载示例](~/media/shared/download.png) 下载示例](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02)

此页上的“注意”指出了 Xamarin.Forms 与书中所述内容的不同之处。

> [!NOTE]
> 在 Xamarin.Forms 应用程序中，占据屏幕空间的对象称为视觉对象元素，由 [`VisualElement`](xref:Xamarin.Forms.VisualElement) 类封装。

视觉对象元素可以分为与以下类对应的三个类别： [页](xref:Xamarin.Forms.Page)

- [布局](xref:Xamarin.Forms.Layout)
- [视图](xref:Xamarin.Forms.View)
- `Page` 导数占据整个屏幕，或几乎占据整个屏幕。

通常，页面的子级是组织视觉对象子元素的 `Layout` 导数。 `Layout` 的子级可以是其他 `Layout` 类或 `View` 导数（通常称为元素），它们是诸如文本、位图、滑块、按钮、列表框等的较为熟悉的对象。 本章将演示如何通过 [`Label`](xref:Xamarin.Forms.Label)（它是显示文本的 `View` 导数）来创建应用程序。

例如 Hello

## <a name="say-hello"></a>安装 Xamarin 平台后，可以在 Visual Studio 或 Visual Studio for Mac 中创建新的 Xamarin.Forms 解决方案。

[Hello](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello) 解决方案为通用代码使用可移植类库。 可移植类库已替换为 .NET Standard 库。

> [!NOTE]
> 书中的所有示例代码都已转换为使用 .NET Standard 库。 此示例将演示如何在 Visual Studio 中创建 Xamarin.Forms 解决方案，而无需进行任何修改。

解决方案由四个项目组成： [Hello](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello)，一个由其他项目共享的可移植类库 (PCL)

- [Hello.Droid](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.Droid)，一个适用于 Android 的应用程序项目
- [Hello.iOS](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.iOS)，一个适用于 iOS 的应用程序项目
- [Hello.UWP](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.UWP)，一个适用于通用 Windows 平台（Windows 10 和 Windows 10 移动版）的应用程序项目
- Xamarin.Forms 不再支持 Windows 8.1、Windows Phone 8.1 或 Windows 10 移动版，但 Xamarin.Forms 应用程序可在 Windows 10 桌面上运行。

> [!NOTE]
> 可以将这些应用程序项目中的任何一个作为启动项目，然后在设备或模拟器上生成并运行该程序。

在许多 Xamarin.Forms 程序中，将不会修改应用程序项目。

这些通常是启动程序所需的小存根。 你的主要关注点是所有应用程序的公共库。 在文件内

## <a name="inside-the-files"></a>Hello 程序显示的视觉对象是在 [`App`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello/App.cs) 类的构造函数中定义的。

`App` 派生自 Xamarin.Forms 类 [`Application`](xref:Xamarin.Forms.Application)。 适用于 Xamarin.Forms 的 Visual Studio 解决方案模板创建带有 XAML 文件的页面。

> [!NOTE]
> 在本书[第 7 章](chapter07.md)之前不会涉及 XAML。 Hello PCL 项目的“引用”部分包括以下 Xamarin.Forms 程序集 ：

Xamarin.Forms.Core

- Xamarin.Forms.Xaml
- Xamarin.Forms.Platform
- 五个应用程序项目的“引用”部分包括适用于各个平台的其他程序集：

Xamarin.Forms.Platform.Android

- Xamarin.Forms.Platform.iOS
- Xamarin.Forms.Platform.UWP
- Xamarin.Forms.Platform.WinRT
- Xamarin.Forms.Platform.WinRT.Tablet
- Xamarin.Forms.Platform.WinRT.Phone
- 这些项目的“引用”部分不再列出程序集。

> [!NOTE]
> 相反，项目文件包含引用 Xamarin.Forms NuGet 包的 PackageReference 标记。 Visual Studio 中的“引用”部分列出了 Xamarin.Forms 包，而不是 Xamarin.Forms 程序集 。 每个应用程序项目都包含对 `Xamarin.Forms` 名称空间中的静态 `Forms.Init` 方法的调用。

这将初始化 Xamarin.Forms 库。 为每个平台定义了不同版的 `Forms.Init`。 在以下类中可以找到对该方法的调用： iOS：[`AppDelegate`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.iOS/AppDelegate.cs)

- Android：[`MainActivity`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs)
- UWP：[`App` 类、`OnLaunched` 方法](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs)
- 此外，每个平台都必须实例化共享库中的 `App` 类位置。

在以下类中调用 `LoadApplication`，会发生这种情况： iOS：[`AppDelegate`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.iOS/AppDelegate.cs)

- Android：[`MainActivity`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs)
- UWP：[`MainPage`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.UWP/MainPage.xaml.cs)
- 否则，这些应用程序项目是正常的“不执行任何操作”程序。

PCL 或 SAP？

## <a name="pcl-or-sap"></a>可以使用可移植类库 (PCL) 或 Shared Asset Project (SAP) 中的通用代码创建 Xamarin.Forms 解决方案。

若要创建 SAP 解决方案，请在 Visual Studio 中选择“共享”选项。 HelloSap[](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/HelloSap) 解决方案演示了未经修改的 SAP 模板。 可移植类库已替换为 .NET Standard 库。

> [!NOTE]
> 书中的所有示例代码都已转换为使用 .NET Standard 库。 否则，PCL 和 .NET Standard 库在概念上非常相似。 库方法将所有通用代码捆绑在平台应用程序项目引用的库项目中。

借助 SAP 方法，通用代码实际上存在于所有平台应用程序项目中，并在它们之间共享。 大多数 Xamarin.Forms 开发人员更愿意使用库方法。

在本书中，大多数解决方案都使用一个库。 使用 SAP 的用户在项目名称中包含一个 Sap 后缀。 借助 SAP 方法，共享项目中的代码可以通过使用具有以下预定义标识符的 C# 预处理程序指令（`#if`#`elif` 和 `#endif`）为各种平台执行不同的代码：

iOS：`__IOS__`

- Android：`__ANDROID__`
- UWP：`WINDOWS_UWP`
- 在共享库中，可以在运行时确定在哪个平台上运行，这将在本章后面进行讨论。

文本标签

## <a name="labels-for-text"></a>[Greetings](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Greetings) 解决方案演示了如何向 Greetings 项目添加新的 C# 文件。

该文件定义一个名为 `GreetingsPage` 的类，该类派生自 `ContentPage`。 在本书中，大多数项目都包含单个 `ContentPage` 导数，其名称是带有后缀 `Page` 的项目名称。 `GreetingsPage` 构造函数实例化一个 [`Label`](xref:Xamarin.Forms.Label) 视图，该视图是显示文本的 Xamarin.Forms 视图。

[`Text`](xref:Xamarin.Forms.Label.Text) 属性设置为 `Label` 显示的文本。 该程序将 `Label` 设置为 `ContentPage` 的 `Content`属性。 然后，`App` 类的构造函数实例化 `GreetingsPage` 并将其设置为 `MainPage` 属性。 文本显示在页面的左上角。

在 iOS 上，这意味着它与页面的状态栏重叠。 该问题有以下几种解决方法： 解决方案 1.

### <a name="solution-1-include-padding-on-the-page"></a>在页面上包含填充 在页面上设置 [`Padding`](xref:Xamarin.Forms.Page.Padding) 属性。

`Padding` 的类型为 [`Thickness`](xref:Xamarin.Forms.Thickness)，一个包含四个属性的结构： `Padding` 定义页面内排除内容的区域。

- [`Left`](xref:Xamarin.Forms.Thickness.Left)
- [`Top`](xref:Xamarin.Forms.Thickness.Top)
- [`Right`](xref:Xamarin.Forms.Thickness.Right)
- [`Bottom`](xref:Xamarin.Forms.Thickness.Bottom)

这样，`Label` 不会覆盖 iOS 状态栏。 解决方案 2.

### <a name="solution-2-include-padding-just-for-ios-sap-only"></a>包括仅适用于 iOS 的填充（仅 SAP） 仅在 iOS 上使用带有 C# 预处理器指令的 SAP 设置“填充”属性。

[GreetingsSap](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/GreetingsSap) 解决方案对此进行了演示。 解决方案 3.

### <a name="solution-3-include-padding-just-for-ios-pcl-or-sap"></a>包括仅适用于 iOS 的填充（PCL 或 SAP） 在本书使用的 Xamarin.Forms 版本中，可以使用 [`Device.OnPlatform`](xref:Xamarin.Forms.Device.OnPlatform(System.Action,System.Action,System.Action,System.Action)) 或 [`Device.OnPlatform<T>`](xref:Xamarin.Forms.Device.OnPlatform*) 静态方法选择 PCL 或 SAP 中特定于 iOS 的 `Padding` 属性。

这些方法现在已弃用 `Device.OnPlatform` 方法用于运行特定于平台的代码或选择特定于平台的值。

在内部，它们使用 [`Device.OS`](xref:Xamarin.Forms.Device.OS) 静态只读属性，该属性返回 [`TargetPlatform`](xref:Xamarin.Forms.TargetPlatform) 枚举的成员： 适用于 UWP 设备的 [`Windows`](xref:Xamarin.Forms.TargetPlatform.Windows)。

- [`iOS`](xref:Xamarin.Forms.TargetPlatform.iOS)
- [`Android`](xref:Xamarin.Forms.TargetPlatform.Android)
- 现已弃用 `Device.OnPlatform` 方法、`Device.OS` 属性和 `TargetPlatform` 枚举。

请改用 [`Device.RuntimePlatform`](xref:Xamarin.Forms.Device.RuntimePlatform) 属性，并将 `string` 返回值与以下静态字段进行比较： [`iOS`](xref:Xamarin.Forms.Device.iOS)，字符串“iOS”

- [`Android`](xref:Xamarin.Forms.Device.Android)，字符串“Android”
- [`UWP`](xref:Xamarin.Forms.Device.UWP)，字符串“UWP”，指通用 Windows 平台
- [`Device.Idiom`](xref:Xamarin.Forms.Device.Idiom) 静态只读属性是相关的。

这将返回 [`TargetIdiom`](xref:Xamarin.Forms.TargetIdiom) 的成员，该成员具有以下成员： [`Unsupported`](xref:Xamarin.Forms.TargetIdiom.Unsupported) 未使用

- [`Desktop`](xref:Xamarin.Forms.TargetIdiom.Desktop)
- [`Tablet`](xref:Xamarin.Forms.TargetIdiom.Tablet)
- [`Phone`](xref:Xamarin.Forms.TargetIdiom.Phone)
- 对于 iOS 和 Android，`Tablet` 和 `Phone` 之间边界的纵向宽度是 600 个单位。

对于 Windows 平台，`Desktop` 表示在 Windows 10 下运行的 UWP 应用程序，而 `Phone` 表示在 Windows 10 应用程序下运行的 UWP 应用程序。 解决方案 3a.

## <a name="solution-3a-set-margin-on-the-label"></a>在标签上设置边距 [`Margin`](xref:Xamarin.Forms.View.Margin) 属性引入的时间过晚，无法包含在本书中，但是它的类型也为 `Thickness`，可以在 `Label` 上进行设置，以定义视图之外的区域，该区域包含在视图布局的计算之中。

`Padding` 属性仅可用于 [`Layout`](xref:Xamarin.Forms.Layout) 和 [`Page`](xref:Xamarin.Forms.Page) 导数。

`Margin` 属性可用于所有 [`View`](xref:Xamarin.Forms.View) 导数。 解决方案 4.

## <a name="solution-4-center-the-label-within-the-page"></a>在页面内居中标签 可以将 `Label` 在 `Page` 内居中（或将其置于其他八个位置之一），方法是将 `Label` 的 [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) 和 [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) 属性设置为类型为 [`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions) 的值。

`LayoutOptions` 结构定义以下两个属性： 类型为 [`LayoutAlignment`](xref:Xamarin.Forms.LayoutAlignment) 的 [`Alignment`](xref:Xamarin.Forms.LayoutOptions.Alignment) 属性，一个包含四个成员的枚举：[`Start`](xref:Xamarin.Forms.LayoutAlignment.Start)（意味着左侧或顶部，取决于方向）、[`Center`](xref:Xamarin.Forms.LayoutAlignment.Center) [`End`](xref:Xamarin.Forms.LayoutAlignment.End)（意味着右侧或底部，取决于方向）和 [`Fill`](xref:Xamarin.Forms.LayoutAlignment.Fill)。

- 类型为 `bool` 的 [`Expands`](xref:Xamarin.Forms.LayoutOptions.Expands) 属性。

- 通常，不直接使用这些属性。

相反，这两个属性的组合由类型为 `LayoutOptions` 的八个静态只读属性提供： `HorizontalOptions` 和 `VerticalOptions` 是 Xamarin.Forms 布局中最重要的属性，将在以下章节中进行更详细地讨论：[**第 4 章.** 滚动堆栈](chapter04.md)。

- [`LayoutOptions.Start`](xref:Xamarin.Forms.LayoutOptions.Start)
- [`LayoutOptions.Center`](xref:Xamarin.Forms.LayoutOptions.Center)
- [`LayoutOptions.End`](xref:Xamarin.Forms.LayoutOptions.End)
- [`LayoutOptions.Fill`](xref:Xamarin.Forms.LayoutOptions.Fill)
- [`LayoutOptions.StartAndExpand`](xref:Xamarin.Forms.LayoutOptions.StartAndExpand)
- [`LayoutOptions.CenterAndExpand`](xref:Xamarin.Forms.LayoutOptions.CenterAndExpand)
- [`LayoutOptions.EndAndExpand`](xref:Xamarin.Forms.LayoutOptions.EndAndExpand)
- [`LayoutOptions.FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand)

下面介绍将 `Label` 的 `HorizontalOptions` 和 `VerticalOptions` 属性都设置为 `LayoutOptions.Center` 的结果：

[![Greetings 程序的三倍屏幕截图](images/ch02fg05-small.png "水平和垂直居中标签")](images/ch02fg05-large.png#lightbox "水平和垂直居中标签")

解决方案 5.

## <a name="solution-5-center-the-text-within-the-label"></a>在标签内居中文本 还可以居中文本（或将其置于页面的其他八个位置），方法是将 `Label` 的 [`HorizontalTextAlignment`](xref:Xamarin.Forms.Label.HorizontalTextAlignment) 和 [`VerticalTextAlignment`](xref:Xamarin.Forms.Label.VerticalTextAlignment) 属性设置为 [`TextAlignment`](xref:Xamarin.Forms.TextAlignment) 枚举的成员：

[`Start`](xref:Xamarin.Forms.TextAlignment.Start)，表示左侧或顶部（取决于方向）

- [`End`](xref:Xamarin.Forms.TextAlignment.End)，表示右侧或底部（取决于方向）
- [`Center`](xref:Xamarin.Forms.TextAlignment.Center)
- 这两个属性仅由 `Label` 定义，而 `HorizontalAlignment` 和 `VerticalAlignment` 属性由 `View` 定义并由所有 `View` 导数继承。

视觉对象结果可能看起来很相似，但是正如下一章所述，它们是非常不同的。 相关链接

## <a name="related-links"></a>[第 2 章全文 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch02-Apr2016.pdf)

- [第 2 章示例](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02)
- [第 2 章 F# 示例](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/FS)
- [Xamarin.Forms 入门](~/get-started/index.yml)
- 水平和垂直居中标签
