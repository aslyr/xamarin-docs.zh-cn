---
title: title:“摘要：第 1 章. Xamarin.Forms 适合什么场景？”
description: description:"《使用 Xamarin.Forms 创建移动应用》- 摘要：摘要：第 1 章. Xamarin.Forms 适合什么场景？”
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F3F864FF-EE70-49D0-90D1-388889037625
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 48b2fb429d206f6582886c94d4d99839d790dc8d
ms.sourcegitcommit: ea9269b5d9e3d68b61bb428560a10034117ee457
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/10/2020
ms.locfileid: "84136920"
---
# <a name="summary-of-chapter-1-how-does-xamarinforms-fit-in"></a>ms.prod: xamarin ms.technology: xamarin-forms ms.assetid:F3F864FF-EE70-49D0-90D1-388889037625 author: davidbritch ms.author: dabritch ms.date:2018 年 7 月 19 日 no-loc: [Xamarin.Forms, Xamarin.Essentials] 摘要：第 1 章.

Xamarin.Forms 适合什么场景？

> [!NOTE]
> [![下载示例](~/media/shared/download.png) 下载示例](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter01)

此页上的“注意”指出了 Xamarin.Forms 与书中所述内容的不同之处。 编程中最令人不快的作业之一是将代码库从一个平台移植到另一个平台，尤其是在该平台涉及到不同的编程语言时。

## <a name="cross-platform-mobile-development"></a>在移植代码时也存在重构代码的诱惑，但如果必须并行维护两个平台，那么这两个代码库之间的差异会使将来的维护更加困难。

跨平台移动开发 在面向移动平台时，此问题很常见。 目前有两个主要的移动平台，即运行 iOS 操作系统的 Apple 系列的 iPhone 和 iPad，以及在各种手机和平板电脑上运行的 Android 操作系统。

另一个重要的平台是 Microsoft 的通用 Windows 平台 (UWP)，它允许单个程序同时针对两个 Windows 10。

## <a name="the-c-and-net-solution"></a>希望以这些平台为目标的软件供应商必须处理不同的用户界面模式、三个不同的开发环境、三个不同的编程接口，以及&mdash;可能是最尴尬的&mdash;三种不同的编程语言：适用于 iPhone 和 iPad 的 Objective-C、适用于 Android 的 Java 和适用于 Windows 的 C#。

C# 和 .NET 解决方案 尽管 Objective-C、Java 和 C# 都是从 C 编程语言派生出来的，但它们的发展道路却截然不同。 C# 是这些语言中最新的，并且已经以非常有用的方式成熟起来。

此外，C# 与称为 .NET 的整个编程基础结构密切相关，这为数学、调试、反射、集合、全球化、文件 I/O、网络、安全性、线程处理、Web 服务、数据处理以及 XML 和 JSON 读写提供支持。 Xamarin 目前提供面向使用 C# 和 .NET 的本机 Mac、iOS 和 Android API 的工具。 这些工具称为 Xamarin.Mac、Xamarin.iOS 和 Xamarin.Android（统称为 Xamarin 平台）。

这些是用 .NET 惯例表示这些平台的本机 API 的库和绑定。 开发人员可以使用 Xamarin 平台通过 C# 语言编写面向 Mac、iOS 或 Android 的应用程序。 但在面向多个平台时，在目标平台之间共享某些代码会非常有意义。 这涉及到将程序分成平台相关的代码（通常涉及用户界面）和独立于平台的代码，通常只需要基本的 .NET 框架。

> [!NOTE]
> 此独立于平台的代码既可以位于可移植类库 (PCL)，也可以驻留在共享项目中，通常称为“共享资产项目”或 SAP。 可移植类库已替换为 .NET Standard 库。

## <a name="introducing-xamarinforms"></a>书中的所有示例代码都已转换为使用 .NET Standard 库。

Xamarin.Forms 简介 面向多个移动平台时，Xamarin.Forms 允许更多的代码共享。

- 为 Xamarin.Forms 编写的单个程序可以面向以下平台：
- iOS（适用于在 iPhone、iPad 和 iPod touch 上运行的程序）
- Android（适用于在 Android 手机和平板电脑上运行的程序）

> [!NOTE]
> 面向 Windows 10 的通用 Windows 平台 Xamarin.Forms 不再支持 Windows 8.1、Windows Phone 8.1 或 Windows 10 移动版，但 Xamarin.Forms 应用程序可在 Windows 10 桌面上运行。

此外，还提供对 [Mac](~/xamarin-forms/platform/other/mac.md)、[WPF](~/xamarin-forms/platform/other/wpf.md)、[GTK#](~/xamarin-forms/platform/other/gtk.md) 和 [Tizen](~/xamarin-forms/platform/other/tizen.md) 平台的预览支持。 库或 SAP 中存在大量 Xamarin.Forms 程序。

每个平台都包含调用此共享代码的小型应用程序存根。

Xamarin.Forms API 映射到每个平台的本机控件，使每个平台都保持其特有的外观：

[![平台视觉对象共享的三倍屏幕截图](images/ch01fg03-small.png "每个平台上的 Xamarin.Forms 控件")](images/ch01fg03-large.png#lightbox "每个平台上的 Xamarin.Forms 控件")

从左到右的屏幕截图显示 iPhone 和 Android 手机： 在每个屏幕上，页面都包含一个用于显示文本的 Xamarin.Forms [`Label`](xref:Xamarin.Forms.Label)、用于启动操作的 [`Button`](xref:Xamarin.Forms.Button)、用于选择“开启/关闭”值的 [`Switch`](xref:Xamarin.Forms.Switch) 以及用于在连续范围内指定一个值的 [`Slider`](xref:Xamarin.Forms.Slider)。

这四个视图都是 [`ContentPage`](xref:Xamarin.Forms.ContentPage)上的 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 的子项。 页面还附带 Xamarin.Forms 工具栏，其中包含几个 [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem) 对象。

这些对象在 iOS 和 Android 屏幕顶部以及 Windows 10 移动版屏幕底部作为图标显示。 Xamarin.Forms 还支持 XAML，这是 Microsoft 为多个应用程序平台开发的 Extensible Application Markup Language。

上述程序的所有视觉对象都在 XAML 中定义，如 [PlatformVisuals](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter01/PlatformVisuals) 示例中所示。 Xamarin.Forms 程序可以确定运行它的平台，并相应地执行不同代码。 更有力的是，开发人员可以为各种平台编写自定义代码，并以独立于平台的方式从 Xamarin.Forms 程序运行该代码。

开发人员还可以通过为每个平台编写呈现器来创建其他控件。

## <a name="your-development-environment"></a>虽然 Xamarin.Forms 对于业务线应用程序、创建原型或快速进行概念证明演示而言是一个不错的解决方案，但对于需要矢量图形或复杂触摸交互的应用程序，这种方法不太理想。

开发环境

开发环境取决于要面向的平台以及要使用的计算机。 如果要面向 iOS，则需要安装了 Xcode 和 Xamarin 平台的 Mac。 支持 Android 也需要安装 Java 和所需的 SDK。

然后，可以使用 Visual Studio for Mac 同时面向 iOS 和 Android。 安装 Visual Studio 后，你将能够在电脑上面向 iOS、Android 和所有 Windows 平台。

但是，在 Visual Studio 中面向 iOS 仍需要安装了 Xcode 和 Xamarin 平台的 Mac。

## <a name="installation"></a>可以在通过 USB 连接到计算机的实际设备上或在仿真器上测试程序。

安装

创建和生成 Xamarin.Forms 应用程序之前，应尝试单独创建和生成 iOS 应用程序、Android 应用程序和 UWP 应用程序，具体取决于要面向的平台和开发环境。

- Xamarin 和 Microsoft 网站包含有关如何执行此操作的信息：
- [iOS 入门](~/ios/get-started/index.md)
- [Android 入门](~/android/get-started/index.md)

[Windows 开发人员中心](https://dev.windows.com)

## <a name="related-links"></a>为这些单独的平台创建和运行项目后，就可以轻松创建和运行 Xamarin.Forms 应用程序。

- 相关链接
- [第 1 章全文 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch01-Apr2016.pdf)
