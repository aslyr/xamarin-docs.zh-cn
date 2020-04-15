---
title: 摘要：第 27 章. 自定义呈现器
description: 使用 Xamarin.Forms 创建移动应用：摘要：第 27 章. 自定义呈现器
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 49961953-9336-4FD4-A42F-6D9B05FF52E7
author: davidbritch
ms.author: dabritch
ms.date: 07/18/2018
ms.openlocfilehash: fd4014fa4db4e90596c100d454cf0467512240a4
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/13/2020
ms.locfileid: "70760495"
---
# <a name="summary-of-chapter-27-custom-renderers"></a>摘要：第 27 章. 自定义呈现器

[![下载示例](~/media/shared/download.png) 下载示例](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27)

> [!NOTE] 
> 此页上的“注意”指出了 Xamarin.Forms 与书中所述内容的不同之处。

Xamarin.Forms 元素（如 `Button`）是通过在名为 `ButtonRenderer` 的类中封装的特定于平台的按钮呈现的。  例如，[iOS 版 `ButtonRenderer`](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.iOS/Renderers/ButtonRenderer.cs)、[Android 版 `ButtonRenderer`](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.Android/Renderers/ButtonRenderer.cs) 以及 [UWP 版 `ButtonRenderer`](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.UAP/ButtonRenderer.cs)。

本章介绍如何编写自己的呈现器来创建可映射到特定于平台的对象的自定义视图。

## <a name="the-complete-class-hierarchy"></a>完整的类层次结构

有四个程序集包含 Xamarin.Forms 平台特定的代码。
可以访问以下链接，查看 GitHub 上的源代码：

- [**Xamarin.Forms.Platform**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform)（非常小）
- [**Xamarin.Forms.Platform.iOS**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.iOS)
- [**Xamarin.Forms.Platform.Android**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.Android)
- [**Xamarin.Forms.Platform.UAP**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.UAP)

> [!NOTE]
> 本书中提到的 `WinRT` 程序集不再作为此解决方案的一部分。 

[PlatformClassHierarchy  ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/PlatformClassHierarchy) 示例展示了能有效执行平台的程序集的类层次结构。

你会注意到有一个名为 `ViewRenderer` 的重要类。 这是在创建特定于平台的呈现器时派生的那个类。 它存在三种不同的版本，因为它与目标平台的视图系统紧密关联：

iOS 版 [`ViewRenderer<TView, TNativeView>`](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.iOS/ViewRenderer.cs#L25) 具有以下泛型参数：

- `TView` 仅用于 [`Xamarin.Forms.View`](xref:Xamarin.Forms.View)
- `TNativeView` 仅用于 [`UIKit.UIView`](xref:UIKit.UIView)

Android 版 [`ViewRenderer<TView, TNativeView>`](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.Android/ViewRenderer.cs#L17) 具有以下泛型参数：

- `TView` 仅用于 [`Xamarin.Forms.View`](xref:Xamarin.Forms.View)
- `TNativeView` 仅用于 [`Android.Views.View`](xref:Android.Views.View)

UWP 版 [`ViewRenderer<TElement, TNativeElement>`](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.UAP/ViewRenderer.cs#L6) 具有不同的命名泛型参数：

- `TElement` 仅用于 [`Xamarin.Forms.View`](xref:Xamarin.Forms.View)
- `TNativeElement` 仅用于 [`Windows.UI.Xaml.FrameworkElement`](/uwp/api/Windows.UI.Xaml.FrameworkElement)

编写呈现器时，将从 `View` 派生一个类，然后编写多个 `ViewRenderer` 类（每个受支持的平台一个）。 每个特定于平台的实现都将引用一个本机类，该类派生自你指定为 `TNativeView` 或 `TNativeElement` 参数的类型。

## <a name="hello-custom-renderers"></a>初识自定义呈现器

[HelloRenderers  ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/HelloRenderers) 程序引用它自己的 [`App`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers/App.cs) 类中名为 `HelloView` 的自定义视图。

[`HelloView`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers/HelloView.cs) 类包含在 HelloRenderers  项目中，它只派生自 `View`。

HelloRenderers.iOS  项目中的 [`HelloViewRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers.iOS/HelloViewRenderer.cs) 类派生自 `ViewRenderer<HelloView, UILabel>`。 在 `OnElementChanged` 重写中，它创建了一个本机 iOS `UILabel` 并调用 `SetNativeControl`。

HelloRenderers.Droid  项目中的 [`HelloViewRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers.Droid/HelloViewRenderer.cs) 类派生自 `ViewRenderer<HelloView, TextView>`。 在 `OnElementChanged` 重写中，它创建了一个 Android `TextView` 并调用 `SetNativeControl`。

HelloRenderers.UWP  中的 [`HelloViewRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers.UWP/HelloViewRenderer.cs) 类以及其他 Windows 项目均派生自 `ViewRenderer<HelloView, TextBlock>`。 在 `OnElementChanged` 重写中，它创建了一个 Windows `TextBlock` 并调用 `SetNativeControl`。

所有 `ViewRenderer` 导数都包含程序集级别的 `ExportRenderer` 属性，该属性将 `HelloView` 类与特定 `HelloViewRenderer` 类相关联。 下面是 Xamarin.Forms 在各个平台项目中定位呈现器的示例：

[![Hello 视图的三倍屏幕截图](images/ch27fg02-small.png "自定义呈现器")](images/ch27fg02-large.png#lightbox "自定义呈现器")

## <a name="renderers-and-properties"></a>呈现器和属性

下一组呈现器实现了椭圆绘图，它们位于 [Xamarin.FormsBook.Platform  ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform) 解决方案的不同项目中。

[`EllipseView`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/EllipseView.cs) 类位于 Xamarin.FormsBook.Platform  平台中。 此类与 `BoxView` 类似，只定义了一个属性：`Color` 类型的 `Color`。

呈现器可以通过重写呈现器中的 `OnElementPropertyChanged` 方法，将为 `View` 设置的属性值传输到本机对象。 在此方法中（并且在大多数呈现器中），可以使用两个属性：

- `Element`，Xamarin.Forms 元素
- `Control`，本机视图、小组件或控件对象

这些属性的类型由 `ViewRenderer` 的泛型参数确定。 在本例中，`Element` 为 `EllipseView` 类型。

因此，`OnElementPropertyChanged` 重写可以通过某种转换将 `Element` 的 `Color` 值传输到本机 `Control` 对象。 三个呈现器如下所示：

- iOS：[`EllipseViewRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/EllipseViewRenderer.cs)，它使用椭圆的 [`EllipseUIView`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/EllipseUIView.cs) 类。
- Android：[`EllipseViewRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/EllipseViewRenderer.cs)，它使用椭圆的 [`EllipseDrawableView`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/EllipseDrawableView.cs) 类。
- UWP：[`EllipseViewRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/EllipseViewRenderer.cs)，它可以使用本机 Windows [`Ellipse`](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse) 类。

[EllipseDemo  ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/EllipseDemo) 类将显示以下几个 `EllipseView` 对象：

[![椭圆演示的三倍屏幕截图](images/ch27fg03-small.png "EllipseView 自定义呈现器")](images/ch27fg03-large.png#lightbox "EllipseView 自定义呈现器")

[BouncingBall  ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/BouncingBall) 将 `EllipseView` 弹离屏幕边缘。

## <a name="renderers-and-events"></a>呈现器和事件

呈现器也可以间接生成事件。 [`StepSlider`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/StepSlider.cs) 类与正常的 Xamarin.Forms `Slider` 类似，但前者允许在 `Minimum` 和 `Maximum` 值之间指定多个不连续的阶段。

三个呈现器如下所示：

- iOS：[`StepSliderRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/StepSliderRenderer.cs)
- Android：[`StepSliderRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/StepSliderRenderer.cs)
- UWP：[`StepSliderRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/StepSliderRenderer.cs)

呈现器检测对本机控件的更改，然后调用 `SetValueFromRenderer`，这会引用在 `StepSlider` 中定义的可绑定属性，这种更改将导致 `StepSlider` 触发 `ValueChanged` 事件。

[StepSliderDemo  ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/StepSliderDemo) 示例展示了此新滑块。

## <a name="related-links"></a>相关链接

- [第 27 章全文 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch27-Apr2016.pdf)
- [第 27 章示例](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27)
- [自定义呈现器](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
