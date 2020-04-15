---
title: Xamarin.iOS 中 NSObject 的泛型子类
description: 本文档介绍了如何创建 NSObject 的泛型子类。 具体探究了什么可以做，什么不可以做，并讨论了静态注册器和性能。
ms.prod: xamarin
ms.assetid: BB99EBD7-308A-C865-1829-4DFFDB1BBCA4
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: 279fcac1611038613bf442e1b766fda45dd5a429
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/13/2020
ms.locfileid: "73022367"
---
# <a name="generic-subclasses-of-nsobject-in-xamarinios"></a>Xamarin.iOS 中 NSObject 的泛型子类

## <a name="using-generics-with-nsobjects"></a>将泛型与 NSObject 结合使用

可以在 `NSObject` 的子类中使用泛型（例如 [UIView](xref:UIKit.UIView)）：

```csharp
class Foo<T> : UIView {
    public Foo (CGRect x) : base (x) {}
    public override void Draw (CoreGraphics.CGRect rect)
    {
        Console.WriteLine ("T: {0}. Type: {1}", typeof (T), GetType ().Name);
    }
}
```

由于 `NSObject` 的子类对象是在 Objective-C 运行时注册的，因此通过 `NSObject` 类型的泛型子类可以做什么存在一些限制。

## <a name="considerations-for-generic-subclasses-of-nsobject"></a>NSObject 的泛型子类的注意事项

本文档详细介绍了对 `NSObjects` 的泛型子类的有限支持所存在的限制。

### <a name="generic-type-arguments-in-member-signatures"></a>成员签名中的泛型类型参数

向 Objective-C 公开的成员签名中的所有泛型类型参数都必须有 `NSObject` 约束。

**规范**：

```csharp
class Generic<T> : NSObject where T: NSObject
{
    [Export ("myMethod:")]
    public void MyMethod (T value)
    {
    }
}
```

**原因**：由于泛型类型参数是 `NSObject`，因此可以向 Objective-C 安全地公开 `myMethod:` 的选择器签名（它始终是 `NSObject` 或其子类）。

**错误**：

```csharp
class Generic<T> : NSObject
{
    [Export ("myMethod:")]
    public void MyMethod (T value)
    {
    }
}
```

**原因**：无法为 Objective-C 代码可调用的已导出成员创建 Objective-C 签名，因为签名因泛型类型 `T` 的确切类型而异。

**规范**：

```csharp
class Generic<T> : NSObject
{
    T storage;

    [Export ("myMethod:")]
    public void MyMethod (NSObject value)
    {
    }
}
```

**原因**：可以有不受约束的泛型类型参数，只要它们不是已导出成员签名的一部分。

**规范**：

```csharp
class Generic<T, U> : NSObject where T: NSObject
{
    [Export ("myMethod:")]
    public void MyMethod (T value)
    {
        Console.WriteLine (typeof (U));
    }
}
```

**原因**：Objective-C 导出的 `MyMethod` 中的 `T` 参数被约束为 `NSObject`，不受约束的类型 `U` 不是签名的一部分。

### <a name="instantiations-of-generic-types-from-objective-c"></a>Objective-C 中的泛型类型实例化

不允许使用 Objective-C 中的泛型类型实例化。 当在 xib 或情节提要中使用托管类型时，通常会发生这种情况。

不妨使用下面的类定义，它公开需要使用 `IntPtr` 的构造函数（通过本机 Objective-C 实例构造 C# 对象的 Xamarin.iOS 方法）：

```csharp
class Generic<T> : NSObject where T : NSObject
{
    public Generic () {}
    public Generic (IntPtr ptr) : base (ptr) {}
}
```

虽然上述构造规范，但在运行时这会在 Objective-C 尝试为其创建实例时抛出异常。

出现这种情况是因为，Objective-C 没有泛型类型的概念，并且无法指定要创建的确切泛型类型。

可以通过创建泛型类型的专用子类来解决此问题。 例如：

```csharp
class Generic<T> : NSObject where T : NSObject
{
    public Generic () {}
    public Generic (IntPtr ptr) : base (ptr) {}
}

class GenericUIView : Generic<UIView>
{
}
```

现在不再有歧义了，可在 xib 或情节提要中使用类 `GenericUIView`。

## <a name="no-support-for-generic-methods"></a>不支持泛型方法

### <a name="generic-methods-are-not-allowed"></a>不允许使用泛型方法。

以下代码无法编译：

```csharp
class MyClass : NSObject
{
    [Export ("myMethod")]
    public void MyMethod<T> (T argument)
    {
    }
}
```

**原因**：这是不允许的，因为在通过 Objective-C 调用方法时，Xamarin.iOS 不知道要对类型参数 `T` 使用哪个类型。

另一种方法是创建专用方法，并将其导出：

```csharp
class MyClass : NSObject
{
    [Export ("myMethod")]
    public void MyUIViewMethod (UIView argument)
    {
        MyMethod<UIView> (argument);
    }
    public void MyMethod<T> (T argument)
    {
    }
}
```

### <a name="no-exported-static-members-allowed"></a>不允许使用已导出静态成员

不能向 Objective-C 公开 `NSObject` 的泛型子类中托管的静态成员。

不受支持的方案示例：

```csharp
class Generic<T> : NSObject where T : NSObject
{
    [Export ("myMethod:")]
    public static void MyMethod ()
    {
    }

    [Export ("myProperty")]
    public static T MyProperty { get; set; }
}
```

**原因：** 就像泛型方法一样，Xamarin.iOS 运行时需要能够知道要对泛型类型参数 `T` 使用什么类型。

对于实例成员，使用的是实例本身（因为决不会有实例 `Generic<T>`，始终为 `Generic<SomeSpecificClass>`），但对于静态成员，此信息并不存在。

请注意，即使相关成员没有以任何方式使用类型参数 `T`，这也是适用的。

在这种情况下，另一种方法是创建专用子类：

```csharp
class GenericUIView : Generic<UIView>
{
    [Export ("myUIViewMethod")]
    public static void MyUIViewMethod ()
    {
        MyMethod ();
    }

    [Export ("myProperty")]
    public static UIView MyUIViewProperty {
        get { return MyProperty; }
        set { MyProperty = value; }
    }
}

class Generic<T> : NSObject where T : NSObject
{
    public static void MyMethod () {}
    public static T MyProperty { get; set; }
}
```

## <a name="performance"></a>性能

静态注册器无法照常在生成时解析泛型类型中的已导出成员，而必须在运行时查找。 也就是说，通过 Objective-C 调用此类方法比通过非泛型类调用成员稍微慢一些。
