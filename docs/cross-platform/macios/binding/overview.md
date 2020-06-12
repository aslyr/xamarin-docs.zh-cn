---
title: Objective-C 绑定概述
description: 本文档概述创建 Objective-C 代码的 C# 绑定的各种方法，包括命令行绑定、绑定项目和 Objective Sharpie。 还介绍了绑定的工作原理。
ms.prod: xamarin
ms.assetid: 9EE288C5-8952-C5A9-E542-0BD847300EC6
author: davidortinau
ms.author: daortin
ms.date: 11/25/2015
ms.openlocfilehash: ca83f6ced2e9c2f5380d3bf760e00d613cb0acb0
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/09/2020
ms.locfileid: "84570968"
---
# <a name="overview-of-objective-c-bindings"></a>Objective-C 绑定概述

_绑定过程工作原理的详细信息_

绑定用于 Xamarin 的 Objective-C 库需要三个步骤：

1. 编写 C#“API 定义”来说明如何在 .NET 中公开本机 API，以及如何将其映射到基础 Objective-C。 这是使用标准 C# 构造（如 `interface` 和各种绑定属性）实现的（请参阅此[简单示例](~/cross-platform/macios/binding/objective-c-libraries.md#Binding_an_API)）。

2. 使用 C# 编写了“API 定义”后，就可以对其进行编译以生成“绑定”程序集了。 此操作可以在[命令行](#command-line-bindings)上完成，也可以在 Visual Studio for Mac 或 Visual Studio 中使用[绑定项目](#bindingproject)来完成。

3. 然后，该“绑定”程序集将添加到你的 Xamarin 应用程序项目中，方便你使用定义的 API 访问本机功能。
   绑定项目与你的应用程序项目完全分开。

   > [!NOTE]
   > 借助 [Objective Sharpie](#objectivesharpie)，可以自动执行第 1 步。 它会检查 Objective-C API 并生成建议的 C#“API 定义”。 你可以自定义由 Objective Sharpie 创建的文件，并在绑定项目中（或在命令行上）使用它们来创建绑定程序集。 Objective Sharpie 不会自行创建绑定，它只是较大过程中的一个可选部分。

除此之外，还可以了解有关其[工作原理](#howitworks)的更多技术细节，这将有助于你编写绑定。

## <a name="command-line-bindings"></a>命令行绑定

对于 Xamarin.iOS，可以使用 `btouch-native`（如果你使用的是 Xamarin.Mac，则相应地使用 `bmac-native`）来直接生成绑定。 它的工作方式是将你手动（或使用 Objective Sharpie）创建的 C# API 定义传递到命令行工具（对于 iOS，为 `btouch-native`；对于 Mac，为 `bmac-native`）。

调用这些工具的一般语法如下：

```csharp
# Use this for Xamarin.iOS:
bash$ /Developer/MonoTouch/usr/bin/btouch-native -e cocos2d.cs -s:enums.cs -x:extensions.cs
```

```csharp
# Use this for Xamarin.Mac:
bash$ bmac-native -e cocos2d.cs -s:enums.cs -x:extensions.cs
```

上面的命令将在当前目录中生成 `cocos2d.dll` 文件，并且它将包含你可以在项目中使用的完全绑定的库。 如果使用绑定项目（如[下面](#bindingproject)所述），则这是 Visual Studio for Mac 用于创建绑定的工具。

<a name="bindingproject"></a>

## <a name="binding-project"></a>绑定项目

可以在 Visual Studio for Mac 或 Visual Studio 中创建绑定项目（Visual Studio 仅支持 iOS 绑定），这样可以更轻松地编辑和生成用于绑定的 API 定义（与使用命令行相反）。

请参阅此[入门指南](~/cross-platform/macios/binding/objective-c-libraries.md#Getting_Started)，了解如何创建和使用绑定项目来生成绑定。

<a name="objectivesharpie"></a>

## <a name="objective-sharpie"></a>目标 Sharpie

Objective Sharpie 是另一个单独的命令行工具，可在创建绑定的初始阶段提供帮助。 它不会自行创建绑定，而是自动执行为目标本机库生成 API 定义的初始步骤。

请参阅 [Objective Sharpie docs](~/cross-platform/macios/binding/objective-sharpie/index.md)，了解如何将本机库、本机框架和 CocoaPods 分析为可内置到绑定中的 API 定义。

<a name="howitworks"></a>

## <a name="how-binding-works"></a>绑定的工作原理

可以结合使用 [[Register]](xref:Foundation.RegisterAttribute) 属性、[[Export]](xref:Foundation.ExportAttribute) 属性，以及 [Objective-C 选择器手动调用](~/ios/internals/objective-c-selectors.md)方式，手动绑定新的（先前未绑定的）Objective-C 类型。

首先，查找要绑定的类型。 为了便于讨论（以及尽量简单化），我们将绑定 [NSEnumerator](https://developer.apple.com/documentation/foundation/nsenumerator) 类型（已在 [Foundation.NSEnumerator](xref:Foundation.NSEnumerator) 中绑定；下面的实现仅用于示例目的）。

其次，需要创建 C# 类型。 我们可能需要将其放置在名称空间中；由于 Objective-C 不支持名称空间，因此我们需要使用 `[Register]` 属性来更改 Xamarin.iOS 将在 Objective-C 运行时注册的类型名称。 C# 类型也必须继承自 [Foundation.NSObject](xref:Foundation.NSObject)：

```csharp
namespace Example.Binding {
    [Register("NSEnumerator")]
    class NSEnumerator : NSObject
    {
        // see steps 3-5
    }
}
```

第三，查看 Objective-C 文档，并为每个要使用的选择器创建 [ObjCRuntime.Selector](xref:ObjCRuntime.Selector) 实例。 将它们放在类主体中：

```csharp
static Selector selInit       = new Selector("init");
static Selector selAllObjects = new Selector("allObjects");
static Selector selNextObject = new Selector("nextObject");
```

第四，类型将需要提供构造函数。 必须将构造函数调用链接到基类构造函数。 这些 `[Export]` 属性允许 Objective-C 代码使用指定的选择器名称来调用构造函数：

```csharp
[Export("init")]
public NSEnumerator()
    : base(NSObjectFlag.Empty)
{
    Handle = Messaging.IntPtr_objc_msgSend(this.Handle, selInit.Handle);
}
```

```csharp
// This constructor must be present so that Xamarin.iOS
// can create instances of your type from Objective-C code.
public NSEnumerator(IntPtr handle)
    : base(handle)
{
}
```

第五，为第 3 步中声明的每个选择器提供方法。 这些方法将使用 `objc_msgSend()` 来调用本机对象上的选择器。 请注意使用 [Runtime.GetNSObject()](xref:ObjCRuntime.Runtime.GetNSObject*) 将 `IntPtr` 转换为适当类型的 `NSObject`（子）类型。 如果需要从 Objective-C 代码中调用该方法，则成员必须是虚拟的。

```csharp
[Export("nextObject")]
public virtual NSObject NextObject()
{
    return Runtime.GetNSObject(
        Messaging.IntPtr_objc_msgSend(this.Handle, selNextObject.Handle));
}
```

```csharp
// Note that for properties, [Export] goes on the get/set method:
public virtual NSArray AllObjects {
    [Export("allObjects")]
    get {
        return (NSArray) Runtime.GetNSObject(
            Messaging.IntPtr_objc_msgSend(this.Handle, selAllObjects.Handle));
    }
}
```

放在一起后如下所示：

```csharp
using System;
using Foundation;
using ObjCRuntime;

namespace Example.Binding {
    [Register("NSEnumerator")]
    class NSEnumerator : NSObject
    {
        static Selector selInit       = new Selector("init");
        static Selector selAllObjects = new Selector("allObjects");
        static Selector selNextObject = new Selector("nextObject");

        [Export("init")]
        public NSEnumerator()
            : base(NSObjectFlag.Empty)
        {
            Handle = Messaging.IntPtr_objc_msgSend(this.Handle,
                selInit.Handle);
        }

        public NSEnumerator(IntPtr handle)
            : base(handle)
        {
        }

        [Export("nextObject")]
        public virtual NSObject NextObject()
        {
            return Runtime.GetNSObject(
                Messaging.IntPtr_objc_msgSend(this.Handle,
                    selNextObject.Handle));
        }

        // Note that for properties, [Export] goes on the get/set method:
        public virtual NSArray AllObjects {
            [Export("allObjects")]
            get {
                return (NSArray) Runtime.GetNSObject(
                    Messaging.IntPtr_objc_msgSend(this.Handle,
                        selAllObjects.Handle));
            }
        }
    }
}
```
