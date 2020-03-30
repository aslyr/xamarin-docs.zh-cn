---
title: 应用更改
description: 如何开始在 Xamarin Android 项目中使用“应用更改”。
ms.assetid: 38950B0C-8880-448F-B12E-08D5BFE87D0D
author: JonDouglas
ms.author: jodou
ms.date: 03/09/2020
ms.openlocfilehash: 03bace97908a53ac6112cd2600b20d429fe2f521
ms.sourcegitcommit: ec112800a76089ab1db66fe24b8bbcc510e067b4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/24/2020
ms.locfileid: "80215312"
---
# <a name="apply-changes"></a>应用更改

“应用更改”让你能够将资源更改推送到正在运行的应用，而无需重新启动应用。 当你想要部署和测试小型增量更改，同时保留设备或仿真器的当前状态时，这有助于控制重新启动应用的次数。

“应用更改”使用 [Android JVMTI 实现](https://docs.oracle.com/javase/8/docs/platform/jvmti/jvmti.html#bci)中的功能，运行 Android 8.0 （API 级别 26）或更高版本的设备或仿真器支持这些功能。

## <a name="requirements"></a>要求

下面的列表显示了使用“应用更改”的要求：

- **Visual Studio** - 在 Windows 上，请更新到 Visual Studio 2019 版本 16.5 或更高版本。 在 macOS 上，请更新到 Visual Studio 2019 for Mac 版本 8.5 或更高版本。
- **Xamarin.Android** - 必须随 Visual Studio 一起安装 Xamarin.Android 10.2 或更高版本（在 Windows 上，Xamarin.Android 将作为“使用 .NET 的移动开发”工作负载的一部分自动安装，并作为 Visual Studio for Mac 安装程序的一部分安装   ）。
- **Android SDK** - 必须通过 Android SDK 管理器安装 Android API 28 或更高版本。
- **目标设备或仿真器** - 设备或仿真器必须运行 Android 8.0（API 级别 26）或更高版本。

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

## <a name="get-started"></a>入门

若要开始使用“应用更改”，需要确保设备或仿真器运行 Android 8.0（API 级别 26）或更高版本。 然后，在调试或不调试的情况下运行 Android 应用程序。

你可以使用以下方法与“应用更改”交互：

1. 工具栏图标  。 你可以单击“应用更改”工具栏图标，将更改应用于目标设备或仿真器。

    [![应用更改 - 工具栏图标](apply-changes-images/Apply-Changes-Toolbar.png)](apply-changes-images/Apply-Changes-Toolbar.png#lightbox)

2. 键盘快捷方式  。 你可以使用 Shift + Alt + F5 键盘快捷方式，将更改应用于目标设备或仿真器  。
3. “调试”菜单  。 你可以使用“调试”>“应用更改”菜单项，将更改应用于目标设备或仿真器  。

    [![“应用更改”-“调试”菜单](apply-changes-images/Apply-Changes-Debug-Menu.png)](apply-changes-images/Apply-Changes-Debug-Menu.png#lightbox)

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

## <a name="get-started"></a>入门

若要开始使用“应用更改”，需要确保设备或仿真器运行 Android 8.0（API 级别 26）或更高版本。 然后，在调试或不调试的情况下运行 Android 应用程序。

你可以使用以下方法与“应用更改”交互：

1. 键盘快捷方式  。 你可以使用 ⌥ + ⇧ + R 键盘快捷方式，将更改应用于目标设备或仿真器  。
2. “运行”菜单  。 你可以使用“运行”>“应用更改”菜单项，将更改应用于目标设备或仿真器  。

    [![“应用更改”-“调试”菜单](apply-changes-images/Apply-Changes-Debug-Menu-Mac.png)](apply-changes-images/Apply-Changes-Debug-Menu-Mac.png#lightbox)

-----

## <a name="limitations"></a>限制

以下更改需要重新启动应用程序：

- 更改 C# 代码。
- 添加或删除资源。
- 更改 AndroidManifest.xml。
- 更改本机库（.so 文件）。

## <a name="related-links"></a>相关链接

- [应用更改](https://developer.android.com/studio/run#apply-changes)
