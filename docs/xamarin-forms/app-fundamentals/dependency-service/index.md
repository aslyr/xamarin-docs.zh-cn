---
title: title:"Xamarin.FormsDependencyService”说明：“Xamarin.Forms DependencyService 类是允许 Xamarin.Forms 应用程序从共享代码调用本机平台功能的服务定位器。”
description: 'ms.prod: xamarin ms.assetid:403479F2-6751-41F2-ADCE-3AF595062FE4 ms.technology: xamarin-forms author: davidbritch ms.author: dabritch ms.date:2019/06/05 no-loc: [Xamarin.Forms, Xamarin.Essentials]'
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
ms.sourcegitcommit: ea9269b5d9e3d68b61bb428560a10034117ee457
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/10/2020
ms.locfileid: "84138367"
---
# <a name="xamarinforms-dependencyservice"></a>Xamarin.Forms DependencyService

## <a name="introduction"></a>[介绍](introduction.md)

[`DependencyService`](xref:Xamarin.Forms.DependencyService) 类是允许 Xamarin.Forms 应用程序从共享代码调用本机平台功能的服务定位器。

## <a name="registration-and-resolution"></a>[注册及解决方法](registration-and-resolution.md)

必须使用 [`DependencyService`](xref:Xamarin.Forms.DependencyService) 注册平台实现，然后从共享代码进行解析，才能调用它们。

## <a name="picking-a-photo-from-the-library"></a>[从库中选取照片](photo-picker.md)

本文介绍如何使用 Xamarin.Forms [`DependencyService`](xref:Xamarin.Forms.DependencyService) 类从手机的图片库中选取照片。
