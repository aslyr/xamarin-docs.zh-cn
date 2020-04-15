---
title: 第 13 章摘要。 位图
description: 使用 Xamarin.Forms 创建移动应用：第 13 章摘要。 位图
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 5D153857-B6B7-4A14-8FB9-067DE198C2C7
author: davidbritch
ms.author: dabritch
ms.date: 07/18/2018
ms.openlocfilehash: e4746ed94a008d382ce15bb9cd7c52365d9ba574
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/13/2020
ms.locfileid: "76725526"
---
# <a name="summary-of-chapter-13-bitmaps"></a>第 13 章摘要。 位图

[![下载示例](~/media/shared/download.png) 下载示例](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13)

> [!NOTE]
> 此页上的注释指出了 Xamarin.Forms 与书中提供的内容不同的地方。

Xamarin.Forms [`Image`](xref:Xamarin.Forms.Image) 元素显示位图。 所有 Xamarin.Forms 平台都支持 JPEG、PNG、GIF 和 BMP 文件格式。

Xamarin.Forms 中的位图来自四个位置：

- 通过 URL 指定的 Web
- 作为共享库中的资源嵌入
- 作为平台应用程序项目中的资源嵌入
- 可由 .NET `Stream` 对象引用的任何位置，包括 `MemoryStream`

共享库中的位图资源是独立于平台的，而平台项目中的位图资源是特定于平台的。

> [!NOTE]
> 本书的正文引用了可移植类库，这些类库已替换为 .NET Standard 库。 书中的所有示例代码都已转换为使用 .NET Standard 库。

指定位图的方式是将 `Image` 的 [`Source`](xref:Xamarin.Forms.Image.Source) 属性设置为 [`ImageSource`](xref:Xamarin.Forms.ImageSource) 类型的对象，该对象是具有三个派生项的抽象类：

- [`UriImageSource`](xref:Xamarin.Forms.UriImageSource)，用于在将 `Uri` 对象设置为其 [`Uri`](xref:Xamarin.Forms.UriImageSource.Uri) 属性的基础上，通过 Web 访问位图
- [`FileImageSource`](xref:Xamarin.Forms.FileImageSource)，用于在将文件夹和文件路径设置为其 [`File`](xref:Xamarin.Forms.FileImageSource.File) 属性的基础上，访问存储在平台应用程序项目中的位图
- [`StreamImageSource`](xref:Xamarin.Forms.StreamImageSource)，用于使用 .NET `Stream` 对象加载位图，该对象通过从设置为其 [`Stream`](xref:Xamarin.Forms.StreamImageSource.Stream) 属性的 `Func` 返回 `Stream` 来指定

或者，更常见的方法是使用 `ImageSource` 类的以下静态方法，所有这些静态方法都会返回 `ImageSource` 对象：

- [`ImageSource.FromUri`](xref:Xamarin.Forms.ImageSource.FromUri(System.Uri))，用于基于 `Uri` 对象通过 Web 访问位图
- [`ImageSource.FromResource`](xref:Xamarin.Forms.ImageSource.FromResource*)，用于访问作为嵌入的资源存储在应用程序 PCL 中的位图；[`ImageSource.FromResource`](xref:Xamarin.Forms.ImageSource.FromResource(System.String,System.Type)) 或 [`ImageSource.FromResource`](xref:Xamarin.Forms.ImageSource.FromResource(System.String,System.Reflection.Assembly)) 用于访问其他源程序集中的位图
- [`ImageSource.FromFile`](xref:Xamarin.Forms.ImageSource.FromFile(System.String))，用于访问平台应用程序项目中的位图
- [`ImageSource.FromStream`](xref:Xamarin.Forms.ImageSource.FromStream(System.Func{System.IO.Stream}))，用于基于 `Stream` 对象加载位图

没有与 `Image.FromResource` 方法等效的类。 如需控制缓存，则 `UriImageSource` 类非常有用。 `FileImageSource` 类在 XAML 中很有用。 `StreamImageSource` 用于异步加载 `Stream` 对象，而 `ImageSource.FromStream` 是同步的。

## <a name="platform-independent-bitmaps"></a>独立于平台的位图

[WebBitmapCode](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/WebBitmapCode) 项目使用 `ImageSource.FromUri` 通过 Web 加载位图  。 `Image` 元素设置为 `ContentPage` 的 `Content` 属性，因此将其约束为页面的大小。 无论位图的大小如何，受约束的 `Image` 元素都会拉伸至其容器的大小，并且位图在 `Image` 元素内以其最大尺寸显示，同时保持位图的纵横比。 位图以外的 `Image` 区域可以使用 [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) 着色。

[WebBitmapXaml](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/WebBitmapXaml) 示例与此类似，但只是将 `Source` 属性设置为 URL  。 转换由 [`ImageSourceConverter`](xref:Xamarin.Forms.ImageSourceConverter) 类处理。

### <a name="fit-and-fill"></a>适应和填充

可以通过将 `Image` 的 [`Aspect`](xref:Xamarin.Forms.Image.Aspect) 属性设置为 [`Aspect`](xref:Xamarin.Forms.Aspect) 枚举的以下成员之一来控制位图的拉伸方式：

- [`AspectFit`](xref:Xamarin.Forms.Aspect.AspectFit)：遵循纵横比（默认）
- [`Fill`](xref:Xamarin.Forms.Aspect.Fill)：填充区域，不遵循纵横比
- [`AspectFill`](xref:Xamarin.Forms.Aspect.AspectFill)：填充区域但遵循纵横比，通过裁剪部分位图来完成

### <a name="embedded-resources"></a>嵌入的资源

可将位图文件添加到 PCL 或 PCL 中的文件夹。 向其授予 EmbeddedResource 的生成操作   。 [ResourceBitmapCode](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ResourceBitmapCode) 示例演示了如何使用 `ImageSource.FromResource` 加载该文件  。 传递给该方法的资源名称的组成为（按前后顺序）：程序集名称、一个点、可选的文件夹名称、一个点、文件名。

该程序将 `Image` 的 `VerticalOptions` 和 `HorizontalOptions` 属性设置为 `LayoutOptions.Center`，从而使 `Image` 元素不受约束。 `Image` 和位图的大小相同：

- 在 iOS 和 Android 上，`Image` 是位图的像素大小。 位图像素和屏幕像素之间存在一对一的映射。
- 在通用 Windows 平台上，`Image` 是位图的像素大小（以与设备无关单位表示）。 在大多数设备上，每个位图像素占用多个屏幕像素。

[StackedBitmap](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/StackedBitmap) 示例会将 `Image` 放入采用 XAML 的垂直 `StackLayout` 中  。 名为 [`ImageResourceExtension`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter13/StackedBitmap/StackedBitmap/StackedBitmap/ImageResourceExtension.cs) 的标记扩展可帮助引用采用 XAML 的嵌入的资源。 此类仅从其所在的程序集中加载资源，因此不能将其放置在库中。

### <a name="more-on-sizing"></a>详细介绍调整大小

通常需要在所有平台之间一致地调整位图的大小。
在试验 StackedBitmap 时，可以对垂直 `StackLayout` 中的 `Image` 元素设置 `WidthRequest`，以使平台之间的大小保持一致，但只能使用此方法来减小大小  。

此外，还可以设置 `HeightRequest` 以使平台上的图像大小保持一致，但是位图的受限宽度将会限制此方法的通用性。 对于垂直 `StackLayout` 中的图像，应避免使用 `HeightRequest`。

最好的方法是首先使用宽度大于电话宽度的位图（以与设备无关单位表示），然后将 `WidthRequest` 设置为所需宽度（以与设备无关单位表示）。 [DeviceIndBitmapSize](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/DeviceIndBitmapSize) 示例对此进行了演示  。

[MadTeaParty](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/MadTeaParty) 显示了 Lewis Carroll 的《爱丽丝梦游仙境》的第 7 章，其中包含 John Tenniel 的原始插图   ：

[![“疯狂的茶会”的三重屏幕截图](images/ch13fg16-small.png "疯帽子茶会一书文本")](images/ch13fg16-large.png#lightbox "疯帽子茶会一书文本")

### <a name="browsing-and-waiting"></a>浏览和等待

[ImageBrowser](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ImageBrowser) 示例允许用户浏览 Xamarin 网站上存储的库存图像  。 它使用.NET [`WebRequest`](xref:System.Net.WebRequest) 类下载带有位图列表的 JSON 文件。

> [!NOTE]
> Xamarin.Forms 程序应使用 [`HttpClient`](xref:System.Net.Http.HttpClient) 而不是 [`WebRequest`](xref:System.Net.WebRequest) 来通过 Internet 访问文件。

该程序使用 [`ActivityIndicator`](xref:Xamarin.Forms.ActivityIndicator) 来指示正在进行的活动。 加载每个位图时，`Image` 的只读 [`IsLoading`](xref:Xamarin.Forms.Image.IsLoading) 属性为 `true`。 `IsLoading` 属性由可绑定属性提供支持，因此当该属性更改时会触发 `PropertyChanged` 事件。 该程序会向此事件附加一个处理程序，并使用 `IsLoaded` 的当前设置来设置 `ActivityIndicator` 的 [`IsRunning`](xref:Xamarin.Forms.ActivityIndicator.IsRunning) 属性。

## <a name="streaming-bitmaps"></a>流式处理位图

`ImageSource.FromStream` 方法基于 .NET `Stream` 创建 `ImageSource`。 必须向该方法传递一个可返回 `Stream` 对象的 `Func` 对象。

### <a name="accessing-the-streams"></a>访问流

[BitmapStreams](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/BitmapStreams) 示例演示如何使用 `ImaageSource.FromStream` 方法来加载作为嵌入的资源存储的位图，以及加载 Web 上的位图  。

### <a name="generating-bitmaps-at-run-time"></a>在运行时生成位图

所有 Xamarin.Forms 平台都支持解压缩的 BMP 文件格式，该格式易于在代码中进行构造，然后存储在 `MemoryStream` 中。 采用此方法可在运行时通过算法创建位图，如在 Xamrin.FormsBook.Toolkit 库中的 [`BmpMaker`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BmpMaker.cs) 类中实现的那样  。

“Do It Yourself”[**DiyGradientBitmap**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/DiyGradientBitmap) 示例演示如何使用 `BmpMaker` 创建带有渐变图像的位图。

## <a name="platform-specific-bitmaps"></a>特定于平台的位图

所有 Xamarin.Forms 平台都允许在平台应用程序程序集中存储位图。 当 Xamarin.Forms 应用程序检索时，这些平台位图的类型为 [`FileImageSource`](xref:Xamarin.Forms.FileImageSource)。 它们用于：

- [`MenuItem`](xref:Xamarin.Forms.MenuItem) 的 [`Icon`](xref:Xamarin.Forms.MenuItem.Icon) 属性
- [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem) 的 [`Icon`](xref:Xamarin.Forms.MenuItem.Icon) 属性
- `Button` 的 [`Image`](xref:Xamarin.Forms.Button) 属性

平台程序集已包含图标和初始屏幕的位图：

- 在 iOS 项目中的“资源”文件夹中 
- 在 Android 项目中的“资源”文件夹的子文件夹中 
- 在 Windows 项目中的“资产”文件夹中（虽然 Windows 平台没有将位图限制在该文件夹中） 

[PlatformBitmaps](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/PlatformBitmaps) 示例使用代码显示来自平台应用程序项目的图标  。

### <a name="bitmap-resolutions"></a>位图分辨率

所有平台都允许针对不同的设备分辨率存储多个版本的位图图像。 在运行时，将根据设备的屏幕分辨率加载正确的版本。

在 iOS 上，这些位图通过文件名的后缀进行区分：

- 160 DPI 设备没有后缀（与设备无关单位为 1 个像素）
- 320 DPI 设备的后缀为“@2x”（DIU 为 2 个像素）
- 480 DPI 设备的后缀为“@3x”（DIU 为 3 个像素）

要以 1 英寸正方形显示的位图将存在三个版本：

- 160 像素正方形的 MyImage.jpg
- 320 像素正方形的 MyImage@2x.jpg
- 480 像素正方形的 MyImage@3x.jpg

程序将此位图称为 MyImage.jpg，但在运行时会根据屏幕分辨率检索正确的版本。 如果不受约束，位图将始终以 160 个与设备无关单位呈现。

对于 Android，位图会存储在“资源”文件夹的多个子文件夹中  ：

- drawable-ldpi（低 DPI）用于 120 DPI 设备（DIU 为 0.75 个像素）
- drawable-mdpi（中）用于 160 DPI 设备（DIU 为 1 个像素）
- drawable-hdpi（高）用于 240 DPI 设备（DIU 为 1.5 个像素）
- drawable-xhdpi（超高）用于 320 DPI 设备（DIU 为 2 个像素）
- drawable-xxhdpi（超超高）用于 480 DPI 设备（DIU 为 3 个像素）
- drawable-xxxhdpi（超超超高）用于 640 DPI 设备（DIU 为 4 个像素）

对于要以 1 平方英寸呈现的位图，该位图的不同版本将具有相同的名称，但大小不同，并存储在以下文件夹中：

- drawable-ldpi/120 像素正方形的 MyImage.jpg
- drawable-mdpi/160 像素正方形的 MyImage.jpg
- drawable-hdpi/240 像素正方形的 MyImage.jpg
- drawable-xhdpi/320 像素正方形的 MyImage.jpg
- drawable-xxhdpi/480 像素正方形的 MyImage.jpg
- drawable-xxxhdpi/640 像素正方形的 MyImage.jpg

位图将始终以 160 个与设备无关单位呈现。 （标准的 Xamarin.Forms 解决方案模板仅包括 hdpi、xhdpi 和 xxhdpi 文件夹。）

UWP 项目支持位图命名方案，该方案包含一个比例因子（每个与设备无关单位中像素所占的百分比），例如：

- 320 像素正方形的 MyImage.scale-200.jpg

只有部分百分比值有效。 本书的示例程序仅包含带有 scale-200 后缀的图像，但是当前的 Xamarin.Forms 解决方案模板包括 scale-100、scale-125、scale-150 和 scale-400      。

向平台项目添加位图时，生成操作应为  ：

- iOS：BundleResource 
- Android：AndroidResource 
- UWP：**内容**

[ImageTap](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ImageTap) 示例创建了两个类似按钮的对象，它们由安装了 `TapGestureRecognizer` 的 `Image` 元素组成  。 对像应为 1 英寸的正方形。 使用 `OnPlatform` 和 `On` 对象设置 `Image` 的 `Source` 属性，以在平台上引用可能不同的文件名。 位图图像包含指示其像素大小的数字，因此可以查看所检索和呈现的位图大小。

### <a name="toolbars-and-their-icons"></a>工具栏及其图标

特定于平台的位图的主要用途之一是 Xamarin.Forms 工具栏，该工具栏是通过将 [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem) 对象添加到 `Page` 定义的 [`ToolbarItems`](xref:Xamarin.Forms.Page.ToolbarItems) 集合进行构造的。 `ToobarItem` 派生自 [`MenuItem`](xref:Xamarin.Forms.MenuItem)，并从中继承一些属性。

最重要的 `ToolbarItem` 属性是：

- [`Text`](xref:Xamarin.Forms.MenuItem.Text) 用于可能显示的文本（具体取决于平台和 `Order`）
- `FileImageSource` 类型的 [`Icon`](xref:Xamarin.Forms.MenuItem.Icon)，用于可能显示的文本（具体取决于平台和 `Order`）
- [`ToolbarItemOrder`](xref:Xamarin.Forms.ToolbarItemOrder) 类型的 [`Order`](xref:Xamarin.Forms.ToolbarItem.Order)，这是具有三个成员的枚举：[`Default`](xref:Xamarin.Forms.ToolbarItemOrder.Default)、[`Primary`](xref:Xamarin.Forms.ToolbarItemOrder.Primary) 和 [`Secondary`](xref:Xamarin.Forms.ToolbarItemOrder.Secondary)。

`Primary` 项的数量应限制为三个或四个。 应为所有项添加一个 `Text` 设置。 对于大多数平台，只有 `Primary` 项需要 `Icon`，而对于 Windows 8.1，所有项都需要 `Icon`。 图标应为 32 个与设备无关单位的正方形。 `FileImageSource` 类型指示它们是特定于平台的。

当点击时，`ToolbarItem` 会触发 [`Clicked`](xref:Xamarin.Forms.MenuItem.Clicked) 事件，很像一个 `Button`。 `ToolbarItem` 还支持 [`Command`](xref:Xamarin.Forms.MenuItem.Command) 和 [`CommandParameter`](xref:Xamarin.Forms.MenuItem.CommandParameter) 属性，它们通常用于连接 MVVM。 （请参阅[第 18 章，MVVM](chapter18.md)）。

iOS 和 Android 都要求显示工具栏的页面是 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) 或通过 `NavigationPage` 导航到的页面。 [ToolbarDemo](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ToolbarDemo) 程序使用 `ContentPage` 参数将其 `App` 类的 `MainPage` 属性设置为 [`NavigationPage` 构造函数](xref:Xamarin.Forms.NavigationPage.%23ctor(Xamarin.Forms.Page))，并演示工具栏的构造和事件处理程序  。

### <a name="button-images"></a>按钮图像

还可以使用特定于平台的位图将 `Button` 的 [`Image`](xref:Xamarin.Forms.Button.Image) 属性设置为 32 个与设备无关单位的正方形的位图，如 [ButtonImage](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ButtonImage) 示例所示  。

> [!NOTE]
> 按钮上图像的使用已得到改进。 请参阅[使用带有按钮的位图](~/xamarin-forms/user-interface/button.md#using-bitmaps-with-buttons)。

## <a name="related-links"></a>相关链接

- [第 13 章全文 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch13-Apr2016.pdf)
- [第 13 章示例](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13)
- [使用图像](~/xamarin-forms/user-interface/images.md)
- [使用带有按钮的位图](~/xamarin-forms/user-interface/button.md#using-bitmaps-with-buttons)
