---
title: 已知问题 & 解决方法
description: 本文档介绍 Xamarin Workbooks 的已知问题和解决方法。 它讨论了 CultureInfo 问题、JSON 问题等。
ms.prod: xamarin
ms.assetid: 495958BA-C9C2-4910-9BAD-F48A425208CF
author: davidortinau
ms.author: daortin
ms.date: 03/30/2017
ms.openlocfilehash: aa6bbf9336acbff8558744220b9f4b8634f46421
ms.sourcegitcommit: 952db1983c0bc373844c5fbe9d185e04a87d8fb4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/23/2020
ms.locfileid: "86997223"
---
# <a name="known-issues--workarounds"></a>已知问题 & 解决方法

## <a name="persistence-of-cultureinfo-across-cells"></a>单元格之间的 CultureInfo 持久性

`System.Threading.CurrentThread.CurrentCulture` `System.Globalization.CultureInfo.CurrentCulture` 由于[mono 的实现中的 `AppContext.SetSwitch` Bug][appcontext-bug] ，在基于 mono 的工作簿目标（Mac、iOS 和 Android）上，对工作簿单元格的设置或不会保持不变。

### <a name="workarounds"></a>解决方法

- 设置应用程序-域本地 `DefaultThreadCurrentCulture` ：

```csharp
using System.Globalization;
CultureInfo.DefaultThreadCurrentCulture = new CultureInfo("de-DE")
```

- 或者，更新为1.2.1 或更高版本的工作簿，这将重写对和的分配， `System.Threading.CurrentThread.CurrentCulture` `System.Globalization.CultureInfo.CurrentCulture` 以提供所需的行为（围绕 Mono 错误）。

## <a name="unable-to-use-newtonsoftjson"></a>无法使用 Newtonsoft.Js

### <a name="workaround"></a>解决方法

- 更新到将在9.0.1 上安装 Newtonsoft.Js的1.2.1 工作簿。
  当前在 alpha 通道中的工作簿1.3 支持版本10和更高版本。

### <a name="details"></a>详细信息

已升级上的 Newtonsoft.Js发布，它会使其依赖于与提供给支持的工作簿版本冲突 `dynamic` 。 此问题在工作簿1.3 预览版本中进行了介绍，但现在我们通过将 Newtonsoft.Js固定到9.0.1 版来解决此问题。

根据10或更高版本上的 Newtonsoft.Js明确的 NuGet 包仅在 alpha 通道中的工作簿1.3 中受支持。

## <a name="code-tooltips-are-blank"></a>代码工具提示为空

在用于 Mac 工作簿应用程序的 "Safari/WebKit" 中，在["摩纳哥编辑器"][monaco-bug]中有一个 bug，该 bug 会导致不带文本的代码工具提示呈现。

![带有文本的摩纳哥工具提示呈现](general-images/monaco-signature-help-bug.png)

### <a name="workaround"></a>解决方法

- 在工具提示显示后单击它将强制呈现文本。

- 或更新到1.2.1 或更高版本的工作簿

[appcontext-bug]: https://bugzilla.xamarin.com/show_bug.cgi?id=54448
[monaco-bug]: https://github.com/Microsoft/monaco-editor/issues/408

## <a name="skiasharp-renderers-are-missing-in-workbooks-13"></a>工作簿1.3 中缺少 SkiaSharp 呈现器

从工作簿1.3 开始，我们移除了在工作簿0.99.0 中提供的 SkiaSharp 呈现器，并使用我们的[SDK](~/tools/workbooks/sdk/index.md)来支持提供呈现器的 SkiaSharp。

### <a name="workaround"></a>解决方法

- 将 SkiaSharp 更新为 NuGet 中的最新版本。 撰写本文时，这是1.57.1 的。

## <a name="related-links"></a>相关链接

- [报告 Bug](~/tools/workbooks/install.md#reporting-bugs)
