---
title: Xamarin.Forms 导航
description: 本指南介绍如何在 Xamarin.Forms 应用中执行导航。 Xamarin.Forms 提供多种不同的页面导航体验，具体取决于所使用的页面类型。
ms.prod: xamarin
ms.assetid: BC5D0C6C-D5A9-4B12-A492-ED1F570CEC87
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 65aa0f060e4d48834017a334d69b2f21645825f3
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/22/2020
ms.locfileid: "86937184"
---
# <a name="xamarinforms-navigation"></a>Xamarin.Forms 导航

Xamarin.Forms 提供多种不同的页面导航体验，具体取决于所使用的页面类型。

![Xamarin.Forms 页面类型](images/page-types.png)

或者，Xamarin.Forms Shell 应用程序使用基于 URI 的导航体验（不强制使用设置的导航层次结构）。 有关详细信息，请参阅 [Xamarin.Forms Shell 导航](~/xamarin-forms/app-fundamentals/shell/navigation.md)。

## <a name="hierarchical-navigation"></a>[分层导航](hierarchical.md)

[`NavigationPage`](xref:Xamarin.Forms.NavigationPage) 类提供分层导航体验，用户可以随心所欲地向前或向后导航页面。 此类将导航实现为 [`Page`](xref:Xamarin.Forms.Page) 对象的后进先出 (LIFO) 堆栈。

## <a name="tabbedpage"></a>[TabbedPage](tabbed-page.md)

Xamarin.Forms [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) 包含一系列选项卡和较大的详细信息区域，其中每个选项卡都可将内容加载到详细信息区域。

## <a name="carouselpage"></a>[CarouselPage](carousel-page.md)

Xamarin.Forms [`CarouselPage`](xref:Xamarin.Forms.CarouselPage) 页很像一个库，用户可从一侧轻扫到另一侧以浏览内容页面。

## <a name="masterdetailpage"></a>[MasterDetailPage](master-detail-page.md)

Xamarin.Forms [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) 页面用于管理两个相关信息页，其中一个为显示项的母版页，另一个为详细信息页，显示母版页上各项的详细信息。

## <a name="modal-pages"></a>[模式页](modal.md)

Xamarin.Forms 还支持模式页面。 模式页面鼓励用户完成独立任务，在完成或取消该任务之前，不允许导航离开该任务。
