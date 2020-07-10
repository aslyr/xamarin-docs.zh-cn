---
title: 使用 Javadoc 命名参数
description: 本文说明如何使用 Java 项目生成的 Javadoc 恢复 Java 绑定项目中的参数名称。
ms.prod: xamarin
ms.assetid: 59E8EF16-1322-486A-BB16-353804B77356
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/20/2017
ms.openlocfilehash: bcfc5778ed4e5486d188f4eefbd32811792b44e5
ms.sourcegitcommit: a3f13a216fab4fc20a9adf343895b9d6a54634a5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/02/2020
ms.locfileid: "85852982"
---
# <a name="naming-parameters-with-javadoc"></a>使用 Javadoc 命名参数

> [!IMPORTANT]
> 我们当前正在调查 Xamarin 平台上的自定义绑定使用情况。 请参与[此调查](https://www.surveymonkey.com/r/KKBHNLT)，告诉我们将来应该进行哪些开发工作。

_本文说明如何使用 Java 项目生成的 Javadoc 恢复 Java 绑定项目中的参数名称。_

## <a name="overview"></a>概述

绑定现有的 Java 库时，与绑定 API 相关的一些元数据会丢失。 方法的参数名称尤其会出现这种情况。 参数名称将显示为 `p0`、`p1` 等。这是因为 Java `.class` 文件不保留 Java 源代码中使用的参数名称。 

如果 Xamarin.Android Java 绑定项目可以从原始库访问 Javadoc HTML，则可以提供参数名称。 

## <a name="integrating-javadoc-html-into-a-java-binding-project"></a>将 Javadoc HTML 集成到 Java 绑定项目中

将 Javadoc HTML 集成到 Java 绑定项目是一个手动过程，包括以下步骤： 

1. 下载库的 Javadoc
2. 编辑 `.csproj` 文件并添加 `<JavaDocPaths>` 属性：
3. 清理并重建项目

完成此操作后，原始 Java 参数名称应出现在由 Java 绑定项目绑定的 API 中。 

> [!NOTE]
> JavaDoc 输出的内容各异。 .JAR 绑定工具链不能全面支持每个可能的排列，因此某些参数命名可能不正确。

## <a name="summary"></a>总结

本文介绍如何在 Java 绑定项目中使用 Javadoc 为绑定 API 提供有效参数名称。 
