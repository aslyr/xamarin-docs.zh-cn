---
title: Xamarin.Forms提货
description: Xamarin.Forms选取器将显示项目的简短列表，用户可从中选择项。 本文介绍如何使用选取器类从数据列表中选择文本项。
ms.prod: xamarin
ms.assetid: D4815A4B-104B-4294-951B-BD8F2EC33C86
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/20/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 30f5aefe5fcb327a7c3333bee8e8b553e2630f57
ms.sourcegitcommit: 08290d004d1a7e7ac579bf1f96abf8437921dc70
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/07/2020
ms.locfileid: "87918010"
---
# <a name="no-locxamarinforms-picker"></a>Xamarin.Forms提货

_"选取器" 视图是用于从数据列表中选择文本项的控件。_

Xamarin.Forms [`Picker`](xref:Xamarin.Forms.Picker) 显示项的简短列表，用户可从该列表中选择项。 `Picker` 定义以下属性:

- [`CharacterSpacing`](xref:Xamarin.Forms.Picker.CharacterSpacing)类型为的， `double` 它是所显示的项的字符之间的间距 `Picker` 。
- [`FontAttributes`](xref:Xamarin.Forms.Picker.FontAttributes)类型 [`FontAttributes`](xref:Xamarin.Forms.FontAttributes) 为的，默认为 [`FontAtributes.None`](xref:Xamarin.Forms.FontAttributes.None) 。
- [`FontFamily`](xref:Xamarin.Forms.Picker.FontFamily)类型 `string` 为的，默认为 `null` 。
- [`FontSize`](xref:Xamarin.Forms.Picker.FontSize)类型 `double` 为的，默认值为-1.0。
- `HorizontalTextAlignment`类型为的， [`TextAlignment`](xref:Xamarin.Forms.TextAlignment) 它是所显示的文本的水平对齐方式 `Picker` 。
- [`ItemsSource`](xref:Xamarin.Forms.Picker.ItemsSource)类型 `IList` 、要显示的项的源列表（默认为） `null` 。
- [`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex)对于类型 `int` ，则为选定项的索引，默认值为-1。
- [`SelectedItem`](xref:Xamarin.Forms.Picker.SelectedItem)类型为的 `object` 选定项，默认为 `null` 。
- [`TextColor`](xref:Xamarin.Forms.Picker.TextColor)类型的 [`Color`](xref:Xamarin.Forms.Color) ，用于显示文本的颜色，默认值为 [`Color.Default`](xref:Xamarin.Forms.Color.Default) 。
- [`Title`](xref:Xamarin.Forms.Picker.Title)类型 `string` 为的，默认为 `null` 。
- [`TitleColor`](xref:Xamarin.Forms.Picker.TitleColor)类型的 [`Color`](xref:Xamarin.Forms.Color) ，用于显示文本的颜色 `Title` 。
- `VerticalTextAlignment`类型为的， [`TextAlignment`](xref:Xamarin.Forms.TextAlignment) 它是所显示的文本的垂直对齐方式 `Picker` 。

所有属性都是由对象支持的 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) ，这意味着它们可以采用样式，属性可以是数据绑定的目标。 [`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex)和 [`SelectedItem`](xref:Xamarin.Forms.Picker.SelectedItem) 属性的默认绑定模式为 [`BindingMode.TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay) ，这意味着它们可以是使用[模型-视图-ViewModel (MVVM) ](~/xamarin-forms/enterprise-application-patterns/mvvm.md)体系结构的应用程序中数据绑定的目标。 有关设置字体属性的信息，请参阅[字体](~/xamarin-forms/user-interface/text/fonts.md)。

[`Picker`](xref:Xamarin.Forms.Picker)第一次显示时不显示任何数据。 相反，其属性的值 [`Title`](xref:Xamarin.Forms.Picker.Title) 在 iOS 和 Android 平台上显示为占位符：

[![初始选取器显示](images/picker-initial.png)](images/picker-initial-large.png#lightbox "初始选取器显示")

当 [`Picker`](xref:Xamarin.Forms.Picker) 获得焦点时，将显示其数据，用户可以选择项：

[![选择项的选取器](images/picker-selection.png)](images/picker-selection-large.png#lightbox "选择项的选取器")

[`Picker`](xref:Xamarin.Forms.Picker) [`SelectedIndexChanged`](xref:Xamarin.Forms.Picker.SelectedIndexChanged) 当用户选择某一项时，将引发一个事件。 选择后，将显示所选项目 `Picker` ：

![选择后选取器](images/picker-after-selection.png)

使用数据填充的方法有两种 [`Picker`](xref:Xamarin.Forms.Picker) ：

- 将 [`ItemsSource`](xref:Xamarin.Forms.Picker.ItemsSource) 属性设置为要显示的数据。 这是推荐采用的方法。 有关详细信息，请参阅[设置选取器的 System.windows.controls.itemscontrol.itemssource 属性](populating-itemssource.md)。
- 添加要显示到集合中的数据 [`Items`](xref:Xamarin.Forms.Picker.Items) 。 此方法是使用数据填充的原始过程 [`Picker`](xref:Xamarin.Forms.Picker) 。 有关详细信息，请参阅[将数据添加到选取器的项集合](populating-items.md)。

## <a name="related-links"></a>相关链接

- [选取器](xref:Xamarin.Forms.Picker)
