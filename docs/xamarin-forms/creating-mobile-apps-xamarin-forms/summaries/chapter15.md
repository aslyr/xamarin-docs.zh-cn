---
title: title:“摘要：第 15 章. 交互式界面”说明："《使用 Xamarin.Forms 创建移动应用》- 摘要：“第 15 章：
description: '交互式界面”ms.prod: xamarin ms.technology: xamarin-forms ms.assetid:F54E86F4-1CDA-474E-9B09-242060C2C13D author: davidbritch ms.author: dabritch ms.date:2017/11/07 no-loc: [Xamarin.Forms, Xamarin.Essentials] “第 15 章：'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F54E86F4-1CDA-474E-9B09-242060C2C13D
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 9bb930c2d0e4b6281b1aa48589ea5245eedc1a60
ms.sourcegitcommit: ea9269b5d9e3d68b61bb428560a10034117ee457
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/10/2020
ms.locfileid: "84136742"
---
# <a name="summary-of-chapter-15-the-interactive-interface"></a>交互式界面”摘要 [![下载示例](~/media/shared/download.png) 下载示例](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15)

本章节探讨了八个可便于与用户交互的 `View` 衍生物。

视图概述

## <a name="view-overview"></a>Xamarin.Forms 包含 20 个派生自 `View`（而不是 `Layout`）的可实例化类。

前面的章节介绍了其中六个类： `Label`：[**第 2 章：应用剖析**](chapter02.md)

- `BoxView`：[**第 3 章：滚动堆叠**](chapter03.md)
- `Button`：[**第 6 章：按钮单击**](chapter06.md)
- `Image`：[**第 13 章：位图**](chapter13.md)
- `ActivityIndicator`：[**第 13 章：位图**](chapter13.md)
- `ProgressBar`：[**第 14 章：AbsoluteLayout**](chapter14.md)
- 本章节中的八个视图可有效便于用户与基本 .NET 数据类型交互：

数据类型

|视图|[`Slider`](xref:Xamarin.Forms.Slider), [`Stepper`](xref:Xamarin.Forms.Stepper)|
|--- |--- |
|`Double`|[`Entry`](xref:Xamarin.Forms.Entry)、[`Editor`](xref:Xamarin.Forms.Editor)、[`SearchBar`](xref:Xamarin.Forms.SearchBar)|
|`Boolean`|[`Switch`](xref:Xamarin.Forms.Switch)|
|`String`|[`DatePicker`](xref:Xamarin.Forms.DatePicker), [`TimePicker`](xref:Xamarin.Forms.TimePicker)|
|`DateTime`|可以将这些视图视为基础数据类型的可视化交互表示。|

下一章节（[第 16 章：数据绑定](chapter16.md)）更深入地探讨了此概念。 以下章节将介绍其余六个视图：

`WebView`：[**第 16 章：数据绑定**](chapter16.md)

- `Picker`：[**第 19 章：集合视图**](chapter19.md)
- `ListView`：[**第 19 章：集合视图**](chapter19.md)
- `TableView`：[**第 19 章：集合视图**](chapter19.md)
- `Map`：[**第 28 章：位置和地图**](chapter28.md)
- `OpenGLView`：本电子图书未涵盖（Windows 平台不支持）
- 滑块和步进器

## <a name="slider-and-stepper"></a>[`Slider`](xref:Xamarin.Forms.Slider) 和 [`Stepper`](xref:Xamarin.Forms.Stepper) 都允许用户从范围中选择一个数值。

`Slider` 是连续范围，而 `Stepper` 则涉及离散值。 滑块基础知识

### <a name="slider-basics"></a>[`Slider`](xref:Xamarin.Forms.Slider) 是一个水平条，表示从左侧最小值到右侧最大值的值范围。

它定义了三个公共属性： `double` 类型的 [`Value`](xref:Xamarin.Forms.Slider.Value)（默认值为 0）

- `double` 类型的 [`Minimum`](xref:Xamarin.Forms.Slider.Minimum)（默认值为 0）
- `double` 类型的 [`Maximum`](xref:Xamarin.Forms.Slider.Maximum)（默认值为 1）
- 支持这些属性的可绑定属性可确保它们是一致的：

对于所有这三个属性，为可绑定属性指定的 [`coerceValue`](xref:Xamarin.Forms.BindableProperty.CoerceValueDelegate) 方法可确保 `Value` 介于 `Minimum` 和 `Maximum` 之间。

- 如果 `Minimum` 设置为大于或等于 `Maximum` 的值，`MinimumProperty` 的 [`validateValue`](xref:Xamarin.Forms.BindableProperty.ValidateValueDelegate) 方法就会返回 `false`；对于 `MaximumProperty`，也是类似的情况。
- `validateValue` 方法返回 `false` 会导致 `ArgumentException` 抛出。 如果 `Value` 属性更改（无论是以编程方式更改，还是用户控制 `Slider`），`Slider` 会触发含 [`ValueChangedEventArgs`](xref:Xamarin.Forms.ValueChangedEventArgs) 参数的 [`ValueChanged`](xref:Xamarin.Forms.Slider.ValueChanged) 事件。

[SliderDemo](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SliderDemo) 示例展示了 `Slider` 的简单用法。

常见错误

### <a name="common-pitfalls"></a>在代码和 XAML 中，`Minimum` 和 `Maximum` 属性都是按照指定顺序设置的。

请务必初始化这些属性，以便 `Maximum` 总是大于 `Minimum`。 否则，将引发异常。 初始化 `Slider` 属性可能会导致 `Value` 属性更改，并触发 `ValueChanged` 事件。

应确保 `Slider` 事件处理程序不会访问在页面初始化期间尚未创建的视图。 `ValueChanged` 事件不会在 `Slider` 初始化期间触发，除非 `Value` 属性更改。

可以直接使用代码来调用 `ValueChanged` 处理程序。 颜色选择滑块

### <a name="slider-color-selection"></a>[RgbSliders](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/RgbSliders) 程序包含三个 `Slider` 元素，可便于通过指定 RGB 值以交互方式选择颜色：

[![R G B 滑块的三倍屏幕截图](images/ch15fg03-small.png "RGB 滑块")](images/ch15fg03-large.png#lightbox "RGB 滑块")

[TextFade](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/TextFade) 示例使用两个 `Slider` 元素跨 `AbsoluteLayout` 移动两个 `Label` 元素，并让其中一个元素淡入另一个元素。

步进器区别

### <a name="the-stepper-difference"></a>[`Stepper`](xref:Xamarin.Forms.Stepper) 定义与 `Slider` 相同的属性和事件，区别在于 `Maximum` 属性初始化为 100，且 `Stepper` 定义第四个属性：

`double` 类型的 [`Increment`](xref:Xamarin.Forms.Stepper.Increment)（初始化为 1）

- 从视觉上看，`Stepper` 包含两个标记为 &ndash; 和 + 的按钮。

按 &ndash; 会让 `Value` 按 `Increment` 递减到最小值 `Minimum`。 按 + 会让 `Value` 按 `Increment` 递增到最大值 `Maximum`。 [StepperDemo](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/StepperDemo) 示例对此进行了展示。

开关和复选框

## <a name="switch-and-checkbox"></a>[`Switch`](xref:Xamarin.Forms.Switch) 允许用户指定布尔值。

开关基础知识

### <a name="switch-basics"></a>从视觉上看，`Switch` 包含可以禁用和启用的切换。

此类定义了一个属性： `bool` 类型的 [`IsToggled`](xref:Xamarin.Forms.Switch.IsToggled)

- `Switch` 定义一个事件：

[`Toggled`](xref:Xamarin.Forms.Switch.Toggled)：附带了 [`ToggledEventArgs`](xref:Xamarin.Forms.ToggledEventArgs) 对象，在 `IsToggled` 属性更改时触发。

- [SwitchDemo](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SwitchDemo) 程序展示了 `Switch`。

传统复选框

### <a name="a-traditional-checkbox"></a>一些开发人员可能更倾向于传统的 `CheckBox`，而不是 `Switch`。

[Xamarin.FormsBook.Toolkit](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 库包含派生自 `ContentView` 的 `CheckBox` 类。 `CheckBox` 由 [CheckBox.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CheckBox.xaml) 和 [CheckBox.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CheckBox.xaml.cs) 文件实现。 `CheckBox` 定义了三个属性（`Text`、`FontSize` 和 `IsChecked`）和 `CheckedChanged` 事件。 [CheckBoxDemo](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/CheckBoxDemo) 示例展示了此 `CheckBox`。

键入文本

## <a name="typing-text"></a>Xamarin.Forms 定义了三个可便于用户输入和编辑文本的视图：

[`Entry`](xref:Xamarin.Forms.Entry)：用于单行文本

- [`Editor`](xref:Xamarin.Forms.Editor)：用于多行文本
- [`SearchBar`](xref:Xamarin.Forms.SearchBar)：用于执行搜索的单行文本。
- `Entry` 和 `Editor` 派生自 [`InputView`](xref:Xamarin.Forms.InputView)，后者又派生自 `View`。

`SearchBar` 直接派生自 `View`。 键盘和焦点

### <a name="keyboard-and-focus"></a>在无物理键盘的电话和平板电脑上，`Entry`、`Editor` 和 `SearchBar` 元素都会弹出虚拟键盘。

此键盘在屏幕上的出现与输入焦点有关。 视图的 [`IsVisible`](xref:Xamarin.Forms.VisualElement.IsVisible) 和 [`IsEnabled`](xref:Xamarin.Forms.VisualElement.IsEnabled) 属性必须同时设置为 `true`，才能获得输入焦点。 输入焦点涉及两个方法、一个只读属性和两个事件。

这些都是由 `VisualElement` 定义的： [`Focus`](xref:Xamarin.Forms.VisualElement.Focus) 方法：尝试对元素设置输入焦点，并在成功时返回 `true`

- [`Unfocus`](xref:Xamarin.Forms.VisualElement.Unfocus) 方法：删除元素的输入焦点
- [`IsFocused`](xref:Xamarin.Forms.VisualElement.IsFocused) 只读属性：指明元素是否有输入焦点
- [`Focused`](xref:Xamarin.Forms.VisualElement.Focused) 事件：指明元素何时获得输入焦点
- [`Unfocused`](xref:Xamarin.Forms.VisualElement.Unfocused) 事件：指明元素何时失去输入焦点
- 选择键盘

### <a name="choosing-the-keyboard"></a>派生 `Entry` 和 `Editor` 的 [`InputView`](xref:Xamarin.Forms.InputView) 类只定义了一个属性：

[`Keyboard`](xref:Xamarin.Forms.Keyboard) 类型的 [`Keyboard`](xref:Xamarin.Forms.InputView.Keyboard)

- 这指明显示哪种类型的键盘。

一些键盘更适合输入 URI 或数字。 使用 `Keyboard` 类，可以使用静态 [`Keyboard.Create`](xref:Xamarin.Forms.Keyboard.Create(Xamarin.Forms.KeyboardFlags)) 方法定义键盘，此方法包含 [`KeyboardFlags`](xref:Xamarin.Forms.KeyboardFlags) 类型的参数，这一类型又是包含以下位标志的枚举：

`None`：设置为 0

- [`CapitalizeSentence`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeSentence)：设置为 1
- [`Spellcheck`](xref:Xamarin.Forms.KeyboardFlags.Spellcheck)：设置为 2
- [`Suggestions`](xref:Xamarin.Forms.KeyboardFlags.Suggestions)：设置为 4
- [`All`](xref:Xamarin.Forms.KeyboardFlags.All)：设置为 \xFFFFFFFF
- 如果在需要一段或多段文本时使用多行 [`Editor`](xref:Xamarin.Forms.Editor)，最好调用 `Keyboard.Create` 来选择键盘。

对于单行 [`Entry`](xref:Xamarin.Forms.Entry)，`Keyboard` 的以下静态只读属性非常有用： [`Numeric`](xref:Xamarin.Forms.Keyboard.Numeric)：用于带或不带小数点的正数。

- [`Default`](xref:Xamarin.Forms.Keyboard.Default)
- [`Text`](xref:Xamarin.Forms.Keyboard.Text)
- [`Chat`](xref:Xamarin.Forms.Keyboard.Chat)
- [`Url`](xref:Xamarin.Forms.Keyboard.Url)
- [`Email`](xref:Xamarin.Forms.Keyboard.Email)
- [`Telephone`](xref:Xamarin.Forms.Keyboard.Telephone)
- [`KeyboardTypeConverter`](xref:Xamarin.Forms.KeyboardTypeConverter) 允许在 XAML 中指定这些属性，如 [EntryKeyboards](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/EntryKeyboards) 程序所示。

条目属性和事件

### <a name="entry-properties-and-events"></a>单行 [`Entry`](xref:Xamarin.Forms.Entry) 定义了以下属性：

`string` 类型的 [`Text`](xref:Xamarin.Forms.InputView.Text)：`Entry` 中显示的文本

- `Color` 类型的 [`TextColor`](xref:Xamarin.Forms.InputView.TextColor)
- `string` 类型的 [`FontFamily`](xref:Xamarin.Forms.Entry.FontFamily)
- `double` 类型的 [`FontSize`](xref:Xamarin.Forms.Entry.FontSize)
- `FontAttributes` 类型的 [`FontAttributes`](xref:Xamarin.Forms.Entry.FontAttributes)
- `bool` 类型的 [`IsPassword`](xref:Xamarin.Forms.Entry.IsPassword)：导致字符被掩码
- `string` 类型的 [`Placeholder`](xref:Xamarin.Forms.InputView.Placeholder)：表示在键入任何内容前 `Entry` 中显示的颜色暗淡文本
- `Color` 类型的 [`PlaceholderColor`](xref:Xamarin.Forms.InputView.PlaceholderColor)
- `Entry` 还定义了两个事件：

包含 [`TextChangedEventArgs`](xref:Xamarin.Forms.TextChangedEventArgs) 对象的 [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged)：在 `Text` 属性更改时触发

- [`Completed`](xref:Xamarin.Forms.Entry.Completed)：在用户完成并关闭键盘时触发。
- 用户以特定于平台的方式指明完成 [QuadraticEquations](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/QuadaticEquations) 示例展示了这两个事件。

编辑器区别

### <a name="the-editor-difference"></a>多行 [`Editor`](xref:Xamarin.Forms.Editor) 定义了与 `Entry` 相同的 `Text` 和 `Font` 属性，但没有定义其他属性。

`Editor` 还定义了与 `Entry` 相同的两个属性。 [JustNotes](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/JustNotes) 是保存和还原 `Editor` 内容的自由格式笔记程序。

SearchBar

### <a name="the-searchbar"></a>由于 [`SearchBar`](xref:Xamarin.Forms.SearchBar) 不是派生自 `InputView`，因此它没有 `Keyboard` 属性。

但它确实有 `Entry` 定义的全部 `Text`、`Font` 和 `Placeholder` 属性。 此外，`SearchBar` 还定义了其他三个属性： `Color` 类型的 [`CancelButtonColor`](xref:Xamarin.Forms.SearchBar.CancelButtonColor)

- [`ICommand`](xref:System.Windows.Input.ICommand) 类型的 [`SearchCommand`](xref:Xamarin.Forms.SearchBar.SearchCommand)：用于与数据绑定和 MVVM 一起使用
- `Object` 类型的 [`SearchCommandParameter`](xref:Xamarin.Forms.SearchBar.SearchCommandParameter)：用于与 `SearchCommand` 一起使用
- 特定于平台的“取消”按钮清除文本。

`SearchBar` 还有特定于平台的“搜索”按钮。 按下其中一个按钮会触发 `SearchBar` 定义的两个事件之一： [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged)：附带了 [`TextChangedEventArgs`](xref:Xamarin.Forms.TextChangedEventArgs) 对象

- [SearchBarDemo](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SearchBarDemo) 示例展示了 `SearchBar`。
- [`SearchButtonPressed`](xref:Xamarin.Forms.SearchBar.SearchButtonPressed)

日期和时间选择

## <a name="date-and-time-selection"></a>[`DatePicker`](xref:Xamarin.Forms.DatePicker) 和 [`TimePicker`](xref:Xamarin.Forms.TimePicker) 视图实现特定于平台的控件，允许用户指定日期或时间。

DatePicker

### <a name="the-datepicker"></a>[`DatePicker`](xref:Xamarin.Forms.DatePicker) 定义了四个属性：

`DateTime` 类型的 [`MinimumDate`](xref:Xamarin.Forms.DatePicker.MinimumDate)：初始化为 1900 年 1 月 1 日

- `DateTime` 类型的 [`MaximumDate`](xref:Xamarin.Forms.DatePicker.MaximumDate)：初始化为 2100 年 12 月 31 日
- `DateTime` 类型的 [`Date`](xref:Xamarin.Forms.DatePicker.Date)：初始化为 `DateTime.Today`
- `string` 类型的 [`Format`](xref:Xamarin.Forms.DatePicker.Format)：初始化为“d”（短日期模式）的 .NET 格式设置字符串，在美国将日期显示为“7/20/1969”。
- 可以在 XAML 中设置 `DateTime` 属性，具体方法是将属性表达为属性元素，并使用区域性固定的短日期格式（“7/20/1969”）。

[DaysBetweenDates](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/DaysBetweenDates) 示例计算用户选择的两个日期之间的天数。   

TimePicker（还是 TimeSpanPicker？）

### <a name="the-timepicker-or-is-it-a-timespanpicker"></a>[`TimePicker`](xref:Xamarin.Forms.TimePicker) 定义了两个属性，没有定义事件：

`TimeSpan` 类型（而不是 `DateTime`）的 [`Time`](xref:Xamarin.Forms.TimePicker.Time)：表示从午夜起经过的时间

- `string` 类型的 [`Format`](xref:Xamarin.Forms.TimePicker.Format)：初始化为“t”（短时间模式）的 .NET 格式设置字符串，在美国将时间显示为“1:45 PM”。
- [SetTimer](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SetTimer) 程序展示了如何使用 `TimePicker` 指定计时器的时间。

此程序只有在前台时才能运行。 SetTimer 还展示了如何使用 `Page` 的 [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String)) 方法来显示警报框。

相关链接

## <a name="related-links"></a>[第 15 章全文 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch15-Apr2016.pdf)

- [第 15 章示例](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15)
- [滑块](~/xamarin-forms/user-interface/slider.md)
- [项](~/xamarin-forms/user-interface/text/entry.md)
- [编辑器](~/xamarin-forms/user-interface/text/editor.md)
- [DatePicker](~/xamarin-forms/user-interface/datepicker.md)
- RGB 滑块
