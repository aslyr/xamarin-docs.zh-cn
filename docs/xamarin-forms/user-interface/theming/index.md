---
title: 主题 Xamarin.Forms 应用程序
description: Xamarin.Forms应用程序通过为每个主题创建 ResourceDictionary 并使用 DynamicResource 标记扩展加载资源来支持主题。
ms.prod: xamarin
ms.assetId: BF92AEDD-EF23-4D08-A972-B089066E75F9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/22/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 80660ae7d3af0fe5948a5ae4ffdb35d2f9c2a40f
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/18/2020
ms.locfileid: "84136131"
---
# <a name="theming-a-xamarinforms-application"></a>主题 Xamarin.Forms 应用程序

## <a name="theme-an-application"></a>[主题应用程序](theming.md)

可以 Xamarin.Forms 通过 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 为每个主题创建，然后使用标记扩展加载资源，在应用程序中实现主题 `DynamicResource` 。

## <a name="respond-to-system-theme-changes"></a>[响应系统主题更改](system-theme-changes.md)

设备通常包括浅色和深色主题，它们分别引用可在操作系统级别设置的各种外观首选项。 应用程序应遵守这些系统主题，并在系统主题发生更改时立即做出响应。
