---
ms.topic: include
author: jamesmontemagno
ms.author: jamont
ms.date: 05/11/2020
ms.openlocfilehash: b5396061de657690fa3f4cdf68e8a94382918613
ms.sourcegitcommit: 83cf2a4d99546751c6394510a463a2b2a8bf75b8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/13/2020
ms.locfileid: "83149737"
---
此 API 在 Android 上使用运行时权限。 请确保 Xamarin.Essentials 已完全初始化，并且在你的应用中设置了权限处理。

在 Android 项目的 `MainLauncher` 或任何启动的 `Activity` 中，必须在 `OnCreate` 方法中初始化 Xamarin.Essentials：

```csharp
protected override void OnCreate(Bundle savedInstanceState) 
{
    //...
    base.OnCreate(savedInstanceState);
    Xamarin.Essentials.Platform.Init(this, savedInstanceState); // add this line to your code, it may also be called: bundle
    //...
}    
```

若要处理 Android 上的运行时权限，Xamarin.Essentials 必须接收任何 `OnRequestPermissionsResult`。 将以下代码添加到所有 `Activity` 类：

```csharp
public override void OnRequestPermissionsResult(int requestCode, string[] permissions, Android.Content.PM.Permission[] grantResults)
{
    Xamarin.Essentials.Platform.OnRequestPermissionsResult(requestCode, permissions, grantResults);

    base.OnRequestPermissionsResult(requestCode, permissions, grantResults);
}
```
