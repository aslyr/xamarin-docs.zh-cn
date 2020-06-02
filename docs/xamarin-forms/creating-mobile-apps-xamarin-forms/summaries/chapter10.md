---
title: ''
description: ''
Creating Mobile Apps with Xamarin.Forms: Summary of Chapter 10. XAML markup extensions''
ms.prod: ''
ms.technology: ''
ms.assetid: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 8f23034df684e778677e4f2e480e1c41807536fb
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/28/2020
ms.locfileid: "84136807"
---
# <a name="summary-of-chapter-10-xaml-markup-extensions"></a>摘要：第 10 章. XAML 标记扩展

[![下载示例](~/media/shared/download.png) 下载示例](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10)

通常，XAML 分析器会根据基本 .NET 数据类型的标准转换或通过 [`TypeConverterAttribute`](xref:Xamarin.Forms.TypeConverterAttribute) 附加到属性或其类型的 [`TypeConverter`](xref:Xamarin.Forms.TypeConverter) 派生，将设置为属性值的任何字符串转换为属性的类型。

但是有时从其他源（例如，字典中的项、静态属性或字段的值或从某种类型的计算）设置属性会非常方便。

这是 XAML 标记扩展执行的操作。 尽管名称如此，但 XAML 标记扩展并不是 XML 的扩展。 XAML 始终是合法的 XML。

## <a name="the-code-infrastructure"></a>代码基础结构

XAML 标记扩展是实现 [`IMarkupExtension`](xref:Xamarin.Forms.Xaml.IMarkupExtension) 接口的类。 这种类通常在其名称的末尾带有 `Extension` 一词，但通常在 XAML 中不带有该后缀。

XAML 的所有实现均支持以下 XAML 标记扩展：

- [`StaticExtension`](xref:Xamarin.Forms.Xaml.StaticExtension) 支持的 `x:Static`
- [`ReferenceExtension`](xref:Xamarin.Forms.Xaml.ReferenceExtension) 支持的 `x:Reference`
- [`TypeExtension`](xref:Xamarin.Forms.Xaml.TypeExtension) 支持的 `x:Type`
- [`NullExtension`](xref:Xamarin.Forms.Xaml.NullExtension) 支持的 `x:Null`
- [`ArrayExtension`](xref:Xamarin.Forms.Xaml.ArrayExtension) 支持的 `x:Array`

XAML 的许多实现都支持这四个 XAML 标记扩展，包括 Xamarin.Forms：

- [`StaticResourceExtension`](xref:Xamarin.Forms.Xaml.StaticResourceExtension) 支持的 `StaticResource`
- [`DynamicResourceExtension`](xref:Xamarin.Forms.Xaml.DynamicResourceExtension) 支持的 `DynamicResource`
- [`BindingExtension`](xref:Xamarin.Forms.Xaml.BindingExtension) 支持的 `Binding`&mdash;将在以下章节中进行探讨：[第 16 章数据绑定](chapter16.md)
- [`TemplateBindingExtension`](xref:Xamarin.Forms.Xaml.TemplateBindingExtension) 支持的 `TemplateBinding`&mdash;本书未涉及

Xamarin.Forms 中包含与 [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) 相关的附加 XAML 标记扩展：

- [`ConstraintExpression`](xref:Xamarin.Forms.ConstraintExpression)&mdash;本书未涉及

## <a name="accessing-static-members"></a>访问静态成员

使用 [`x:Static`](xref:Xamarin.Forms.Xaml.StaticExtension) 元素可将属性设置为公共静态属性、字段或枚举成员的值。 将 [`Member`](xref:Xamarin.Forms.Xaml.StaticExtension.Member) 属性设置为静态成员。 通常用大括号来指定 `x:Static` 和成员名称会更加容易。 不需要包含 `Member` 属性的名称，只需包含成员本身。 此常见语法如 [**SharedStatics**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/SharedStatics) 示例中所示。 静态字段本身在 [`AppConstants`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter10/SharedStatics/SharedStatics/SharedStatics/AppConstants.cs) 类中定义。 这种技术允许你建立通过程序使用的常量。

使用其他 XML 命名空间声明，你可以引用 .NET Framework 中定义的公共静态属性、字段或枚举成员，如 [SystemStatics](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/SystemStatics) 示例中所示。

## <a name="resource-dictionaries"></a>资源字典

`VisualElement` 类将定义一个名为 [`Resources`](xref:Xamarin.Forms.VisualElement.Resources) 的属性，可以将其设置为 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 类型的对象。 在 XAML 中，可以将项目存储在此字典中，并使用 `x:Key` 属性对其进行标识。 存储在资源字典中的项将在对该项的所有引用之间共享。

### <a name="staticresource-for-most-purposes"></a>StaticResource 适用于大多数情况

在大多数情况下，将使用 [`StaticResource`](xref:Xamarin.Forms.Xaml.StaticResourceExtension) 标记扩展来引用资源字典中的项，如 [ResourceSharing](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/ResourceSharing) 示例所示。 可以在大括号内使用 `StaticResourceExtension` 元素或 `StaticResource`：

[![资源共享的三倍屏幕截图](images/ch10fg03-small.png "资源共享")](images/ch10fg03-large.png#lightbox "资源共享")

请勿混淆 `x:Static` 标记扩展和 `StaticResource` 标记扩展。

### <a name="a-tree-of-dictionaries"></a>字典树

当 XAML 分析器遇到 `StaticResource` 时，它将开始在可视化树中搜索匹配键，然后在应用程序的 `App` 类中查找 `ResourceDictionary`。 这样，位于可视化树中较深层的资源字典中的项可覆盖位于可视化树中较高层的资源字典。 [ResourceTrees](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/ResourceTrees) 示例对此进行了演示。

### <a name="dynamicresource-for-special-purposes"></a>DynamicResource 适用于特殊情况

在 `InitializeComponent` 调用期间生成可视化树时，`StaticResource` 标记扩展会导致从字典中检索项。 `StaticResource` 的一种替代方法是 [`DynamicResource`](xref:Xamarin.Forms.Xaml.DynamicResourceExtension)，这种方法可维护字典键的链接，并在键引用的项更改时更新目标。

[DynamicVsStatic](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/DynamicVsStatic) 示例说明了 `StaticResource` 和 `DynamicResource` 之间的区别。

由 `DynamicResource` 设置的属性必须由可绑定属性提供支持，如[第 11 章，可绑定基础结构](chapter11.md)中所述。

## <a name="lesser-used-markup-extensions"></a>较少使用的标记扩展

使用 [`x:Null`](xref:Xamarin.Forms.Xaml.NullExtension) 标记扩展将属性设置为 `null`。

使用 [`x:Type`](xref:Xamarin.Forms.Xaml.TypeExtension) 标记扩展将属性设置为 .NET `Type` 对象。

使用 [`x:Array`](xref:Xamarin.Forms.Xaml.ArrayExtension) 定义一个数组。 通过将 [`Type`] 属性设置为 `x:Type` 标记扩展来指定数组成员的类型。

## <a name="a-custom-markup-extension"></a>自定义标记扩展

通过使用 [`ProvideValue`](xref:Xamarin.Forms.Xaml.IMarkupExtension.ProvideValue(System.IServiceProvider)) 方法编写实现 [`IMarkupExtension`](xref:Xamarin.Forms.Xaml.IMarkupExtension) 接口的类，可以创建自己的 XAML 标记扩展。

[`HslColorExtension`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/HslColorExtension.cs) 类满足以下要求。 它基于名为 `H`、`S``L` 和 `A` 的属性值创建类型为 `Color` 的值。 此类是名为 [Xamarin.FormsBook.Toolkit](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 的 Xamarin.Forms 库中的第一项，它在本书中构建，并在本书中使用。

[CustomExtensionDemo](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/CustomExtensionDemo) 示例演示了如何引用此库并使用自定义标记扩展。

## <a name="related-links"></a>相关链接

- [第 10 章全文 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch10-Apr2016.pdf)
- [第 10 章示例](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10)
- [XAML 标记扩展](~/xamarin-forms/xaml/markup-extensions/index.md)
