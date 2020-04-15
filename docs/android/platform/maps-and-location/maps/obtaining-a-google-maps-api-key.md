---
title: 获取 Google Maps API 密钥
description: 如何获取 Google Maps API 密钥以便向应用添加地图功能。
ms.prod: xamarin
ms.assetid: D5969C57-3444-465E-D6FF-249AEE62E127
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/25/2018
ms.openlocfilehash: 371876d087c7027d4cfe2d2d9ada8b0dbedb5dd5
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/13/2020
ms.locfileid: "75488967"
---
# <a name="obtaining-a-google-maps-api-key"></a>获取 Google Maps API 密钥

若要在 Android 中使用 Google Maps 功能，需要向 Google 注册 Maps API 密钥。 在执行此操作之前，你只能在应用程序中看到一个空白网格，而不是地图。 必须获取 Google Maps Android API v2 密钥，较旧的 Google Maps Android API 密钥 v1 将不起作用。

获取 Maps API v2 密钥涉及以下步骤：

1. 检索用于对应用程序进行签名的密钥存储的 SHA-1 指纹。
2. 在 Google API Console 中创建一个项目。
3. 获取 API 密钥。

## <a name="obtaining-your-signing-key-fingerprint"></a>获取签名密钥指纹

若要从 Google 请求 Maps API 密钥，需要了解用于对应用程序进行签名的密钥存储的 SHA-1 指纹。
通常，这意味着必须要确定用于调试密钥存储的 SHA-1 指纹，以及用于对应用程序进行签名以供发布的密钥存储的 SHA-1 指纹。

<!-- markdownlint-disable MD001 -->

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

默认情况下，用于对 Xamarin.Android 应用程序的调试版本进行签名的密钥存储位于以下位置：

C:\\Users\\[USERNAME]\\AppData\\Local\\Xamarin\\Mono for Android\\debug.keystore 

可通过从 JDK 运行 `keytool` 命令来获取有关密钥存储的信息。 此工具通常可在 Java bin 目录中找到：

\\Program Files\\Android\\jdk\\microsoft_dist_openjdk_[VERSION]\\bin\\keytool.exe 

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

默认情况下，用于对 Xamarin.Android 应用程序的调试版本进行签名的密钥存储位于以下位置：

/Users/[USERNAME]/.local/share/Xamarin/Mono for Android/debug.keystore 

可通过从 JDK 运行 `keytool` 命令来获取有关密钥存储的信息。 此工具通常可在 Java bin 目录中找到：

/System/Library/Java/JavaVirtualMachines/[VERSION].jdk/Contents/Home/bin/keytool 

-----

使用以下命令运行 keytool（使用上面显示的文件路径）：

```shell
keytool -list -v -keystore [STORE FILENAME] -alias [KEY NAME] -storepass [STORE PASSWORD] -keypass [KEY PASSWORD]
```

### <a name="debugkeystore-example"></a>Debug.keystore 示例

对于默认调试密钥（由系统自动创建用于调试），请使用此命令：

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

```cmd
keytool.exe -list -v -keystore "C:\Users\[USERNAME]\AppData\Local\Xamarin\Mono for Android\debug.keystore" -alias androiddebugkey -storepass android -keypass android
```

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

```bash
keytool -list -v -keystore /Users/[USERNAME]/.local/share/Xamarin/Mono\ for\ Android/debug.keystore -alias androiddebugkey -storepass android -keypass android
```

-----

### <a name="production-keys"></a>生产密钥

将应用部署到 Google Play 时，必须[使用私钥进行签名](~/android/deploy-test/signing/index.md)。
`keytool` 将需要与私钥详细信息以及用于创建生产 Google Maps API 密钥的生成的 SHA-1 指纹一起运行。 注意，请先使用正确的 Google Maps API 密钥更新 AndroidManifest.xml 文件，然后再进行部署  。

### <a name="keytool-output"></a>Keytool 输出

控制台窗口中应该会显示如下输出：

```shell
Alias name: androiddebugkey
Creation date: Jan 01, 2016
Entry type: PrivateKeyEntry
Certificate chain length: 1
Certificate[1]:
Owner: CN=Android Debug, O=Android, C=US
Issuer: CN=Android Debug, O=Android, C=US
Serial number: 4aa9b300
Valid from: Mon Jan 01 08:04:04 UTC 2013 until: Mon Jan 01 18:04:04 PST 2033
Certificate fingerprints:
    MD5:  AE:9F:95:D0:A6:86:89:BC:A8:70:BA:34:FF:6A:AC:F9
    SHA1: BB:0D:AC:74:D3:21:E1:43:07:71:9B:62:90:AF:A1:66:6E:44:5D:75
    Signature algorithm name: SHA1withRSA
    Version: 3
```

本指南后续部分将使用 SHA-1 指纹（在 SHA1 后面列出  ）。

## <a name="creating-an-api-project"></a>创建 API 项目

检索到签名密钥存储的 SHA-1 指纹后，需要在 Google API Console 中创建一个新项目（或将 Google Maps Android API v2 服务添加到现有项目中）。

1. 在浏览器中，导航到 [Google Developer Console API 和服务仪表板](https://console.developers.google.com/apis/dashboard/)，并单击“选择项目”  。 单击某个项目名称，或单击“新建项目”以创建一个新项目  ：

   [![Google Developer Console“创建项目”按钮](obtaining-a-google-maps-api-key-images/01-google-developer-console-vs-sml.png)](obtaining-a-google-maps-api-key-images/01-google-developer-console-vs.png#lightbox)

2. 如果创建了新项目，请在显示的“新建项目”对话框中输入项目名称  。 此对话框将根据项目名称生成唯一的项目 ID。 接下来，单击“创建”按钮，如以下示例中所示  ：

   [![新项目命名为 XamarinMapsDemo](obtaining-a-google-maps-api-key-images/02-new-project-vs-sml.png)](obtaining-a-google-maps-api-key-images/02-new-project-vs.png#lightbox)

3. 项目将在一分钟后创建完成，然后系统会将你转到项目的“仪表板”页  。 在此处单击“启用 API 和服务”  ：

   [![在“库”部分单击“Google Maps Android API”](obtaining-a-google-maps-api-key-images/03-api-selection-vs-sml.png)](obtaining-a-google-maps-api-key-images/03-api-selection-vs.png#lightbox)

4. 从“API 库”页中，单击“用于 Android 的 Maps SDK”   。 在下一页中，单击“启用”，为此项目启用服务  ：

   [![在“仪表板”部分单击“启用”按钮](obtaining-a-google-maps-api-key-images/04-enable-api-vs-sml.png)](obtaining-a-google-maps-api-key-images/04-enable-api-vs.png#lightbox)

此时 API 项目已创建完成，并且其中添加了 Google Maps Android API v2。 但是，你还无法在项目中使用此 API，需要先为其创建凭据。 下一节将介绍如何创建 API 密钥并将 Xamarin.Android 应用程序列入允许列表，使其有权使用此密钥。

## <a name="obtaining-the-api-key"></a>获取 API 密钥

创建 Google Developer Console API 项目后，需要创建一个 Android API 密钥  。 Xamarin.Android 应用程序必须具有 API 密钥，才能获得对 Android Map API v2 的访问权限。

1. 在显示的“用于 Android 的 Maps SDK”页中（在上一步中单击“启用”后），转到“凭据”选项卡，然后单击“创建凭据”按钮     ：

   [![用于 Android 的 Maps SDK 凭据消息](obtaining-a-google-maps-api-key-images/05-api-is-enabled-vs-sml.png)](obtaining-a-google-maps-api-key-images/05-api-is-enabled-vs.png#lightbox)

2. 单击“API 密钥”  ：

   [![将凭据添加到项目对话框](obtaining-a-google-maps-api-key-images/06-add-credentials-to-your-project-vs-sml.png)](obtaining-a-google-maps-api-key-images/06-add-credentials-to-your-project-vs.png#lightbox)

3. 单击此按钮后，将生成 API 密钥。 接下来，需要限制此密钥，以便只有你的应用可以使用此密钥调用 API。 单击“限制密钥”  ：

   [![在“凭据”页上单击“限制密钥”](obtaining-a-google-maps-api-key-images/07-generate-api-key-vs-sml.png)](obtaining-a-google-maps-api-key-images/07-generate-api-key-vs.png#lightbox)

4. 将名称字段从“API 密钥 1”更改为能帮助你记住该密钥的用途的名称（本例中使用 XamarinMapsDemoKey）    。 接下来，单击“Android 应用”单选按钮  ：

   [![在“凭据”页选择 Android 应用](obtaining-a-google-maps-api-key-images/08-key-restriction-vs-sml.png)](obtaining-a-google-maps-api-key-images/08-key-restriction-vs.png#lightbox)

5. 若要添加 SHA-1 指纹，请单击“+ 添加包名称和指纹”  ：

   [![单击“添加包名称和指纹”](obtaining-a-google-maps-api-key-images/09-add-package-fingerprint-vs-sml.png)](obtaining-a-google-maps-api-key-images/09-add-package-fingerprint-vs.png#lightbox)

6. 输入应用的包名称，然后输入 SHA-1 证书指纹（可通过 `keytool` 获取，如本指南前面所述）。 在下面的示例中，已输入 `XamarinMapsDemo` 的包名称，后跟从 debug.keystore 获取的 SHA-1 证书指纹  ：

   [![输入的包名称为 com.xamarin.docs.android.map](obtaining-a-google-maps-api-key-images/10-enter-package-and-sha1-vs-sml.png)](obtaining-a-google-maps-api-key-images/10-enter-package-and-sha1-vs.png#lightbox)

7. 注意，为了使 APK 访问 Google Maps，务必包含用于对 APK 进行签名的每个密钥存储（调试和发布）的 SHA-1 指纹和包名称。 例如，如果你使用一台计算机进行调试，将另一台计算机用于生成发布 APK，则应包含第一台计算机的调试密钥存储的 SHA-1 证书指纹，以及第二台计算机的发布密钥存储的 SHA-1 证书指纹。 单击“+ 添加包名称和指纹”，以添加其他指纹和包名称，如以下示例中所示  ：

   [![添加其他指纹会创建另一个 SHA-1 证书](obtaining-a-google-maps-api-key-images/11-second-fingerprint-vs-sml.png)](obtaining-a-google-maps-api-key-images/11-second-fingerprint-vs.png#lightbox)

8. 单击“保存”  按钮保存更改。 接下来，返回到 API 密钥列表。 如果你有以前创建的其他 API 密钥，则它们也会在此处列出。 在本例中，仅列出了一个 API 密钥（已在前面的步骤中创建）：

   [![XamarinMapsDemoKey 显示在 API 密钥列表中](obtaining-a-google-maps-api-key-images/12-list-of-apis-vs-sml.png)](obtaining-a-google-maps-api-key-images/12-list-of-apis-vs.png#lightbox)

## <a name="connect-the-project-to-a-billable-account"></a>将项目连接到计费帐户

从 2018 年 6 月 11 日开始，如果未将项目连接到计费帐户，API 密钥将不起作用（即使移动应用仍可免费使用该服务）。

1. 单击汉堡菜单按钮，然后选择“计费”页  ：

   [![选择汉堡菜单计费部分](obtaining-a-google-maps-api-key-images/13-goto-billing-vs-sml.png)](obtaining-a-google-maps-api-key-images/13-goto-billing-vs.png#lightbox)

2. 单击“关联计费帐户”，然后在显示的弹出窗口中单击“创建计费帐户”（如果没有帐户，系统会指导你创建一个新帐户），从而将项目关联到一个计费帐户   ：

   [![将项目关联到计费帐户](obtaining-a-google-maps-api-key-images/14-link-billing-account-vs-sml.png)](obtaining-a-google-maps-api-key-images/14-link-billing-account-vs.png#lightbox)

## <a name="adding-the-key-to-your-project"></a>将密钥添加到项目中

最后，将此 API 密钥添加到 Xamarin.Android 应用的 AndroidManifest.XML  文件中。 在下面的示例中，`YOUR_API_KEY` 将被替换为在前面的步骤中生成的 API 密钥：

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    android:versionName="4.10" package="com.xamarin.docs.android.mapsandlocationdemo"
    android:versionCode="10">
...
  <application android:label="@string/app_name">
    <!-- Put your Google Maps V2 API Key here. -->
    <meta-data android:name="com.google.android.maps.v2.API_KEY" android:value="YOUR_API_KEY" />
    <meta-data android:name="com.google.android.gms.version" android:value="@integer/google_play_services_version" />
  </application>
</manifest>
```

## <a name="related-links"></a>相关链接

- [Google API Console](https://code.google.com/apis/console/)
- [Google Maps API 密钥](https://developers.google.com/maps/documentation/android/start#the_google_maps_api_key)
- [keytool](https://docs.oracle.com/javase/6/docs/technotes/tools/windows/keytool.html.)
