---
title: 文本在Xamarin.Forms
description: Xamarin.Forms具有三个用于处理文本的主要视图，本文介绍如何使用它们在应用程序中输入和显示文本 Xamarin.Forms 。
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 579ff44e9c58d7eea538d5478e99b4c480d44ac0
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/28/2020
ms.locfileid: "84136183"
---
# <a name="text-in-xamarinforms"></a>文本在Xamarin.Forms

[![下载示例](~/media/shared/download.png) 下载示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)

_使用 Xamarin.Forms 输入或显示文本。_

Xamarin.Forms有三个主要视图用于处理文本：

- **[标签](#Label)** &mdash;用于呈现单行或多行文本。 可以在同一行中显示具有多个格式设置选项的文本。
- **[条目](#Entry)** &mdash;用于输入只是一行的文本。 条目具有密码模式。
- **[编辑器](#Editor)** &mdash;用于输入可能需要多行的文本。

可以使用内置或自定义[样式](#Styles)更改文本外观，某些控件支持自定义[字体](#Fonts)。

<a name="Label" />

## <a name="label"></a>[标签](label.md)

`Label`视图用于显示文本。 它可以显示多行文本或一行文本。 `Label`可以使用内联中使用的多个格式设置选项呈现文本。 当标签视图无法容纳在一行上时，它可以换行或截断文本。

![标签示例](images/label.png)

有关更多详细信息，请参阅[标签](label.md)文章。

有关自定义标签中使用的字体的信息，请参阅[字体](fonts.md)。

<a name="Entry" />

## <a name="entry"></a>[条目](entry.md)

`Entry`用于接受单行文本输入。 `Entry`提供对颜色和字体的控制。 `Entry`具有密码模式，并且可以在输入文本之前显示占位符文本。

![条目示例](images/entry.png)

有关详细[信息，请参阅文章。](entry.md)

请注意，与不同 `Label` ， `Entry` 不能有自定义字体设置。

<a name="Editor" />

## <a name="editor"></a>[编辑器](editor.md)

`Editor`用于接受多行文本输入。 `Editor`提供对颜色和字体的控制。

![编辑器示例](images/editor.png)

有关详细信息，请参阅[编辑器](editor.md)文章。

<a name="Fonts" />

## <a name="fonts"></a>[字体](fonts.md)

许多控件都支持在每个平台上使用内置字体的不同字体设置，或应用中包含的自定义字体。 有关更多详细信息，请参阅[字体](fonts.md)一文。

<a name="Styles" />

## <a name="styles"></a>[样式](styles.md)

请参阅使用[样式](~/xamarin-forms/user-interface/styles/index.md)了解如何设置应用于多个控件的字体、[颜色](~/xamarin-forms/user-interface/colors.md)和其他显示属性。

## <a name="related-links"></a>相关链接

- [文本（示例）](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)
