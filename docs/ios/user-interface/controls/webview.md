---
title: Xamarin 中的 Web 视图
description: 本文档介绍了 Xamarin iOS 应用可显示 web 内容的各种方式。 它讨论了 WKWebView、SFSafariViewController、Safari 和应用传输安全性。
ms.prod: xamarin
ms.assetid: 84886CF4-2B2B-4540-AD92-7F0B791952D1
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/22/2017
ms.openlocfilehash: a9dce962c35e5f9cfdcd674da9ad71cf8935e7d4
ms.sourcegitcommit: 6c60914b380ff679bbffd7790edd4d5e18005d0a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/21/2020
ms.locfileid: "80070320"
---
# <a name="web-views-in-xamarinios"></a>Xamarin 中的 Web 视图

在 iOS 的生存期内，应用开发人员可以通过多种方式在其应用中合并 web 视图功能。 大多数用户都利用其 iOS 设备上的内置 Safari web 浏览器，因此，从其他应用程序获得的 web 视图功能与此体验一致。 它们期望使用相同的笔势，性能应为 par，并具有相同的功能。

iOS 11 引入了对 `WKWebView` 和 `SFSafariViewController`的新更改。 有关这些内容的详细信息，请参阅[iOS 11 指南中的 Web 更改](~/ios/platform/introduction-to-ios11/web.md)。

## <a name="wkwebview"></a>WKWebView

iOS 8 中引入了 `WKWebView`，应用程序开发人员可以实现类似于 mobile Safari 的 web 浏览界面。 这在某种程度上是因为 `WKWebView` 使用 Nitro Javascript 引擎，这是移动 Safari 使用的同一个引擎。 应始终在 UIWebView 上使用 `WKWebView`，因为这样可能会提高性能、内置用户友好的手势，以及网页与应用程序之间的轻松交互。

可以通过与 UIWebView 几乎完全相同的方式将 `WKWebView` 添加到应用程序中，但作为开发人员，你可以更好地控制 UI/UX 和功能。 创建和显示 web 视图对象将显示请求的页面，但你可以控制视图的显示方式、用户可以导航的方式，以及用户如何退出视图。  

下面的代码可用于在 Xamarin iOS 应用程序中启动 `WKWebView`：

```csharp
WKWebView webView = new WKWebView(View.Frame, new WKWebViewConfiguration());
View.AddSubview(webView);

var url = new NSUrl("https://docs.microsoft.com");
var request = new NSUrlRequest(url);
webView.LoadRequest(request);
```

需要注意的是，`WKWebView` 在 `WebKit` 命名空间中，因此必须将此 using 指令添加到类的顶部。

`WKWebView` 也可用于 Xamarin 应用程序中，如果要创建跨平台 Mac/iOS 应用，则应使用该应用。

[处理 Javascript 警报](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/web_view/handle_javascript_alerts)食谱还提供有关将 WKWebView 与 JavaScript 一起使用的信息。

## <a name="sfsafariviewcontroller"></a>SFSafariViewController

 `SFSafariViewController` 是提供应用程序提供的 web 内容的最新方式，可在 iOS 9 及更高版本中使用。 与 `UIWebView` 或 `WKWebView`不同，`SFSafariViewController` 是一个视图控制器，因此不能与其他视图一起使用。 你应以新的视图控制器形式显示 `SFSafariViewController`，其方式与显示任何视图控制器的方式相同。

 `SFSafariViewController` 本质上是可以嵌入到应用中的 "小型 safari"。 与 WKWebView 一样，它使用相同的 Nitro Javascript 引擎，还提供了一系列附加的 Safari 功能，如自动填充、读者，以及在移动 Safari 中共享 cookie 和数据的功能。 您的应用程序不能访问用户与 `SFSafariViewController` 之间的交互。 您的应用程序将不能访问任何默认的 Safari 功能。

默认情况下，它还实现 "**完成**" 按钮，使用户能够轻松返回到你的应用程序、前进和后退导航按钮，使用户能够在网页堆栈中导航。 此外，它还为用户提供了一个地址栏，使他们可以放心地使用它们。 地址栏不允许用户更改 url。

这些实现无法更改，因此，如果您的应用程序想要在不进行任何自定义的情况下显示网页，则最好将其用作默认浏览器 `SFSafariViewController`。

下面的代码可用于在 Xamarin iOS 应用程序中启动 `SFSafariViewController`：

```csharp
var sfViewController = new SFSafariViewController(url);

PresentViewController(sfViewController, true, null);
```

这会生成以下 web 视图：

[![使用 SFSafariViewController 的示例 web 视图](webview-images/sfsafariviewcontroller.png)](webview-images/sfsafariviewcontroller.png#lightbox)

## <a name="safari"></a>Safari

还可以通过使用以下代码，从应用内打开移动 Safari 应用：

```csharp
var url = new NSUrl("https://docs.microsoft.com");

UIApplication.SharedApplication.OpenUrl(url);
```

这会生成以下 web 视图：

[![Safari 中显示的网页](webview-images/safari.png)](webview-images/safari.png#lightbox)

通常应始终避免将用户从应用导航到 Safari。 大多数用户不需要在应用程序外导航，因此，如果离开应用，用户可能永远都不会返回它，实质上是终止了订婚。

iOS 9 改进使用户可以通过 Safari 页面左上角提供的 "后退" 按钮轻松返回到应用。

## <a name="app-transport-security"></a>应用传输安全性

应用传输安全性或*ATS*是由 Apple 在 iOS 9 中引入的，以确保所有 internet 通信都符合安全连接最佳实践。

有关 ATS 的详细信息，包括如何在应用中实现它，请参阅[应用传输安全](~/ios/app-fundamentals/ats.md)指南。

## <a name="uiwebview-deprecation"></a>UIWebView 弃用

`UIWebView` 是 Apple 在您的应用程序中提供 web 内容的传统方法。 它已在 iOS 2.0 中发布，并已在8.0 后弃用。

> [!IMPORTANT]
> `UIWebView` 已弃用。 [从2020年4月起，将不接受使用此控件的新应用，并且12月2020将不会接受使用此控制的应用更新](https://developer.apple.com/news/?id=12232019b)。
>
> [Apple 的 `UIWebView` 文档](https://developer.apple.com/documentation/uikit/uiwebview)建议应用应改用[`WKWebView`](#wkwebview) 。
>
> 使用 Xamarin.Forms 时，若要查看关于 `UIWebView` 的弃用警告 (ITMS-90809) 的相关资源，请参阅 [Xamarin.Forms WebView](~/xamarin-forms/user-interface/webview.md#uiwebview-deprecation-and-app-store-rejection-itms-90809) 文档。

在过去六个月（或如此）提交 iOS 应用程序的开发人员可能会收到来自应用商店的警告，其中关于 `UIWebView` 被弃用。

Api 的弃用功能很常见。 Xamarin 使用自定义属性来向开发人员提供通知，并向开发人员推荐替换项。 这种情况的不同之处在于，不太常见的情况是，在提交时 Apple 的 App Store**将强制实施**弃用。

遗憾的是，从 `Xamarin.iOS.dll` 中删除 `UIWebView` 类型是[二进制的重大更改](https://docs.microsoft.com/dotnet/core/compatibility/categories#binary-compatibility)。 此更改会中断现有的第三方库，包括一些可能不受支持或甚至重新编译的部分（例如，已关闭的源）。 这只会为开发人员带来额外的问题。 因此，*我们尚未删除该类型。*

从[Xamarin 13.16](https://docs.microsoft.com/xamarin/ios/release-notes/13/13.16)开始，新的检测和工具可帮助你从 `UIWebView`迁移。

### <a name="detection"></a>检测

如果 have'nt 最近提交到 Apple 应用商店的 iOS 应用程序，你可能会想知道此情况是否适用于你的应用程序。

若要了解，你可以将 `--warn-on-type-ref=UIKit.UIWebView` 添加到项目的**其他 mtouch 参数。** 这**会对应用**程序中不推荐使用的 `UIWebView` （及其所有依赖项）发出警告。 在执行托管链接器**之前**和**之后**，将使用不同的警告来报告类型。

使用 `-warnaserror:`可以将警告转换为错误。 如果要确保在验证后不添加 `UIWebView` 的新依赖关系，这会很有用。 例如：

* 如果在预链接的程序集中找到了引用，`-warnaserror:1502` 将报告错误。
* 如果在链接后的程序集中找到了引用，`-warnaserror:1503` 将报告错误。

如果前/后链接结果不起作用，还可以解除警告。 例如：

* 如果在预链接的程序集中找到了引用，`-nowarn:1502` 将**不**会报告警告。
* 如果在链接后的程序集中找到了引用，`-nowarn:1503` 将**不**会报告警告。

### <a name="removal"></a>删除

每个应用程序都是唯一的。 从应用程序中删除 `UIWebView` 可能需要不同的步骤，具体取决于其使用方式和位置。 最常见的情况如下：

- 在应用程序中没有使用 `UIWebView`。 一切正常。 提交到 AppStore 时，**不**应出现警告。 您无需执行任何其他操作。
- 直接使用应用程序 `UIWebView`。 首先，删除对 `UIWebView`的使用，例如，将其替换为较新的 `WKWebView` （iOS 8）或 `SFSafariViewController` （iOS 9）类型。 完成此完成后，托管链接器将看不到对 `UIWebView` 的任何引用，最终的应用程序二进制文件将不会进行跟踪。
- 间接使用。 某些第三方库（托管或本机）可以在应用程序使用 `UIWebView`。 首先将外部依赖项更新到其最新版本，因为这种情况可能已在新版本中得到解决。 否则，请与这些库的维护人员联系，询问其更新计划。

或者，您也可以尝试以下方法：

1. 如果使用的是**Xamarin**，请阅读此[博客文章](https://devblogs.microsoft.com/xamarin/uiwebview-deprecation-xamarin-forms/)。
1. 在整个项目上（至少在使用 `UIWebView`的依赖项上）启用托管链接器，以便在未引用的情况下*将其删除*。 这将解决此问题，但可能需要额外的工作来使代码链接器安全。
1. 如果无法更改托管链接器设置，请参阅下面的特殊情况。

#### <a name="applications-cannot-use-the-linker-or-change-its-settings"></a>应用程序不能使用链接器（或更改其设置）

如果由于某种原因而未使用托管链接器（例如， **not** **link**），则 `UIWebView` 符号将保留在二进制应用中，并将其提交到 Apple，并可能被拒绝。

一种*说服力*解决方案是将 `--optimization=force-rejected-types-removal` 添加到项目的**其他 mtouch 参数**。 这将从应用程序中删除 `UIWebView` 的跟踪。 不过，引用该类型的任何代码都**不**会正常工作（应异常或崩溃）。 仅当确定在运行时无法访问代码时，才应使用此方法（即使可通过静态分析访问它）。

#### <a name="support-for-ios-7x-or-earlier"></a>支持 iOS 7、windows （或更早版本）

从 v2.0 起，`UIWebView` 已成为 iOS 的一部分。 最常见的替换 `WKWebView` （iOS 8）和 `SFSafariViewController` （iOS 9）。 如果你的应用程序仍然支持较旧的 iOS 版本，你应该考虑以下选项：

* 使 iOS 8 成为最低目标版本（生成时间决策）。
* 仅当应用在 iOS 8 + 上运行（运行时决策）时才使用 `WKWebView`。

#### <a name="applications-not-submitted-to-apple"></a>未提交到 Apple 的应用程序

如果你的应用程序未提交到 Apple，你应该计划离开弃用的 API，因为在将来的 iOS 版本中可将其删除。 不过，您可以使用自己的时间表执行此转换。

## <a name="related-links"></a>相关链接

- [Webview （示例）](https://docs.microsoft.com/samples/xamarin/ios-samples/webview)
