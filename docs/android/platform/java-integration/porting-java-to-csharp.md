---
title: 将 Java 移植到 C# for Xamarin.Android
description: 在 Xamarin.Android 应用程序中使用 Java 的第三个方法是将 Java 源代码移植到 C#。
ms.prod: xamarin
ms.assetid: 39E528BD-010F-47FC-BE48-8E7848E30454
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 04/05/2016
ms.openlocfilehash: 8f96fcc4aadcd8f082d55dc568b2517f048edaf2
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/10/2020
ms.locfileid: "73027192"
---
# <a name="porting-java-to-c-for-xamarinandroid"></a>将 Java 移植到 C# for Xamarin.Android

这种方法可能会受到以下组织的青睐：

- **正在将技术堆栈从 Java 切换到 C# 的组织。**
- **必须维护同一产品的 C# 版和 Java 版的组织。**
- **希望拥有 .NET 版常用 Java 库的组织。**

可以通过两种方法将 Java 代码移植到 C#。 第一种方法是手动移植代码。 这种方法需要由熟练的开发人员操作，他们既了解 .NET 和 Java，又熟悉每种语言的正确用法。 这种方法最适用于移植少量代码或希望完全从 Java 迁移到 C# 的组织。

第二种移植方法是使用代码转换器（例如，[Sharpen](https://github.com/mono/sharpen)）来尝试自动执行此过程。 [Sharpen](https://github.com/mono/sharpen) 是 Versant 的开放源代码转换器，最初用于将 db4o  的代码从 Java 移植到 C#。 db4o 是 Versant 使用 Java 开发然后移植到 .NET 的面向对象的数据库。 对于必须存在两种语言版本并且需要两者之间具有一定平衡性的项目，使用代码转换器可能更合适。

通过 [ngit](https://github.com/mono/ngit) 项目示例，可以看到自动代码转换工具的意义。
Ngit 是 Java 项目 [jgit](https://eclipse.org/) 的端口。
Jgit 本身是 [Git](https://git-scm.com/) 源代码管理系统的 Java 实现。 为了从 Java 生成 C# 代码，ngit 程序员使用自定义的自动化系统从 jgit 中提取 Java 代码，应用一些修补程序以适应转换过程，然后运行“Sharpen”，后者生成 C# 代码。 这使得 ngit 项目能够从在 jgit 上进行的连续且持续的工作中获益。

启动一个自动代码转换工具通常需要进行大量的工作，这可能会成为使用的障碍。 在许多情况下，手动将 Java 移植到 C# 可能会更容易。

## <a name="related-links"></a>相关链接

- [Sharpen 转换工具](https://github.com/mono/sharpen)
