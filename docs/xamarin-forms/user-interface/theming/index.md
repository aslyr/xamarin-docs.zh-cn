---
title: 为 Xamarin.Forms 应用程序设置主题
description: Xamarin。窗体应用程序通过为每个主题创建 ResourceDictionary 并使用 DynamicResource 标记扩展加载资源来支持主题。
ms.prod: xamarin
ms.assetId: BF92AEDD-EF23-4D08-A972-B089066E75F9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/22/2020
ms.openlocfilehash: 5988437b40ac875b8b59f9af0f25d4b5c60ded97
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "82517128"
---
# <a name="theming-a-xamarinforms-application"></a>主题： Xamarin 应用程序

## <a name="theme-an-application"></a>[主题应用程序](theming.md)

可以通过[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)为每个主题创建，然后使用`DynamicResource`标记扩展加载资源，在 Xamarin 应用程序中实现主题。

## <a name="respond-to-system-theme-changes"></a>[响应系统主题更改](system-theme-changes.md)

设备通常包括浅色和深色主题，它们分别引用可在操作系统级别设置的各种外观首选项。 应用程序应遵守这些系统主题，并在系统主题发生更改时立即做出响应。
