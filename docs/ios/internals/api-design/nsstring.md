---
title: Xamarin.iOS 和 Xamarin.Mac 中的 NSString
description: 本文档介绍 Xamarin.iOS 如何以透明方式将 NSString 对象转换为 C# 字符串对象（当此情况未发生时）。
ms.prod: xamarin
ms.assetid: 785744B3-42E2-4590-8F41-435325E609B9
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: f744f4ed5619e4e7f4a9d85897c4451bf7e5b9bc
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/10/2020
ms.locfileid: "73022351"
---
# <a name="nsstring-in-xamarinios-and-xamarinmac"></a>Xamarin.iOS 和 Xamarin.Mac 中的 NSString

Xamarin.iOS 和 Xamarin.Mac 的设计都要求使用 API 来公开本机 .NET 字符串类型 (`string`)，以便通过 C# 和其他 .NET 编程语言进行字符串操作，并将字符串公开为 API 公开的数据类型，而不是 `NSString` 数据类型。

这意味着开发人员无需在特殊类型 (`Foundation.NSString`) 中保留用于调用 Xamarin.iOS 和 Xamarin.Mac API（统一）的字符串，他们可以在所有操作中继续使用 Mono 的 `System.String`，而在 Xamarin.iOS 或 Xamarin.Mac 中的 API 需要字符串时，我们的 API 绑定将负责封送信息。

例如，类型为 `NSString` 的 `UILabel` 上的 Objective-C“text”属性声明如下：

```objc
@property(nonatomic, copy) NSString *text
```

这在 Xamarin.iOS 中公开为：

```csharp
class UILabel {
    public string Text { get; set; }
}
```

在后台，此属性的实现将 C# 字符串封送到 `NSString` 中，并以与 Objective-C 相同的方式调用 `objc_msgSend` 方法。

有少量的第三方 Objective-C API 不使用 `NSString`，而使用 C 字符串（“char  ”）。 在这些情况下，仍可以使用 C# 字符串数据类型，但必须使用 [[PlainString]](~/cross-platform/macios/binding/objective-c-libraries.md) 属性来通知绑定生成器，告知它此字符串不应作为 `NSString` 封送，而应作为 C 字符串封送。

 <a name="Exceptions_to_the_Rule" />

## <a name="exceptions-to-the-rule"></a>规则的例外情况

在 Xamarin.iOS 和 Xamarin.Mac 中，此规则有一个例外。 如果 `NSString` 方法可以执行指针比较而不是内容比较，我们将决定何时公开 `string`，以及何时破例公开 `NSString`。

当 Objective-C API 使用公共 `NSString` 常数作为表示某个操作的令牌，而不是比较字符串的实际内容时，可能会发生这种情况。

在这些情况下，将公开 `NSString` API，有少数 API 具有此属性。 你还会注意到，在某些类中公开了 NSString 属性。 对于通知之类的项，这些 `NSString` 属性都是公开的。 这些属性通常如下所示：

```csharp
class Foo {
     public NSString FooNotification { get; }
}
```

当你想要注册由运行时广播的特定事件时，通知是用于 `NSNotification` 类的键。

这些键通常类似于：

```csharp
class Foo {
     public NSString FooBarKey { get; }
}
```

在 API 中公开 `NSString` 的另一个场景是作为令牌，用作在 iOS 或 OS X 中将 `NSDictionary` 对象作为参数的某些 API 的参数。 字典通常包含 `NSString` 键。 按照惯例，Xamarin.iOS 通过添加“键”名称来命名静态 `NSString` 属性。
