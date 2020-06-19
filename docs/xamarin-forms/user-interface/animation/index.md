---
title: 动画Xamarin.Forms
description: Xamarin.Forms包括其自己的动画基础结构，它可以简单地创建简单的动画，同时也足以创建复杂的动画。
ms.prod: xamarin
ms.assetid: AC0B4127-ECA3-44DA-8A24-A2B10A275083
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/14/2016
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 88a671c4d28d62a5f73e90a7b2fa9c45b7dbe8b1
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/18/2020
ms.locfileid: "84128994"
---
# <a name="animation-in-xamarinforms"></a>动画Xamarin.Forms

_Xamarin 包含自己的动画基础结构，它可以简单地创建简单的动画，同时也足以创建复杂的动画。_

Xamarin.Forms动画类以不同的视觉对象属性为目标，典型动画会在一段时间内逐渐将属性从一个值更改为另一个值。 请注意，动画类没有 XAML 接口 Xamarin.Forms 。 但是，动画可以封装在[行为](~/xamarin-forms/app-fundamentals/behaviors/index.md)中，然后从 XAML 进行引用。

## <a name="simple-animations"></a>[简单动画](simple.md)

[`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions)类提供扩展方法，这些方法可用于构造旋转、缩放、平移和淡化实例的简单动画 [`VisualElement`](xref:Xamarin.Forms.VisualElement) 。 本文演示如何使用类创建和取消动画 `ViewExtensions` 。

## <a name="easing-functions"></a>[缓动函数](easing.md)

Xamarin.Forms包含一个 [`Easing`](xref:Xamarin.Forms.Easing) 类，它允许您指定一个传输函数，该函数控制动画在运行时的速度如何提高或减速。 本文演示如何使用预定义的缓动函数，以及如何创建自定义缓动函数。

## <a name="custom-animations"></a>[自定义动画](custom.md)

[`Animation`](xref:Xamarin.Forms.Animation)类是所有动画的构建基块 Xamarin.Forms ，类中的扩展方法可 [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) 创建一个或多个 `Animation` 对象。 本文演示了如何使用 `Animation` 类创建和取消动画，同步多个动画，以及创建自定义动画，以对未被现有动画方法进行动画处理的属性进行动画处理。
