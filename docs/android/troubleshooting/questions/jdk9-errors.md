---
title: Xamarin.Android 和 Java 开发工具包 9
description: 本文介绍如何在 Xamarin.Android 中处理 Java 开发工具包 (JDK) 9 或更高版本的错误。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 7DCF0985-F77D-4A68-AC54-10C9846E189A
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 08/29/2018
ms.openlocfilehash: 2ea7c9b9f900bc339d183c2f5b317792ebec5232
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/10/2020
ms.locfileid: "73026835"
---
# <a name="xamarinandroid-and-java-development-kit-9-or-later"></a>Xamarin.Android 和 Java 开发工具包 9 或更高版本

本文介绍如何在 Xamarin.Android 中处理 Java 开发工具包 (JDK) 9 或更高版本的错误  。

## <a name="overview"></a>概述

Xamarin.Android 使用 Java 开发工具包 (JDK) 与 Android SDK 集成，以构建 Android 应用并运行 Android 设计器。 最新版本的 Android SDK（API 24 和更高版本）需要 JDK 8 (1.8) 或 Microsoft Mobile OpenJDK 预览版。 由于 Google 提供的 Android SDK 工具尚未与 JDK 9 兼容，因此 JDK 9 或更高版本无法使用 Xamarin.Android  。

## <a name="jdk-errors"></a>JDK 错误

如果尝试使用高于 JDK 8 的 JDK 版本构建 Xamarin.Android 项目，系统会显示显式错误，指示不支持此 JDK 版本。 例如：

```shell
Building with JDK Version `9.0.4` is not supported. Please install JDK version `1.8.0`. See https://aka.ms/xamarin/jdk9-errors
```

若要解决这些错误，必须安装 JDK 8 (1.8)，如[如何更新 Java 开发工具包 (JDK) 版本？](~/android/troubleshooting/questions/update-jdk.md)所述。
或者，可以安装 [Microsoft Mobile OpenJDK 预览版](~/android/get-started/installation/openjdk.md) Microsoft Mobile OpenJDK 最终将替代 JDK 8 进行 Xamarin Android 开发。

## <a name="checking-the-jdk-version"></a>检查 JDK 版本

可以输入以下命令（JDK `bin` 目录必须位于 `PATH`）查看已安装的 Java 版本：

```shell
java -version
```

如果安装了 JDK 9，你将看到如下消息：

```shell
java version "9.0.4"
Java(TM) SE Runtime Environment (build 9.0.4+11)
Java HotSpot(TM) 64-Bit Server VM (build 9.0.4+11, mixed mode)
```

如果安装了 JDK 9 或更高版本，则必须安装 Java JDK 8 (1.8) 或 Microsoft Mobile OpenJDK 预览版。 有关如何安装 JDK 8 的信息，请参阅[如何更新 Java 开发工具包 (JDK) 版本？](~/android/troubleshooting/questions/update-jdk.md)。 有关如何安装 Microsoft Mobile OpenJDK 的信息，请参阅 [Microsoft Mobile OpenJDK 预览版](~/android/get-started/installation/openjdk.md)。

请注意，无需卸载更高版本的 JDK；但必须确保 Xamarin 使用的是 JDK 8 而不是更高版本的 JDK。 在 Visual Studio 中，请单击“工具”>“选项”>“Xamarin”>“Android 设置”  。 如果 Java 开发工具包位置未设置为 JDK 8 位置（如 C:\\Program Files\\Java\\jdk1.8.0_111），请单击“更改”，然后将其设置为 JDK 8 的安装位置    。 在 Visual Studio for Mac 中，导航到“首选项”>“项目”>“SDK 位置”>“Android”>“Java SDK (JDK)”，然后单击“浏览”以更新此路径   。

## <a name="known-issues-with-jdk-9"></a>JDK 9 的已知问题

### <a name="apksigner"></a>apksigner

apksigner 和 JDK 9 都有一个已知问题：`apksigner.bat` 文件使用 `-Djava.ext.dirs` 而不是 JDK 9 所需的 `-classpath` 调用 `apksigner.jar`。 建议使用 JDK 8 (1.8)。 有关如何安装 JDK 8 的信息，请参阅[如何更新 Java 开发工具包 (JDK) 版本？](~/android/troubleshooting/questions/update-jdk.md)

如果安装了 JDK 9，请确保未对 `PATH` 环境变量设置以下路径，因为该路径仍指向 JDK 9：`C:\ProgramData\Oracle\Java\javapath`。 将其删除后，命令行中的 `java-version` 应显示 JDK 8。
