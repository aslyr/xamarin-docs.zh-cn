---
title: Xamarin.Forms使用 XAML 样式设置应用样式
description: 本指南说明如何 Xamarin.Forms 使用 XAML 样式自定义应用程序的外观。
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 72effe15d3456b5a48cbf5d09e889600134ac686
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/28/2020
ms.locfileid: "84138796"
---
# <a name="styling-xamarinforms-apps-using-xaml-styles"></a>Xamarin.Forms使用 XAML 样式设置应用样式

## <a name="introduction"></a>[简介](introduction.md)

Xamarin.Forms应用程序通常包含多个具有相同外观的控件。 设置各个控件的外观可能是重复性的并且容易出错。 相反，可以通过对控件类型进行分组和设置属性来创建自定义控件外观的样式。

## <a name="explicit-styles"></a>[显式样式](explicit.md)

*显式*样式是通过设置控件的属性有选择地应用于控件的样式 [`Style`](xref:Xamarin.Forms.NavigableElement.Style) 。

## <a name="implicit-styles"></a>[隐式样式](implicit.md)

*隐式*样式是指所有相同的控件使用的样式 [`TargetType`](xref:Xamarin.Forms.Style.TargetType) ，无需每个控件都引用该样式。

## <a name="global-styles"></a>[全局样式](application.md)

可以通过将样式添加到应用程序的来全局使用样式 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 。 这有助于避免跨页面或控件的样式的重复。

## <a name="style-inheritance"></a>[样式继承](inheritance.md)

样式可以从其他样式继承，以减少重复并实现重复使用。

## <a name="dynamic-styles"></a>[动态样式](dynamic.md)

样式不会对属性更改做出响应，并在应用程序持续时间内保持不变。 但是，应用程序可以使用动态资源在运行时动态响应样式更改。

## <a name="device-styles"></a>[设备样式](device.md)

Xamarin.Forms在类中包含六个*动态*样式，称为*设备*样式 [`Devices.Styles`](xref:Xamarin.Forms.Device.Styles) 。 所有六种样式只能应用于 [`Label`](xref:Xamarin.Forms.Label) 实例。

## <a name="style-classes"></a>[样式类](style-class.md)

Xamarin.Forms样式类可以将多个样式应用于控件，而无需采用样式继承。
