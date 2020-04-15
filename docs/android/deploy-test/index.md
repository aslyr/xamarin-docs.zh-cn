---
title: 测试、优化和部署 Xamarin.Android 应用
description: 本部分包含的指南介绍如何测试应用程序、优化应用程序的性能、做好发布准备、使用证书对应用程序进行签名以及将它发布到应用商店
ms.prod: xamarin
ms.assetid: 568C0B85-EFF3-AF6F-5605-95055193D367
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 04/25/2018
ms.openlocfilehash: 4b7d3d19ce8766ccdbfc41163fcad44074e832b8
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/13/2020
ms.locfileid: "80159753"
---
# <a name="deployment-and-testing"></a>部署和测试

本部分包含的指南介绍如何测试应用程序、优化应用程序的性能、做好发布准备、使用证书对应用程序进行签名以及将它发布到应用商店。

## <a name="application-package-sizes"></a>[应用程序包大小](app-package-size.md)

本文将讨论 Xamarin.Android 应用程序包的组成部分，以及可用于在开发的调试和发布阶段进行高效包部署的相关策略。

## <a name="apply-changes"></a>[应用更改](apply-changes.md)

本指南介绍“应用更改”功能，此功能可让你将资源更改推送到正在运行的应用，而无需重新启动应用。

## <a name="building-apps"></a>[构建应用](building-apps/index.md)

本部分介绍生成过程的工作原理，并说明如何生成特定于 ABI 的 APK。

## <a name="command-line-emulator"></a>[命令行仿真器](command-line-emulator.md)

本文简要介绍如何通过命令行启动仿真器。

## <a name="debugging"></a>[调试](~/android/deploy-test/debugging/index.md)

这部分中的指南帮助你使用 Android 仿真器、实际 Android 设备和调试日志调试应用。

## <a name="setting-the-debuggable-attribute"></a>[设置可调式属性](~/android/deploy-test/debuggable-attribute.md)

本文介绍如何设置可调式属性，以便 `adb` 等工具能够与 JVM 进行通信。

## <a name="environment"></a>[环境](environment.md)

本文介绍 Xamarin.Android 执行环境以及影响程序执行的 Android 系统属性。

## <a name="gdb"></a>[GDB](gdb.md)

本文介绍如何使用 `gdb` 调试 Xamarin.Android 应用程序。

## <a name="installing-a-system-app"></a>[安装系统应用](install-system-app.md)

本指南介绍如何将 Xamarin.Android 应用作为 Android 设备上的系统应用程序或作为自定义 ROM 的一部分安装。

## <a name="linking-on-android"></a>[在 Android 上链接](linker.md)

本文讨论 Xamarin.Android 用于缩减应用程序最终大小的链接过程。 其中描述了可以执行的各种级别的链接，并就缓解使用链接器可能导致的错误提供了一些指导和故障排除建议。

## <a name="xamarinandroid-performance"></a>[Xamarin.Android 性能](~/android/deploy-test/performance.md)

可以通过许多方法提高使用 Xamarin.Android 构建的应用程序的性能。 总体上，这些方法可以极大地降低由 CPU 执行的工作量和应用程序占用的内存量。

## <a name="profiling-android-apps"></a>[分析 Android 应用](~/android/deploy-test/profiling.md)

本指南介绍如何使用探查器工具来检查 Android 应用的性能和内存使用情况。

## <a name="preparing-an-application-for-release"></a>[做好应用程序发布准备](~/android/deploy-test/release-prep/index.md)

应用程序经编码和测试后，必须准备一个包进行分发。 准备此包的第一个任务是生成供发布的应用程序，其中主要涉及到设置应用程序的一些属性。

## <a name="signing-the-android-application-package"></a>[对 Android 应用程序包进行签名](~/android/deploy-test/signing/index.md)

了解如何创建 Android 签名标识、为 Android 应用程序创建新签名证书以及使用签名证书对应用程序进行签名。 此外，本主题还介绍了如何将应用导出到磁盘以进行临时发布  。 生成的 APK 可以旁加载到 Android 设备中，而无需经过应用商店。

## <a name="publishing-an-application"></a>[发布应用程序](~/android/deploy-test/publishing/index.md)

这一系列文章介绍了公开分发使用 Xamarin.Android 创建的应用程序的步骤。 可以通过电子邮件、专用 Web 服务器、Google Play 或适用于 Android 的 Amazon 应用商店等通道进行分发。
