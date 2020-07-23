---
title: ApiDefinitions & StructsAndEnums 文件
description: '本文档介绍了客观 Sharpie 生成的 ApiDefinitions.cs 和 StructsAndEnums.cs 文件。 然后，将使用这些文件来访问 c # 中的目标 C 代码。'
ms.prod: xamarin
ms.assetid: AC2087C0-BA54-46D8-B70C-6972941C8F73
author: davidortinau
ms.author: daortin
ms.date: 03/29/2017
ms.openlocfilehash: 8d4a05745d8d2ec6e05abd519aef4b9827655e06
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/22/2020
ms.locfileid: "86930424"
---
# <a name="apidefinitions--structsandenums-files"></a>ApiDefinitions & StructsAndEnums 文件

如果客观 Sharpie 已成功运行，则会生成 `Binding/ApiDefinitions.cs` 和 `Binding/StructsAndEnums.cs` 文件。
这两个文件将添加到 Visual Studio for Mac 中的绑定项目，或者直接传递到 `btouch` 或 `bmac` 工具生成最终绑定。

在*某些*情况下，这些生成的文件可能都是您所需要的，但是，开发人员更频繁地需要手动修改这些生成的文件来修复任何无法由该工具自动处理的问题（例如，用[ `Verify` 特性](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md)标记的那些问题）。

一些后续步骤包括：

- **调整名称**：有时需要调整方法和类的名称，以匹配 .NET Framework 设计准则。
- **方法或属性**：目标 Sharpie 使用的试探法有时会选取要转换为属性的方法。 此时，您可以确定这是否是预期的行为。
- **挂钩事件**：可将类与委托类链接，并为这些类自动生成事件。
- **挂钩通知**：不能从纯头文件中提取通知的 API 协定，这将需要到 API 文档的行程。 如果需要强类型的通知，则需要更新结果。
- **API 特选**：此时，你可以选择提供额外的构造函数、添加方法（以允许 c # 初始化构造语法）、运算符重载并在额外定义文件上实现你自己的接口。

请参阅[绑定 API](~/cross-platform/macios/binding/objective-c-libraries.md)说明，了解如何将这些文件放入绑定进程中，如下图所示：

![此关系图中显示了绑定过程](apidefinitions-structsandenums-images/binding-flowchart.png)

有关这些文件的内容的详细信息，请参阅[绑定类型参考](~/cross-platform/macios/binding/binding-types-reference.md)。
