---
title: Xamarin.Forms常见问题
ms.topic: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: edd6cfefe18ff3d5cc97ec58f3bce867f11df7c8
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/28/2020
ms.locfileid: "84135871"
---
# <a name="xamarinforms-frequently-asked-questions"></a>Xamarin.Forms常见问题

## <a name="can-i-update-the-xamarinforms-default-template-to-a-newer-nuget-packageupdate-forms-templatemd"></a>[是否可以将 Xamarin.Forms 默认模板更新到较新的 NuGet 包？](update-forms-template.md)
本指南使用 Xamarin.Forms .NET Standard 库模板作为示例，但相同的常规方法也适用于 Xamarin.Forms 共享项目模板。

## <a name="why-doesnt-the-visual-studio-xaml-designer-work-for-xamarinforms-xaml-filesforms-xaml-designermd"></a>[为什么 Visual Studio XAML 设计器不适用于 Xamarin.Forms xaml 文件？](forms-xaml-designer.md)
Xamarin.Forms当前不支持 XAML 文件的可视化设计器。

## <a name="android-build-error-the-linkassemblies-task-failed-unexpectedly"></a>[Android 生成错误： "LinkAssemblies" 任务意外失败](android-linkassemblies-error.md)
`The "LinkAssemblies" task failed unexpectedly`生成使用窗体的 Xamarin Android 项目时，可能会看到一条错误消息。 当链接器处于活动状态时（通常在*发布*版本上），会发生这种情况，以减小应用包的大小;这种情况发生是因为 Android 目标不会更新为最新框架。 

## <a name="why-does-my-xamarinformsmaps-android-project-fail-with-compiletodalvik--unexpected-top-level-errormaps-compiletodalvik-errormd"></a>["为什么要这样做 Xamarin.Forms 。地图 Android 项目失败，出现 COMPILETODALVIK：意外的顶级错误？ "](maps-compiletodalvik-error.md)
此错误可能出现在 Visual Studio for Mac 的错误板中，或在 Visual Studio 的 "生成输出" 窗口中。在 Android 项目中使用 Xamarin.Forms 。地图. 最常见的解决方法是增加 Xamarin Android 项目的 Java 堆大小。
