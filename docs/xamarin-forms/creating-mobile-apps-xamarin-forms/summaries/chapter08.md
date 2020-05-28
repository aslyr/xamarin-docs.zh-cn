---
title: ''
description: ''
Creating Mobile Apps with Xamarin.Forms: Summary of Chapter 8. Code and XAML in harmony''
ms.prod: ''
ms.technology: ''
ms.assetid: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 90db8b4f11095a2a56c82c3f563844efbcf7e2b1
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/28/2020
ms.locfileid: "84136820"
---
# <a name="summary-of-chapter-8-code-and-xaml-in-harmony"></a>摘要：第 8 章. 协调代码和 XAML

[![下载示例](~/media/shared/download.png) 下载示例](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08)

本章将更深入地探讨 XAML，尤其是代码与 XAML 的交互方式。

## <a name="passing-arguments"></a>传递参数

在一般情况下，在 XAML 中实例化的类必须具有公共的无参数构造函数，生成的对象通过属性设置进行初始化。 但是，还有两种方法可以实例化和初始化对象。

尽管这些是通用方法，但它们通常与 MVVM 视图模型结合使用。

### <a name="constructors-with-arguments"></a>带有参数的构造函数

[ParameteredConstructorDemo](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/ParameteredConstructorDemo) 示例演示了如何使用 `x:Arguments` 标记指定构造函数参数。 这些参数必须由指示参数类型的元素标记进行分隔。 对于基本 .NET 数据类型，可以使用以下标记：

- `x:Object`
- `x:Boolean`
- `x:Byte`
- `x:Int16`
- `x:Int32`
- `x:Int64`
- `x:Single`
- `x:Double`
- `x:Decimal`
- `x:Char`
- `x:String`
- `x:TimeSpan`
- `x:Array`
- `x:DateTime`

### <a name="can-i-call-methods-from-xaml"></a>能否从 XAML 调用方法？

[FactoryMethodDemo](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/FactoryMethodDemo) 示例演示了如何使用 `x:FactoryMethod` 元素指定被调用以创建对象的工厂方法。 此类工厂方法必须是公共的和静态的，并且必须创建定义该类型的对象。 （例如，[`Color.FromRgb`](xref:Xamarin.Forms.Color.FromRgb(System.Double,System.Double,System.Double)) 方法合格，因为它是公共的和静态的，并且返回类型为 `Color` 的值。）在 `x:Arguments` 标记中指定工厂方法的参数。

## <a name="the-xname-attribute"></a>x:Name 属性

借助 `x:Name` 属性，允许为 XAML 中实例化的对象命名。 这些名称的规则与 C# 变量名称的规则相同。 在构造函数中返回 `InitializeComponent` 调用之后，代码隐藏文件可以引用这些名称以访问相应的 XAML 元素。 XAML 分析器实际上将名称转换为生成的分部类中的私有字段。

[XamlClock](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/XamlClock) 示例演示如何使用 `x:Name`，从而让代码隐藏文件能够使 XAML 中定义的两个 `Label` 元素随当前日期和时间进行更新。

同一名称不能用于同一页面上的多个元素。 如果使用 `OnPlatform` 为每个平台创建并行命名对象，那么这是一个特殊的问题。 [PlatformSpecificLabele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/PlatformSpecificLabels) 示例演示了执行此类操作的更好方法。

## <a name="custom-xaml-based-views"></a>自定义基于 XAML 的视图

有几种方法可以避免在 XAML 中重复标记。 一种常见的方法是创建一个新的基于 XAML 的类，该类派生自 [`ContentView`](xref:Xamarin.Forms.ContentView)。 [ColorViewList](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/ColorViewList) 示例演示了此方法。 `ColorView` 类派生自 `ContentView` 以显示特定的颜色及其名称，而 `ColorViewListPage` 类通常派生自 `ContentPage` 并显式创建 17 个 `ColorView` 实例。

访问 XAML 中的 `ColorView` 类需要另一个 XML 命名空间声明，对于同一程序集中的类，该声明通常称为 `local`。

## <a name="events-and-handlers"></a>事件和处理程序

可以在 XAML 中将事件分配给事件处理程序，但是事件处理程序本身必须在代码隐藏文件中实现。 [XamlKeypad](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/XamlKeypad) 演示了如何在 XAML 中生成键盘用户界面以及如何在代码隐藏文件中实现 `Clicked` 处理程序。

## <a name="tap-gestures"></a>点击手势

任何 `View` 对象都可以获取触控输入并根据该输入生成事件。 `View` 类定义了一个 [`GestureRecognizers`](xref:Xamarin.Forms.View.GestureRecognizers) 集合属性，该属性可以包含一个或多个派生自 [`GestureRecognizer`](xref:Xamarin.Forms.GestureRecognizer) 的类实例。

[`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) 生成 [`Tapped`](xref:Xamarin.Forms.TapGestureRecognizer.Tapped) 事件。 [MonkeyTap](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/MonkeyTap) 程序演示了如何将 `TapGestureRecognizer` 对象附加到四个 `BoxView` 元素来创建模拟游戏：

[![Monkey tap 的三倍屏幕截图](images/ch08fg07-small.png "模拟游戏")](images/ch08fg07-large.png#lightbox "模拟游戏")

但是 MonkeyTap 程序确实需要声音。 （请参阅[下一章](chapter09.md)。）

## <a name="related-links"></a>相关链接

- [第 8 章全文 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch08-Apr2016.pdf)
- [第 8 章示例](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08)
- [第 8 章 F# 示例](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/FS/XamlKeypad)
- [在 XAML 中传递参数](~/xamarin-forms/xaml/passing-arguments.md)
