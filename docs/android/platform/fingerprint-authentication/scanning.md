---
title: 扫描指纹
ms.prod: xamarin
ms.assetid: 1CDDC096-77E0-47B3-BE0B-8953E2DDACD3
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/23/2016
ms.openlocfilehash: 61edd0e4b532f18a8fc28502e5bb990703068776
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/10/2020
ms.locfileid: "73027493"
---
# <a name="scanning-for-fingerprints"></a>扫描指纹

至此，已了解如何让 Xamarin.Android 应用程序做好使用指纹身份验证的准备。接下来我们将回到 `FingerprintManager.Authenticate` 方法，并讨论它在 Android 6.0 指纹身份验证中的地位。 下面的列表快速概述了指纹身份验证工作流：

1. 调用 `FingerprintManager.Authenticate`，同时传递 `CryptoObject` 和 `FingerprintManager.AuthenticationCallback` 实例。 `CryptoObject` 用于确保指纹身份验证结果未遭篡改。 
2. 子类化 [FingerprintManager.AuthenticationCallback](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html) 类。 当指纹身份验证开始时，将向 `FingerprintManager` 提供此类的实例。 在指纹扫描程序完成时，它会调用此类的回拨方法之一。
3. 编写代码以将 UI 更新为，让用户知道设备已启动指纹扫描程序，并等待用户交互。 
4. 在指纹扫描程序完成时，Android 会对上一步中提供的 `FingerprintManager.AuthenticationCallback` 实例调用方法，以将结果返回给应用程序。
5. 应用程序会通知用户指纹身份验证结果，并适当回应结果。 

下面的代码片段示例展示了要启动扫描指纹的活动中的方法：

```csharp
protected void FingerPrintAuthenticationExample()
{
    const int flags = 0; /* always zero (0) */

    // CryptoObjectHelper is described in the previous section.
    CryptoObjectHelper cryptoHelper = new CryptoObjectHelper();    
    
    // cancellationSignal can be used to manually stop the fingerprint scanner. 
    cancellationSignal = new Android.Support.V4.OS.CancellationSignal();
    
    FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(this);
    
    // AuthenticationCallback is a base class that will be covered later on in this guide.
    FingerprintManagerCompat.AuthenticationCallback authenticationCallback = new MyAuthCallbackSample(this);

    // Start the fingerprint scanner.
    fingerprintManager.Authenticate(cryptoHelper.BuildCryptoObject(), flags, cancellationSignal, authenticationCallback, null);
}
```

接下来将更详细地讨论 `Authenticate` 方法中的每个参数：

- 第一个参数是加密  对象，指纹扫描程序用它来帮助验证指纹扫描的结果。 此对象可能是 `null`，在这种情况下，应用程序必须盲目信任指纹结果没有被篡改。 建议将 `CryptoObject` 实例化并提供给 `FingerprintManager`，而不是使用 null。 [创建 CryptObject](~/android/platform/fingerprint-authentication/creating-a-cryptoobject.md) 详细介绍了如何根据 `Cipher` 实例化 `CryptoObject`。
- 第二个参数始终为零。 Android 文档将此标识为一组标志，最有可能保留以供日后使用。 
- 第三个参数是 `cancellationSignal` 对象，用于禁用指纹扫描程序，并取消当前请求。 这是 [Android CancellationSignal](https://developer.android.com/reference/android/os/CancellationSignal.html)，而不是 .NET 框架中的类型。
- 第四个参数是必需的，它是子类化 `AuthenticationCallback` 抽象类的类。 此类上的方法会获得调用，以向客户端指示 `FingerprintManager` 何时完成且结果是什么。 由于关于实现 `AuthenticationCallback` 有很多需要理解的地方，因此将在[它自己的部分](~/android/platform/fingerprint-authentication/fingerprint-authentication-callbacks.md)中详细介绍。
- 第五个参数是可选的 `Handler` 实例。 如果 `Handler` 对象已提供，`FingerprintManager` 会在处理来自指纹硬件的消息时使用此对象中的 `Looper`。 通常情况下，不需要提供 `Handler`，FingerprintManager 会使用应用程序的 `Looper`。

## <a name="cancelling-a-fingerprint-scan"></a>取消指纹扫描

用户（或应用程序）可能需要在指纹扫描启动后取消它。 在这种情况下，请对被提供给为了启动指纹扫描而调用的 `FingerprintManager.Authenticate` 的 [`CancellationSignal`](https://developer.android.com/reference/android/os/CancellationSignal.html) 调用 [`IsCancelled`](https://developer.android.com/reference/android/os/CancellationSignal.html#isCanceled()) 方法。

至此，已了解 `Authenticate` 方法。接下来将更详细地研究一些更重要的参数。 首先来看看[响应身份验证回拨](~/android/platform/fingerprint-authentication/fingerprint-authentication-callbacks.md)，它介绍了如何子类化 [FingerprintManager.AuthenticationCallback](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html)，以便 Android 应用程序能够回应指纹扫描程序提供的结果。

## <a name="related-links"></a>相关链接

- [CancellationSignal](https://developer.android.com/reference/android/os/CancellationSignal.html)
- [FingerprintManager.AuthenticationCallback](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html)
- [FingerprintManager.CryptoObject](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.CryptoObject.html)
- [FingerprintManagerCompat.CryptoObject](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.CryptoObject.html)
- [FingerprintManager](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html)
- [FingerprintManagerCompat](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.html)
