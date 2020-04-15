---
title: 响应身份验证回调
ms.prod: xamarin
ms.assetid: 6533AFC9-1A1C-4897-A154-4D4ECFE27761
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/06/2017
ms.openlocfilehash: 8dc06740355bd95828e1a1bd8d9d15a2ef37e6b2
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/13/2020
ms.locfileid: "73027533"
---
# <a name="responding-to-authentication-callbacks"></a>响应身份验证回调

指纹扫描器以后台方式在自己的线程中运行，完成运行后，它将从 UI 线程调用 `FingerprintManager.AuthenticationCallback` 的方法之一，以报告扫描结果。 Android 应用程序必须提供自己的处理程序，来扩展此抽象类，并实现以下所有方法：

- **`OnAuthenticationError(int errorCode, ICharSequence errString)`** &ndash; 存在不可恢复的错误时调用它。 应用程序或用户只能通过尽可能地重试来修正此情况。
- **`OnAuthenticationFailed()`** &ndash; 检测到指纹但设备无法识别它时会调用此方法。
- **`OnAuthenticationHelp(int helpMsgId, ICharSequence helpString)`** &ndash; 出现可恢复的错误时调用它，例如扫描手指以快速扫过扫描程序。
- **`OnAuthenticationSucceeded(FingerprintManagerCompati.AuthenticationResult result)`** &ndash; 识别出指纹时调用它。

若在调用 `Authenticate` 时使用了 `CryptoObject`，建议从 `OnAuthenticationSuccessful` 调用 `Cipher.DoFinal`。
密码被篡改或以错误方式初始化时，`DoFinal` 将引发异常，并指示指纹扫描器结果可能已从此应用程序外部被篡改。

> [!NOTE]
> 建议保持此回调类相对轻量级，并且无应用程序特定的逻辑。 回调应充当 Android 应用程序与指纹扫描器之间的“交警”。

## <a name="a-sample-authentication-callback-handler"></a>身份验证回调处理程序示例

下面的类是最小 `FingerprintManager.AuthenticationCallback` 实现示例： 

```csharp
class MyAuthCallbackSample : FingerprintManagerCompat.AuthenticationCallback
{
    // Can be any byte array, keep unique to application.
    static readonly byte[] SECRET_BYTES = {1, 2, 3, 4, 5, 6, 7, 8, 9};
    // The TAG can be any string, this one is for demonstration.
    static readonly string TAG = "X:" + typeof (SimpleAuthCallbacks).Name;

    public MyAuthCallbackSample()
    {
    }

    public override void OnAuthenticationSucceeded(FingerprintManagerCompat.AuthenticationResult result)
    {
        if (result.CryptoObject.Cipher != null) 
        {
            try
            {
                // Calling DoFinal on the Cipher ensures that the encryption worked.
                byte[] doFinalResult = result.CryptoObject.Cipher.DoFinal(SECRET_BYTES);
    
                // No errors occurred, trust the results.              
            }
            catch (BadPaddingException bpe)
            {
                // Can't really trust the results.
                Log.Error(TAG, "Failed to encrypt the data with the generated key." + bpe);
            }
            catch (IllegalBlockSizeException ibse)
            {
                // Can't really trust the results.
                Log.Error(TAG, "Failed to encrypt the data with the generated key." + ibse);
            }
        }
        else
        {
            // No cipher used, assume that everything went well and trust the results.
        }
    }

    public override void OnAuthenticationError(int errMsgId, ICharSequence errString)
    {
        // Report the error to the user. Note that if the user canceled the scan,
        // this method will be called and the errMsgId will be FingerprintState.ErrorCanceled.
    }

    public override void OnAuthenticationFailed()
    {
        // Tell the user that the fingerprint was not recognized.
    }

    public override void OnAuthenticationHelp(int helpMsgId, ICharSequence helpString)
    {
        // Notify the user that the scan failed and display the provided hint.
    }
}
```

`OnAuthenticationSucceeded` 查看调用 `Authentication` 时是否向 `FingerprintManager` 提供了 `Cipher`。 若已提供，则对密码调用 `DoFinal` 方法。 这会关闭 `Cipher`，将其还原到初始状态。 若密码出现了问题，`DoFinal` 将引发异常，并将验证操作视为已失败。

`OnAuthenticationError` 和 `OnAuthenticationHelp` 回调分别会收到一个指示何种问题的整数。 下面的部分说明各个可能的帮助或错误代码。 这两个回调的作用相似 &ndash; 用于通知应用程序指纹验证已失败。 它们的区别在于严重性。 `OnAuthenticationHelp` 是用户可恢复的错误，例如扫指纹的速度过快；`OnAuthenticationError` 是更为严重的错误，例如指纹扫描器损坏。

请注意，通过 `CancellationSignal.Cancel()` 消息取消指纹扫描时，将调用 `OnAuthenticationError`。 `errMsgId` 参数的值将为 5 (`FingerprintState.ErrorCanceled`)。 `AuthenticationCallbacks` 实现处理此情况的方式可能会不同于其他错误，具体取决于要求。 

指纹扫描成功但它与已注册设备的任何指纹都不匹配时，将调用 `OnAuthenticationFailed`。 

## <a name="help-codes-and-error-message-ids"></a>帮助代码和错误消息 ID 

可从 FingerprintManager 类的 [Android SDK 文档](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html#FINGERPRINT_ACQUIRED_GOOD)查看错误代码和帮助代码的列表和说明。 Xamarin.Android 使用 `Android.Hardware.Fingerprints.FingerprintState` 枚举表示这些值：

- **`AcquiredGood`** &ndash;（值 0）获得的图像完好。

- **`AcquiredImagerDirty`** &ndash;（值 3）传感器上可能有或已检测到灰尘，导致指纹图像过于杂乱。 例如，出现多次 `AcquiredInsufficient` 或确实在传感器上检测到灰尘后（像素卡滞、刈痕等），返回此值是正常的。 返回此值时，用户应采取措施来清理传感器。

- **`AcquiredInsufficient`** &ndash;（值 2）检测到的情况（如皮肤干燥）或传感器可能有灰尘（请参阅 `AcquiredImagerDirty`）导致指纹图像过于杂乱，难以处理。

- **`AcquiredPartial`** &ndash;（值 1）仅检测到了部分指纹图像。 注册时，应通知用户需如何操作来解决此问题，例如&ldquo;用力按压传感器。&rdquo;

- **`AcquiredTooFast`** &ndash;（值 5）动作过快导致指纹不完整。 多数情况下此问题适用于线阵传感器，但是如果在采集指纹期间手指移动，也可能会发生此问题。 应要求用户慢速移动手指（线阵传感器），或在传感器上停留较长时间。

- **`AcquiredToSlow`** &ndash;（值 4）由于缺少动作，指纹图像不可读。 多数情况下此问题适用于要求轻扫动作的线阵传感器。

- **`ErrorCanceled`** &ndash;（值 5）指纹传感器不可用导致操作取消。 例如，在切换用户、设备锁定或其他挂起的操作阻止或禁用此操作时，可能会发生此问题。

- **`ErrorHwUnavailable`** &ndash;（值 1）硬件不可用。 请稍后再试。

- **`ErrorLockout`** &ndash;（值 7）操作次数过多导致 API 锁定，操作被取消。

- **`ErrorNoSpace`** &ndash;（值 4）针对注册等操作返回的错误状态；剩余存储不足以完成操作，导致操作无法完成。

- **`ErrorTimeout`** &ndash;（值 3）当前请求运行时间过长时，会返回此错误状态。 这旨在防止程序无限期等待指纹传感器。 此超时特定于平台和传感器，但通常是 30 秒左右。

- **`ErrorUnableToProcess`** &ndash;（值 2）传感器无法处理当前图像时会返回此错误状态。

## <a name="related-links"></a>相关链接

- [Cipher](https://docs.oracle.com/javase/7/docs/api/javax/crypto/Cipher.html)
- [AuthenticationCallback](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html)
- [AuthenticationCallback](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.AuthenticationCallback.html)
