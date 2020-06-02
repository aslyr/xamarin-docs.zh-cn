---
title: ''
description: ''
Creating Mobile Apps with Xamarin.Forms: Summary of Chapter 23. Triggers and behaviors''
ms.prod: ''
ms.technology: ''
ms.assetid: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 9a0206354254f79756e29f834c85837240736eca
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/28/2020
ms.locfileid: "84136651"
---
# <a name="summary-of-chapter-23-triggers-and-behaviors"></a>“第 23 章： 触发器和行为”摘要

[![下载示例](~/media/shared/download.png) 下载示例](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23)

触发器和行为是类似的，因为它们都用于 XAML 文件，以简化元素交互（除了使用数据绑定之外），并扩展 XAML 元素的功能。 触发器和行为几乎总是与可视化用户界面对象一起使用。

为了支持触发器和行为，`VisualElement` 和 `Style` 支持两个集合属性：

- `IList<TriggerBase>` 类型的 [`VisualElement.Triggers`](xref:Xamarin.Forms.VisualElement.Triggers) 和 [`Style.Triggers`](xref:Xamarin.Forms.Style.Triggers)
- `IList<Behavior>` 类型的 [`VisualElement.Behaviors`](xref:Xamarin.Forms.VisualElement.Behaviors) 和 [`Style.Behaviors`](xref:Xamarin.Forms.Style.Behaviors)

## <a name="triggers"></a>触发器

触发器是一种条件（属性更改或触发事件），可生成响应（另一个属性更改或运行某代码）。 `VisualElement` 和 `Style` 的 `Triggers` 属性的类型为 `IList<TriggersBase>`。 [`TriggerBase`](xref:Xamarin.Forms.TriggerBase) 是抽象类，以下四个密封类由其派生而来：

- [`Trigger`](xref:Xamarin.Forms.Trigger)：用于基于属性更改的响应
- [`EventTrigger`](xref:Xamarin.Forms.EventTrigger)：用于基于事件触发的响应
- [`DataTrigger`](xref:Xamarin.Forms.DataTrigger)：用于基于数据绑定的响应
- [`MultiTrigger`](xref:Xamarin.Forms.MultiTrigger)：用于基于多个触发器的响应

触发器总是对其属性由触发器更改的元素设置。

### <a name="the-simplest-trigger"></a>最简单的触发器

[`Trigger`](xref:Xamarin.Forms.Trigger) 类检查属性值是否更改，并通过设置相同元素的另一个属性来响应。

`Trigger` 定义了三个属性：

- `BindableProperty` 类型的 [`Property`](xref:Xamarin.Forms.Trigger.Property)
- `Object` 类型的 [`Value`](xref:Xamarin.Forms.Trigger.Value)
- `IList<SetterBase>` 类型的 [`Setters`](xref:Xamarin.Forms.Trigger.Setters)：`Trigger` 的内容属性

此外，`Trigger` 要求设置以下继承自 `TriggerBase` 的属性：

- [`TargetType`](xref:Xamarin.Forms.TriggerBase.TargetType)：指明 `Trigger` 附加到的元素的类型

`Property` 和 `Value` 组成了条件，`Setters` 集合是响应。 如果指明的 `Property` 有 `Value` 指明的值，就会应用 `Setters` 集合中的 `Setter` 对象。 如果 `Property` 有其他值，就会删除 Setters。 `Setter` 定义了两个属性，与 `Trigger` 的前两个属性相同：

- `BindableProperty` 类型的 [`Property`](xref:Xamarin.Forms.Setter.Property)
- `Object` 类型的 [`Value`](xref:Xamarin.Forms.Setter.Value)

[EntryPop](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EntryPop) 示例展示了应用于 `Entry` 的 `Trigger` 如何在 `Entry` 的 `IsFocused` 属性为 `true` 时通过 `Scale` 属性增加 `Entry` 的大小。

可以在代码中设置 `Trigger`（如 [EntryPopCode](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EntryPopCode) 示例所示），尽管并不常见。

[StyledTriggers](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/StyledTriggers) 示例展示了如何在 `Style` 中设置 `Trigger` 以应用于多个 `Entry` 元素。

### <a name="trigger-actions-and-animations"></a>触发器操作和动画

还可以根据触发器运行一小段代码。 此代码可以是定目标到属性的动画。 一种常见的方法是使用 [`EventTrigger`](xref:Xamarin.Forms.EventTrigger)，它定义了两个属性：

- `string` 类型的 [`Event`](xref:Xamarin.Forms.EventTrigger.Event)：事件名称
- `IList<TriggerAction>` 类型的 [`Actions`](xref:Xamarin.Forms.EventTrigger.Actions)：要在响应中运行的操作列表。

若要使用此类，需要编写派生自 [`TriggerAction<T>`](xref:Xamarin.Forms.TriggerAction`1) 的类（通常为 `TriggerAction<VisualElement>`）。 可以在此类中定义属性。 这些是普通 CLR 属性，而不是可绑定属性，因为 `TriggerAction` 不是派生自 `BindableObject`。 必须重写在操作调用时调用的 [`Invoke`](xref:Xamarin.Forms.TriggerAction`1.Invoke*) 方法。 参数是目标元素。

以 [Xamarin.FormsBook.Toolkit](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 库中的 [`ScaleAction`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ScaleAction.cs) 类为例。 它调用 `ScaleTo` 属性来为元素的 `Scale` 属性添加动画效果。 由于其中一个属性的类型为 `Easing`，因此 [`EasingConverter`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/EasingConverter.cs) 类可便于在 XAML 中使用标准 `Easing` 静态字段。

[EntrySwell](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EntrySwell) 示例展示了如何从监视 `Focused` 和 `Unfocused` 事件的 `EventTrigger` 对象调用 `ScaleAction`。

[CustomEasingSwell](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/CustomEasingSwell) 示例展示了如何在代码隐藏文件中为 `ScaleAction` 定义自定义缓动函数。

也可以使用 `Trigger`（与 `EventTrigger` 不同）调用操作。 这就要求知道 `TriggerBase` 定义了两个集合：

- `IList<TriggerAction>` 类型的 [`EnterActions`](xref:Xamarin.Forms.TriggerBase.EnterActions)
- `IList<TriggerAction>` 类型的 [`ExitActions`](xref:Xamarin.Forms.TriggerBase.ExitActions)

[EnterExitSwell](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EnterExitSwell) 示例展示了如何使用这些集合。

### <a name="more-event-triggers"></a>更多事件触发器

[Xamarin.FormsBook.Toolkit](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 库中的 [`ScaleUpAndDownAction`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ScaleUpAndDownAction.cs) 类调用 `ScaleTo` 两次可进行纵向扩展和缩减。 [ButtonGrowth](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ButtonGrowth) 示例在样式化 `EventTrigger` 中使用此类，以在用户按下 `Button` 时提供视觉反馈。 这种双重动画也可以通过在 [`DelayedScaleAction`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/DelayedScaleAction.cs) 类型的集合中使用两个操作来实现

Xamarin.FormsBook.Toolkit 库中的 [`ShiverAction`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ShiverAction.cs) 类定义了可自定义的抖动操作。 [ShiverButtonDemo](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ShiverButtonDemo) 示例对此进行了展示。

Xamarin.FormsBook.Toolkit 库中的 [`NumericValidationAction`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NumericValidationAction.cs) 类仅限于 `Entry` 元素，并在 `Text` 属性不是 `double` 时将 `TextColor` 属性设置为红色。 [TriggerEntryValidation](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/TriggerEntryValidation) 示例对此进行了展示。

### <a name="data-triggers"></a>数据触发器

[`DataTrigger`](xref:Xamarin.Forms.DataTrigger) 类似于 `Trigger`，区别在于前者不监视属性值更改，而是监视数据绑定。 这样一来，一个元素中的属性就能影响另一个元素中的属性。

`DataTrigger` 定义了三个属性：

- `BindingBase` 类型的 [`Binding`](xref:Xamarin.Forms.DataTrigger.Binding)
- `Object` 类型的 [`Value`](xref:Xamarin.Forms.DataTrigger.Value)
- `IList<SetterBase>` 类型的 [`Setters`](xref:Xamarin.Forms.DataTrigger.Setters)

[GenderColors](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/GenderColors) 示例需要 [SchoolOfFineArt](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/SchoolOfFineArt) 库，并根据 `Sex` 属性将学生姓名的颜色设置为蓝色或粉色：

[![性别颜色的三倍屏幕截图](images/ch23fg04-small.png "性别颜色")](images/ch23fg04-large.png#lightbox "性别颜色")

[ButtonEnabler](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ButtonEnabler) 示例在 `Entry` 的 `Text` 属 性的 `Length` 属性等于 0 时，将 `Entry` 的 `IsEnabled` 属性设置为 `False`。 请注意，`Text` 属性初始化为空字符串；它默认为 `null`，`DataTrigger` 无法正常运行。

### <a name="combining-conditions-in-the-multitrigger"></a>在 MultiTrigger 中合并条件

[`MultiTrigger`](xref:Xamarin.Forms.MultiTrigger) 是条件集合。 如果条件都为 `true`，就会应用 Setters。 此类定义了两个属性：

- `IList<Condition>` 类型的 [`Conditions`](xref:Xamarin.Forms.MultiTrigger.Conditions)
- `IList<Setter>` 类型的 [`Setters`](xref:Xamarin.Forms.MultiTrigger.Setters)

[`Condition`](xref:Xamarin.Forms.Condition) 是抽象类，有两个后代类：

- [`PropertyCondition`](xref:Xamarin.Forms.Condition)：像 `Trigger` 一样有 [`Property`](xref:Xamarin.Forms.PropertyCondition.Property) 和 [`Value`](xref:Xamarin.Forms.PropertyCondition.Value) 属性
- [`BindingCondition`](xref:Xamarin.Forms.BindingCondition)：像 `DataTrigger` 一样有 [`Binding`](xref:Xamarin.Forms.BindingCondition.Binding) 和 [`Value`](xref:Xamarin.Forms.BindingCondition.Value) 属性

在 [AndConditions](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/AndConditions) 示例中，仅当四个 `Switch` 元素都处于启用状态时，才会为 `BoxView` 着色。

[OrConditions](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/OrConditions) 示例展示了如何在四个 `Switch` 元素中任何一个处于启用状态时为 `BoxView` 着色。 这需要应用德·摩根定律，并完全逆反逻辑。

合并 AND 和 OR 逻辑并不容易，通常需要不可见的 `Switch` 元素来实现中间结果。 [XorConditions](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/XorConditions) 示例展示了 `Button` 如何在两个 `Entry` 元素之一中有某文本键入（而不是两个元素都有）时启用。

## <a name="behaviors"></a>行为

可以使用触发器执行的任何操作也能通过行为实现，但行为始终需要派生自 [`Behavior<T>`](xref:Xamarin.Forms.Behavior`1) 的类，并重写以下两种方法：

- [`OnAttachedTo`](xref:Xamarin.Forms.Behavior`1.OnAttachedTo*)
- [`OnDetachingFrom`](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom*)

参数是行为附加到的元素。 通常情况下，`OnAttachedTo` 方法附加一些事件处理程序，而 `OnDetachingFrom` 则将它们拆离开来。 由于这样的类通常会保存某种状态，因此通常无法在 `Style` 中共享。

[BehaviorEntryValidation](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/BehaviorEntryValidation) 示例类似于 TriggerEntryValidation，区别在于前者使用行为 &mdash;，即 [Xamarin.FormsBook.Toolkit](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 库中的 [`NumericValidationBehavior`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NumericValidationBehavior.cs) 类。

### <a name="behaviors-with-properties"></a>包含属性的行为

`Behavior<T>` 派生自 `Behavior`，而后者又派生自 `BindableObject`，因此可以对行为定义可绑定属性。 这些属性在数据绑定中可以是活动的。

[EmailValidationDemo](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EmailValidationDemo) 程序对此进行了展示，它使用 [Xamarin.FormsBook.Toolkit](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 库中的 [`ValidEmailBehavior`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ValidEmailBehavior.cs) 类。 `ValidEmailBehavior` 有只读的可绑定属性，并用作数据绑定中的源。

[EmailValidationConv](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EmailValidationConv) 示例使用此相同行为来显示另一种类型的指示器，以指明电子邮件地址是有效的。

[EmailValidationTrigger](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EmailValidationTrigger) 示例是上一示例的变体。 ButtonGlide 结合使用 `DataTrigger` 与相应行为。

### <a name="toggles-and-check-boxes"></a>切换和复选框

可以在类（如 [Xamarin.FormsBook.Toolkit](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 库中的 [`ToggleBehavior`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ToggleBehavior.cs)）中封装切换按钮的行为，然后完全在 XAML 中定义切换的所有视觉对象。

[ToggleLabel](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ToggleLabel) 示例结合使用 `ToggleBehavior` 与 `DataTrigger`，以将包含两个文本字符串的 `Label` 用于切换。

[FormattedTextToggle](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/FormattedTextToggle) 示例通过在两个 `FormattedString` 对象之间切换，扩展了此概念。

Xamarin.FormsBook.Toolkit 库中的 [`ToggleBase`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ToggleBase.cs) 类派生自 `ContentView`，定义了 `IsToggled` 属性，并为切换逻辑合并了 `ToggleBehavior`。 这样，就可以更轻松地在 XAML 中定义切换按钮，如 [TraditionalCheckBox](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/TraditionalCheckBox) 示例所示。

[SwitchCloneDemo](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/SwitchCloneDemo) 包括派生自 `ToggleBase` 的 [`SwitchClone`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter23/SwitchCloneDemo/SwitchCloneDemo/SwitchCloneDemo/SwitchClone.cs) 类，并使用 [`TranslateAction`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/TranslateAction.cs) 类构造类似于 Xamarin.Forms `Switch` 的切换按钮。

Xamarin.FormsBook.Toolkit 中的 [`RotateAction`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/RotateAction.cs) 提供了动画，用于在 [LeverToggle](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/LeverToggle) 示例中创建动画控制杆。

### <a name="responding-to-taps"></a>响应点击

`EventTrigger` 的一个缺点是，无法将它附加到 `TapGestureRecognizer` 来响应点击。 [Xamarin.FormsBook.Toolkit](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 中的 [`TapBehavior`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/TapBehavior.cs) 旨在解决此问题

[BoxViewTapShiver](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/BoxViewTapShiver) 示例使用 `TapBehavior`，以对点击的 `BoxView` 元素使用前面的 `ShiverAction`。

[ShiverViews](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ShiverViews) 示例展示了如何通过封装 `ShiverView` 类来减少标记。

### <a name="radio-buttons"></a>单选按钮

[Xamarin.FormsBook.Toolkit](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 库中还有 [`RadioBehavior`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/RadioBehavior.cs) 类，用于创建按 `string` 组名分组的单选按钮。

[RadioLabels](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/RadioLabels) 程序对其单选按钮使用文本字符串。 [RadioStyle](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/RadioStyle) 示例使用 `Style` 实现选中按钮和未选中按钮的外观差异。 [RadioImages](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/RadioImages) 示例对其单选按钮使用分格图像：

[![单选按钮图像的三倍屏幕截图](images/ch23fg17-small.png "单选按钮图像")](images/ch23fg17-large.png#lightbox "单选按钮图像")

[TraditionalRadios](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/TraditionalRadios) 示例绘制传统外观的单选按钮，即圆里面有点。

### <a name="fades-and-orientation"></a>淡化和方向

最后一个示例 [MultiColorSliders](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/MultiColorSliders) 允许使用单选按钮在三种不同的颜色选择视图之间切换。 这三个视图使用 [Xamarin.FormsBook.Toolkit](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 库中的 [`FadeEnableAction`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/FadeEnableAction.cs) 淡入和淡出。

此程序还使用 Xamarin.FormsBook.Toolkit 库中的 [`GridOrientationBehavior`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/GridOrientationBehavior.cs) 来响应方向更改（即在纵向和横向之间变化）。

## <a name="related-links"></a>相关链接

- [第 23 章全文 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch23-Apr2016.pdf)
- [第 23 章示例](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23)
- [使用触发器](~/xamarin-forms/app-fundamentals/triggers.md)
- [Xamarin.Forms 行为](~/xamarin-forms/app-fundamentals/behaviors/creating.md)
