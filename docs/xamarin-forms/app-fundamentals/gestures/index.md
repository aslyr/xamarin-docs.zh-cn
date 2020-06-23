---
title: Xamarin.Forms 手势
description: 本指南介绍如何使用 Xamarin.Forms 手势识别器检测用户与 Xamarin.Forms 应用程序中视图的交互。
ms.prod: xamarin
ms.assetid: 0E197A51-2304-4C09-A710-C7FF24A89F15
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/25/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 5e1e93f74ab8ef6d63213a8fbdc7ec45a794cf55
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/18/2020
ms.locfileid: "84137872"
---
# <a name="xamarinforms-gestures"></a>Xamarin.Forms 手势

可使用手势识别器检测用户与 Xamarin.Forms 应用程序中视图的交互。

Xamarin.Forms [`GestureRecognizer`](xref:Xamarin.Forms.GestureRecognizer) 类支持在 [`View`](xref:Xamarin.Forms.View) 实例上使用点击、收缩、平移和轻扫手势。

## <a name="adding-a-tap-gesture-recognizer"></a>[添加点击手势识别器](tap.md)

点击手势用于点击检测，可通过 [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) 类识别。

## <a name="adding-a-pinch-gesture-recognizer"></a>[添加收缩手势识别器](pinch.md)

收缩手势用于执行交互式缩放，可通过 [`PinchGestureRecognizer`](xref:Xamarin.Forms.PinchGestureRecognizer) 类识别。

## <a name="adding-a-pan-gesture-recognizer"></a>[添加平移手势识别器](pan.md)

平移手势用于检测手指在屏幕上的移动并将该移动应用于内容，可通过 [`PanGestureRecognizer`](xref:Xamarin.Forms.PanGestureRecognizer) 类识别。

## <a name="adding-a-swipe-gesture-recognizer"></a>[添加轻扫手势识别器](swipe.md)

手指在水平或垂直方向上滑过屏幕即产生轻扫手势，该手势通常用于启动内容导航。 使用 [`SwipeGestureRecognizer`](xref:Xamarin.Forms.SwipeGestureRecognizer) 类可识别轻扫手势。
