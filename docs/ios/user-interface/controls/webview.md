---
title: Xamarin.iOS 中的 Web 视图
description: 本文档介绍了 Xamarin.iOS 应用显示 Web 内容的各种方式。 它讨论了 WKWebView、SFSafariView控制器、Safari 和应用程序传输安全性。
ms.prod: xamarin
ms.assetid: 84886CF4-2B2B-4540-AD92-7F0B791952D1
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/22/2017
ms.openlocfilehash: cc6c87452bf5312d66e33b7c49d3dc216eceb7d6
ms.sourcegitcommit: b93754b220fca3d6e3d131341e3cfbe233d10f84
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/03/2020
ms.locfileid: "80628296"
---
# <a name="web-views-in-xamarinios"></a>Xamarin.iOS 中的 Web 视图

在 iOS 的生命周期中，Apple 发布了许多应用开发人员在其应用中集成 Web 视图功能的方法。 大多数用户在其 iOS 设备上使用内置的 Safari Web 浏览器，因此希望其他应用的 Web 视图功能与此体验一致。 他们期望相同的手势工作，性能是平和的，功能相同。

iOS 11 引入了新的更改`WKWebView`，`SFSafariViewController`对 和 。 有关这些的详细信息，请参阅[iOS 11 指南中的 Web 更改](~/ios/platform/introduction-to-ios11/web.md)。

## <a name="wkwebview"></a>WKWebView

`WKWebView`iOS 8 中引入了，允许应用开发人员实现类似于移动 Safari 的 Web 浏览界面。 部分原因在于`WKWebView`使用 Nitro Javascript 引擎，该引擎与移动 Safari 使用的引擎相同。 `WKWebView`由于性能提高（内置了用户友好的手势），以及网页和应用程序之间交互的便利性，因此应始终在 UIWebView 上使用。

`WKWebView`可以以与 UIWebView 几乎相同的方式添加到应用中，但是作为开发人员，您可以对 UI/UX 和功能有更多的控制。 创建和显示 Web 视图对象将显示请求的页面，但您可以控制视图的显示方式、用户导航方式以及用户退出视图的方式。  

以下代码可用于在 Xamarin.iOS 应用中启动 ： `WKWebView`

```csharp
WKWebView webView = new WKWebView(View.Frame, new WKWebViewConfiguration());
View.AddSubview(webView);

var url = new NSUrl("https://docs.microsoft.com");
var request = new NSUrlRequest(url);
webView.LoadRequest(request);
```

请务必注意，`WKWebView`该名称空间位于`WebKit`命名空间中，因此您必须使用指令将此指令添加到类的顶部。

`WKWebView`也可以在 Xamarin.Mac 应用中使用，如果您正在创建跨平台 Mac/iOS 应用，则应使用它。

[处理 JavaScript 警报](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/web_view/handle_javascript_alerts)配方还提供有关使用 WKWebView 与 Javascript 一起使用的信息。

## <a name="sfsafariviewcontroller"></a>SFSafariViewController

 `SFSafariViewController`是从你的应用提供 Web 内容的最新方式，可在 iOS 9 及更高版本中提供。 与`UIWebView``WKWebView`或`SFSafariViewController`是 视图控制器，因此不能与其他视图一起使用。 您应该以新的`SFSafariViewController`视图控制器方式呈现，就像呈现任何视图控制器一样。

 `SFSafariViewController`本质上是一个"迷你野生动物园"，可以嵌入到你的应用中。 与 WKWebView 一样，它使用相同的 Nitro Javascript 引擎，但也提供了一系列额外的 Safari 功能，如自动填充、读取器，以及与移动 Safari 共享 Cookie 和数据的能力。 用户和 之间的`SFSafariViewController`交互对应用是无法访问的。 你的应用将无法访问任何默认的 Safari 功能。

默认情况下，它还实现了 **"完成"** 按钮，允许用户轻松返回到你的应用，以及前进和后退导航按钮，允许用户浏览一堆网页。 此外，它还为用户提供了一个地址栏，让他们安心，他们出现在预期的网页上。 地址栏不允许用户更改 URL。

无法更改这些实现，因此`SFSafariViewController`，如果应用希望在没有任何自定义的情况下显示网页，则最好用作默认浏览器。

以下代码可用于在 Xamarin.iOS 应用中启动 ： `SFSafariViewController`

```csharp
var sfViewController = new SFSafariViewController(url);

PresentViewController(sfViewController, true, null);
```

这将生成以下 Web 视图：

[![带有 SFSafariView控制器的示例 Web 视图](webview-images/sfsafariviewcontroller.png)](webview-images/sfsafariviewcontroller.png#lightbox)

## <a name="safari"></a>Safari

还可以使用以下代码从应用内打开移动 Safari 应用：

```csharp
var url = new NSUrl("https://docs.microsoft.com");

UIApplication.SharedApplication.OpenUrl(url);
```

这将生成以下 Web 视图：

[![在 Safari 中展示的网页](webview-images/safari.png)](webview-images/safari.png#lightbox)

通常应始终避免将用户从应用导航到 Safari。 大多数用户不会期望在应用程序外部进行导航，因此，如果您远离应用导航，用户可能永远不会返回它，这实质上会扼杀参与度。

iOS 9 的改进允许用户通过 Safari 页面左上角提供的后退按钮轻松返回到你的应用。

## <a name="app-transport-security"></a>应用传输安全性

应用传输安全 *（ATS）* 由 Apple 在 iOS 9 中引入，以确保所有互联网通信都符合安全连接最佳实践。

有关 ATS 的详细信息（包括如何在应用中实现它），请参阅[应用传输安全](~/ios/app-fundamentals/ats.md)指南。

## <a name="uiwebview-deprecation"></a>UIWebView 弃用

`UIWebView`是 Apple 在应用中提供 Web 内容的传统方式。 它在 iOS 2.0 中发布，并且自 8.0 起已被弃用。

> [!IMPORTANT]
> `UIWebView` 已弃用。 自 2020 年 4 月起，使用此控件的新应用[将不接受 App Store，并且在 2020 年 12 月前不接受使用此控件的应用更新](https://developer.apple.com/news/?id=12232019b)。
>
> [苹果的`UIWebView`文件显示](https://developer.apple.com/documentation/uikit/uiwebview)，应用应该改用[`WKWebView`](#wkwebview)。
>
> 使用 Xamarin.Forms 时，若要查看关于 `UIWebView` 的弃用警告 (ITMS-90809) 的相关资源，请参阅 [Xamarin.Forms WebView](~/xamarin-forms/user-interface/webview.md#uiwebview-deprecation-and-app-store-rejection-itms-90809) 文档。

在过去六个月（或更长时间）提交 iOS 应用程序的开发人员可能会收到来自 App Store 的警告，警告`UIWebView`他们被弃用。

API 的弃用很常见。 Xamarin.iOS 使用自定义属性向开发人员发出信号（如果可用，建议替换）。 这次不同，更不常见的是，苹果的App Store将在提交时**强制**弃用。

不幸的是，`UIWebView`从`Xamarin.iOS.dll`中删除类型是[二进制破坏更改](https://docs.microsoft.com/dotnet/core/compatibility/categories#binary-compatibility)。 此更改将破坏现有的第三方库，包括一些可能不支持甚至重新编译的库（例如，关闭源）。 这只会给开发人员带来其他问题。 因此，我们*尚未*删除类型。

从[Xamarin.iOS 13.16](https://docs.microsoft.com/xamarin/ios/release-notes/13/13.16)开始，新的检测和工具可帮助您从`UIWebView`进行迁移。

### <a name="detection"></a>检测

如果您最近没有向 Apple App Store 提交 iOS 应用程序，您可能会怀疑这种情况是否适用于您的应用程序。

要找出答案，您可以添加到`--warn-on-type-ref=UIKit.UIWebView`项目的 **"附加 mtouch 参数"。** 这将警告**对**应用程序`UIWebView`内部弃用的任何引用（及其所有依赖项）。 不同的警告用于在托管链接器**before**执行**之前和之后**报告类型。

警告和其他警告一样，可以使用 转换为错误`-warnaserror:`。 如果要确保验证后不会添加新的依赖项，`UIWebView`这非常有用。 例如：

* `-warnaserror:1502`如果在预链接程序集中找到任何引用，将报告错误。
* `-warnaserror:1503`将在链接后程序集中找到任何引用时报告错误。

如果预/后链接结果没有用，也可以静默警告。 例如：

* `-nowarn:1502`如果在预链接程序集中找到任何引用，**将不会**报告警告。
* `-nowarn:1503`如果在链接后程序集中找到任何引用，**将不会**报告警告。

### <a name="removal"></a>删除

每个应用程序都是独一无二的。 根据`UIWebView`使用方式和位置，从应用程序中删除可能需要不同的步骤。 最常见的方案如下：

- 应用程序`UIWebView`内部没有用。 一切都很好 提交到 AppStore 时**不应**有警告。 您不需要任何其他内容。
- 应用程序直接使用`UIWebView`。 首先删除 使用的`UIWebView`，例如，将其替换为较新的`WKWebView`（iOS 8） 或`SFSafariViewController`（iOS 9） 类型。 完成此项后，托管链接器不应看到任何引用，`UIWebView`最终应用二进制文件将没有跟踪。
- 间接使用。 `UIWebView`可以存在于应用程序使用的某些第三方库中，无论是托管库还是本机库。 首先，将外部依赖项更新到其最新版本，因为这种情况可能已经在较新版本中解决。 如果没有，请与库的维护者联系，询问其更新计划。

或者，您可以尝试以下方法：

1. 如果您使用的是**Xamarin.Forms**，请阅读此[博客文章](https://devblogs.microsoft.com/xamarin/uiwebview-deprecation-xamarin-forms/)。
1. 启用托管链接器（在整个项目上，或者至少在使用`UIWebView`依赖项上启用），以便*删除*它（如果未引用）。 这将解决此问题，但可能需要其他工作来使代码链接器安全。
1. 如果无法更改托管链接器设置，请参阅下面的特殊情况。

#### <a name="applications-cannot-use-the-linker-or-change-its-settings"></a>应用程序无法使用链接器（或更改其设置）

如果由于某种原因**您不使用**托管链接器（例如，**不要链接**），则`UIWebView`该符号将保留在您提交给 Apple 的二进制应用程序中，并且可能会被拒绝。

一*个强有力的*解决方案是添加到`--optimize=force-rejected-types-removal`你的项目**的附加mtouch论点**。 这将从应用程序中删除跟踪`UIWebView`。 但是，任何引用该类型的**代码将无法正常工作**（预期出现异常或崩溃）。 仅当确定代码在运行时无法访问时（即使可以通过静态分析访问代码），才应使用此方法。

#### <a name="support-for-ios-7x-or-earlier"></a>支持 iOS 7.x（或更早版本）

`UIWebView`自 v2.0 以来一直是 iOS 的一部分。 最常见的更换是`WKWebView`（iOS 8） 和`SFSafariViewController`（iOS 9）。 如果应用程序仍然支持较旧的 iOS 版本，则应考虑以下选项：

* 使 iOS 8 成为最低目标版本（生成时间决策）。
* 仅当应用`WKWebView`在 iOS 8+ 上运行时（运行时决策）时才使用。

#### <a name="applications-not-submitted-to-apple"></a>未提交给 Apple 的申请

如果您的应用程序未提交到 Apple，则应计划远离已弃用的 API，因为可以在将来的 iOS 版本中将其删除。 但是，您可以使用自己的时间表执行此转换。

## <a name="related-links"></a>相关链接

- [网页视图（示例）](https://docs.microsoft.com/samples/xamarin/ios-samples/webview)
