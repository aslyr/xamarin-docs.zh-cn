---
ms.openlocfilehash: 22a8542c0e829db8b889abc2c7fe5f5ab9d19ed4
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/13/2020
ms.locfileid: "69527636"
---

在 Visual Studio 和 Mac 生成代理中生成 iOS 应用时，.app 应用程序包不会复制回 Windows 计算机。 Xamarin Tools for Visual Studio 7.4 新增了 `CopyAppBundle` 属性，以便 CI 内部版本可以将 .app 应用程序包复制回 Windows。

若要使用此功能，请将 `CopyAppBundle` 属性添加到 .csproj 中要应用此功能的属性组下。 例如，下面的示例展示了如何为定目标到“iPhoneSimulator”  的“调试”  内部版本，将 .app 应用程序包复制回 Windows 计算机：

```xml
<PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Debug|iPhoneSimulator' ">
    <CopyAppBundle>true</CopyAppBundle>
</PropertyGroup>
```
