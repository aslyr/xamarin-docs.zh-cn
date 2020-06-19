---
title: 为什么 Visual Studio XAML 设计器不适用于 Xamarin.Forms xaml 文件？
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: cab2eefb-c52f-4d81-866e-8f1feabbdd64
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 9f9cf570da0d8e078d1d05e74dc0f65994080c6c
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/18/2020
ms.locfileid: "84135884"
---
# <a name="why-doesnt-the-visual-studio-xaml-designer-work-for-xamarinforms-xaml-files"></a>为什么 Visual Studio XAML 设计器不适用于 Xamarin.Forms xaml 文件？

Xamarin.Forms当前不支持 XAML 文件的可视化设计器。 因此，当尝试使用编码在 Visual Studio 的*XAML Ui 设计器*或*Xaml ui 设计器*中打开窗体 XAML 文件时，将引发以下错误消息：

> "无法用所选编辑器打开此文件。 请选择另一个编辑器。 "

[ Xamarin.Forms XAML 基础](~/xamarin-forms/xaml/xaml-basics/index.md)指南中介绍了这种限制：

> "尚不存在用于在应用程序中生成 XAML 的可视化设计器 Xamarin.Forms ，因此必须手动编写所有 xaml。"

但是， Xamarin.Forms 可以通过选择 "**视图" > 其他 Windows > Xamarin.Forms 预览**器菜单选项来显示 XAML 预览程序。
