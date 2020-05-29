---
标题：说明： ' 本文介绍了中提供的 Windows 平台支持 Xamarin.Forms 。 "
ms-chap： assetid： ms-chap： author： ms. 作者： ms. 日期：非 loc：
- 'Xamarin.Forms'
- 'Xamarin.Essentials'

---

# <a name="windows-platform-features"></a>Windows 平台功能

开发 Xamarin.Forms 适用于 Windows 平台的应用程序需要 Visual Studio。 "[支持的平台" 页](~/get-started/supported-platforms.md)包含有关先决条件的详细信息。

![](images/allhanselman.png "Xamarin.Forms Applications Running on Windows")

## <a name="platform-specifics"></a>平台特定内容

平台说明允许使用仅在特定平台上可用的功能，而无需实现自定义呈现器或效果。

以下特定于平台的功能适用于 Xamarin.Forms 通用 Windows 平台（UWP）上的视图、页面和布局：

- 设置的访问密钥 [`VisualElement`](xref:Xamarin.Forms.VisualElement) 。 有关详细信息，请参阅[Windows 上的 VisualElement 访问密钥](visualelement-access-keys.md)。
- 禁用受支持的旧版颜色模式 [`VisualElement`](xref:Xamarin.Forms.VisualElement) 。 有关详细信息，请参阅[VisualElement 旧版 Color Mode On Windows](legacy-color-mode.md)。

针对 UWP 上的视图提供了以下特定于平台的功能 Xamarin.Forms ：

- 检测 [`Entry`](xref:Xamarin.Forms.Entry) 、和实例中的文本内容的读取顺序 [`Editor`](xref:Xamarin.Forms.Editor) [`Label`](xref:Xamarin.Forms.Label) 。 有关详细信息，请参阅[Windows 上的 InputView 读取顺序](inputview-reading-order.md)。
- 启用中的点击手势支持 [`ListView`](xref:Xamarin.Forms.ListView) 。 有关详细信息，请参阅[ListView SelectionMode On Windows](listview-selectionmode.md)。
- 启用要更改的的请求方向 `RefreshView` 。 有关详细信息，请参阅[Windows 上的 RefreshView 拉取方向](refreshview-pulldirection.md)。
- 使 [`SearchBar`](xref:Xamarin.Forms.SearchBar) 能够与拼写检查引擎进行交互。 有关详细信息，请参阅[Windows 上的 SearchBar 拼写检查](searchbar-spell-check.md)。
- 启用 [`WebView`](xref:Xamarin.Forms.WebView) 以显示 UWP 消息对话框中的 JavaScript 警报。 有关详细信息，请参阅[Windows 上的 Web 视图 JavaScript 警报](webview-javascript-alert.md)。

对于 UWP 上的页面，提供了以下特定于平台的功能 Xamarin.Forms ：

- 折叠 [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) 导航栏。 有关详细信息，请参阅[Windows 上的 MasterDetailPage 导航栏](masterdetailpage-navigation-bar.md)。
- 设置工具栏位置选项。 有关详细信息，请参阅[在 Windows 上放置页面工具栏](page-toolbar-placement.md)。
- 允许在工具栏上显示页面图标 [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) 。 有关详细信息，请参阅 [Windows 上的 TabbedPage 图标](tabbedpage-icons.md)。

对于 UWP 上的类，提供了以下特定于平台的功能 Xamarin.Forms [`Application`](xref:Xamarin.Forms.Application) ：

- 指定将从其加载图像资产的项目中的目录。 有关详细信息，请参阅[Windows 上的默认图像目录](default-image-directory.md)。

## <a name="platform-support"></a>平台支持

Xamarin.FormsVisual Studio 中提供的模板包含通用 Windows 平台（UWP）项目。

> [!NOTE]
> Xamarin.Forms1.x 和2.x 支持_Windows Phone 8 个 Silverlight_， _Windows Phone 8.1_， _Windows 8.1_应用程序开发。 但是，这些项目类型已弃用。

## <a name="getting-started"></a>开始使用

转到文件 > Visual Studio 中的**新 > 项目**，然后选择一个**跨平台 > 空白应用（ Xamarin.Forms ）** 模板以开始。

较早 Xamarin.Forms 的解决方案或在 macOS 上创建的解决方案将不会有以上列出的所有 Windows 项目（但需要手动添加）。 如果你想要作为目标的 Windows 平台尚未处于解决方案中，请访问[设置说明](installation/index.md)以添加所需的 windows 项目类型。

## <a name="samples"></a>示例

Charles Petzold 的[所有示例都](https://github.com/xamarin/xamarin-forms-book-preview-2)是通过[* Xamarin.Forms *](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md)包含通用 Windows 平台（适用于 Windows 10）项目创建移动应用。

["Scott Hanselman" 演示应用](https://github.com/jamesmontemagno/Hanselman.Forms)单独提供，还包括 Apple Watch 和 android 磨损项目（分别使用 Xamarin 和 xamarin），而 Xamarin.Forms 不会在这些平台上运行。

## <a name="related-links"></a>相关链接

- [设置 Windows 项目](~/xamarin-forms/platform/windows/installation/index.md)
