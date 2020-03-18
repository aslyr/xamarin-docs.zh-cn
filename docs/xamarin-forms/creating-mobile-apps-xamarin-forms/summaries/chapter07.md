---
title: “第 7 章： XAML 与代码”摘要
description: 使用 Xamarin.Forms 创建移动应用：“第 7 章： XAML 与代码”摘要
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: E91F387B-CE90-481C-8D90-CB25519BFD2B
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
ms.openlocfilehash: ce4dde3716176daf826678809339afb84c25d84a
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/10/2020
ms.locfileid: "61334724"
---
# <a name="summary-of-chapter-7-xaml-vs-code"></a>“第 7 章： XAML 与代码”摘要

[![下载示例](~/media/shared/download.png) 下载示例](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07)

> [!NOTE]
> 此页上的“注意”指出了 Xamarin.Forms 与本电子图书中材料有出入的地方。

Xamarin.Forms 支持一种基于 XML 的标记语言，称为“Extensible Application Markup Language (XAML)”（读作“zammel”）。 若要定义 Xamarin.Forms 应用程序的用户界面布局，以及定义用户界面元素与基础数据之间的绑定，除了 C# 之外，还可以使用 XAML。

## <a name="properties-and-attributes"></a>属性和特性

Xamarin.Forms 类和结构成为 XAML 中的 XML 元素，这些类和结构的属性成为 XML 特性。 类通常必须有无参数公共构造函数，才能在 XAML 中进行实例化。 XAML 中设置的所有属性都必须有公共 `set` 访问器。

对于基本数据类型的属性（`string`、`double`、`bool` 等），XAML 分析程序使用标准 `TryParse` 方法，将特性设置转换为这些类型。 XAML 分析程序还可以轻松处理枚举类型，并能在枚举类型标记有 `Flags` 特性时合并枚举成员。

为了协助 XAML 分析程序，更复杂的类型（或这些类型的属性）可以包含 [`TypeConverterAttribute`](xref:Xamarin.Forms.TypeConverterAttribute)，它标识派生自支持从字符串值转换为这些类型的 [`TypeConverter`](xref:Xamarin.Forms.TypeConverter) 的类。 例如，[`ColorTypeConverter`](xref:Xamarin.Forms.ColorTypeConverter) 将颜色名称和字符串（如“#rrggbb”）转换为 `Color` 值。

## <a name="property-element-syntax"></a>属性元素语法

在 XAML 中，类以及用它们创建的对象表示为 XML 元素。 这些称为“对象元素”  。 这些对象的大多数属性都表示为 XML 特性。 这些称为“属性特性”  。

有时，必须将属性设置为无法用简单字符串表示的对象。 在这种情况下，XAML 支持称为“属性元素”  的标记，它是由用句点分隔的类名和属性名称组成。 然后，对象元素就可以出现在属性元素标记对中。

## <a name="adding-a-xaml-page-to-your-project"></a>向项目添加 XAML 页

Xamarin.Forms 可移植类库在首次创建时可以包含 XAML 页，你也可以将 XAML 页添加到现有项目。 在添加新项的对话框中，选择引用 XAML 页的项，或选择 `ContentPage` 和 XAML （而不是 `ContentView`）。

> [!NOTE]
> 自本章节撰写后，Visual Studio 选项已更改。

创建了两个文件：一个是文件扩展名为 .xaml 的 XAML 文件，另一个是扩展名为 .xaml.cs 的 C# 文件。 C# 文件通常是指 XAML 代码隐藏  文件。 代码隐藏文件是派生自 `ContentPage` 的分部类定义。 在生成时，不仅会分析 XAML，还会为相同的类生成另一个分部类定义。 生成的这个类包含 `InitializeComponent` 方法，它是从代码隐藏文件的构造函数中调用。

运行时期间，在 `InitializeComponent` 调用结束时，XAML 文件的所有元素都已实例化和初始化，就像已在 C# 代码中创建一样。

XAML 文件中的根元素是 `ContentPage`。 根标记包含至少两个 XML 命名空间声明，一个用于 Xamarin.Forms 元素，另一个用于为所有 XAML 实现的内部元素和特性定义 `x` 前缀。 根标记还包含 `x:Class` 特性，它指明了派生自 `ContentPage` 的类的命名空间和名称。 这与代码隐藏文件中的命名空间和类名一致。

[CodePlusXaml  ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07) 示例展示了如何结合使用 XAML 和代码。

## <a name="the-xaml-compiler"></a>XAML 编译器

Xamarin.Forms 包含 XAML 编译器，但要视需要根据 [`XamlCompilationAttribute`](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute) 的使用来择是否使用它。 如果 XAML 未编译，XAML 会在生成时得到分析，且 XAML 文件会嵌入 PCL 中，其中也是在运行时分析 XAML 文件。 如果 XAML 已编译，生成流程会将 XAML 转换为二进制格式，且运行时处理更高效。

## <a name="platform-specificity-in-the-xaml-file"></a>XAML 文件中的平台特征

在 XAML 中，[`OnPlatform`](xref:Xamarin.Forms.OnPlatform`1) 类可用于选择平台相关标记。 这是泛型类，必须使用与目标类型匹配的 `x:TypeArguments` 特性进行实例化。 [`OnIdiom`](xref:Xamarin.Forms.OnIdiom`1) 类与它类似，但使用频率要低得多。

自本书出版后，`OnPlatform` 的用法已更改。 最初，它是与 `iOS`、`Android` 和 `WinPhone` 属性一起使用。 现在，它是与 [`On`](xref:Xamarin.Forms.On) 子对象一起使用。 将 [`Platform`](xref:Xamarin.Forms.On.Platform) 属性设置为，与 [`Device`](xref:Xamarin.Forms.Device) 类的公共字段 `const` 一致的字符串。 将 [`Value`](xref:Xamarin.Forms.On.Value) 属性设置为，与 `OnPlatform` 标记的 `x:TypeArguments` 特性一致的值。

[ScaryColorList  ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07/ScaryColorList) 示例展示了 `OnPlatform`，之所以这样命名是因为它包含几乎完全相同的 XAML 块。 这种重复标记的存在表明应使用技术来减少这种情况。

## <a name="the-content-property-attributes"></a>内容属性特性

一些属性元素非常频繁地出现，如 `ContentPage` 的根元素上的 `<ContentPage.Content>` 标记，或围住 `StackLayout` 的子元素的 `<StackLayout.Children>` 标记。

每个类都允许在类上使用 [`ContentPropertyAttribute`](xref:Xamarin.Forms.ContentPropertyAttribute) 来标识一个属性。 对于此属性，属性元素标记不是必需的。 `ContentPage` 将它的内容属性定义为 `Content`，`Layout<T>`（`StackLayout` 派生自的类）将它的内容属性定义为 `Children`。 这些属性元素标记不是必需的。

`Label` 的属性元素是 `Text`。

## <a name="formatted-text"></a>带格式文本

[TextVariations  ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07/TextVariations) 示例举了多个例子来展示如何设置 `Label` 的 `Text` 和 `FormattedText` 属性。 在 XAML 中，`Span` 对象显示为 `FormattedString` 对象的子元素。

 当多行字符串设置为 `Text` 属性时，行尾字符转换为空格字符；但当多行字符串显示为 `Label` 或 `Label.Text` 标记的内容时，将保留行尾字符：

 [![文本变体共享的三倍屏幕截图](images/ch07fg03-small.png "格式化文本变体")](images/ch07fg03-large.png#lightbox "格式化文本变体")

## <a name="related-links"></a>相关链接

- [第 7 章全文 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch07-Apr2016.pdf)
- [第 7 章示例](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07)
- [第 7 章 F# 示例](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07/FS/CodePlusXaml)
- [XAML 基础知识](~/xamarin-forms/xaml/xaml-basics/index.md)
