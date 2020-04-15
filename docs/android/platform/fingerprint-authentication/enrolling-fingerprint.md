---
title: 注册指纹
description: 如何在 Android 设备或模拟器上设置锁屏界面并注册指纹。
ms.prod: xamarin
ms.assetid: 52092F63-00EE-4F8B-A49F-65C9CCBA7EF2
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: c0290dfa3b4aa301a07a589f78577899e8282158
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/13/2020
ms.locfileid: "73027597"
---
# <a name="enrolling-a-fingerprint"></a>注册指纹

## <a name="enrolling-a-fingerprint-overview"></a>注册指纹概述

只有当设备已配置有指纹身份验证时，Android 应用程序才能使用指纹身份验证。 本指南将介绍如何在 Android 设备或模拟器上注册指纹。 模拟器没有实际硬件来执行指纹扫描，但可以借助 Android Debug Bridge 来模拟指纹扫描（如下所述）。  本指南将介绍如何在 Android 设备上启用锁屏界面，并注册指纹身份验证。

## <a name="requirements"></a>要求

若要注册指纹，必须有运行 API 级别 23 (Android 6.0) 的 Android 设备或模拟器。

若要使用 Android Debug Bridge (ADB)，需要熟悉命令提示符，且 adb  可执行文件必须位于 Bash、PowerShell 或命令提示符环境的 PATH 中。

## <a name="configuring-a-screen-lock-and-enrolling-a-fingerprint"></a>配置锁屏界面并注册指纹 

若要设置锁屏界面，请按照以下步骤操作：

1. 依次转到“设置”>“安全”  ，然后选择“锁屏界面”  ：

    ![在“安全”屏幕上选择“锁屏界面”的位置](enrolling-fingerprint-images/testing-01.png)

2. 在随即出现的下一个屏幕上，可以选择和配置一种锁屏界面安全方法： 

    ![选择“清扫”、“图案”、“PIN”或“密码”](enrolling-fingerprint-images/testing-02.png)

   选择并完成一种可用的锁屏界面方法。

3. 配置锁屏界面后，依次返回到“设置”>“安全”  页，然后选择“指纹”  ：

    ![在“安全”屏幕上选择“指纹”的位置](enrolling-fingerprint-images/testing-03.png)

4. 然后，按照以下顺序向设备添加指纹：

    [![向设备添加指纹的屏幕截图序列](enrolling-fingerprint-images/testing-04-sml.png)](enrolling-fingerprint-images/testing-04.png#lightbox)

5. 在最后一个屏幕中，系统会提示你将手指放在指纹扫描程序上： 

    ![提示你将手指放在传感器上的屏幕](enrolling-fingerprint-images/testing-05.png)

    如果使用的是 Android 设备，请用手指触摸扫描程序来完成此过程。 

### <a name="simulating-a-fingerprint-scan-on-the-emulator"></a>在模拟器上模拟指纹扫描

在 Android 模拟器上，可以使用 Android Debug Bridge 来模拟指纹扫描。 在 OS X 上，启动终端会话，而在 Windows 上，则启动命令提示符或 Powershell 会话，并运行 `adb`：

```shell
$ adb -e emu finger touch 1
```

值 1  是“已扫描”的手指的 finger\_id  。 它是为每个虚拟指纹分配的唯一整数。 以后，当应用运行时，只要模拟器提示你输入指纹，就可以运行这个相同的 ADB 命令。可以运行 `adb` 命令，并向它传递 finger\_id  ，以模拟指纹扫描。

在指纹扫描完成后，Android 会通知你指纹已添加：  

![显示“指纹已添加!”的屏幕](enrolling-fingerprint-images/testing-06.png)

## <a name="summary"></a>总结 

本指南介绍了如何在 Android 设备或 Android 模拟器中设置锁屏界面并注册指纹。 
