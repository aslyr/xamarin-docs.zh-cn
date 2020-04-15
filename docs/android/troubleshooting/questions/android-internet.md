---
title: 为何我的 Android 发布版本无法连接到 Internet？
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: A6FE770B-A19A-4BF8-95E9-2CF880D4AFC5
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/09/2018
ms.openlocfilehash: 5996cfa3c0a18fc186ea862a2b3d7910594e1281
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/13/2020
ms.locfileid: "73027013"
---
# <a name="why-cant-my-android-release-build-connect-to-the-internet"></a>为何我的 Android 发布版本无法连接到 Internet？

## <a name="cause"></a>原因

导致此问题的最常见原因是调试版本中自动包含 INTERNET 权限，但在发布版本中必须手动设置  。 这是因为 Internet 权限用于允许调试器附加到进程，如[此处](~/android/deploy-test/building-apps/build-process.md)对“DebugSymbols”所述。

## <a name="fix"></a>修补程序

若要解决此问题，可以在 Android 清单中要求使用 Internet 权限。 可以通过清单编辑器或清单的源代码来实现此目的：

- 在编辑器中修补：在 Android 项目中，转到“属性”>“AndroidManifest.xml”>“必需权限”，然后勾选 Internet  

- 在源代码中修补：在源编辑器中打开 AndroidManifest，然后在 `<Manifest>` 标记中添加权限标记：

    ```xml
    <Manifest>
    ...
    <uses-permission android:name="android.permission.INTERNET" />
    </Manifest>
    ```
