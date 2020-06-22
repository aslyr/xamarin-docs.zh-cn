---
title: title:“摘要：第 16 章. 数据绑定”说明："《使用 Xamarin.Forms 创建移动应用》- 摘要：第 16 章摘要。
description: '数据绑定”ms.prod: xamarin ms.technology: xamarin-forms ms.assetid:ED997DB0-C229-4868-A5FB-928703B377D6 author: davidbritch ms.author: dabritch ms.date:2018/07/18 no-loc: [Xamarin.Forms, Xamarin.Essentials] 第 16 章摘要。'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: ED997DB0-C229-4868-A5FB-928703B377D6
author: davidbritch
ms.author: dabritch
ms.date: 07/18/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: ece93730100001e8339a5f50cdb7ac437d96fa62
ms.sourcegitcommit: ea9269b5d9e3d68b61bb428560a10034117ee457
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/10/2020
ms.locfileid: "84136729"
---
# <a name="summary-of-chapter-16-data-binding"></a>数据绑定 [![下载示例](~/media/shared/download.png) 下载示例](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16)

此页上的“注意”指出了 Xamarin.Forms 与书中所述内容的不同之处。

> [!NOTE] 
> 程序员经常会编写事件处理程序以检测某对象属性的更改时间，并使用该事件处理程序更改另一个对象中的属性值。

可以使用数据绑定方法自动执行此流程。 数据绑定通常在 XAML 中定义，且属于用户界面定义。 通常，这些数据绑定会将用户界面对象连接到基础数据。

[第 18 章 MVVM](chapter18.md) 中详细介绍了此技术。 但数据绑定还可以连接两个或多个用户界面元素。 本章中，大多数数据绑定早期示例均演示了此方法。 关于绑定的基础知识

## <a name="binding-basics"></a>数据绑定涉及多个属性、方法和类：

[`Binding`](xref:Xamarin.Forms.Binding) 类派生自 [`BindingBase`](xref:Xamarin.Forms.BindingBase) 且可封装数据绑定的许多特性

- [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) 属性由 [`BindableObject`](xref:Xamarin.Forms.BindableObject) 类定义
- [`SetBinding`](xref:Xamarin.Forms.BindableObject.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase)) 方法也由 [`BindableObject`](xref:Xamarin.Forms.BindableObject) 类定义
- [`BindableObjectExtensions`](xref:Xamarin.Forms.BindableObjectExtensions) 类定义其他三个 `SetBinding` 方法
- 以下两个类支持用于绑定的 XAML 标记扩展：

[`BindingExtension`](xref:Xamarin.Forms.Xaml.BindingExtension) 支持 `Binding` 标记扩展

- [`ReferenceExtension`](xref:Xamarin.Forms.Xaml.ReferenceExtension) 支持 `x:Reference` 标记扩展
- 数据绑定涉及两个接口：

`System.ComponentModel` 命名空间中的 [`INotifyPropertyChanged`](xref:System.ComponentModel.INotifyPropertyChanged) 用于在属性更改时进行通知

- [`IValueConverter`](xref:Xamarin.Forms.IValueConverter) 用于定义数据绑定中将值从一种类型转换为另一种类型的小型类
- 数据绑定连接同一对象或两个不同对象（更常见）的两个属性。

这两个属性称为源和目标 。 通常，源属性更改会导致目标属性发生更改，但有时情况相反。 不考虑： 目标属性必须由 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 支持

- 源属性通常是可实现 [`INotifyPropertyChanged`](xref:System.ComponentModel.INotifyPropertyChanged) 的类的成员
- 属性更改值时，可实现 `INotifyPropertyChanged` 的类会触发 [`PropertyChanged`](xref:System.ComponentModel.INotifyPropertyChanged.PropertyChanged) 事件。

`BindableObject` 可实现 `INotifyPropertyChanged` 并在 `BindableProperty` 支持的属性更改值时自动触发 `PropertyChanged` 事件，但你可以自己编写可实现 `INotifyPropertyChanged` 的类，而无需从 `BindableObject` 派生。 代码和 XAML

## <a name="code-and-xaml"></a>[OpacityBindingCode](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/OpacityBindingCode) 示例演示如何在代码中设置数据绑定：

源是 `Slider` 的 `Value` 属性

- 目标是 `Label` 的 `Opacity` 属性
- 将 `Label` 对象的 `BindingContext` 设置为 `Slider` 对象即可连接两个对象。

调用 `Label` 上的 [`SetBinding`](xref:Xamarin.Forms.BindableObjectExtensions.SetBinding*) 扩展方法（可引用以字符串形式表示的 `Slider` 的 `OpacityProperty` 可绑定属性和 `Value` 属性）即可连接这两个属性。 然后，操作 `Slider` 会使 `Label` 淡入和淡出。

[OpacityBindingXaml](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/OpacityBindingXaml) 是与 XAML 中所设数据绑定相同的程序。

`Label` 的 `BindingContext` 设置为 `x:Reference` 标记扩展，其引用 `Slider`，而 `Label` 的 `Opacity` 属性设置为 `Binding` 标记扩展，其 [`Path`](xref:Xamarin.Forms.Binding.Path) 属性引用 `Slider` 的 `Value` 属性。 源和 BindingContext

## <a name="source-and-bindingcontext"></a>[BindingSourceCode](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingSourceCode) 示例显示代码中的替代方法。

将 [`Source`](xref:Xamarin.Forms.Binding.Source) 属性设置为 `Slider` 对象并将 [`Path`](xref:Xamarin.Forms.Binding.Path) 属性设置为“Value”即可创建 `Binding` 对象。 然后在 `Label` 对象上调用 `BindableObject` 的 [`SetBinding`](xref:Xamarin.Forms.BindableObject.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase)) 方法。 [`Binding` constructor](xref:Xamarin.Forms.Binding.%23ctor(System.String,Xamarin.Forms.BindingMode,Xamarin.Forms.IValueConverter,System.Object,System.String,System.Object)) 也可能已用于定义 `Binding` 对象。

[BindingSourceXaml](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingSourceXaml) 示例显示 XAML 中的类似方法。

`Label` 的 `Opacity` 属性设置为 `Binding` 标记扩展，其中 [`Path`](xref:Xamarin.Forms.Binding.Path) 设置为 `Value` 属性，[`Source`](xref:Xamarin.Forms.Binding.Source) 设置为嵌入式 `x:Reference` 标记扩展。 总而言之，以下两种方式可以引用绑定源对象：

通过目标的 `BindingContext` 属性

- 通过 `Binding` 对象自身的 `Source` 属性
- 如果同时指定两者，则第二种优先。

`BindingContext` 的优点在于它通过可视化树传播。 如果将多个目标属性绑定到同一源对象，这非常方便。 [WebViewDemo](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/WebViewDemo) 程序使用 [`WebView`](xref:Xamarin.Forms.WebView) 元素演示此技术。

用于向后导航和向前导航的两个 `Button` 元素从引用 `WebView` 的父级继承 `BindingContext`。 然后，两个按钮的 `IsEnabled` 属性具有简单的 `Binding` 标记扩展，适用于基于 `WebView` 的 [`CanGoBack`](xref:Xamarin.Forms.WebView.CanGoBack) 和 [`CanGoForward`](xref:Xamarin.Forms.WebView.CanGoForward) 只读属性的设置的按钮 `IsEnabled` 属性。 绑定模式

## <a name="the-binding-mode"></a>将 `Binding` 的 [`Mode` ](xref:Xamarin.Forms.BindingBase.Mode) 属性设置为 [`BindingMode`](xref:Xamarin.Forms.BindingMode) 枚举的成员：

[`OneWay`](xref:Xamarin.Forms.BindingMode.OneWay)，使源属性的更改影响目标

- [`OneWayToSource`](xref:Xamarin.Forms.BindingMode.OneWayToSource)，使目标属性的更改影响源
- [`TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay)，使源和目标的更改彼此影响
- [`Default`](xref:Xamarin.Forms.BindingMode.Default)，使用创建目标 `BindableProperty` 时指定的 [`DefaultBindingMode`](xref:Xamarin.Forms.BindableProperty.DefaultBindingMode)。
- 如果未指定，则常规可绑定属性默认为 `OneWay`，只读可绑定属性默认为 `OneWayToSource`。 `BindingMode` 枚举现还包括 `OnTime`，用于仅在绑定上下文发生更改时而不是在源属性发生更改时应用绑定。

> [!NOTE]
> MVVM 场景中可能成为数据绑定目标的属性的 `DefaultBindingMode` 通常为 `TwoWay`。

这些是： `Slider` 和 `Stepper` 的 `Value` 属性

- `Switch` 的 `IsToggled` 属性
- `Entry`、`Editor` 和 `SearchBar` 的 `Text` 属性
- `DatePicker` 的 `Date` 属性
- `TimePicker` 的 `Time` 属性
- [BindingModes](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingModes) 示例演示了数据绑定的四种绑定模式，其中目标是 `Label` 的 `FontSize` 属性，源是 `Slider` 的 `Value` 属性。

这使每个 `Slider` 可以控制相应的 `Label` 的字号。 但 `Slider` 元素未初始化，因为 `FontSize` 属性的 `DefaultBindingMode` 是 `OneWay`。 [ReverseBinding](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/ReverseBinding) 示例在 `Slider`（可引用每个 `Label` 的 `FontSize` 属性）的 `Value` 属性上设置绑定。

这似乎是向后的，但更适合初始化 `Slider` 元素，因为 `Slider` 的 `Value` 属性的 `DefaultBindingMode` 是 `TwoWay`。 [![反向绑定的三个屏幕截图](images/ch16fg06-small.png "反向绑定")](images/ch16fg06-large.png#lightbox "反向绑定")

这类似于在 MVVM 中定义绑定的方式，你将经常使用这种类型的绑定。

字符串格式设置

## <a name="string-formatting"></a>如果目标属性是 `string` 类型，可以使用 `BindingBase` 定义的 [`StringFormat`](xref:Xamarin.Forms.BindingBase.StringFormat) 属性将源转换为 `string`。

将 `StringFormat` 属性设置为 .NET 格式设置字符串，该字符串将与静态 [`String.Format`](xref:System.String.Format(System.String,System.Object)) 格式一起用于显示对象。 在标记扩展中使用此格式设置字符串时，用单引号将它括起来，以免大括号被误以为是嵌入的标记扩展。 [ShowViewValues](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/ShowViewValues) 示例演示如何在 XAML 中使用 `StringFormat`。

[WhatSizeBindings](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/WhatSizeBindings) 示例演示如何绑定 `ContentPage` 的 `Width` 和 `Height` 属性以显示页面的大小。

为什么称其为“路径”？

## <a name="why-is-it-called-path"></a>`Binding` 的 [`Path`](xref:Xamarin.Forms.Binding.Path) 属性被称为“路径”是因为它可以是由句点分隔的一系列属性和索引器。

[BindingPathDemos](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingPathDemos) 示例显示了多个示例。 绑定值转换器

## <a name="binding-value-converters"></a>如果绑定的源属性和目标属性是不同类型，可以使用绑定转换器在类型之间进行转换。

这个类可实现 [`IValueConverter`](xref:Xamarin.Forms.IValueConverter) 接口，且包含两个方法：将源转换为目标的 [`Convert`](xref:Xamarin.Forms.IValueConverter.Convert(System.Object,System.Type,System.Object,System.Globalization.CultureInfo))，以及将目标转换为源的 [`ConvertBack`](xref:Xamarin.Forms.IValueConverter.ConvertBack(System.Object,System.Type,System.Object,System.Globalization.CultureInfo))。 [Xamarin.FormsBook.Toolkit](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 库中的 [`IntToBoolConverter`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/IntToBoolConverter.cs) 类是将 `int` 转换为 `bool` 的示例。

[ButtonEnabler](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/ButtonEnabler) 示例对此进行了演示，该示例在 `Entry` 中至少已键入一个字符时才启用 `Button`。 [`BoolToStringConverter`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BoolToStringConverter.cs) 类将 `bool` 转换为 `string`，并定义两个属性以指定应针对 `false` 和 `true` 值而返回的文本。

[`BoolToColorConverter`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BoolToColorConverter.cs) 类似。
[SwitchText](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/SwitchText) 示例演示如何使用这两个转换器基于 `Switch` 设置以不同颜色显示不同文本。 通用的 [`BoolToObjectConverter`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BoolToObjectConverter.cs) 可替代 `BoolToStringConverter` 和 `BoolToColorConverter`，并且可用作任何类型的通用 `bool`-to-object 转换器。

绑定和自定义视图

## <a name="bindings-and-custom-views"></a>可以使用数据绑定简化自定义控件。

[`NewCheckBox.cs`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NewCheckBox.xaml.cs) 代码文件定义 `Text`、`TextColor`、`FontSize`、`FontAttributes` 和 `IsChecked` 属性，但对于控件的视觉对象而言完全没有逻辑。 而 [`NewCheckBox.cs.xaml`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NewCheckBox.xaml) 文件基于代码隐藏文件中定义的属性，在 `Label` 元素上通过数据绑定包含控件视觉对象的所有标记。
[NewCheckBoxDemo](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/NewCheckBoxDemo) 示例演示 `NewCheckBox` 自定义控件。

相关链接

## <a name="related-links"></a>[第 16 章全文 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch16-Apr2016.pdf)

- [第 16 章示例](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16)
- [数据绑定](~/xamarin-forms/app-fundamentals/data-binding/index.md)
- 反向绑定
