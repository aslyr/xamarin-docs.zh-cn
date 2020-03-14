---
title: Xamarin. Forms 公共控件属性、方法和事件
description: 本文介绍了在 VisualElement 类中定义的、通常用于派生类的常见属性、方法和事件。
ms.prod: xamarin
ms.assetId: 85A0CCF5-C1D8-40BB-927F-A4D944E5534D
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 08/21/2019
ms.openlocfilehash: 6d10e665c6461655440ddfb2c524cb56a14337f6
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/13/2020
ms.locfileid: "79305767"
---
# <a name="xamarinforms-common-control-properties-methods-and-events"></a>Xamarin. Forms 公共控件属性、方法和事件

Xamarin `VisualElement` 类是 Xamarin. Forms 应用程序中使用的大多数控件的基类。 `VisualElement` 类定义了许多用于派生类的[属性](#properties)、[方法](#methods)和[事件](#events)。

## <a name="properties"></a>属性

以下属性在 `VisualElement` 实例上可用。 有关完整列表，请参阅[VISUALELEMENT API Properties](xref:Xamarin.Forms.VisualElement#properties)。

### [`AnchorX`](xref:Xamarin.Forms.VisualElement.AnchorX)

`AnchorX` 属性是一个 `double` 值，它定义 X 轴上的中心点以实现变换（如缩放和旋转）。 默认值为0.5。

### [`AnchorY`](xref:Xamarin.Forms.VisualElement.AnchorY)

`AnchorY` 属性是一个 `double` 值，它定义 X 轴上的中心点以实现变换（如缩放和旋转）。 默认值为0.5。

### [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor)

`BackgroundColor` 属性是确定控件背景色的 `Color`。 如果设置，则背景将为默认 `Color` 对象，该对象呈现为透明。

### [`Behaviors`](xref:Xamarin.Forms.VisualElement.Behaviors)

`Behaviors` 属性是 `Behavior` 对象的 `List`。 利用行为，你可以通过将可重用的功能添加到 `Behaviors` 列表中，将其附加到元素。 有关 `Behavior` 类的详细信息，请参阅[Xamarin. Forms 行为](~/xamarin-forms/app-fundamentals/behaviors/index.md)。

### [`Bounds`](xref:Xamarin.Forms.VisualElement.Bounds)

`Bounds` 属性是一个只读 `Rectangle` 对象，表示控件占用的空间。 `Bounds` 属性值在布局周期期间分配。 `Rectangle` `struct` 包含有用的属性和方法来测试矩形的交集和包容。 有关详细信息，请参阅[Xamarin 矩形 API](xref:Xamarin.Forms.Rectangle)。

### [`Effects`](xref:Xamarin.Forms.Element.Effects)

`Effects` 属性是从 `Element`（x： Xamarin）类继承的 `Effect` 对象的 `List`。 效果允许自定义本机控件，通常用于小样式更改。 有关 `Effect` 类的详细信息，请参阅[Xamarin. Forms 效果](~/xamarin-forms/app-fundamentals/effects/index.md)。

### [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection)

`FlowDirection` 属性是 `FlowDirection` 枚举值。 流方向可以设置为 `MatchParent`、`LeftToRight`或 `RightToLeft`，并确定布局顺序和方向。 `FlowDirection` 属性通常用于支持从右向左阅读的语言。

### [`Height`](xref:Xamarin.Forms.VisualElement.Height)

`Height` 属性是一个只读 `double` 值，用于描述控件的呈现高度。 `Height` 属性是在布局周期中计算的，不能直接设置。 可以使用[HeightRequest 属性](#heightrequest)请求控件的高度。

### [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest)

`HeightRequest` 属性是一个 `double` 值，该值确定控件所需的高度。 控件的绝对高度可能与请求的值不匹配。 有关详细信息，请参阅[请求属性](#request-properties)。

### [`InputTransparent`](xref:Xamarin.Forms.VisualElement.InputTransparent)

`InputTransparent` 属性是一个确定控件是否接收用户输入的 `bool`。 默认值为 `false`，确保元素接收输入。 设置此属性时，会将其传输到子元素。 如果将 `InputTransparent` 属性设置为对 layout 类 `true`，将导致布局中的所有元素都不接收输入。

### [`IsEnabled`](xref:Xamarin.Forms.VisualElement.IsEnabled)

`IsEnabled` 属性是一个 `bool` 值，该值确定控件是否响应用户输入。 默认值为 `true`。 如果将此属性设置为 false，则控件不接受用户输入。

### [`IsFocused`](xref:Xamarin.Forms.VisualElement.IsFocused)

`IsFocused` 属性是一个 `bool` 值，该值描述控件当前是否为重点对象。 对控件调用[`Focus`](#focus)方法将导致将 `IsFocused` 值设置为 true。 调用[`Unfocus`](#unfocus)方法会将此属性设置为 false。

### [`IsTabStop`](xref:Xamarin.Forms.VisualElement.IsTabStop)

`IsTabStop` 属性是一个 `bool` 值，该值定义在用户使用 tab 键向前移动控件时，控件是否接收焦点。 如果此属性为 false，则 `TabIndex` 属性将不起作用。

### [`IsVisible`](xref:Xamarin.Forms.VisualElement.IsVisible)

`IsVisible` 属性是一个 `bool` 值，该值确定是否呈现控件。 将不会显示 "`IsVisible`" 属性设置为 "false" 的控件，并且不会将其视为布局周期中的空间计算，也不能接受用户输入。

### [`MinimumHeightRequest`](xref:Xamarin.Forms.VisualElement.MinimumHeightRequest)

`MinimumHeightRequest` 属性是一个 `double` 值，该值确定在两个元素争用有限空间时如何处理溢出。 设置 `MinimumHeightRequest` 属性允许布局过程将元素向下缩放到所请求的最小尺寸。 如果未指定 `MinimumHeightRequest`，则默认值为-1，并且布局过程会将 `HeightRequest` 视为最小值。 这意味着没有 `MinimumHeightRequest` 值的元素将不具有可缩放的高度。

有关详细信息，请参阅[最小请求属性](#minimum-request-properties)。

### [`MinimumWidthRequest`](xref:Xamarin.Forms.VisualElement.MinimumWidthRequest)

`MinimumWidthRequest` 属性是一个 `double` 值，该值确定在两个元素争用有限空间时如何处理溢出。 设置 `MinimumWidthRequest` 属性允许布局过程将元素向下缩放到所请求的最小尺寸。 如果未指定 `MinimumWidthRequest`，则默认值为-1，并且布局过程会将 `WidthRequest` 视为最小值。 这意味着没有 `MinimumWidthRequest` 值的元素将无法缩放宽度。

有关详细信息，请参阅[最小请求属性](#minimum-request-properties)。

### [`Opacity`](xref:Xamarin.Forms.VisualElement.Opacity)

`Opacity` 属性 `double` 是0到1之间的一个值，该值确定在呈现过程中控件的不透明度。 此属性的默认值为1.0。 范围在0到1范围内的值将为限制。 仅当[`IsVisible`](#isvisible)属性 `true`时，才应用 `Opacity` 属性。 不透明度是以迭代方式应用的。 因此，如果父控件具有0.5 不透明度，而其子控件具有0.5 不透明度，则该子控件将以有效的0.25 不透明度值呈现。 将输入控件的 `Opacity` 属性设置为0具有未定义的行为。

### [`Parent`](xref:Xamarin.Forms.Element.Parent)

`Parent` 属性继承自 `Element` 类。 此属性是一个 `Element` 对象，它是控件的父级。 如果将元素添加为另一个元素的子元素，则通常会在元素上自动设置 `Parent` 属性。

### [`Resources`](xref:Xamarin.Forms.VisualElement.Resources)

`Resources` 属性是一个使用键/值对填充的 `ResourceDictionary` 实例，这些键/值对通常在运行时通过 XAML 填充。 此字典允许应用程序开发人员在编译时和运行时重复使用 XAML 中定义的对象。 字典中的键是从 XAML 标记的 `x:Key` 特性填充的。 从 XAML 创建的对象将插入到指定键的 `ResourceDictionary` 中。 初始化后。

有关详细信息，请参阅[资源字典](~/xamarin-forms/xaml/resource-dictionaries.md)。

### [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation)

`Rotation` 属性是一个介于0到360之间的 `double` 值，用于定义围绕 Z 轴的旋转（以度为单位）。 此属性的默认值为 0。 相对于[`AnchorX`](#anchorx)和[`AnchorY`](#anchory)值应用旋转。

### [`RotationX`](xref:Xamarin.Forms.VisualElement.RotationX)

`RotationX` 属性是一个介于0到360之间的 `double` 值，用于定义围绕 X 轴的旋转（以度为单位）。 此属性的默认值为 0。 相对于[`AnchorX`](#anchorx)和[`AnchorY`](#anchory)值应用旋转。

### [`RotationY`](xref:Xamarin.Forms.VisualElement.RotationY)

`RotationY` 属性是0到360之间的一个 `double` 值，用于定义围绕 Y 轴的旋转（以度为单位）。 此属性的默认值为 0。 相对于[`AnchorX`](#anchorx)和[`AnchorY`](#anchory)值应用旋转。

### [`Scale`](xref:Xamarin.Forms.VisualElement.Scale)

`Scale` 属性是定义控件比例的 `double` 值。 此属性的默认值为1.0。 缩放是相对于[`AnchorX`](#anchorx)和[`AnchorY`](#anchory)值应用的。

### [`ScaleX`](xref:Xamarin.Forms.VisualElement.ScaleX)

`ScaleX` 属性是一个 `double` 值，它定义控件沿 X 轴的刻度。 此属性的默认值为1.0。 `ScaleX` 属性是相对于[`AnchorX`](#anchorx)值应用的。

### [`ScaleY`](xref:Xamarin.Forms.VisualElement.ScaleY)

`ScaleY` 属性是一个 `double` 值，它定义控件沿 Y 轴的刻度。 此属性的默认值为1.0。 `ScaleY` 属性是相对于[`AnchorY`](#anchory)值应用的。

### [`Style`](xref:Xamarin.Forms.NavigableElement.Style)

`Style` 属性继承自 `NavigableElement` 类。 此属性是 `Style` 类的实例。 `Style` 类包含触发器、setter 和行为，用于定义可视元素的外观和行为。 有关详细信息，请参阅[Xamarin. FORMS XAML 样式](~/xamarin-forms/user-interface/styles/xaml/index.md)。

### [`StyleClass`](xref:Xamarin.Forms.NavigableElement.StyleClass)

`StyleClass` 属性是 `string` 对象的列表，这些对象表示 `Style` 类的名称。 此属性继承自 `NavigableElement` 类。 `StyleClass` 属性允许将多个样式属性应用到 `VisualElement` 实例。 有关详细信息，请参阅[Xamarin. Forms 样式类](~/xamarin-forms/user-interface/styles/xaml/style-class.md)。

### [`TabIndex`](xref:Xamarin.Forms.VisualElement.TabIndex)

`TabIndex` 属性是一个 `int` 值，该值定义使用 tab 键向前滚动控件时的控件顺序。 `TabIndex` 属性是在 `VisualElement` 类实现的 `ITabStopElement` 接口上定义的属性的实现。

### [`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX)

`TranslationX` 属性是定义要在 X 轴上应用的增量转换的 `double` 值。 平移在布局之后应用，通常用于应用动画。 将元素转换为其父容器的边界外，我会阻止输入工作。

有关详细信息，请参阅[Xamarin 中的动画](~/xamarin-forms/user-interface/animation/index.md)。

### [`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY)

`TranslationY` 属性是定义要在 Y 轴上应用的增量转换的 `double` 值。 平移在布局之后应用，通常用于应用动画。 将元素转换为其父容器的边界外，我会阻止输入工作。

有关详细信息，请参阅[Xamarin 中的动画](~/xamarin-forms/user-interface/animation/index.md)。

### [`Triggers`](xref:Xamarin.Forms.VisualElement.Triggers)

`Triggers` 属性是 `TriggerBase` 对象的只读 `List`。 触发器允许应用程序开发人员在 XAML 中表达操作，这些操作更改控件的视觉外观，以响应事件或属性更改。 有关详细信息，请参阅[Xamarin。窗体触发器](~/xamarin-forms/app-fundamentals/triggers.md)。

### [`Visual`](xref:Xamarin.Forms.VisualElement.Visual)

`Visual` 属性是一个 `IVisual` 实例，它允许创建呈现器并选择性地将其应用到 `VisualElement` 实例中。 `Visual` 属性设置为与其父级匹配，因此在组件上定义呈现器也适用于该组件的任何子级。 如果未对控件或其上级设置自定义呈现器，则将使用默认的 Xamarin 呈现器。 有关详细信息，请参阅[Xamarin。 Forms 视觉对象](~/xamarin-forms/user-interface/visual/index.md)。

### [`Width`](xref:Xamarin.Forms.VisualElement.Width)

`Width` 属性是一个只读 `double` 值，用于描述控件的呈现宽度。 `Width` 属性是在布局周期中计算的，不能直接设置。 可以使用[WidthRequest 属性](#widthrequest)请求控件的宽度。

### [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest)

`WidthRequest` 属性是一个 `double` 值，该值确定控件所需的宽度。 控件的绝对宽度可能与请求的值不匹配。 有关详细信息，请参阅[请求属性](#request-properties)。

### [`X`](xref:Xamarin.Forms.VisualElement.X)

`X` 属性是一个只读 `double` 值，用于描述控件的当前 X 位置。

### [`Y`](xref:Xamarin.Forms.VisualElement.Y)

`Y` 属性是一个只读 `double` 值，用于描述控件的当前 Y 位置。

## <a name="methods"></a>方法

`VisualElement` 类中提供了以下方法。 有关完整列表，请参阅[VISUALELEMENT API 方法](xref:Xamarin.Forms.VisualElement#methods)。

### [`FindByName`](xref:Xamarin.Forms.Element.FindByName*)

`FindByName` 方法继承自 `Element` 类，并且具有以下签名：

```csharp
public object FindByName (string name)
```

此方法搜索所提供 `name` 参数的所有子元素，并返回具有指定名称的元素。 如果未发现匹配项，则返回 `null`。

### [`Focus`](xref:Xamarin.Forms.VisualElement.Focus)

`Focus` 方法尝试将焦点设置到元素上。 此方法具有以下签名：

```csharp
public bool Focus ()
```

如果成功设置了键盘焦点，则 `Focus` 方法返回 `true` 并且如果方法调用未导致焦点更改，则返回 `false`。 元素必须能够接收此方法工作的焦点。 对处于屏幕外或未实现的元素调用 `Focus` 方法具有未定义的行为。

### [`Unfocus`](xref:Xamarin.Forms.VisualElement.Unfocus)

`Unfocus` 方法尝试将焦点移到元素上。 此方法具有以下签名：

```csharp
public void Unfocus ()
```

元素必须已有此方法的焦点才能工作。

## <a name="events"></a>Events

`VisualElement` 类中提供了以下事件。 有关完整列表，请参阅[Xamarin VisualElement 事件](xref:Xamarin.Forms.VisualElement#events)。

### [`Focused`](xref:Xamarin.Forms.VisualElement.Focused)

只要 `VisualElement` 实例接收到焦点，就会引发 `Focused` 事件。 此事件不是通过 Xamarin stack 冒泡，而是直接从本机控件接收的。 此事件由[`IsFocused`](#isfocused)属性 setter 发出。

### [`SizeChanged`](xref:Xamarin.Forms.VisualElement.SizeChanged)

当 `VisualElement` 实例 `Height` 或 `Width` 属性发生更改时，将引发 `SizeChanged` 事件。 如果开发人员希望直接响应大小变化，而不是响应更改后事件，则应改为实现[`OnSizeAllocated`](xref:Xamarin.Forms.VisualElement.OnSizeAllocated*)虚方法。

### [`Unfocused`](xref:Xamarin.Forms.VisualElement.Unfocused)

只要 `VisualElement` 实例失去焦点，就会引发 `Unfocused` 事件。 此事件不是通过 Xamarin stack 冒泡，而是直接从本机控件接收的。 此事件由[`IsFocused`](#isfocused)属性 setter 发出。

## <a name="units-of-measurement"></a>度量单位

Android、iOS 和 UWP 平台都有不同的度量单位，它们在不同的设备上可能有所不同。 Xamarin 使用独立于平台的度量单位，在设备和平台之间规范化单元。 Xamarin 中每英寸有160单位，即每厘米64单位。

## <a name="request-properties"></a>请求属性

名称包含 "request" 的属性定义所需的值，这可能与实际呈现的值不匹配。 例如，`HeightRequest` 可能设置为150，但如果布局仅允许100单元空间，则该控件的呈现 `Height` 将仅为100。 呈现的大小受可用空间和包含的组件影响。

## <a name="minimum-request-properties"></a>最小请求属性

最小请求属性包括 `MinimumHeightRequest` 和 `MinimumWidthRequest`，旨在实现更精确地控制元素如何相互处理溢出。 但是，与这些属性相关的布局行为有一些重要的注意事项。

### <a name="unspecified-minimum-property-values"></a>未指定的最小属性值

如果未设置最小值，则 "最小值" 属性默认为-1。 布局过程将忽略此值，并将绝对值视为最小值。 此行为的实际结果是没有指定最小值的元素**将不会**收缩。 **将**收缩具有指定最小值的元素。

以下 XAML 显示了水平 `StackLayout`中的两个 `BoxView` 元素：

```xaml
<StackLayout Orientation="Horizontal">
    <BoxView HeightRequest="100" BackgroundColor="Purple" WidthRequest="500"></BoxView>
    <BoxView HeightRequest="100" BackgroundColor="Green" WidthRequest="500" MinimumWidthRequest="250"></BoxView>
</StackLayout>
```

第一个 `BoxView` 实例请求宽度为500，未指定最小宽度。 第二个 `BoxView` 实例请求宽度为500，最小宽度为250。 如果父 `StackLayout` 元素的宽度不足以满足其请求宽度500的两个组件，则布局过程将考虑第一个 `BoxView` 实例，因为没有指定其他有效的最小值。 允许第二个 `BoxView` 实例向下缩放到250，并将其缩小以适应其宽度（250个单位）。

如果所需行为适用于第一个 `BoxView` 实例，以便在没有最小宽度的情况下缩小，则必须将 `MinimumWidthRequest` 设置为一个有效的值，例如0。

### <a name="minimum-and-absolute-property-values"></a>最小和绝对属性值

如果最小值大于绝对值，则行为是不确定的。 例如，如果 `MinimumWidthRequest` 设置为100，则 `WidthRequest` 属性不应超过100。 指定最小属性值时，应始终指定一个绝对值，以确保绝对值大于最小值。

### <a name="minimum-properties-within-a-grid"></a>网格内的最小属性

`Grid` 布局具有自己的系统，用于相对调整行和列的大小。 使用 `Grid` 布局中的 `MinimumWidthRequest` 或 `MinimumHeightRequest` 不会产生效果。 有关详细信息，请参阅[Xamarin Grid](~/xamarin-forms/user-interface/layouts/grid.md)。

## <a name="related-links"></a>相关链接

* [VisualElement API 文档](xref:Xamarin.Forms.VisualElement)
