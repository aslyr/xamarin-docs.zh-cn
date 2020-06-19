---
title: Xamarin.Forms视图
description: Xamarin.Forms视图是跨平台移动用户界面的构建基块。 本文列出了包含在中的视图 Xamarin.Forms 。
ms.prod: xamarin
ms.assetid: AC070686-A423-4A98-8BB6-0B9F94C062CC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/16/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 8c15626e405645d28a785c32d276860f9751ea25
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/18/2020
ms.locfileid: "84132361"
---
# <a name="xamarinforms-views"></a>Xamarin.Forms视图

[![下载示例](~/media/shared/download.png) 下载示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery/)

_Xamarin. Forms 视图是跨平台移动用户界面的构建基块。_

视图是用户界面对象（如标签、按钮和滑块），这些对象通常称为*控件*或其他图形编程环境中的*小组件*。 Xamarin.Forms所有从类派生的所支持视图 [`View`](xref:Xamarin.Forms.View) 。 它们可以分为多个类别：

## <a name="views-for-presentation"></a>用于演示的视图

### <a name="boxview"></a>BoxView

|     |    |
| --- | ---|
| [`BoxView`](xref:Xamarin.Forms.BoxView)显示由属性着色的实心矩形 [`Color`](xref:Xamarin.Forms.BoxView.Color) 。 `BoxView`的默认大小请求为40x40。 对于其他大小，则分配 [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) 和 [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) 属性。<br /><br />[API 文档](xref:Xamarin.Forms.BoxView)  / [指南](~/xamarin-forms/user-interface/boxview.md)  / [示例 1](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-basicboxview)、 [2](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-textdecoration)、 [3](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-listviewcolors/)、 [4](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-gameoflife)、 [5](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-dotmatrixclock)和[6](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-boxviewclock) | [![BoxView 示例](views-images/BoxView.png "BoxView 示例")](views-images/BoxView-Large.png#lightbox "BoxView 示例")<br />[此页](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/BoxViewDemoPage.cs)  /  的 c # 代码[XAML 页](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/BoxViewDemoPage.xaml) |
|     |     |

### <a name="expander"></a>Expander

|     |     |
| --- | --- |
| `Expander`提供可扩展容器来托管任何内容，由标头和内容组成。 将 `Header` 属性设置为 [`View`](xref:Xamarin.Forms.View) ，它将显示为标题，并将属性设置为，在 `Content` [`View`](xref:Xamarin.Forms.View) 通过点击展开标题时将显示的。<br /><br />[指南](~/xamarin-forms/user-interface/expander.md)  / [示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-expanderdemos) | [![扩展器示例](views-images/Expander.png "扩展器示例")](views-images/Expander-Large.png#lightbox "扩展器示例")<br /> [此页](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ExpanderDemoPage.cs)  /  的 c # 代码[XAML 页](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ExpanderDemoPage.xaml) |
|     |     |

### <a name="label"></a>标签

|     |     |
| --- | --- |
| [`Label`](xref:Xamarin.Forms.Label)用常量或可变格式设置显示单行文本字符串或文本的多行块。 将 [`Text`](xref:Xamarin.Forms.Label.Text) 属性设置为字符串进行格式设置，或将属性设置为 [`FormattedText`](xref:Xamarin.Forms.Label.FormattedText) 一个 [`FormattedString`](xref:Xamarin.Forms.FormattedString) 对象以进行可变格式设置。<br /><br />[API 文档](xref:Xamarin.Forms.Label)  / [指南](~/xamarin-forms/user-interface/text/label.md)  / [示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text) | [![标签示例](views-images/Label.png "标签示例")](views-images/Label-Large.png#lightbox "标签示例")<br /> [此页](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/LabelDemoPage.cs)  /  的 c # 代码[XAML 页](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/LabelDemoPage.xaml) |
|     |     |

### <a name="image"></a>映像

|     |     |
| --- | --- |
| [`Image`](xref:Xamarin.Forms.Image)显示位图。 可以通过 Web 下载位图，将其作为资源嵌入到公共项目或平台项目中，或使用 .NET 对象创建 `Stream` 。<br /><br />[API 文档](xref:Xamarin.Forms.Image)  / [指南](~/xamarin-forms/user-interface/images.md)  / [示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithimages) | [![图像示例](views-images/Image.png "图像示例")](views-images/Image-Large.png#lightbox "图像示例")<br />[此页](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ImageDemoPage.cs)  /  的 c # 代码[XAML 页](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageDemoPage.xaml) |
|     |     |

### <a name="map"></a>映射

|     |     |
| --- | --- |
| [`Map`](xref:Xamarin.Forms.Maps.Map)显示地图。 ** Xamarin.Forms 。** 必须安装 map NuGet 包。 Android 和通用 Windows 平台需要地图授权密钥。<br /><br />[API 文档](xref:Xamarin.Forms.Maps.Map)  / [指南](~/xamarin-forms/user-interface/map/index.md)  / [示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps/) | [![地图示例](views-images/Map.png "地图示例")](views-images/Map-Large.png#lightbox "地图示例")<br />[此页](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/MapDemoPage.cs)  /  的 c # 代码[XAML 页](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MapDemoPage.xaml) |
|     |     |

### <a name="mediaelement"></a>MediaElement

|     |     |
| --- | --- |
| [`MediaElement`](xref:Xamarin.Forms.MediaElement)播放视频或音频。 根据 [`Source`](xref:Xamarin.Forms.MediaElement.Source) 属性是设置为还是，可以从 URL 或本地文件播放媒体 [`UriMediaSource`](xref:Xamarin.Forms.UriMediaSource) [`FileMediaSource`](xref:Xamarin.Forms.FileMediaSource) 。<br /><br />[API 文档](xref:Xamarin.Forms.MediaElement)  / [指南](~/xamarin-forms/user-interface/mediaelement.md)  / [示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-mediaelementdemos) | [![MediaElement 示例](views-images/MediaElement.png "MediaElement 示例")](views-images/MediaElement-Large.png#lightbox "MediaElement 示例")<br />[此页](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/MediaElementDemoPage.cs)  /  的 c # 代码[XAML 页](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MediaElementDemoPage.xaml) |
|     |     |

### <a name="openglview"></a>OpenGLView

|     |     |
| --- | --- |
| [`OpenGLView`](xref:Xamarin.Forms.OpenGLView)显示 iOS 和 Android 项目中的 OpenGL 图形。 不支持通用 Windows 平台。 IOS 和 Android 项目需要引用**1.0 OpenTK**程序集或**OpenTK** 1.0.0.0 版程序集。 `OpenGLView`更易于在共享项目中使用;如果在 .NET Standard 库中使用，则还需要依赖项服务（如示例代码所示）。<br /><br />这是内置于中的唯一图形功能 Xamarin.Forms ，但 Xamarin.Forms 应用程序也可以使用 [`SkiaSharp`](~/xamarin-forms/user-interface/graphics/skiasharp/index.md) 、或呈现图形 [`UrhoSharp`](~/xamarin-forms/user-interface/graphics/urhosharp.md) 。<br /><br />[API 文档](xref:Xamarin.Forms.OpenGLView)<br /><br /> | [![OpenGLView 示例](views-images/OpenGLView.png "OpenGLView 示例")](views-images/OpenGLView-Large.png#lightbox "OpenGLView 示例")<br />[此页](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/OpenGLViewDemoPage.cs)  /  的 c # 代码带有[代码隐藏](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/OpenGLViewDemoPage.xaml.cs)的[XAML 页](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/OpenGLViewDemoPage.xaml) |
|     |     |

### <a name="webview"></a>WebView

|     |     |
| --- | --- |
| [`WebView`](xref:Xamarin.Forms.WebView)显示网页或 HTML 内容，具体取决于属性是否 [`Source`](xref:Xamarin.Forms.WebView.Source) 设置为 [`UriWebViewSource`](xref:Xamarin.Forms.UrlWebViewSource) 或 [`HtmlWebViewSource`](xref:Xamarin.Forms.HtmlWebViewSource) 对象。<br /><br />[API 文档](xref:Xamarin.Forms.WebView)  / [指南](~/xamarin-forms/user-interface/webview.md)  / [示例 1](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithwebview)和[2](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-webview) | [![Web 视图示例](views-images/WebView.png "Web 视图示例")](views-images/WebView-Large.png#lightbox "Web 视图示例")<br />[此页](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/WebViewDemoPage.cs)  /  的 c # 代码[XAML 页](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/WebViewDemoPage.xaml) |
|     |     |

## <a name="views-that-initiate-commands"></a>启动命令的视图

### <a name="button"></a>Button

|     |     |
| --- | --- |
| [`Button`](xref:Xamarin.Forms.Button)是一个矩形对象，用于显示文本，并 [`Clicked`](xref:Xamarin.Forms.Button.Clicked) 在按下时触发事件。<br /><br />[API 文档](xref:Xamarin.Forms.Button)  / [指南](~/xamarin-forms/user-interface/button.md)  / [示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos/) | [![按钮示例](views-images/Button.png "按钮示例")](views-images/Button-Large.png#lightbox "按钮示例")<br /> [此页](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ButtonDemoPage.cs)  /  的 c # 代码带有[代码隐藏](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ButtonDemoPage.xaml.cs)的[XAML 页](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ButtonDemoPage.xaml) |
|     |     |

### <a name="imagebutton"></a>ImageButton

|     |     |
| --- | --- |
| `ImageButton`是一个矩形对象，它显示图像，并 `Clicked` 在按下时触发事件。<br /><br /> [指南](~/xamarin-forms/user-interface/imagebutton.md)  / [示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery) | [![ImageButton 示例](views-images/ImageButton.png "ImageButton 示例")](views-images/ImageButton-Large.png#lightbox "ImageButton 示例")<br /> [此页](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ImageButtonDemoPage.cs)  /  的 c # 代码带有[代码隐藏](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageButtonDemoPage.xaml.cs)的[XAML 页](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageButtonDemoPage.xaml) |
|     |     |

### <a name="radiobutton"></a>RadioButton

|     |     |
| --- | --- |
| `RadioButton`允许从集中选择一个选项，并在 `CheckedChanged` 选择发生时触发事件。<br /><br />[指南](~/xamarin-forms/user-interface/radiobutton.md)  / [示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-radiobuttondemos/) | [![单选按钮示例](views-images/RadioButton.png "单选按钮示例")](views-images/RadioButton-Large.png#lightbox "单选按钮示例")<br /> [此页](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/RadioButtonDemoPage.cs)  /  的 c # 代码带有[代码隐藏](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/RadioButtonDemoPage.xaml.cs)的[XAML 页](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/RadioButtonDemoPage.xaml) |
|     |     |

### <a name="refreshview"></a>RefreshView

|     |     |
| --- | --- |
| `RefreshView`是一个容器控件，提供可滚动内容的请求刷新功能。 `ICommand` `Command` 当触发刷新时，将执行由属性定义的，而 `IsRefreshing` 属性指示控件的当前状态。<br /><br /> [指南](~/xamarin-forms/user-interface/refreshview.md)  / [示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery) | [![RefreshView 示例](views-images/RefreshView.png "RefreshView 示例")](views-images/RefreshView-Large.png#lightbox "RefreshView 示例")<br /> [此页](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/RefreshViewDemoPage.cs)  /  的 c # 代码带有[代码隐藏](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/RefreshViewDemoPage.xaml.cs)的[XAML 页](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/RefreshViewDemoPage.xaml) |
|     |     |

### <a name="searchbar"></a>搜索栏

|     |     |
| --- | --- |
| [`SearchBar`](xref:Xamarin.Forms.SearchBar)显示用户键入文本字符串的区域，以及指示应用程序执行搜索的按钮（或键盘键）。 [`Text`](xref:Xamarin.Forms.InputView.Text)属性提供对文本的访问，该 [`SearchButtonPressed`](xref:Xamarin.Forms.SearchBar.SearchButtonPressed) 事件指示已按下该按钮。<br /><br />[API 文档](xref:Xamarin.Forms.SearchBar)  / [指南](~/xamarin-forms/user-interface/searchbar.md)  / [示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-searchbardemos/) | [![SearchBar 示例](views-images/SearchBar.png "SearchBar 示例")](views-images/SearchBar-Large.png#lightbox "SearchBar 示例")<br /> [此页](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SearchBarDemoPage.cs)  /  的 c # 代码带有[代码隐藏](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SearchBarDemoPage.xaml.cs)的[XAML 页](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SearchBarDemoPage.xaml) |
|     |     |

### <a name="swipeview"></a>SwipeView

|     |     |
| --- | --- |
| `SwipeView`是一个容器控件，该控件环绕一项内容，并提供通过轻扫手势显示的上下文菜单项。 每个菜单项都由表示 `SwipeItem` ，该属性具有在 `Command` `ICommand` 点击项时执行的属性。<br /><br /> [指南](~/xamarin-forms/user-interface/swipeview.md)  / [示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery) | [![SwipeView 示例](views-images/SwipeView.png "SwipeView 示例")](views-images/SwipeView-Large.png#lightbox "SwipeView 示例")<br /> [此页](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SwipeViewDemoPage.cs)  /  的 c # 代码带有[代码隐藏](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwipeViewDemoPage.xaml.cs)的[XAML 页](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwipeViewDemoPage.xaml) |
|     |     |

## <a name="views-for-setting-values"></a>设置值的视图

### <a name="checkbox"></a>CheckBox

|     |     |
| --- | --- |
| `CheckBox`允许用户使用可以选中或空的按钮类型选择布尔值。 `IsChecked`属性是的状态 `CheckBox` ，在 `CheckedChanged` 状态更改时激发事件。<br /><br />API 文档/[指南](~/xamarin-forms/user-interface/checkbox.md)  /  [示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-checkboxdemos) | [![复选框示例](views-images/CheckBox.png "复选框示例")](views-images/CheckBox-Large.png#lightbox "复选框示例")<br />[此页](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/CheckBoxPage.cs)  /  的 c # 代码[XAML 页](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/CheckBoxPage.xaml) |
|     |     |

### <a name="slider"></a>Slider

|     |     |
| --- | --- |
| [`Slider`](xref:Xamarin.Forms.Slider)允许用户 `double` 从使用和属性指定的连续范围中选择值 [`Minimum`](xref:Xamarin.Forms.Slider.Minimum) [`Maximum`](xref:Xamarin.Forms.Slider.Maximum) 。<br /><br />[API 文档](xref:Xamarin.Forms.Slider)  / [指南](~/xamarin-forms/user-interface/slider.md)  / [示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-sliderdemos) | [![滑块示例](views-images/Slider.png "滑块示例")](views-images/Slider-Large.png#lightbox "滑块示例")<br />[此页](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SliderDemoPage.cs)  /  的 c # 代码[XAML 页](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SliderDemoPage.xaml) |
|     |     |

### <a name="stepper"></a>步进器

|     |     |
| --- | --- |
| [`Stepper`](xref:Xamarin.Forms.Stepper)允许用户 `double` 从使用 [`Minimum`](xref:Xamarin.Forms.Stepper.Minimum) 、 [`Maximum`](xref:Xamarin.Forms.Stepper.Maximum) 和属性指定的增量值的范围中选择值 [`Increment`](xref:Xamarin.Forms.Stepper.Increment) 。<br /><br />[API 文档](xref:Xamarin.Forms.Stepper)   / [指南](~/xamarin-forms/user-interface/stepper.md)  / [示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-stepperdemos) | [![分档器示例](views-images/Stepper.png "分档器示例")](views-images/Stepper-Large.png#lightbox "分档器示例")<br />[此页](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/StepperDemoPage.cs)  /  的 c # 代码[XAML 页](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/StepperDemoPage.xaml) |
|     |     |

### <a name="switch"></a>开关

|     |     |
| --- | --- |
| [`Switch`](xref:Xamarin.Forms.Switch)采用 "开/关" 开关的形式，允许用户选择一个布尔值。 [`IsToggled`](xref:Xamarin.Forms.Switch.IsToggled)属性是开关的状态，在 [`Toggled`](xref:Xamarin.Forms.Switch.Toggled) 状态更改时激发事件。<br /><br />[API 文档](xref:Xamarin.Forms.Switch)  / [指南](~/xamarin-forms/user-interface/switch.md)  / [示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-switchdemos/) | [![Switch 示例](views-images/Switch.png "Switch 示例")](views-images/Switch-Large.png#lightbox "Switch 示例")<br />[此页](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SwitchDemoPage.cs)  /  的 c # 代码[XAML 页](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwitchDemoPage.xaml) |
|     |     |

### <a name="datepicker"></a>DatePicker

|     |     |
| --- | --- |
| [`DatePicker`](xref:Xamarin.Forms.DatePicker)允许用户使用平台日期选取器选择日期。 使用和属性设置允许的日期范围 [`MinimumDate`](xref:Xamarin.Forms.DatePicker.MinimumDate) [`MaximumDate`](xref:Xamarin.Forms.DatePicker.MaximumDate) 。 该 [`Date`](xref:Xamarin.Forms.DatePicker.Date) 属性是所选的日期，而该 [`DateSelected`](xref:Xamarin.Forms.DatePicker.DateSelected) 属性发生更改时将触发该事件。<br /><br />[API 文档](xref:Xamarin.Forms.DatePicker)  / [指南](~/xamarin-forms/user-interface/datepicker.md)  / [示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-datepicker) | [![DatePicker 示例](views-images/DatePicker.png "DatePicker 示例")](views-images/DatePicker-Large.png#lightbox "DatePicker 示例")<br />[此页](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/DatePickerDemoPage.cs)  /  的 c # 代码[XAML 页](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/DatePickerDemoPage.xaml) |
|     |     |

### <a name="timepicker"></a>TimePicker

|     |     |
| --- | --- |
| [`TimePicker`](xref:Xamarin.Forms.TimePicker)允许用户使用平台时间选取器选择时间。 该 [`Time`](xref:Xamarin.Forms.TimePicker.Time) 属性是所选时间。 应用程序可以 `Time` 通过安装事件的处理程序来监视属性中的更改 [`PropertyChanged`](xref:Xamarin.Forms.BindableObject.PropertyChanged) 。<br /><br />[API 文档](xref:Xamarin.Forms.TimePicker)  / [指南](~/xamarin-forms/user-interface/timepicker.md)  / [示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-timepicker) | [![TimePicker 示例](views-images/TimePicker.png "TimePicker 示例")](views-images/TimePicker-Large.png#lightbox "TimePicker 示例")<br />[此页](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TimePickerDemoPage.cs)  /  的 c # 代码[XAML 页](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TimePickerDemoPage.xaml) |
|     |     |

## <a name="views-for-editing-text"></a>用于编辑文本的视图

这两个类派生自 [`InputView`](xref:Xamarin.Forms.InputView) 类，该类定义 [`Keyboard`](xref:Xamarin.Forms.InputView.Keyboard) 属性。

### <a name="entry"></a>条目

|     |     |
| --- | --- |
| [`Entry`](xref:Xamarin.Forms.Entry)允许用户输入和编辑单个文本行。 文本作为属性提供，并且 [`Text`](xref:Xamarin.Forms.InputView.Text) [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged) 和 [`Completed`](xref:Xamarin.Forms.Entry.Completed) 事件在文本更改时激发，而在用户通过点击 enter 键通知完成时触发。<br /><br />使用 [`Editor`](#editor) 输入和编辑多行文本。<br /><br />[API 文档](xref:Xamarin.Forms.Entry)  / [指南](~/xamarin-forms/user-interface/text/entry.md)  / [示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text) | [![条目示例](views-images/Entry.png "条目示例")](views-images/Entry-Large.png#lightbox "条目示例")<br />[此页](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EntryDemoPage.cs)  /  的 c # 代码[XAML 页](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EntryDemoPage.xaml) |
|     |     |

### <a name="editor"></a>编辑器

|     |     |
| --- | --- |
| [`Editor`](xref:Xamarin.Forms.Editor)允许用户输入和编辑多行文本。 文本作为属性提供，并且 [`Text`](xref:Xamarin.Forms.InputView.Text) [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged) [`Completed`](xref:Xamarin.Forms.Editor.Completed) 当文本更改或用户指示完成时，将激发和事件。<br /><br />使用 [`Entry`](#entry) 视图来输入和编辑单个文本行。<br /><br />[API 文档](xref:Xamarin.Forms.Editor)  / [指南](~/xamarin-forms/user-interface/text/editor.md)  / [示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text) | [![条目示例](views-images/Editor.png "编辑器示例")](views-images/Editor-Large.png#lightbox "编辑器示例")<br />[此页](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EditorDemoPage.cs)  /  的 c # 代码[XAML 页](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EditorDemoPage.xaml) |
|     |     |

## <a name="views-to-indicate-activity"></a>指示活动的视图

### <a name="activityindicator"></a>ActivityIndicator

|     |     |
| --- | --- |
| [`ActivityIndicator`](xref:Xamarin.Forms.ActivityIndicator)使用动画来表明，应用程序参与了长时间的活动，而不提供任何进度指示。 [`IsRunning`](xref:Xamarin.Forms.ActivityIndicator.IsRunning)属性控制动画。<br /><br />如果活动的进度已知，请改用 [`ProgressBar`](#progressbar) 。<br /><br />[API 文档](xref:Xamarin.Forms.ActivityIndicator)  / [指南](~/xamarin-forms/user-interface/activityindicator.md)  / [示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-activityindicatordemos/) | [![ActivityIndicator 示例](views-images/ActivityIndicator.png "ActivityIndicator 示例")](views-images/ActivityIndicator-Large.png#lightbox "ActivityIndicator 示例")<br />[此页](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ActivityIndicatorDemoPage.cs)  /  的 c # 代码[XAML 页](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ActivityIndicatorDemoPage.xaml) |
|     |     |

### <a name="progressbar"></a>ProgressBar

|     |     |
| --- | --- |
| [`ProgressBar`](xref:Xamarin.Forms.ProgressBar)使用动画显示应用程序正在经历长时间的活动。 将 [`Progress`](xref:Xamarin.Forms.ProgressBar.Progress) 属性设置为介于0和1之间的值，以指示进度。<br /><br />如果活动的进度未知，请改用 [`ActivityIndicator`](#activityindicator) 。<br /><br />[API 文档](xref:Xamarin.Forms.ProgressBar)  / [指南](~/xamarin-forms/user-interface/progressbar.md)  / [示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-progressbardemos/) | [![ProgressBar 示例](views-images/ProgressBar.png "ProgressBar 示例")](views-images/ProgressBar-Large.png#lightbox "ProgressBar 示例")<br />[此页](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ProgressBarDemoPage.cs)  /  的 c # 代码带有[代码隐藏](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ProgressBarDemoPage.xaml.cs)的[XAML 页](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ProgressBarDemoPage.xaml) |
|     |     |

## <a name="views-that-display-collections"></a>显示集合的视图

### <a name="carouselview"></a>CarouselView

|     |     |
| --- | --- |
| [`CarouselView`](xref:Xamarin.Forms.CarouselView)显示一个可滚动的数据项列表。 将 `ItemsSource` 属性设置为对象的集合，并将属性设置 `ItemTemplate` 为 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 描述如何设置这些项的格式的对象。 此 `CurrentItemChanged` 事件指示当前显示的项已更改，这可作为 `CurrentItem` 属性。<br /><br />[指南](~/xamarin-forms/user-interface/carouselview/index.md)  / [示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/) | [![CarouselView 示例](views-images/CarouselView.png "CarouselView 示例")](views-images/CarouselView-Large.png#lightbox "CarouselView 示例")<br />[此页](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/CarouselViewDemoPage.cs)  /  的 c # 代码[XAML 页](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/CarouselViewDemoPage.xaml) |
|     |     |

### <a name="collectionview"></a>CollectionView

|     |     |
| --- | --- |
| [`CollectionView`](xref:Xamarin.Forms.CollectionView)使用不同的布局规范显示可滚动的数据项列表。 它旨在提供更灵活、更高的性能替代方法 [`ListView`](xref:Xamarin.Forms.ListView) 。 将 `ItemsSource` 属性设置为对象的集合，并将属性设置 `ItemTemplate` 为 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 描述如何设置这些项的格式的对象。 该 `SelectionChanged` 事件指示已进行了选择，这可作为 `SelectedItem` 属性。<br /><br />[指南](~/xamarin-forms/user-interface/collectionview/index.md)  / [示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/) | [![CollectionView 示例](views-images/CollectionView.png "CollectionView 示例")](views-images/CollectionView-Large.png#lightbox "CollectionView 示例")<br />[此页](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/CollectionViewDemoPage.cs)  /  的 c # 代码[XAML 页](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/CollectionViewDemoPage.xaml) |
|     |     |

### <a name="indicatorview"></a>IndicatorView

|     |     |
| --- | --- |
| `IndicatorView`显示表示中的项数的指示器 `CarouselView` 。 将 `CarouselView.IndicatorView` 属性设置为 `IndicatorView` 要显示其指示器的对象 `CarouselView` 。 <br /><br />[指南](~/xamarin-forms/user-interface/indicatorview.md)  / [示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-indicatorviewdemos/) | [![IndicatorView 示例](views-images/IndicatorView.png "IndicatorView 示例")](views-images/IndicatorView-Large.png#lightbox "IndicatorView 示例")<br />[此页](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/IndicatorViewDemoPage.cs)  /  的 c # 代码[XAML 页](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/IndicatorViewDemoPage.xaml) |
|     |     |

### <a name="listview"></a>ListView

|     |     |
| --- | --- |
| [`ListView`](xref:Xamarin.Forms.ListView)派生自 [`ItemsView`](xref:Xamarin.Forms.ItemsView`1) ，并显示可选择数据项的可滚动列表。 将 [`ItemsSource`](xref:Xamarin.Forms.ItemsView`1.ItemsSource) 属性设置为对象的集合，并将属性设置 [`ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) 为 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 描述如何设置这些项的格式的对象。 该 [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) 事件指示已进行了选择，这可作为 [`SelectedItem`](xref:Xamarin.Forms.ListView.SelectedItem) 属性。<br /><br />[API 文档](xref:Xamarin.Forms.ListView)  / [指南](~/xamarin-forms/user-interface/listview/index.md)  / [示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistview/) | [![ListView 示例](views-images/ListView.png "ListView 示例")](views-images/ListView-Large.png#lightbox "ListView 示例")<br />[此页](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ListViewDemoPage.cs)  /  的 c # 代码[XAML 页](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ListViewDemoPage.xaml) |
|     |     |

### <a name="picker"></a>选取器

|     |     |
| --- | --- |
| [`Picker`](xref:Xamarin.Forms.Picker)显示文本字符串列表中的选定项，并允许在点击视图时选择该项。 将 [`Items`](xref:Xamarin.Forms.Picker.Items) 属性设置为字符串列表，或将属性设置为 [`ItemsSource`](xref:Xamarin.Forms.Picker.ItemsSource) 对象的集合。 [`SelectedIndexChanged`](xref:Xamarin.Forms.Picker.SelectedIndexChanged)选择项时触发事件。<br /><br />`Picker`仅当选择项时，才显示项列表。 [`ListView`](#listview) [`TableView`](#tableview) 对保留在页面上的可滚动列表使用或。<br /><br />[API 文档](xref:Xamarin.Forms.Picker)  / [指南](~/xamarin-forms/user-interface/picker/index.md)  / [示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-pickerdemo) | [![选取器示例](views-images/Picker.png "选取器示例")](views-images/Picker-Large.png#lightbox "选取器示例")<br />[此页](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/PickerDemoPage.cs)  /  的 c # 代码带有[代码隐藏](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/PickerDemoPage.xaml.cs)的[XAML 页](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/PickerDemoPage.xaml) |
|     |     |

### <a name="tableview"></a>TableView

|     |     |
| --- | --- |
| [`TableView`](xref:Xamarin.Forms.TableView)显示 [`Cell`](xref:Xamarin.Forms.Cell) 具有可选标头和小标题的类型的行的列表。 将 [`Root`](xref:Xamarin.Forms.TableView.Root) 属性设置为类型的对象 [`TableRoot`](xref:Xamarin.Forms.TableRoot) ，并向 [`TableSection`](xref:Xamarin.Forms.TableSection) 其中添加对象 `TableRoot` 。 每个 `TableSection` 都是对象的集合 `Cell` 。<br /><br />[API 文档](xref:Xamarin.Forms.TableView)  / [指南](~/xamarin-forms/user-interface/tableview.md)  / [示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-tableview) | [![TableView 示例](views-images/TableView.png "TableView 示例")](views-images/TableView-Large.png#lightbox "TableView 示例")<br />[此页](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TableViewFormDemoPage.cs)  /  的 c # 代码[XAML 页](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TableViewFormDemoPage.xaml) |
|     |     |

## <a name="related-links"></a>相关链接

- [Xamarin.FormsFormsGallery 示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)
- [Xamarin.Forms 示例](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Forms)
- [Xamarin.FormsAPI 文档](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
