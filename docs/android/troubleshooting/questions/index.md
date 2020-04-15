---
title: Xamarin.Android 常见问题
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 0F0FDD2B-FFB1-476F-B674-81DB3A5E1CF3
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 08/29/2018
ms.openlocfilehash: 28b13951a0d681ffdb8e643667e496e0b3606628
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/13/2020
ms.locfileid: "76724000"
---
# <a name="android-frequently-asked-questions"></a>Android 常见问题

## <a name="installation--setup"></a>安装和设置

### <a name="which-android-sdk-packages-should-i-install"></a>[应安装哪些 Android SDK 包？](install-android-sdk-packages.md)

安装 Android SDK 不会自动包含开发所需的所有最低版包。 各个开发人员的需求不尽相同，本指南介绍了使用 Xamarin.Android 开发通常需要的包。

### <a name="where-can-i-set-my-android-sdk-locations"></a>[可以在哪里设置 Android SDK 位置？](android-sdk-location.md)

本指南介绍了适用于大多数设置的 Android SDK 的默认设置；以及如何在 Visual Studio for Mac 或 Visual Studio 中视需要更改这些默认设置。

### <a name="how-do-i-update-the-java-development-kit-jdk-version"></a>[如何更新 Java Development Kit (JDK) 版本？](update-jdk.md)

本文介绍如何在 Windows 和 Mac 上更新 Java 开发工具包 (JDK) 版本。

### <a name="can-i-use-java-development-kit-jdk-version-9-or-later"></a>[我可以使用 Java 开发工具包 (JDK) 版本 9 或更高版本吗？](jdk9-errors.md)

Xamarin.Android 需要 JDK 8 或 Microsoft Mobile OpenJDK。 本文列出了一些常见的错误消息，如果你安装了 JDK 9 或更高版本，可能会看到这些消息，还提供了用于检查 JDK 版本的说明。

### <a name="how-can-i-manually-install-the-android-support-libraries-required-by-the-xamarinandroidsupport-packages"></a>[如何手动安装 Xamarin.Android.Support 包所需的 Android 支持库？](install-android-support-library.md)

本指南提供了在 Windows 和 Mac 上安装 `Xamarin.Android.Support.v4` 支持库的示例步骤。

### <a name="what-usb-drivers-do-i-need-to-debug-android-on-windows"></a>[在 Windows 上调试 Android 需要哪些 USB 驱动程序？](android-drivers-debug-windows.md)

在 Windows 中进行开发时，若要在 Android 设备上进行调试，需要安装兼容的 USB 驱动程序。 默认情况下，Android SDK 管理器包含“Google USB 驱动程序”，这样就可以支持 Nexus 设备。
其他设备需要使用设备制造商发布的 USB 驱动程序。 本指南介绍如何查找这些驱动程序以及备用测试方法。

### <a name="is-it-possible-to-connect-to-android-emulators-running-on-a-mac-from-a-windows-vm"></a>[是否可以从 Windows VM 连接到在 Mac 上运行的 Android 仿真器？](connect-android-emulator-mac-windows.md)

本指南介绍使用 Android 仿真器的方法。

## <a name="general-questions"></a>常规问题

### <a name="how-do-i-automate-an-android-nunit-test-project"></a>[如何自动化 Android NUnit 测试项目？](automate-android-nunit-test.md)

本指南介绍设置 Android NUnit 测试项目（而非  Xamarin.UITest 项目）的步骤。 可在[此处](/appcenter/test-cloud/preparing-for-upload)找到 Xamarin.UITest 指南。

### <a name="why-cant-my-android-release-build-connect-to-the-internet"></a>[为何我的 Android 发布版本无法连接到 Internet？](android-internet.md)

导致此问题的最常见原因是调试版本中自动包含 INTERNET 权限，但在发布版本中必须手动设置  。 本指南介绍了如何在发行版本上启用权限。

### <a name="smarter-xamarin-android-support-v4--v13-nuget-packages"></a>[更智能的 Xamarin Android 支持 v4 / v13 NuGet 包](android-support-v4v13-libraries.md)

`Support-v4` 和 `Support-v13` 不能在同一应用中一起使用，也就是说，它们是互斥的。 这是因为 `Support-v13` 实际上包含 `Support-v4` 的所有类型和实现。 如果尝试在同一项目中引用这两者，会遇到“重复的类型”错误。

### <a name="how-do-i-resolve-a-pathtoolongexception-error"></a>[如何解决 PathTooLongException 错误？](path-too-long-exception.md)

本文介绍了如何解决在生成 Xamarin.Android 应用时可能发生的 PathTooLongException 错误  。

## <a name="deprecated"></a>不推荐使用

> [!NOTE]
> 以下文章适用于最新版本的 Xamarin 中已解决的问题。 但是，如果问题是在软件的最新版本上出现的，请提交[新的 bug](~/cross-platform/troubleshooting/questions/howto-file-bug.md) 并附上完整的版本控制信息和生成日志输出记录。

### <a name="what-version-of-xamarinandroid-added-lollipop-support"></a>[哪个版本的 Xamarin.Android 添加了 Lollipop 支持？](xa-lollipop.md)

本指南最初是为 Android L 预览版编写的。Xamarin.Android 4.17 添加了 Android L 预览版支持，Xamarin.Android 4.20 添加了 Android Lollipop 支持。

### <a name="androidsupportv7appcompat---no-resource-found-that-matches-the-given-name-attr-androidactionmodesharedrawable"></a>[Android.Support.v7.AppCompat - 未找到与给定名称匹配的资源：属性“android:actionModeShareDrawable”](missing-action-mode-share-drawable.md)

如果缺少一些必需的 Android SDK 包，则较早版本的 Xamarin 会出现此错误。

### <a name="adjusting-java-memory-parameters-for-the-android-designer"></a>[调整 Android Designer 的 Java 内存参数](android-designer-java-memory.md)

为 Android 设计器启动 `java` 进程时使用的默认内存参数可能与某些系统配置不兼容。 从 Xamarin Studio 5.7.2.7 和 Xamarin for Visual Studio 3.9.344 开始，可以在每个项目的基础上自定义这些设置。

### <a name="my-android-resourcedesignercs-file-will-not-update"></a>[Android Resource.designer.cs 文件不更新](resource-designer-wont-update.md)

Xamarin.Studio 5.1 中的 bug 先前通过部分或完全删除 .csproj 文件中的 xml 代码破坏了 .csproj 文件。 这将导致 Android 生成系统的重要部分（如更新 Android Resource.designer.cs）失败。 从 7 月 15 日的 5.1.4 稳定版本开始，此 bug 已得到修复。但在许多情况下，必须按照本指南中的说明手动修复项目文件。
