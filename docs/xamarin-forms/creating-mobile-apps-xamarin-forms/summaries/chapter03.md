---
title: “第 3 章： 深入了解文本”摘要
description: 使用 Xamarin.Forms 创建移动应用：“第 3 章： 深入了解文本”摘要
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 2E5581A6-4D3E-4BD5-9FDB-ACBA0F0FC734
author: davidbritch
ms.author: dabritch
ms.date: 07/18/2018
ms.openlocfilehash: 69415b59bbd376330454302981e3216c236a16bb
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/13/2020
ms.locfileid: "72980922"
---
# <a name="summary-of-chapter-3-deeper-into-text"></a>“第 3 章： 深入了解文本”摘要

[![下载示例](~/media/shared/download.png) 下载示例](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03)

本章节更深入地探讨了 [`Label`](xref:Xamarin.Forms.Label) 视图，包括颜色、字体和格式设置。

## <a name="wrapping-paragraphs"></a>段落换行

如果 `Label` 的 [`Text`](xref:Xamarin.Forms.Label.Text) 属性包含长文本，`Label` 会自动将它换行为多行，如 [Baskervilles  ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/Baskervilles) 示例所示。 可以嵌入 Unicode 代码（如表示长破折号的“\u2014”）或 C# 字符（如“\r'”）换到新行。

如果 `Label` 的 [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) 和 [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) 属性设置为 `LayoutOptions.Fill`，那么 `Label` 的总尺寸由其容器提供的可用空间决定。 `Label` 被认为是受约束的  。 `Label` 的尺寸就是它的容器尺寸。

如果 `HorizontalOptions` 和 `VerticalOptions` 属性设置为除 `LayoutOptions.Fill` 以外的值，那么 `Label` 的尺寸由文本呈现所需的空间决定，最大尺寸是其容器为 `Label` 提供的可用空间。 `Label` 被认为是不受约束的  ，它决定自己的尺寸。

（注意：术语“受约束”  和“不受约束”  可能是违反常理的，因为不受约束的视图通常小于受约束的视图。 此外，这两个术语与本电子图书前面章节中的用法不一致。）

`Label` 等视图可以在一个维度上受约束，而在另一个维度上不受约束。 `Label` 仅在水平方向受约束时才将文本换行为多行。

如果 `Label` 受约束，它可能会占用比文本呈现所需大得多的空间。 可以在 `Label` 的整个区域内定位文本。 将 [`HorizontalTextAlignment`](xref:Xamarin.Forms.Label.HorizontalTextAlignment) 属性设置为 [`TextAlignment`](xref:Xamarin.Forms.TextAlignment) 枚举的成员（[`Start`](xref:Xamarin.Forms.TextAlignment.Start)、[`Center`](xref:Xamarin.Forms.TextAlignment.Center) 或 [`End`](xref:Xamarin.Forms.TextAlignment.Center)），可以控制段落中所有行的对齐方式。 默认值为 `Start`，即左对齐文本。

将 [`VerticalTextAlignment`](xref:Xamarin.Forms.Label.VerticalTextAlignment) 属性设置为 `TextAlignment` 枚举的成员，可以将文本定位在 `Label` 所占区域的顶部、中间或底部。

将 [`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode) 属性设置为 [`LineBreakMode`](xref:Xamarin.Forms.LineBreakMode) 枚举的成员（[`WordWrap`](xref:Xamarin.Forms.LineBreakMode.WordWrap)、[`CharacterWrap`](xref:Xamarin.Forms.LineBreakMode.CharacterWrap)、[`NoWrap`](xref:Xamarin.Forms.LineBreakMode.NoWrap)、[`HeadTruncation`](xref:Xamarin.Forms.LineBreakMode.HeadTruncation)、[`MiddleTruncation`](xref:Xamarin.Forms.LineBreakMode.MiddleTruncation) 或 [`TailTruncation`](xref:Xamarin.Forms.LineBreakMode.TailTruncation)），可以控制段落中多行的换行或截断方式。

## <a name="text-and-background-colors"></a>文本颜色和背景色

将 `Label` 的 [`TextColor`](xref:Xamarin.Forms.Label.TextColor) 和 [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) 属性设置为 [`Color`](xref:Xamarin.Forms.Color) 值，可以控制文本颜色和背景色。

`BackgroundColor` 应用于 `Label` 所占整个区域的背景。 此尺寸可能比文本显示所需的区域大得多，具体视 `HorizontalOptions` 和 `VerticalOptions` 属性而定。 可以使用 Color 来试验 `HorizontalOptions`、`VerticalOptions`、`HorizontalExeAlignment` 和 `VerticalTextAlignment` 的不同值，看看它们对 `Label` 的尺寸和位置以及文本在 `Label` 中的尺寸和位置有何影响。

## <a name="the-color-structure"></a>颜色结构

使用 [`Color`](xref:Xamarin.Forms.Color) 结构，可以将颜色指定为红-绿-蓝 (RGB) 值，或色调-饱和度-发光度 (HSL) 值，或颜色名称。 还可以使用 alpha 通道来指明透明度。

使用 `Color` 构造函数可以指定：

- [灰色阴影](xref:Xamarin.Forms.Color.%23ctor(System.Double))
- [RGB 值](xref:Xamarin.Forms.Color.%23ctor(System.Double,System.Double,System.Double))
- [包含透明度的 RGB 值](xref:Xamarin.Forms.Color.%23ctor(System.Double,System.Double,System.Double,System.Double))

参数是介于 0 和 1 范围之间的 `double` 值。

还可以使用多个静态方法来创建 `Color` 值：

- [`Color.FromRgb`](xref:Xamarin.Forms.Color.FromRgb(System.Double,System.Double,System.Double))：用于创建介于 0 和 1 范围之间的 `double` RGB 值
- [`Color.FromRgb`](xref:Xamarin.Forms.Color.FromRgb(System.Int32,System.Int32,System.Int32))：用于创建介于 0 和 255 范围之间的整数 RGB 值
- [`Color.FromRgba`](xref:Xamarin.Forms.Color.FromRgba(System.Double,System.Double,System.Double,System.Double))：用于创建包含透明度的 `double` RGB 值
- [`Color.FromRgba`](xref:Xamarin.Forms.Color.FromRgba(System.Int32,System.Int32,System.Int32,System.Int32))：用于创建包含透明度的整数 RGB 值
- [`Color.FromHsla`](xref:Xamarin.Forms.Color.FromHsla(System.Double,System.Double,System.Double,System.Double))：用于创建包含透明度的 `double` HSL 值
- [`Color.FromUint`](xref:Xamarin.Forms.Color.FromUint(System.UInt32))：用于创建 `uint` 值，计算公式为 (B + 256 \* (G + 256 \* (R + 256 \* A)))
- [`Color.FromHex`](xref:Xamarin.Forms.Color.FromHex(System.String))：用于创建十六进制数字格式的 `string`，格式为“#AARRGGBB”、“#RRGGBB”、“#ARGB”或“#RGB”，其中每个字母分别对应 alpha 通道、红色通道、绿色通道和蓝色通道的十六进制数字。 此方法主要用于 XAML 颜色转换，如[第 7 章：XAML 与代码](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter07.md)中所述。

一旦创建，`Color` 值就不可变。 可以从下面的属性获取颜色特征：

- [`R`](xref:Xamarin.Forms.Color.R)
- [`G`](xref:Xamarin.Forms.Color.G)
- [`B`](xref:Xamarin.Forms.Color.B)
- [`A`](xref:Xamarin.Forms.Color.A)
- [`Hue`](xref:Xamarin.Forms.Color.Hue)
- [`Saturation`](xref:Xamarin.Forms.Color.Saturation)
- [`Luminosity`](xref:Xamarin.Forms.Color.Luminosity)

这些全都是介于 0 和 1 范围之间的 `double` 值。

`Color` 还为常见颜色定义了 240 个公共静态只读字段。 截至本电子图书撰写之时，只有 17 种常见颜色可供选择。

另一个公共静态只读字段定义了将所有颜色通道都设置为零的颜色：

- [`Color.Transparent`](xref:Xamarin.Forms.Color.Transparent)

多个实例方法允许通过修改现有颜色来新建颜色：

- [`AddLuminosity`](xref:Xamarin.Forms.Color.AddLuminosity(System.Double))
- [`MultiplyAlpha`](xref:Xamarin.Forms.Color.MultiplyAlpha(System.Double))
- [`WithHue`](xref:Xamarin.Forms.Color.WithHue(System.Double))
- [`WithLuminosity`](xref:Xamarin.Forms.Color.WithLuminosity(System.Double))
- [`WithSaturation`](xref:Xamarin.Forms.Color.WithSaturation(System.Double))

最后，两个静态只读属性定义了特殊颜色值：

- [`Color.Default`](xref:Xamarin.Forms.Color.Default)：所有通道都设置为 &ndash;1
- [`Color.Accent`](xref:Xamarin.Forms.Color.Accent)

`Color.Default` 旨在强制执行平台的配色方案，因此它的含义因平台和上下文而异。 默认情况下，平台的配色方案为：

- iOS：浅色背景上深色文本
- Android：深色背景上浅色文本（在图书中），或浅色背景上深色文本（在示例代码存储库的主  分支中通过 AppCompat 用于 Material Design）
- UWP：浅色背景上深色文本

`Color.Accent` 值生成特定于平台（有时为用户可选择）的颜色，它在深色或浅色背景上都是可见的。

## <a name="changing-the-application-color-scheme"></a>更改应用程序配色方案

各种平台都有默认配色方案，如上面的列表所示。

定目标到 Android 时，可以在 Android.Manifest.xml 文件中指定浅色主题，或按照[添加 AppCompat 和 Material Design](~/xamarin-forms/platform/android/appcompat-material-design.md) 说明操作，切换到浅色背景上深色文本的配色方案。

对于 Windows 平台，配色方案通常是由用户选择的，但你可以在平台的 App.xaml 文件中添加设置为 `Light` 或 `Dark` 的 `RequestedTheme` 特性。 默认情况下，UWP 项目中的 App.xaml 文件包含设置为 `Light` 的 `RequestedTheme` 特性。

## <a name="font-sizes-and-attributes"></a>字号和字体特性

将 `Label` 的 [`FontFamily`](xref:Xamarin.Forms.Label.FontFamily) 属性设置为“Times Roman”等字符串，可以选择字体系列。 不过，需要指定特定平台支持的字体系列，在这方面平台是不一致的。

将 `Label` 的 [`FontSize`](xref:Xamarin.Forms.Label.FontSize) 属性设置为 `double`，可以指定大致字体高度。 若要详细了解如何智能选择字号，请参阅[第 5 章：处理字号](chapter05.md)。

也可以从多个平台相关预设字号中选择一个。 静态 [`Device.GetNamedSize`](xref:Xamarin.Forms.Device.GetNamedSize(Xamarin.Forms.NamedSize,System.Type)) 方法和[重载](xref:Xamarin.Forms.Device.GetNamedSize(Xamarin.Forms.NamedSize,Xamarin.Forms.Element))都根据 [`NamedSize`](xref:Xamarin.Forms.NamedSize) 枚举的成员（[`Default`](xref:Xamarin.Forms.NamedSize.Default)、[`Micro`](xref:Xamarin.Forms.NamedSize.Micro)、[`Small`](xref:Xamarin.Forms.NamedSize.Small)、[`Medium`](xref:Xamarin.Forms.NamedSize.Medium) 和 [`Large`](xref:Xamarin.Forms.NamedSize.Large)）返回与平台相对应的 `double` 字号值。 从 `Medium` 成员返回的值不一定与 `Default` 相同。 [NamedFontSizes  ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/NamedFontSizes) 示例使用这些命名字号显示文本。

将 `Label` 的 [`FontAttributes`](xref:Xamarin.Forms.Label.FontAttributes) 属性设置为这些 [`FontAttributes`](xref:Xamarin.Forms.FontAttributes) 枚举的成员（[`Bold`](xref:Xamarin.Forms.FontAttributes.Bold)、[`Italic`](xref:Xamarin.Forms.FontAttributes.Italic) 或 [`None`](xref:Xamarin.Forms.FontAttributes.None)）。 可以将 `Bold` 和 `Italic` 成员与 C# 位 OR 运算符结合使用。

## <a name="formatted-text"></a>带格式文本

到目前为止，在所有示例中，`Label` 显示的整个文本已被统一格式化。 若要让文本字符串中的格式设置不同，请不要设置 `Label` 的 `Text` 属性。 相反，请将 [`FormattedText`](xref:Xamarin.Forms.Label.FormattedText) 属性设置为类型 [`FormattedString`](xref:Xamarin.Forms.FormattedString) 的对象。

`FormattedString` 有 [`Spans`](xref:Xamarin.Forms.FormattedString.Spans) 属性，此属性是 [`Span`](xref:Xamarin.Forms.Span) 对象集合。 每个 `Span` 对象都有自己的 [`Text`](xref:Xamarin.Forms.Span.Text)、[`FontFamily`](xref:Xamarin.Forms.Span.FontFamily)、[`FontSize`](xref:Xamarin.Forms.Span.FontSize)、[`FontAttributes`](xref:Xamarin.Forms.Span.FontAttributes)、[`ForegroundColor`](xref:Xamarin.Forms.Span.ForegroundColor) 和 [`BackgroundColor`](xref:Xamarin.Forms.Span.BackgroundColor) 属性。

[VariableFormattedText  ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/VarFormText) 示例展示了如何对单行文本使用 `FormattedText` 属性；[VariableFormattedParagraph  ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/VarFormPara) 展示了对整个段落使用的技术，如下所示：

[![可变格式化段落的三倍屏幕截图](images/ch03fg06-small.png "可变格式化标签文本")](images/ch03fg06-large.png#lightbox "可变格式化标签文本")

[NamedFontSizes  ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/NamedFontSizes) 程序使用一个 `Label` 和 `FormattedString` 对象显示每个平台的所有命名字号。

## <a name="related-links"></a>相关链接

- [第 3 章全文 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch03-Apr2016.pdf)
- [第 3 章示例](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03)
- [第 3 章 F# 示例](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/FS)
- [标签](~/xamarin-forms/user-interface/text/label.md)
- [使用颜色](~/xamarin-forms/user-interface/colors.md)
