---
title: 应安装哪些 Android SDK 包？
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: F136AAE0-C6D2-4B0F-8F8C-7A6A94877266
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 05/30/2018
ms.openlocfilehash: 24c70c2e869f59091a1519af6d1165dbea9cc467
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/10/2020
ms.locfileid: "76725058"
---
# <a name="which-android-sdk-packages-should-i-install"></a>应安装哪些 Android SDK 包？

安装 Android SDK 不会自动包含开发所需的所有最低版包。 尽管各个开发人员的需求不尽相同，但使用 Xamarin.Android 进行开发通常需要以下包：

## <a name="tools"></a>工具

从 SDK 管理器的“工具”文件夹中安装最新的工具：

- Android SDK 工具
- Android SDK 平台工具
- Android SDK 生成工具

## <a name="android-platforms"></a>Android 平台

为已设置为最低和目标的 Android 版本安装“SDK 平台”。

示例：

- 目标 API 23
- 最低 API 23

只需要安装适用于 API 23 的 SDK 平台

- 目标 API 23
- 最低 API 15

需要安装适用于 API 15 和 23 的 SDK 平台。 请注意，不需要安装介于最低和目标之间的 API 级别（即使要反向移植到那些 API 级别）。

## <a name="system-images"></a>系统映像

仅当要使用 Google 提供的现成 Android 仿真程序时才需要这些项。 有关更多信息，请参阅 [Android Emulator 设置](~/android/get-started/installation/android-emulator/index.md)

## <a name="extras"></a>附加程序
通常不需要 Android SDK 附加程序；但是了解它们很有用，因为根据用例，可能会需要它们。
