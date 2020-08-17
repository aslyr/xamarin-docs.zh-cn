---
title: Xamarin.Forms 手势
description: 本指南介绍如何使用 Xamarin.Forms 手势识别器检测用户与 Xamarin.Forms 应用程序中视图的交互。
ms.prod: xamarin
ms.assetid: 0E197A51-2304-4C09-A710-C7FF24A89F15
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/04/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 7528afd0971cf06eb69df4ed7c08c3fd6dcc9e22
ms.sourcegitcommit: 08290d004d1a7e7ac579bf1f96abf8437921dc70
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/07/2020
ms.locfileid: "87917819"
---
# <a name="no-locxamarinforms-gestures"></a>Xamarin.Forms 手势

可使用手势识别器检测用户与 Xamarin.Forms 应用程序中视图的交互。

Xamarin.Forms [`GestureRecognizer`](xref:Xamarin.Forms.GestureRecognizer) 类支持对 [`View`](xref:Xamarin.Forms.View) 实例使用点击、收缩、平移、轻扫和拖放手势。

## <a name="add-a-tap-gesture-recognizer"></a>[添加点击手势识别器](tap.md)

点击手势用于点击检测，可通过 [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) 类识别。

## <a name="add-a-pinch-gesture-recognizer"></a>[添加收缩手势识别器](pinch.md)

收缩手势用于执行交互式缩放，可通过 [`PinchGestureRecognizer`](xref:Xamarin.Forms.PinchGestureRecognizer) 类识别。

## <a name="add-a-pan-gesture-recognizer"></a>[添加平移手势识别器](pan.md)

平移手势用于检测手指在屏幕上的移动并将该移动应用于内容，可通过 [`PanGestureRecognizer`](xref:Xamarin.Forms.PanGestureRecognizer) 类识别。

## <a name="add-a-swipe-gesture-recognizer"></a>[添加轻扫手势识别器](swipe.md)

手指在水平或垂直方向上滑过屏幕即产生轻扫手势，该手势通常用于启动内容导航。 使用 [`SwipeGestureRecognizer`](xref:Xamarin.Forms.SwipeGestureRecognizer) 类可识别轻扫手势。

## <a name="add-a-drag-and-drop-gesture-recognizer"></a>[添加拖放手势识别器](drag-and-drop.md)

借助拖放手势，可以使用连续手势将项及其相关的数据包从屏幕上的一个位置拖放到另一个位置。 拖动和放置手势分别是使用 `DragGestureRecognizer` 类和 `DropGestureRecognizer` 类识别的。
