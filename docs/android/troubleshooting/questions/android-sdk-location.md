---
title: 可以在哪里设置 Android SDK 位置？
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 6A9DE6E9-3E27-4DD2-87D2-34E95E5D401C
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 11/16/2017
ms.openlocfilehash: 8685be4bb1cc45ff04dc8d9f7d8e64e7b1483b60
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/13/2020
ms.locfileid: "73027028"
---
# <a name="where-can-i-set-my-android-sdk-locations"></a>可以在哪里设置 Android SDK 位置？

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

在 Visual Studio 中，导航到“工具”>“选项”>“Xamarin”>“Android 设置”，以查看和设置 Android SDK 位置  ：

[![首选项中的“位置”选项卡](android-sdk-location-images/win/01-locations-sml.png)](android-sdk-location-images/win/01-locations.png#lightbox)

每个路径的默认位置如下：

- Java 开发工具包位置： 

    C:\\Program Files\\Java\\jdk1.8.0_131 

- Android SDK 位置： 

    C:\\Program Files (x86)\\Android\\android-sdk 

- Android NDK 位置： 

    C:\\ProgramData\\Microsoft\\AndroidNDK64\\android-ndk-r13b 

注意，NDK 的版本号可能会有所不同。 例如，它可能是 android-ndk-r10e 等早期版本，而不是 android-ndk-r13b   。

若要设置 Android SDK 位置，请将 Android SDK 目录的完整路径输入到“Android SDK 位置”框中  。 你可以在文件资源管理器中导航到 Android SDK 位置，从地址栏复制路径，然后将此路径粘贴到“Android SDK 位置”框  。
例如，如果 Android SDK 位置位于 C:\\Users\\username\\AppData\\Local\\Android\\Sdk，可清除“Android SDK 位置”框中的旧路径，粘贴此路径，然后单击“确定”    。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

在 Visual Studio for Mac 中，导航到“首选项”>“项目”>“SDK 位置”>“Android”  。 在 Android 页中，单击“位置”选项卡以查看和设置 SDK 位置   ：

[![首选项中的“位置”选项卡](android-sdk-location-images/mac/01-locations-sml.png)](android-sdk-location-images/mac/01-locations.png#lightbox)

每个路径的默认位置如下：

- Android SDK 位置： 

     ~/Library/Developer/Xamarin/android-sdk-macosx

- Android NDK 位置： 

     ~/Library/Developer/Xamarin/android-ndk/android-ndk-r14b

- Java SDK (JDK) 位置： 

     /usr

注意，NDK 的版本号可能会有所不同。 例如，它可能是 android-ndk-r10e 等早期版本，而不是 android-ndk-r14b   。

若要设置 Android SDK 位置，请将 Android SDK 目录的完整路径输入到“Android SDK 位置”框中  。 你可以在查找器中选择“Android SDK”文件夹，按 CTRL+&#8984;+I 查看文件夹信息，单击路径并将其拖动到“位置:”的右侧，复制后将其粘贴到“位置”选项卡的“Android SDK 位置”框中     。例如，如果 Android SDK 位置位于 ~/Library/Developer/Android/Sdk，可清除“Android SDK 位置”框中的旧路径，粘贴此路径，然后单击“确定”    。

-----
