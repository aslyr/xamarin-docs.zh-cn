---
title: Xamarin.Forms DependencyService
description: Xamarin.Forms DependencyService 类是允许 Xamarin.Forms 应用程序从共享代码调用本机平台功能的服务定位器。
ms.prod: xamarin
ms.assetid: 403479F2-6751-41F2-ADCE-3AF595062FE4
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/05/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 126e2d2373bad923fe1d66fe355ad811c15fbe4f
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/18/2020
ms.locfileid: "84138367"
---
# <a name="xamarinforms-dependencyservice"></a>Xamarin.Forms DependencyService

## <a name="introduction"></a>[介绍](introduction.md)

[`DependencyService`](xref:Xamarin.Forms.DependencyService) 类是允许 Xamarin.Forms 应用程序从共享代码调用本机平台功能的服务定位器。

## <a name="registration-and-resolution"></a>[注册及解决方法](registration-and-resolution.md)

必须使用 [`DependencyService`](xref:Xamarin.Forms.DependencyService) 注册平台实现，然后从共享代码进行解析，才能调用它们。

## <a name="picking-a-photo-from-the-library"></a>[从库中选取照片](photo-picker.md)

本文介绍如何使用 Xamarin.Forms [`DependencyService`](xref:Xamarin.Forms.DependencyService) 类从手机的图片库中选取照片。
