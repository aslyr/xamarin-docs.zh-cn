---
title: 摘要：第 12 章. 样式
description: 使用 Xamarin.Forms 创建移动应用：摘要：第 12 章. 样式
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 3EAE6BDC-8EFB-464B-A87B-1C35B8387BB3
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
ms.openlocfilehash: 408f171a3c7c690b700f7be21a3dcaff503467d9
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/13/2020
ms.locfileid: "65926908"
---
# <a name="summary-of-chapter-12-styles"></a>摘要：第 12 章. 样式

[![下载示例](~/media/shared/download.png) 下载示例](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12)

在 Xamarin.Forms 中，使用样式，多个视图可共享属性设置的集合。 这样可以减少标记，并能够保持一致的视觉主题。

样式几乎总在标记中定义和使用。 在资源字典中实例化类型为 [`Style`](xref:Xamarin.Forms.Style) 的对象，然后使用 `StaticResource` 或 `DynamicResource` 标记扩展设置为可视元素的 [`Style`](xref:Xamarin.Forms.NavigableElement.Style) 属性。

## <a name="the-basic-style"></a>基本样式

`Style` 要求将其 [`TargetType`](xref:Xamarin.Forms.Style.TargetType) 设置为其所应用的视觉对象的类型。 在资源字典中实例化 `Style` 时（很常见），它还需要一个 `x:Key` 属性。

`Style` 具有类型为 [`Setters`](xref:Xamarin.Forms.Style.Setters) 的内容属性，该属性是 [`Setter`](xref:Xamarin.Forms.Setter) 对象的集合。 每个 `Setter` 将 [`Property`](xref:Xamarin.Forms.Setter.Property) 与 [`Value`](xref:Xamarin.Forms.Setter.Value) 关联。

在 XAML 中，`Property` 设置是 CLR 属性的名称（例如 `Button` 的 `Text` 属性），但是带样式的属性必须由可绑定属性提供支持。 同样，该属性必须在 `TargetType` 设置指示的类中定义，或者由该类继承。

可以使用属性元素 `<Setter.Value>` 指定 `Value` 设置。 这样，用户可以将 `Value` 设置为无法以文本字符串中表示的对象，或设置为 `OnPlatform` 对象或设置为使用 `x:Arguments` 或 `x:FactoryMethod` 实例化的对象。 也可以使用 `StaticResource` 表达式将 `Value` 属性设置为字典中的其他项。

[BasicStyle  ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/BasicStyle) 程序演示了基本语法，并显示了如何使用 `StaticResource` 标记扩展来引用 `Style`：

[![基本样式的三倍屏幕截图](images/ch12fg01-small.png "基本样式")](images/ch12fg01-large.png#lightbox "基本样式")

`Style` 对象和任何在 `Style` 对象中作为 `Value` 设置创建的对象在引用该 `Style` 的所有视图之间共享。 `Style` 不能包含任何无法共享的内容，例如 `View` 导数。

无法在 `Style` 中设置事件处理程序。 无法在 `Style` 中设置 `GestureRecognizers` 属性，因为它不受可绑定属性支持。

## <a name="styles-in-code"></a>代码中的样式

尽管不常见，但你可以在代码中实例化和初始化 `Style` 对象。 [BasicStyleCode  ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/BasicStyleCode) 示例对此进行了演示。

## <a name="style-inheritance"></a>样式继承

`Style` 具有 [`BasedOn`](xref:Xamarin.Forms.Style.BasedOn) 属性，可以将其设置为引用其他样式的 `StaticResource` 标记扩展。 这样，允许从以前的样式继承样式，并添加或替换属性设置。 [StyleInheritance  ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/StyleInheritance) 示例对此进行了演示。

如果 `Style2` 基于 `Style1`，则 `Style2` 的 `TargetType` 必须与 `Style1` 相同或派生自 `Style1`。 存储 `Style1` 的资源字典必须与 `Style2` 是相同的资源字典，或者是可视化树中更高的资源字典。

## <a name="implicit-styles"></a>隐式样式

如果资源字典中的 `Style` 没有 `x:Key` 属性设置，则会自动为其分配字典键，并且 `Style` 对象将成为隐式样式  。 如果视图没有 `Style` 设置且其类型与 `TargetType` 完全匹配，则该视图适用于该样式，如 [ImplicitStyle  ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/ImplicitStyle) 示例所示。

隐式样式可以从具有 `x:Key` 设置的 `Style` 派生，但不能反向派生。 不能显式引用隐式样式。

可以使用样式和 `BasedOn` 实现三种类型的层次结构：

- 从在 `Application` 和 `Page` 上定义的样式到在可视化树中较低布局上定义的样式。
- 从为 `VisualElement` 和 `View` 等基类定义的样式到为特定类定义的样式。
- 从具有显式词典键的样式到隐式样式。

这些层次结构在 [StyleHierarchy  ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/StyleHierarchy) 示例中进行了演示。

## <a name="dynamic-styles"></a>动态样式

资源字典中的样式可以由 `DynamicResource` 而不是 `StaticResource` 引用。 这会使样式成为动态样式  。 如果该样式在资源字典中被另一个具有相同键的样式替换，则使用 `DynamicResource` 引用该样式的视图将自动更改。 此外，缺少具有指定键的字典项将导致 `StaticResource` 引发异常，但不会引发 `DynamicResource`。

可以使用此方法动态更改样式或主题，如 [DynamicStyles  ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/DynamicStyles) 示例所示。

但是，不能将 `BasedOn` 属性设置为 `DynamicResource` 标记扩展，因为 `BasedOn` 不受可绑定属性支持。 若要动态派生样式，请不要设置 `BasedOn`。 而改为将 [`BaseResourceKey`](xref:Xamarin.Forms.Style.BaseResourceKey) 属性设置为要派生样式的字典键。 [DynamicStylesInheritance  ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/DynaStylesInh) 示例对此方法进行了演示。

## <a name="device-styles"></a>设备样式

[`Device.Styles`](xref:Xamarin.Forms.Device.Styles) 嵌套类使用 `Label` 的 `TargetType` 为六个样式定义了十二个静态只读字段，可用于常见类型的文本用法。

其中六个字段的类型为 `Style`，可以在代码中直接将其设置为 `Style` 属性：

- [`BodyStyle`](xref:Xamarin.Forms.Device.Styles.BodyStyle)
- [`TitleStyle`](xref:Xamarin.Forms.Device.Styles.TitleStyle)
- [`SubtitleStyle`](xref:Xamarin.Forms.Device.Styles.SubtitleStyle)
- [`CaptionStyle`](xref:Xamarin.Forms.Device.Styles.CaptionStyle)
- [`ListItemTextStyle`](xref:Xamarin.Forms.Device.Styles.ListItemTextStyle)
- [`ListItemDetailTextStyle`](xref:Xamarin.Forms.Device.Styles.ListItemDetailTextStyle)

其他六个字段的类型为 `string`，可用作动态样式的字典键：

- [`BodyStyleKey`](xref:Xamarin.Forms.Device.Styles.BodyStyleKey) 等于“BodyStyle”
- [`TitleStyleKey`](xref:Xamarin.Forms.Device.Styles.TitleStyleKey) 等于“TitleStyle”
- [`SubtitleStyleKey`](xref:Xamarin.Forms.Device.Styles.SubtitleStyleKey) 等于“SubtitleStyle”
- [`CaptionStyleKey`](xref:Xamarin.Forms.Device.Styles.CaptionStyleKey) 等于“CaptionStyle”
- [`ListItemTextStyleKey`](xref:Xamarin.Forms.Device.Styles.ListItemTextStyleKey) 等于“ListItemTextStyle”
- [`ListItemDetailTextStyleKey`](xref:Xamarin.Forms.Device.Styles.ListItemDetailTextStyleKey) 等于“ListItemDetailTextStyle”

[DeviceStylesList  ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/DeviceStylesList) 示例对这些样式进行了演示。

## <a name="related-links"></a>相关链接

- [第 12 章全文 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch12-Apr2016.pdf)
- [第 12 章示例](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12)
- [样式](~/xamarin-forms/user-interface/styles/index.md)
