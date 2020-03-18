---
title: 将 Java 与 Xamarin.Android 集成
description: Java 生态系统包含各种各样的组件集合。 其中许多组件可以用来缩短开发 Android 应用程序所需的时间。 本文档将介绍并简要概述一些方法，可便于开发人员使用这些现有 Java 组件来改进 Xamarin.Android 应用程序开发体验。
ms.prod: xamarin
ms.assetid: 7B5B8695-1C49-19BF-AE99-948CDCBD2A20
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 01/18/2017
ms.openlocfilehash: ecaa02e036c74074b4fa922ea079355b72ff02e2
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/10/2020
ms.locfileid: "73020097"
---
# <a name="java-integration-with-xamarinandroid"></a>将 Java 与 Xamarin.Android 集成

Java 生态系统包含各种各样的组件集合。其中许多组件可以用来缩短开发 Android 应用程序所需的时间。本文档将介绍并简要概述一些方法，可便于开发人员使用这些现有 Java 组件来改进 Xamarin.Android 应用程序开发体验。 

## <a name="overview"></a>概述

考虑到 Java 生态系统的范围，Xamarin.Android 应用程序所需的任何给定功能很可能已经用 Java 编码。 因此，在创建 Xamarin.Android 应用程序时，尝试重用这些现有库是很有吸引力的。

在 Xamarin.Android 应用程序中重用 Java 库的方法有三种： 

- **创建 Java 绑定库** &ndash; 借助此技术，Xamarin.Android 项目用于创建 Java 类型的 C# 包装器。 然后，Xamarin.Android 应用程序可以引用此项目创建的 C# 包装器，并使用 `.jar` 文件。 

- **Java 本机接口** &ndash; Java 本机  接口  (JNI) 是一种框架，允许非 Java 代码（如 C++ 或 C#）调用在 JVM 中运行的 Java 代码，或由 Java 代码调用。 

- **移植代码** &ndash; 此方法涉及获取 Java 源代码，然后将它转换为 C#。 这可以手动完成，也可以使用自动化工具（如 Sharpen）完成。 

前两种方法的核心是 Java 本机接口  (JNI)。 JNI 是一种框架，允许没用 Java 编写的应用程序与 Java 虚拟机中运行的 Java 代码进行交互。 Xamarin.Android 使用 JNI 为 C# 代码创建绑定  。 

第一种技术是更自动化、更具声明性的方法，用于绑定 Java 库。 它涉及使用 Visual Studio for Mac，或 Xamarin.Android 提供的 Visual Studio 项目类型 &ndash; Java 绑定库。 若要成功创建这些绑定，Java 绑定库可能仍需要一些手动修改，但不会像纯 JNI 方法那么多。 若要详细了解 Java 绑定库，请参阅[绑定 Java 库](~/android/platform/binding-java-library/index.md)。 

第二种技术是使用 JNI，它的工作级别要低得多，但可便于对通常无法通过 Java 绑定库访问的 Java 方法进行更细致的控制和访问。 

第三种技术是将代码从 Java 移植到 C#，与前两种截然不同。 将代码从一种语言移植到另一种语言可能是非常费时费力的过程，但可以借助 Sharpen  工具来减少工作量。 Sharpen 是开放源代码的 Java 到 C# 转换器工具。 

## <a name="summary"></a>总结

本文档简要概述了一些在 Xamarin.Android 应用程序中重用 Java 库的不同方法。 具体介绍了绑定和可调用托管包装器的概念，并讨论了将 Java 代码移植到 C# 的方法。 

## <a name="related-links"></a>相关链接

- [体系结构](~/android/internals/architecture.md)
- [绑定 Java 库](~/android/platform/binding-java-library/index.md)
- [使用 JNI](~/android/platform/java-integration/working-with-jni.md)
- [锐化](https://github.com/slluis/sharpen)
- [Java 本机接口](https://docs.oracle.com/javase/7/docs/technotes~/jni/index.html)
