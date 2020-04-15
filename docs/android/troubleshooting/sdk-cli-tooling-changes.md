---
title: 对 Android SDK 工具的更改
description: 对 Android SDK 管理已安装的 API 级别和 AVD 的方式的更改。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5AC61C00-0FF6-4C2D-80E7-D67A3EE30A5A
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/21/2018
ms.openlocfilehash: 4c559a76d7354fd957d065717ef14d91591d1be0
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/13/2020
ms.locfileid: "73019498"
---
# <a name="changes-to-the-android-sdk-tooling"></a>对 Android SDK 工具的更改

_对 Android SDK 管理已安装的 API 级别和 AVD 的方式的更改。_

## <a name="changes-to-android-sdk-tooling"></a>对 Android SDK 工具的更改

在适用于 Android 的最新版 SDK 工具中，Google 已删除了现有 AVD/SDK 管理器，以支持新的 CLI（命令行接口）工具。 android  程序已被删除，Visual Studio for Mac 和早期版本的 Visual Studio Tools for Xamarin 中的 Google GUI（图形用户界面）管理器将不再适用于 Android SDK Tools 版本 25.2.5。 例如，尝试通过命令行使用 android  程序将导致出现错误消息，如下所示：

```shell
The "android" command is deprecated.
For manual SDK, AVD, and project management, please use Android Studio.
For command-line tools, use tools\bin\sdkmanager.bat
and tools\bin\avdmanager.bat
```

以下各节介绍如何使用 Android SDK 25.3.0 和更高版本管理 Android SDK 和 Android 虚拟设备。

### <a name="ui-tools"></a>UI 工具

Visual Studio 和 Visual Studio for Mac 现在为已停用的基于 Google GUI 的管理器提供替代的 Xamarin：

- 若要下载 Android SDK 工具、平台以及开发 Xamarin.Android 应用所需的其他组件，请使用 [Xamarin Android SDK 管理器](~/android/get-started/installation/android-sdk.md)替代旧的 Google SDK 管理器。

- 若要创建和配置 Android 虚拟设备，请使用 [Android 设备管理器](~/android/get-started/installation/android-emulator/device-manager.md)替代旧的 Google 仿真器管理器。

这些工具在功能上等同于它们所替代的基于 Google GUI 的管理器。

### <a name="cli-tools"></a>CLI 工具

或者，可以使用 CLI 工具来管理和更新仿真器和 Android SDK。 现在，以下程序组成了 Android SDK 工具的命令行界面：

#### <a name="sdkmanager"></a>sdkmanager

**已添加此程序的版本：** Android SDK Tools 25.2.3（2016 年 11 月）及更高版本。

Android SDK 的“tools/bin”  文件夹中有一个名为 sdkmanager  的新程序。 此工具用于在命令行维护 Android SDK。 有关使用此工具的详细信息，请参阅 [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)。

#### <a name="avdmanager"></a>avdmanager

**已添加此程序的版本：** Android SDK Tools 25.3.0（2017 年 3 月）及更高版本。

Android SDK 的“tools/bin”  文件夹中有一个名为 avdmanager  的新程序。 此工具用于维护 Android Emulator 的 AVD。 有关使用此工具的详细信息，请参阅 [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html)。

### <a name="downgrading"></a>降级

可以通过从 [Android 开发人员网站](https://developer.android.com/studio/index.html)安装以前版本的 Android SDK 来降级 Android SDK Tools  版本。

### <a name="using-the-old-gui"></a>使用旧的 GUI

只要你使用的是版本 25.2.5 或更低版本的 Android SDK Tools，就仍可以通过在“tools”文件夹中运行 android 程序来使用原始 GUI     。

## <a name="related-links"></a>相关链接

- [Android SDK 安装](~/android/get-started/installation/android-sdk.md)
- [Android 设备管理器](~/android/get-started/installation/android-emulator/device-manager.md)
- [了解 Android API 级别](~/android/app-fundamentals/android-api-levels.md)
- [SDK Tools 发行说明 (Google)](https://developer.android.com/studio/releases/sdk-tools.html)
- [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)
- [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html)
