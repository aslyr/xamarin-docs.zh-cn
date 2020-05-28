---
title: Xamarin.Forms快速呈现器
description: 本文介绍快速呈现器， Xamarin.Forms 通过平展生成的本机控件层次结构，减少了 Android 上控件的通货膨胀和呈现成本。
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 3f25f4c2da5b2a426673b49045b5d2d05b0c6ac4
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/28/2020
ms.locfileid: "84139069"
---
# <a name="xamarinforms-fast-renderers"></a>Xamarin.Forms快速呈现器

通常，Android 上的大多数原始控件呈现器都由两个视图组成：

- 本机控件，如 `Button` 或 `TextView` 。
- 一个容器， `ViewGroup` 用于处理部分布局工作、动作处理和其他任务。

但是，这种方法有一个性能含义，即为每个逻辑控件创建了两个视图，这将导致更复杂的可视化树需要更多内存，并在屏幕上呈现更多的处理。

快速呈现器将控件的通货膨胀和渲染成本降 Xamarin.Forms 为单一视图。 因此，不创建两个视图并将其添加到视图树，而只是创建一个视图。 这会通过创建更少的对象来提高性能，这反过来意味着不太复杂的视图树和内存使用情况（这也会导致垃圾回收暂停较少）。

快速呈现器适用于 Android 上的以下控件 Xamarin.Forms ：

- [`Button`](xref:Xamarin.Forms.Button)
- [`Image`](xref:Xamarin.Forms.Image)
- [`Label`](xref:Xamarin.Forms.Label)
- [`Frame`](xref:Xamarin.Forms.Frame)

在功能上，这些快速呈现器与旧版呈现器没有什么不同。 从 Xamarin.Forms 4.0 开始，默认情况下，所有应用程序 `FormsAppCompatActivity` 都将使用这些快速呈现器。 所有新控件（包括和）的呈现器都 [`ImageButton`](xref:Xamarin.Forms.ImageButton) [`CollectionView`](xref:Xamarin.Forms.CollectionView) 使用快速呈现器方法。

对于每个应用程序，使用快速呈现器时的性能改进会有所不同，具体取决于布局的复杂性。 例如，在滚动包含数千个行的数据时，对 x2 的性能进行了改进 [`ListView`](xref:Xamarin.Forms.ListView) ，其中每行中的单元由使用快速呈现器的控件组成，这会使滚动更平滑。

> [!NOTE]
> 使用与旧版呈现器相同的方法，可以为快速呈现器创建自定义呈现器。 有关详细信息，请参阅[自定义呈现器](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)。

## <a name="backwards-compatibility"></a>向后兼容性

可以通过以下方法重写快速呈现器：

1. 在调用之前，通过在类中添加以下代码行来启用旧呈现器 `MainActivity` `Forms.Init` ：

    ```csharp
    Forms.SetFlags("UseLegacyRenderers");
    ```

1. 使用面向旧版呈现器的自定义呈现器。 任何现有的自定义呈现器都将继续与旧呈现器一起工作。
1. 指定不同的 `View.Visual` ，例如 `Material` ，使用不同的呈现器。 有关材料视觉对象的详细信息，请参阅[ Xamarin.Forms 材料视觉对象](~/xamarin-forms/user-interface/visual/material-visual.md)。

## <a name="related-links"></a>相关链接

- [自定义呈现器](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
