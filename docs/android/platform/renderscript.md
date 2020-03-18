---
title: Renderscript 简介
description: 本指南介绍了 Renderscript，并说明如何在面向 API 级别 17 或更高级别的 Xamarin.Android 应用程序中使用内部 Renderscript API。
ms.prod: xamarin
ms.assetid: 378793C7-5E3E-40E6-ABEE-BEAEF64E6A47
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/06/2018
ms.openlocfilehash: 884b69b0cdecf4f979cec314b6440974c5bac97d
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/10/2020
ms.locfileid: "73019800"
---
# <a name="an-introduction-to-renderscript"></a>Renderscript 简介

_本指南介绍了 Renderscript，并说明如何在面向 API 级别 17 或更高级别的 Xamarin.Android 应用程序中使用内部 Renderscript API。_

## <a name="overview"></a>概述

Renderscript 是 Google 创建的编程框架，目的在于提高需要大量计算资源的 Android 应用程序的性能。 它是基于 [C99](https://en.wikipedia.org/wiki/C99) 的低级别、高性能 API。 由于它是将在 CPU、GPU 或 DSP 上运行的低级别 API，Renderscript 非常适合用于可能需要执行以下任一操作的 Android 应用：

- 图形
- 图像处理
- 加密
- 信号处理
- 数学例程

Renderscript 将使用 `clang`，并将脚本编译为绑定到 APK 的 LLVM 字节代码。 首次运行应用时，LLVM 字节代码将编译为设备上处理器的机器代码。 此体系结构允许 Android 应用程序利用机器代码的优点，而无需开发人员为设备自身的每个处理器编写该代码。

Renderscript 例程有两个组件：

1. **Renderscript 运行时** &ndash; 这是负责执行 Renderscript 的本机 API。 这包括为应用程序编写的任何 Renderscript。

2. **Android Framework 中的托管包装器** &ndash; 托管类允许 Android 应用控制 Renderscript 运行时和脚本并与之交互。 除了框架提供的用于控制 Renderscript 运行时的类，Android 工具链还会检查 Renderscript 源代码，并生成托管包装类以供 Android 应用程序使用。

下图说明了这些组件之间的关系：

![阐释 Android Framework 如何与 Renderscript 运行时交互的关系图](renderscript-images/renderscript-01.png)

在 Android 应用程序中使用 Renderscripts 有三个重要概念：

1. **上下文** &ndash; Android SDK 提供的托管 API，该 API 将资源分配给 Renderscript，并允许 Android 应用程序通过 Renderscript 传递和接收数据。

2. **_计算核心_** &ndash; 也称为“根内核”  或“内核”  ，这是执行工作的例程。 内核非常类似于 C 函数；它是一个可并行化例程，将在已分配内存中的所有数据上运行。

3. **已分配内存** &ndash; 数据通过[分配 _](xref:Android.Renderscripts.Allocation)_ 在内核中来回传输。 一个内核可以有一个输入和/或一个输出分配。

[Android.Renderscripts](xref:Android.Renderscripts) 命名空间包含用于与 Renderscript 运行时进行交互的类。 特别是，[`Renderscript`](xref:Android.Renderscripts.RenderScript) 类将管理 Renderscript 引擎的生命周期和资源。 Android 应用必须初始化一个或多个 [`Android.Renderscripts.Allocation`](xref:Android.Renderscripts.Allocation)
对象。 分配是一个托管 API，负责分配和访问在 Android 应用与 Renderscript 运行时之间共享的内存。 通常，为输入创建一个分配，并根据需要创建另一个分配来保存内核的输出。 Renderscript 运行时引擎和关联的托管包装类将管理对分配所持有的内存的访问，无需 Android 应用开发人员执行任何其他工作。

分配将包含一个或多个 [Android.Renderscripts.Elements](xref:Android.Renderscripts.Element)。
元素是一种专用类型，用于描述每个分配中的数据。
输出分配的元素类型必须匹配输入元素的类型。 在执行时，Renderscript 将并行遍历输入分配中的每个元素，并将结果写入输出分配。 有以下两种类型的元素：

- **简单类型** &ndash; 概念上与 C 数据类型（`float` 或 `char`）相同。

- **复杂类型** &ndash; 此类型与 C `struct` 类似。

Renderscript 引擎将执行一个运行时检查，以确保每个分配中的元素与内核所需的元素兼容。 如果分配中元素的数据类型与内核期望的数据类型不匹配，将引发异常。

所有 Renderscript 内核都将由 [`Android.Renderscripts.Script`](xref:Android.Renderscripts.Script) 类的子代类型包装
类的新实例。 `Script` 类用于设置 Renderscript 参数，设置相应的 `Allocations`，并运行 Renderscript。 Android SDK 中有两个 `Script` 子类：

- **`Android.Renderscripts.ScriptIntrinsic`** &ndash; 一些更常见的 Renderscript 任务捆绑在 Android SDK 中，可通过 [ScriptIntrinsic](xref:Android.Renderscripts.ScriptIntrinsic) 类的一个子类进行访问。 开发人员无需执行任何额外的步骤即可在应用中使用这些脚本，因为已经提供了这些脚本。

- **`ScriptC_XXXXX`** &ndash; 也称为“用户脚本”  ，这些脚本由开发人员编写并打包在 APK 中。 在编译时，Android 工具链将生成托管包装类，以允许在 Android 应用中使用这些脚本。
  这些生成的类的名称是 Renderscript 文件的名称，以 `ScriptC_` 为前缀。 Xamarin.Android 并不正式支持编写和合并用户脚本，这些内容不在本指南范围内。

对于这两种类型，Xamarin.Android 仅支持 `StringIntrinsic`。 本指南将讨论如何在 Xamarin.Android 应用程序中使用内部脚本。

## <a name="requirements"></a>要求

本指南适用于面向 API 级别 17 或更高级别的 Xamarin.Android 应用程序。 本指南不讨论用户脚本  的使用。

[Xamarin.Android V8 支持库](https://www.nuget.org/packages/Xamarin.Android.Support.v8.RenderScript/)支持面向旧版本 Android SDK 的应用的内部 Renderscript API。 如果将此包添加到 Xamarin.Android 项目，则应允许面向旧版本 Android SDK 的应用程序利用内部脚本。

## <a name="using-intrinsic-renderscripts-in-xamarinandroid"></a>在 Xamarin.Android 中使用内部 Renderscript

内部脚本是执行密集型计算任务的好方法，只需用到很少的额外代码。 已对其进行手动优化，以便在大量设备上提供最佳性能。
内部脚本的运行速度是托管代码的 10 倍，是自定义 C 实现的 2-3 倍，这种情况并不少见。 内部脚本涵盖了许多典型的处理场景。 此内部脚本列表介绍了 Xamarin.Android 中的当前脚本：

- [ScriptIntrinsic3DLUT](xref:Android.Renderscripts.ScriptIntrinsic3DLUT) &ndash; 使用三维查找表将 RGB 转换为 RGBA。 

- [ScriptIntrinsicBLAS](https://developer.android.com/reference/android/renderscript/ScriptIntrinsicBLAS.html) &ndash; 向 [BLAS](http://www.netlib.org/blas/) 提供高性能 Renderscript API。 BLAS（基本线性代数子程序）是提供标准构建基块执行基本矢量和矩阵操作的例程。 

- [ScriptIntrinsicBlend](xref:Android.Renderscripts.ScriptIntrinsicBlend) &ndash; 将两个分配组合在一起。

- [ScriptIntrinsicBlur](xref:Android.Renderscripts.ScriptIntrinsicBlur) &ndash; 将高斯模糊应用于分配。

- [ScriptIntrinsicColorMatrix](xref:Android.Renderscripts.ScriptIntrinsicColorMatrix) &ndash; 将颜色矩阵应用于分配（即更改颜色、调整色调）。

- [ScriptIntrinsicConvolve3x3](xref:Android.Renderscripts.ScriptIntrinsicConvolve3x3) &ndash; 将 3x3 颜色矩阵应用于分配。

- [ScriptIntrinsicConvolve5x5](xref:Android.Renderscripts.ScriptIntrinsicConvolve5x5) &ndash; 将 5x5 颜色矩阵应用于分配。

- [ScriptIntrinsicHistogram](xref:Android.Renderscripts.ScriptIntrinsicHistogram) &ndash; 内部直方图筛选器。

- [ScriptIntrinsicLUT](xref:Android.Renderscripts.ScriptIntrinsicLUT) &ndash; 将每通道查找表应用于缓冲区。

- [ScriptIntrinsicResize](xref:Android.Renderscripts.ScriptIntrinsicResize) &ndash; 用于执行二维分配大小调整的脚本。

- [ScriptIntrinsicYuvToRGB](xref:Android.Renderscripts.ScriptIntrinsicYuvToRGB) &ndash; 将 YUV 缓冲区转换为 RGB。

有关每个内部脚本的详细信息，请参阅 API 文档。

下面介绍了在 Android 应用程序中使用 Renderscript 的基本步骤。

**创建 Renderscript 上下文** &ndash; [`Renderscript`](xref:Android.Renderscripts.RenderScript)
类是 Renderscript 上下文的托管包装器，它将控制初始化、资源管理和清理。 Renderscript 对象是使用 `RenderScript.Create` 工厂方法创建的，该方法采用 Android 上下文（例如活动）作为参数。 下面的代码行演示如何初始化 Renderscript 上下文：

```csharp
Android.Renderscripts.RenderScript renderScript = RenderScript.Create(this);
```

**创建分配** &ndash; 根据内部脚本，可能需要创建一个或两个 `Allocation`。 [`Android.Renderscripts.Allocation`](xref:Android.Renderscripts.Allocation)
类具有多个工厂方法，可帮助为内部脚本实例化分配。 作为一个示例，下面的代码片段演示了如何为位图创建分配。

```csharp
Android.Graphics.Bitmap originalBitmap;
Android.Renderscripts.Allocation inputAllocation = Allocation.CreateFromBitmap(renderScript,
                                                     originalBitmap,
                                                     Allocation.MipmapControl.MipmapFull,
                                                     AllocationUsage.Script);
```

通常，需要创建 `Allocation` 来保存脚本的输出数据。 下面的代码片段展示了如何使用 `Allocation.CreateTyped` helper 来实例化第二个与原始类型相同的 `Allocation`：

```csharp
Android.Renderscripts.Allocation outputAllocation = Allocation.CreateTyped(renderScript, inputAllocation.Type);
```

**实例化脚本包装器** &ndash;每个内部脚本包装类都应具有 helper 方法（通常称为 `Create`）来实例化该脚本的包装器对象。 下面的代码片段演示了如何实例化 `ScriptIntrinsicBlur` 模糊对象。 `Element.U8_4` helper 方法将创建一个元素来描述数据类型，该数据类型包含 4 个 8 位无符号整数值字段，适合保存 `Bitmap` 对象的数据：

```csharp
Android.Renderscripts.ScriptIntrinsicBlur blurScript = ScriptIntrinsicBlur.Create(renderScript, Element.U8_4(renderScript));
```

**指定分配、设置参数和运行脚本** &ndash; `Script` 类提供了一个 `ForEach` 方法来实际运行 Renderscript。 该方法将遍历 `Allocation` 中持有输入数据的每个 `Element`。 在某些情况下，可能需要提供持有输出数据的 `Allocation`。
`ForEach` 将重写输出分配的内容。 为了继续使用前面步骤中的代码片段，本示例展示了如何指定输入分配，设置参数，最后运行脚本（将结果复制到输出分配）：

```csharp
blurScript.SetInput(inputAllocation);
blurScript.SetRadius(25);  // Set a pamaeter
blurScript.ForEach(outputAllocation);
```

你可能想要了解[使用 Renderscript 对图像进行模糊处理](https://github.com/xamarin/recipes/tree/master/Recipes/android/other_ux/drawing/blur_an_image_with_renderscript)的秘诀，它完整演示了如何在 Xamarin.Android 中使用内部脚本。

## <a name="summary"></a>总结

本指南介绍了 Renderscript 以及如何在 Xamarin.Android 应用程序中使用它。 简要讨论了什么是 Renderscript，以及它在 Android 应用程序中的工作方式。 介绍了 Renderscript 中的一些关键组件，以及用户脚本  和内部脚本  的区别。 最后，本指南讨论了在 Xamarin.Android 应用程序中使用内部脚本的步骤。

## <a name="related-links"></a>相关链接

- [Android.Renderscripts 命名空间](xref:Android.Renderscripts)
- [使用 Renderscript 对图像进行模糊处理](https://github.com/xamarin/recipes/tree/master/Recipes/android/other_ux/drawing/blur_an_image_with_renderscript)
- [Renderscript](https://developer.android.com/guide/topics/renderscript/compute.html)
- [教程：Renderscript 入门](https://software.intel.com/en-us/articles/renderscript-basic-sample-for-android-os)
