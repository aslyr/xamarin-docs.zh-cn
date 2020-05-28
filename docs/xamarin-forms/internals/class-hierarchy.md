---
title: Xamarin.FormsControls 类层次结构
description: 开发人员应熟悉用于创建应用程序用户界面的类型的层次结构 Xamarin.Forms 。
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 0087e2bb81c7c9204a782519a9eeb9891adc297a
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/28/2020
ms.locfileid: "84138629"
---
# <a name="xamarinforms-controls-class-hierarchy"></a>Xamarin.FormsControls 类层次结构

Xamarin.Forms由多个命名空间中的数百个类型组成。 开发人员应最熟悉用于创建应用程序的用户界面的类型的层次结构 Xamarin.Forms ，该应用程序位于 `Xamarin.Forms` 命名空间中。

这些类型可以划分为页、布局、视图和单元格。 Xamarin.Forms页面通常占用整个屏幕，所有页面类型都派生自 [`Page`](xref:Xamarin.Forms.Page) 类。 页面通常包含布局，所有布局类型都派生自 [`Layout`](xref:Xamarin.Forms.Layout) 类。 布局通常包含视图和其他布局，并且所有视图类型最终都派生自 [`View`](xref:Xamarin.Forms.View) 类。 最后，单元是用于在和控件的显示数据中使用的专用控件 [`TableView`](xref:Xamarin.Forms.TableView) [`ListView`](xref:Xamarin.Forms.ListView) 。 页面、布局、视图和单元均最终派生自 [`Element`](xref:Xamarin.Forms.Element) 类。

下面的类图显示了通常用于在中生成用户界面的类型的层次结构 Xamarin.Forms ：

[![Xamarin.Forms控件类图](class-hierarchy-images/class-diagram.png "[!基金.非 LOC （Xamarin）] 控件类图")](class-hierarchy-images/class-diagram-large.png#lightbox "[!基金.非 LOC （Xamarin）] 控件类图")

> [!NOTE]
> 可从[此处](class-hierarchy-images/class-diagram-high-resolution.png)下载类图的高分辨率版本。 但请注意，该关系图只显示一种 Shell 类型。

## <a name="related-links"></a>相关链接

- [Xamarin.Forms控件引用](~/xamarin-forms/user-interface/controls/index.md)
