---
title: 摘要：第 11 章. 可绑定的基础结构
description: 使用 Xamarin.Forms 创建移动应用：摘要：第 11 章. 可绑定的基础结构
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 34671C48-0ED4-4B76-A33D-D6505390DC5B
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
ms.openlocfilehash: f9e3326c0f55469cfa84a019a674679d82dfc007
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/10/2020
ms.locfileid: "61334290"
---
# <a name="summary-of-chapter-11-the-bindable-infrastructure"></a>摘要：第 11 章. 可绑定的基础结构

[![下载示例](~/media/shared/download.png) 下载示例](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11)

每个 C# 程序员都熟悉 C# 属性  。 属性包含 set  访问器和/或 get  访问器。 它们通常称为公共语言运行时的 CLR 属性  。

Xamarin.Forms 定义了一个增强属性定义，称为可绑定属性  ，由 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 类封装，并由 [`BindableObject`](xref:Xamarin.Forms.BindableObject) 类提供支持。 这些类相关，但又截然不同：`BindableProperty` 用于定义属性本身；`BindableObject` 与 `object` 相似，因为它是定义可绑定属性的类的基类。

## <a name="the-xamarinforms-class-hierarchy"></a>Xamarin.Forms 类层次结构

[ClassHierarchy  ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/ClassHierarchy) 示例使用反射来显示 Xamarin.Forms 的类层次结构，并演示 `BindableObject` 在此层次结构中发挥的重要作用。 `BindableObject` 派生自 `Object`，是 [`Element`](xref:Xamarin.Forms.Element) 的父类，[`VisualElement`](xref:Xamarin.Forms.VisualElement) 派生自该父类。 这是 [`Page`](xref:Xamarin.Forms.Page) 和 [`View`](xref:Xamarin.Forms.View) 的父类，后者是 [`Layout`](xref:Xamarin.Forms.Layout) 的父类：

[![类层次结构共享的三倍屏幕截图](images/ch11fg01-small.png "类层次结构共享") ](images/ch11fg01-large.png#lightbox "类层次结构共享")

## <a name="a-peek-into-bindableobject-and-bindableproperty"></a>查看 BindableObject 和 BindableProperty

在从 `BindableObject` 派生的类中，许多 CLR 属性被称为“受支持的”可绑定属性。 例如，`Label` 类的 [`Text`](xref:Xamarin.Forms.Label.Text) 属性是 CLR 属性，但是 `Label` 类还定义了类型为 `BindableProperty` 的名为 [`TextProperty`](xref:Xamarin.Forms.Label.TextProperty) 的公共静态只读字段。

应用程序可以正常设置或获取 `Label` 的 `Text` 属性，或者应用程序可以通过使用 `Label.TextProperty` 参数调用 `BindableObject` 定义的 [`SetValue`](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindableProperty,System.Object)) 方法来设置 `Text`。 同样，应用程序可以通过再次使用 `Label.TextProperty` 参数调用 [`GetValue`](xref:Xamarin.Forms.BindableObject.GetValue(Xamarin.Forms.BindableProperty)) 方法来获取 `Text` 属性的值。 [PropertySettings  ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/PropertySettings) 示例对此进行了演示。

实际上，`Text` CLR 属性完全是使用由 `BindableObject` 以及 `Label.TextProperty` 静态属性一起定义的 `SetValue` 和 `GetValue` 方法实现的。

`BindableObject` 和 `BindableProperty` 为以下操作提供支持：

- 提供属性默认值
- 存储其当前值
- 提供验证属性值的机制
- 保持单个类中相关属性之间的一致性
- 响应属性变化
- 在属性即将更改或已更改时触发通知
- 支持数据绑定
- 支持样式
- 支持动态资源

每当由可绑定属性提供支持的属性发生更改时，`BindableObject` 就会触发 [`PropertyChanged`](xref:Xamarin.Forms.BindableObject.PropertyChanged) 事件，以标识已更改的属性。 当属性设置为相同值时，不会触发此事件。

某些属性不受可绑定属性支持，而某些 Xamarin.Forms 类（例如 `Span`）不是从 `BindableObject` 派生的。 只有从 `BindableObject` 派生的类才能支持可绑定属性，因为 `BindableObject` 定义了 `SetValue` 和 `GetValue` 方法。

因为 `Span` 不是从 `BindableObject` 派生的，所以它的任何属性（例如 `Text`）都不受可绑定属性的支持。 这就是上一章的 [DynamicVsStatic  ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/DynamicVsStatic) 示例中，`Span` 的 `Text` 属性上的 `DynamicResource` 设置引发异常的原因。 [DynamicVsStaticCode  ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/DynamicVsStaticCode) 示例演示如何使用 `Element` 定义的 [`SetDynamicResource`](xref:Xamarin.Forms.Element.SetDynamicResource(Xamarin.Forms.BindableProperty,System.String)) 方法在代码中设置动态资源。 第一个参数是类型为 `BindableProperty` 的对象。

同样，`BindableObject` 定义的 [`SetBinding`](xref:Xamarin.Forms.BindableObject.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase)) 方法具有类型为 `BindableProperty` 的第一个参数。

## <a name="defining-bindable-properties"></a>定义可绑定属性

可以使用静态 [`BindableProperty.Create`](xref:Xamarin.Forms.BindableProperty.Create(System.String,System.Type,System.Type,System.Object,Xamarin.Forms.BindingMode,Xamarin.Forms.BindableProperty.ValidateValueDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangedDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangingDelegate,Xamarin.Forms.BindableProperty.CoerceValueDelegate,Xamarin.Forms.BindableProperty.CreateDefaultValueDelegate)) 方法定义自己的可绑定属性，以创建类型为 `BindableProperty` 的静态只读字段。

[Xamarin.FormsBook.Toolkit  ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 库中的 [`AltLabel`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AltLabel.cs) 类对此进行了演示。 该类派生自 `Label`，可让用户指定字号（以磅为单位）。 [PointSizedText  ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/PointSizedText) 示例对此进行了演示。

`BindableProperty.Create` 方法需要四个参数：

- `propertyName`：属性的文本名称（与 CLR 属性名称相同）
- `returnType`：CLR 属性的类型
- `declaringType`：声明属性的类的类型
- `defaultValue`：属性的默认值

由于 `defaultValue` 的类型为 `object`，因此编译器必须能够确定默认值的类型。 例如，如果 `returnType` 为 `double`，则应将 `defaultValue` 设置为类似于 0.0 的值，而不是仅设置为 0，否则类型不匹配将在运行时触发异常。

可绑定属性通常还包括：

- `propertyChanged`：属性更改值时调用的静态方法。 第一个参数是属性已更改的类的实例。

`BindableProperty.Create` 的其他参数并不常见：

- `defaultBindingMode`：与数据绑定结合使用（如以下章节中所述：[第 16 章  数据绑定](chapter16.md)）
- `validateValue`：用于检查有效值的回叫
- `propertyChanging`：一个回叫，指示何时要更改属性
- `coerceValue`：一个回叫，将设定值强制转换为另一个值
- `defaultValueCreate`：一个回叫，创建无法在类（例如，集合）的实例之间共享的默认值

### <a name="the-read-only-bindable-property"></a>只读可绑定属性

可绑定属性可以是只读的。 创建只读可绑定属性需要调用静态方法 [`BindableProperty.CreateReadOnly`](xref:Xamarin.Forms.BindableProperty.CreateReadOnly(System.String,System.Type,System.Type,System.Object,Xamarin.Forms.BindingMode,Xamarin.Forms.BindableProperty.ValidateValueDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangedDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangingDelegate,Xamarin.Forms.BindableProperty.CoerceValueDelegate,Xamarin.Forms.BindableProperty.CreateDefaultValueDelegate)) 来定义类型为 [`BindablePropertyKey`](xref:Xamarin.Forms.BindablePropertyKey) 的专用静态只读字段。

然后，将 CLR 属性 `set` 访问器定义为 `private`，以使用 `BindablePropertyKey` 对象调用 [`SetValue`](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindablePropertyKey,System.Object)) 重载。 这样可以防止在类外部设置属性。

[BaskervillesCount  ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/BaskervillesCount) 示例中使用的 [`CountedLabel`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CountedLabel.cs) 类对此进行了演示。

## <a name="related-links"></a>相关链接

- [第 11 章全文 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch11-Apr2016.pdf)
- [第 11 章示例](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11)
- [可绑定属性](~/xamarin-forms/xaml/bindable-properties.md)
