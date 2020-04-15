---
title: Android Resource.designer.cs 文件不更新
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 3F7376E3-59CC-4722-AEED-BB50E4D952AA
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/19/2017
ms.openlocfilehash: 4683bbaa5aa48c7b5de5fb9a87a4cd3fbc0aeada
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/13/2020
ms.locfileid: "73019515"
---
# <a name="my-android-resourcedesignercs-file-will-not-update"></a>Android Resource.designer.cs 文件不更新

> [!NOTE]
> 此问题已在 Xamarin Studio 5.1.4 及更高版本中得到解决。 但如果 Visual Studio for Mac 中出现此问题，请提交[新的 bug](~/cross-platform/troubleshooting/questions/howto-file-bug.md) 并附上完整的版本控制信息和生成日志输出记录。

Xamarin.Studio 5.1 先前损坏的 .csproj 文件出现 bug，.csproj 文件中的部分或所有 xml 代码被删除。 这会导致 Android 生成系统（如更新 Android Resource.designer.cs）的重要操作失败。 从 7 月 15 日的 5.1.4 稳定版本开始，此 bug 已得到修复。但在许多情况下，必须手动修复项目文件，如下所述。

## <a name="two-possible-approaches-to-fixing-up-the-project-file"></a>可通过以下两种方法修复项目文件

**二者之一：**

1. 创建全新的 Xamarin.Android 应用程序项目，将所有项目属性设置为与旧项目一致，并将所有资源、源文件等添加回该项目。

   **或**

2. 创建原始项目 .csproj 文件的备份副本，然后在文本编辑器中将其打开，并从完全生成的 .csproj 文件中重新添加回缺少的元素。

### <a name="if-this-does-not-solve-the-problem"></a>如果该方法不能解决问题

尝试使用这些元素后，你可能会发现添加回元素并重新生成项目后 Resource.designer.cs 文件将会更新，但仍可能需要关闭并重新打开解决方案才能使代码完全识别 Resource.designer.cs 中包含的新类型。 
