---
title: 控件引用
description: '用于构造应用程序的所有用户界面元素的说明 :::no-loc(Xamarin.Forms)::: 。 本文列出了构成应用程序的用户界面的控件组 :::no-loc(Xamarin.Forms)::: 。'
ms.prod: xamarin
ms.assetid: F2A02DEE-7137-42F4-9C0A-4E1CF75EA08F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/08/2019
no-loc:
- ':::no-loc(Xamarin.Forms):::'
- ':::no-loc(Xamarin.Essentials):::'
ms.openlocfilehash: b9e3f8b61ebfc73a26a967b83f60e005a652563f
ms.sourcegitcommit: 952db1983c0bc373844c5fbe9d185e04a87d8fb4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/23/2020
ms.locfileid: "86997366"
---
# <a name="controls-reference"></a>控件引用

[![下载示例](~/media/shared/download.png) 下载示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery/)

应用程序的用户界面 :::no-loc(Xamarin.Forms)::: 是构造的对象，这些对象映射到每个目标平台的本机控件。 这使得适用于 iOS、Android 和通用 Windows 平台的特定于平台的应用程序使用 :::no-loc(Xamarin.Forms)::: [.NET Standard 库](~/cross-platform/app-fundamentals/net-standard.md)中包含的代码。

用于创建应用程序的用户界面的四个主要控件组 :::no-loc(Xamarin.Forms)::: 如下所示：

- [**页面**](pages.md)
- [**布局**](layouts.md)
- [**视图**](views.md)
- [**单元**](cells.md)

:::no-loc(Xamarin.Forms):::页面通常占据整个屏幕。 页面通常包含一个布局，其中包含视图和其他布局。 单元是用于与和连接的专用 [`TableView`](xref::::no-loc(Xamarin.Forms):::.TableView) 组件 [`ListView`](xref::::no-loc(Xamarin.Forms):::.ListView) 。 类图显示了通常用于在中生成用户界面的类型的层次结构， :::no-loc(Xamarin.Forms)::: 可在[ :::no-loc(Xamarin.Forms)::: Controls 类层次结构](~/xamarin-forms/internals/class-hierarchy.md)找到。

在有关[**页面**](pages.md)、[**布局**](layouts.md)、[**视图**](views.md)和[**单元格**](cells.md)的四篇文章中，介绍了每种类型的控件，并提供了指向其 API 文档的链接、描述其使用的文章（如果存在）以及一个或多个示例程序（如果存在）。 每种类型的控件还附带一个屏幕截图，其中显示了在 iOS 和 Android 设备上运行的[**FormsGallery**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)示例页面。 每个屏幕截图下面都链接到 "c #" 页的源代码、等效的 XAML 页以及 XAML 页的 c # 代码隐藏文件（如果适用）。

> [!NOTE]
> 页面、布局和视图派生自 `VisualElement` 类。 `VisualElement`类提供各种可用于派生类的属性、方法和事件。 有关详细信息，请参阅[VisualElement 属性、方法和事件](common-properties.md)。

除了提供的控件外 :::no-loc(Xamarin.Forms)::: ，第三方控件也可用。 有关详细信息，请参阅[第三方控件](thirdparty.md)。

## <a name="related-links"></a>相关链接

- [:::no-loc(Xamarin.Forms):::FormsGallery 示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)
- [:::no-loc(Xamarin.Forms):::Controls 类层次结构](~/xamarin-forms/internals/class-hierarchy.md)
- [API 文档](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
