---
title: 摘要：第 20 章. 异步和文件 I/O
description: 使用 Xamarin.Forms 创建移动应用：摘要：第 20 章. 异步和文件 I/O
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: D595862D-64FD-4C0D-B0AD-C1F440564247
author: davidbritch
ms.author: dabritch
ms.date: 07/18/2018
ms.openlocfilehash: 283273e6ee28cc5cd1a61169f38bfcd1dd1726d8
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/13/2020
ms.locfileid: "70771038"
---
# <a name="summary-of-chapter-20-async-and-file-io"></a>摘要：第 20 章. 异步和文件 I/O

[![下载示例](~/media/shared/download.png) 下载示例](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20)

> [!NOTE] 
> 此页上的“注意”指出了 Xamarin.Forms 与书中所述内容的不同之处。

 图形用户界面必须按顺序响应用户输入事件。 这意味着，用户输入事件的所有处理都必须发生在单个线程中，通常称为主线程  或 UI 线程  。

用户希望图形用户界面能够快速响应。 这意味着程序必须快速处理用户输入事件。 如果不能，则必须将处理操作归入辅助执行线程。

本书中的几个示例程序已使用 [`WebRequest`](xref:System.Net.WebRequest) 类。 在此类中，[`BeginGetResponse`](xref:System.Net.WebRequest.BeginGetResponse(System.AsyncCallback,System.Object)) 方法会启动一个工作线程，该线程在完成后将调用回调函数。 但是，该回调函数在工作线程中运行，因此，程序必须调用 [`Device.BeginInvokeOnMainThread`](xref:Xamarin.Forms.Device.BeginInvokeOnMainThread(System.Action)) 方法才能访问用户界面。

> [!NOTE]
> Xamarin.Forms 程序应使用 [`HttpClient`](xref:System.Net.Http.HttpClient) 而不是 [`WebRequest`](xref:System.Net.WebRequest) 来通过 Internet 访问文件。 `HttpClient` 支持异步操作。

.NET 和 C# 中提供了更新式的异步处理方法。 这涉及到 [`Task`](xref:System.Threading.Tasks.Task) 和 [`Task<TResult>`](xref:System.Threading.Tasks.Task`1) 类、[`System.Threading`](xref:System.Threading) 与 [`System.Threading.Tasks`](xref:System.Threading.Tasks) 命名空间中的其他类型以及 C# 5.0 `async` 和 `await` 关键字。 这就是本章重点介绍的内容。

## <a name="from-callbacks-to-await"></a>从回调到等待

`Page` 类本身包含三种显示警报框的异步方法：

- [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String)) 返回 `Task` 对象
- [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String,System.String)) 返回 `Task<bool>` 对象
- [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet(System.String,System.String,System.String,System.String[])) 返回 `Task<string>` 对象

`Task` 对象指示这些方法实现基于任务的异步模式 (TAP)。 这些 `Task` 对象从方法快速返回。 `Task<T>` 返回值构成一个“承诺”，即 `TResult` 类型的值在任务完成后可用。 `Task` 返回值指示这是一个将完成但没有返回值的异步操作。

在所有这些情况下，当用户消除警报框时，`Task` 就会完成。  

### <a name="an-alert-with-callbacks"></a>带有回调的警报

[**AlertCallbacks**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/AlertCallbacks) 示例演示如何使用回调方法处理 `Task<bool>` 返回对象和 `Device.BeginInvokeOnMainThread` 调用。

### <a name="an-alert-with-lambdas"></a>使用 lambda 的警报

[**AlertLambdas**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/AlertLambdas) 示例演示如何使用匿名 lambda 函数来处理 `Task` 和 `Device.BeginInvokeOnMainThread` 调用。  

### <a name="an-alert-with-await"></a>使用 await 的警报

这是更直接的方法，涉及 C# 5 中引入的 `async` 和 `await` 关键字。 [**AlertAwait**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/AlertAwait) 示例演示了其用法。

### <a name="an-alert-with-nothing"></a>不包含任何内容的警报

如果异步方法返回 `Task` 而不是 `Task<TResult>`，则当程序无需知道异步任务何时完成时，就不需要使用任何这些方法。 [**NothingAlert**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/NothingAlert) 示例演示了这一点。

### <a name="saving-program-settings-asynchronously"></a>异步保存程序设置

[**SaveProgramChanges**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/SaveProgramSettings) 示例演示如何使用 `Application` 的 [`SavePropertiesAsync`](xref:Xamarin.Forms.Application.SavePropertiesAsync) 方法，在更改程序设置时对其进行保存，而不重写 `OnSleep` 方法。

### <a name="a-platform-independent-timer"></a>独立于平台的计时器

可以使用 [`Task.Delay`](xref:System.Threading.Tasks.Task.Delay(System.Int32)) 来创建与平台无关的计时器。 [**TaskDelayClock**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/TaskDelayClock) 示例演示了这一点。

## <a name="file-inputoutput"></a>文件输入/输出

传统上，.NET [`System.IO`](xref:System.IO) 命名空间已是文件 I/O 支持的来源。 虽然此命名空间中的某些方法支持异步操作，但大多数情况下都不支持。 命名空间还支持多个执行复杂的文件 I/O 函数的简单方法调用。

### <a name="good-news-and-bad-news"></a>好消息和坏消息

Xamarin.Forms 支持的所有平台都支持应用程序本地存储 &mdash; 专用于应用程序的存储。

Xamarin.iOS 和 Xamarin.Android 库包括 Xamarin 专门为这两个平台定制的 .NET 版本。 其中包括来自 `System.IO` 的类，这些类可用于在这两个平台中通过应用程序本地存储执行文件 I/O。

但是，如果在 Xamarin.Forms PCL 中搜索这些 `System.IO` 类，你将找不到它们。 问题在于，Microsoft 完全改变了 Windows 运行时 API 的文件 I/O。 面向 Windows 8.1、Windows Phone 8.1 和通用 Windows 平台的程序不会将 `System.IO` 用于文件 I/O。

这意味着，你将需要使用 [`DependencyService`](xref:Xamarin.Forms.DependencyService)（最先在[**第 9 章.平台特定的 API 调用**](chapter09.md)中进行讨论）来实现文件 I/O。

> [!NOTE]
> 可移植类库已替换为 .NET Standard 2.0 库，.NET Standard 2.0 支持所有 Xamarin.Forms 平台的 [`System.IO`](xref:System.IO) 类型。 不再需要对大多数文件 I/O 任务使用 `DependencyService`。 有关文件 I/O 的更新式方法，请参阅 [Xamarin.Forms 中的文件处理](~/xamarin-forms/data-cloud/data/files.md)。

### <a name="a-first-shot-at-cross-platform-file-io"></a>跨平台文件 I/O 的第一步

[**TextFileTryout**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/TextFileTryout) 示例定义了用于文件 I/O 的 [`IFileHelper`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter20/TextFileTryout/TextFileTryout/TextFileTryout/IFileHelper.cs) 接口，以及所有平台中此接口的实现。 但 Windows 运行时实现不能与此接口中的方法一起使用，因为 Windows 运行时文件 I/O 方法是异步的。

### <a name="accommodating-windows-runtime-file-io"></a>容纳 Windows 运行时文件 I/O

在 Windows 运行时下运行的程序将 [`Windows.Storage`](/uwp/api/Windows.Storage) 和 [`Windows.Storage.Streams`](/uwp/api/Windows.Storage.Streams) 命名空间中的类用于文件 I/O（包括应用程序本地存储）。 由于 Microsoft 确定需要超过 50 毫秒的任何操作都应该是异步的，以避免阻止 UI 线程，因此，这些文件 I/O 方法大多都是异步的。

演示此新方法的代码将位于库中，以便其他应用程序可以使用该代码。

## <a name="platform-specific-libraries"></a>特定于平台的库

在库中存储可重用代码非常有利。 当可重用代码的不同部分适用于完全不同的操作系统时，这种情况明显更难。

[**Xamarin.FormsBook.Platform**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform) 解决方案演示了一种方法。 此解决方案包含七个不同的项目：

- [**Xamarin.FormsBook.Platform**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform)，一般 Xamarin.Forms PCL
- [**Xamarin.FormsBook.Platform.iOS**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS)，iOS 类库
- [**Xamarin.FormsBook.Platform.Android**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android)，Android 类库
- [**Xamarin.FormsBook.Platform.UWP**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.UWP)，通用 Windows 类库
- [**Xamarin.FormsBook.Platform.WinRT**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT)，所有 Windows 平台通用的代码的共享项目

所有单个平台项目（除了 **Xamarin.FormsBook.Platform.WinRT** 之外）都具有对 **Xamarin.FormsBook.Platform** 的引用。 这三个 Windows 项目都具有对 **Xamarin.FormsBook.Platform.WinRT** 的引用。

所有项目都包含一个静态 `Toolkit.Init` 方法，以确保当 Xamarin.Forms 应用程序解决方案中的项目不直接引用库时进行加载。

**Xamarin.FormsBook.Platform** 项目中包含新的 [`IFileHelper`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/IFileHelper.cs) 接口。 所有方法的名称现在都具有 `Async` 后缀，并返回 `Task` 对象。

**Xamarin.FormsBook.Platform.WinRT** 项目包含 Windows 运行时的 [`FileHelper`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/FileHelper.cs) 类。

**Xamarin.FormsBook.Platform.iOS** 项目包含 iOS 的 [`FileHelper`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/FileHelper.cs) 类。 这些方法现在必须是异步的。 某些方法使用在 `StreamWriter` 和 `StreamReader` 中定义的异步方法：[`WriteAsync`](xref:System.IO.StreamWriter.WriteAsync(System.String)) 和 [`ReadToEndAsync`](xref:System.IO.StreamReader.ReadToEndAsync)。 其他方法使用 [`FromResult`](xref:System.Threading.Tasks.Task.FromResult*) 方法将结果转换为 `Task` 对象。

**Xamarin.FormsBook.Platform.Android** 项目包含适用于 Android 的 [`FileHelper`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/FileHelper.cs) 类。

**Xamarin.FormsBook.Platform** 项目还包含一个 [`FileHelper`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/FileHelper.cs) 类，以便轻松使用 `DependencyService` 对象。

若要使用这些库，应用程序解决方案必须包含 **Xamarin.FormsBook.Platform** 解决方案中的所有项目，并且每个应用程序项目必须具有对 **Xamarin.FormsBook.Platform** 中的相应库的引用。

[**TextFileAsync**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/TextFileAsync) 解决方案演示了如何使用 **Xamarin.FormsBook.Platform** 库。 每个项目都具有对 `Toolkit.Init` 的调用。 应用程序使用异步文件 I/O 函数。

### <a name="keeping-it-in-the-background"></a>使其保持在后台

如果库中的方法调用多个异步方法（例如 Windows 运行时 [`FileHelper`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/FileHelper.cs) 类中的 `WriteFileAsync` 和 `ReadFileASync` 方法），则可以通过使用 [`ConfigureAwait`](xref:System.Threading.Tasks.Task`1.ConfigureAwait(System.Boolean)) 方法避免切换回用户界面线程，从而在一定程度上提高效率。

### <a name="dont-block-the-ui-thread"></a>不要阻止 UI 线程！

有时，通过使用方法上的 [`Result`](xref:System.Threading.Tasks.Task`1.Result) 属性，可以避免使用 `ContinueWith` 或 `await`。 应避免这种情况，因为它可能会阻止 UI 线程，甚至挂起应用程序。

## <a name="your-own-awaitable-methods"></a>你自己的可等待方法

你可以通过将代码传递到 [`Task.Run`](xref:System.Threading.Tasks.Task.Run(System.Action)) 方法之一来异步运行一些代码。 可以在处理一些开销的异步方法中调用 `Task.Run`。

下面讨论了各种 `Task.Run` 模式。

### <a name="the-basic-mandelbrot-set"></a>基本 Mandelbrot 集

为了实时绘制 Mandelbrot 集，[**Xamarin.Forms.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 库中的 [`Complex`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/Complex.cs) 结构与 `System.Numerics` 命名空间中的结构类似。

[**MandelbrotSet**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotSet) 示例在其代码隐藏文件中有一个 `CalculateMandeblotAsync` 方法，该方法计算基本的黑白 Mandelbrot 集，并使用 [`BmpMaker`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BmpMaker.cs) 将其放在位图上。

### <a name="marking-progress"></a>标记进度

若要从异步方法报告进度，可以实例化一个 [`Progress<T>`](xref:System.Progress`1) 类，并定义异步方法，使其具有 [`IProgress<T>`](xref:System.IProgress`1) 类型的参数。 [**MandelbrotProgress**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotProgress) 示例对此进行了演示。

### <a name="cancelling-the-job"></a>取消作业

还可以编写可取消的异步方法。 首先，使用名为 [`CancellationTokenSource`](xref:System.Threading.CancellationTokenSource) 的类。 [`Token`](xref:System.Threading.CancellationTokenSource.Token) 属性是 [`CancellationToken`](xref:System.Threading.CancellationToken) 类型的值。 这会传递到异步函数。 程序调用 `CancellationTokenSource` 的 [`Cancel`](xref:System.Threading.CancellationTokenSource.Cancel) 方法（通常是为了响应用户的操作）以取消异步函数。

异步方法可以定期检查 `CancellationToken` 的 [`IsCancellationRequested`](xref:System.Threading.CancellationToken.IsCancellationRequested) 属性，如果该属性为 `true`，则退出；或者只是调用 [`ThrowIfCancellationRequested`](xref:System.Threading.CancellationToken.ThrowIfCancellationRequested) 方法，在这种情况下，方法以 [`OperationCancelledException`](xref:System.OperationCanceledException) 结尾。

[**MandelbrotCancellation**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotCancellation) 示例演示如何使用可取消函数。

### <a name="an-mvvm-mandelbrot"></a>MVVM Mandelbrot

[**MandelbrotXF**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotXF) 示例具有更广泛的用户界面，它主要基于 [`MandelbrotModel`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter20/MandelbrotXF/MandelbrotXF/MandelbrotXF/MandelbrotModel.cs) 和 [`MandelbrotViewModel`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter20/MandelbrotXF/MandelbrotXF/MandelbrotXF/MandelbrotViewModel.cs) 类：

[![Mandelbrot X F 的三倍屏幕截图](images/ch20fg13-small.png "MVVM Mandelbrot")](images/ch20fg13-large.png#lightbox "MVVM Mandelbrot")

## <a name="back-to-the-web"></a>返回到 Web

某些示例中采用的 [`WebRequest`](xref:System.Net.WebRequest) 类使用称为异步编程模型 (APM) 的老式异步协议。 你可以使用 [`TaskFactory`](xref:System.Threading.Tasks.TaskFactory`1) 类中的 `FromAsync` 方法之一将此类转换为新式 TAP 协议。 [**ApmToTap**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/ApmToTap) 示例演示了这一点。

## <a name="related-links"></a>相关链接

- [第 20 章全文 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch20-Apr2016.pdf)
- [第 20 章示例](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20)
- [使用文件](~/xamarin-forms/data-cloud/data/files.md)
