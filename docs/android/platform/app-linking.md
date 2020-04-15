---
title: Android 中的应用链接
description: 本指南讨论 Android 6.0 如何支持应用链接，这是一种允许移动应用响应网站上的 URL 的技术。 它将讨论什么是应用链接、如何在 Android 6.0 应用程序中实现应用链接，以及如何将网站配置为向移动应用授予针对某个域的权限。
ms.prod: xamarin
ms.assetid: 48174E39-19FD-43BC-B54C-9AF11D4B1F91
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: af90c286d2bb960a9f78547dd15c3d98a69529ae
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/13/2020
ms.locfileid: "75487823"
---
# <a name="app-linking-in-android"></a>Android 中的应用链接

本指南讨论 Android 6.0 如何支持应用链接，这是一种允许移动应用响应网站上的 URL 的技术。  它将讨论什么是应用链接、如何在 Android 6.0 应用程序中实现应用链接，以及如何将网站配置为向移动应用授予针对某个域的权限。

## <a name="app-linking-overview"></a>应用链接概述

移动应用程序不再孤立存在 &ndash; 在许多情况下，它们与网站一起成为企业的重要组成部分。 对于企业而言，理想情况是通过网站上启动移动应用程序并在移动应用中显示相关内容的链接，将其 Web 平台与移动应用程序无缝连接。 应用链接  （也称为深层链接  ）是一种技术，它允许移动设备响应 URI 并启动与该 URI 对应的移动应用程序。

Android 通过意向系统处理应用链接  &ndash; 当用户在移动浏览器中单击链接时，移动浏览器会分派 Android 将向已注册应用程序委托的意向。 例如，单击烹饪网站上的链接会打开与该网站关联的移动应用，并向用户显示特定食谱。 如果注册了多个应用程序来处理该意向，则 Android 会引发所谓的消除歧义对话框  ，该对话框会询问用户哪个应用程序以选择应处理此意向的应用程序，例如：

![消除歧义对话框的示例屏幕截图](app-linking-images/01-disambiguation-dialog.png)

Android 6.0 使用自动链接处理改进了这一点。 Android 可以自动将应用程序注册为 URI 的默认处理程序 &ndash; 应用会自动启动并直接导航到相关活动。 Android 6.0 如何确定处理 URI 单击取决于以下条件：

1. 现有应用已与 URI 关联  &ndash; 用户可能已将现有应用与 URI 关联。 在这种情况下，Android 会继续使用该应用程序。
2. 没有与 URI 关联的现有应用，但安装了一个支持应用  &ndash; 在此方案中，用户未指定现有应用，因此 Android 会使用安装的支持应用程序处理请求。
3. 没有与 URI 关联的现有应用，但安装了许多支持应用  &ndash; 因为有多个支持 URI 的应用程序，所以会显示消除歧义对话框，用户必须选择将处理 URI 的应用。

如果用户没有安装支持 URI 的应用，但随后安装了一个应用，则在验证与 URI 关联的网站的关联之后，Android 会将该应用程序设置为 URI 的默认处理程序。

本指南将讨论如何配置 Android 6.0 应用程序以及如何创建和发布数字资产链接文件，以支持 Android 6.0 中的应用链接。

## <a name="requirements"></a>要求

本指南需要 Xamarin.Android 6.1 以及面向 Android 6.0 （API 级别 23）或更高版本的应用程序。

可以使用 Xamarin 组件存储中的 [Rivets NuGet 包](https://www.nuget.org/packages/Rivets/)，在早期版本的 Android 中实现应用链接。 Rivets 包与 Android 6.0 中的应用链接不兼容；它不支持 Android 6.0 应用链接。

## <a name="configuring-app-linking-in-android-60"></a>在 Android 6.0 中配置应用链接

在 Android 6.0 中设置应用链接涉及两个主要步骤：

1. 为网站 URI 添加一个或多个意向筛选器  &ndash; 意向筛选器在如何处理移动浏览器中的 URL 单击方面指导 Android。
2. 在网站上发布数字资产链接 JSON  文件  &ndash; 这是上传到网站，由 Android 用于验证移动应用与网站域之间的关系的文件。 如果不进行此操作，则 Android 无法安装应用作为 URI 的默认处理程序；用户必须手动执行此操作。

<a name="configure-intent-filter" />

### <a name="configuring-the-intent-filter"></a>配置意向筛选器

需要配置意向筛选器，用于将一个 URI （或者可能是一组 URI）从网站映射到 Android 应用程序中的活动。 在 Xamarin.Android 中，通过使用 [IntentFilterAttribute](xref:Android.App.IntentFilterAttribute) 修饰活动来建立此关系。 意向筛选器必须声明以下信息：

- `Intent.ActionView` &ndash; 这会注册意向筛选器以响应请求来查看信息
- `Categories` &ndash;  意向筛选器应注册 [Intent.CategoryBrowsable](xref:Android.Content.Intent.CategoryBrowsable)  和 [Intent.CategoryDefault](xref:Android.Content.Intent.CategoryDefault)  ，以便正确处理 Web URI。
- `DataScheme` &ndash; 意向筛选器必须声明 `http` 和/或 `https`。 这是唯一有效的两个方案。
- `DataHost` &ndash; 这是 URI 源自的域。
- `DataPathPrefix` &ndash; 这是网站上的资源的可选路径。
- `AutoVerify` &ndash; `autoVerify` 属性指示 Android 验证应用程序与网站之间的关系。 下面会对此进行更详细的讨论。

下面的示例演示如何使用 [IntentFilterAttribute](xref:Android.App.IntentFilterAttribute) 处理来自 `https://www.recipe-app.com/recipes` 和 `http://www.recipe-app.com/recipes` 的链接：

```csharp
[IntentFilter(new [] { Intent.ActionView },
              Categories = new[] { Intent.CategoryBrowsable, Intent.CategoryDefault },
              DataScheme = "http",
              DataHost = "recipe-app.com",
              DataPathPrefix = "/recipe",
              AutoVerify=true)]
public class RecipeActivity : Activity
{
    // Code for the activity omitted
}
```

在将应用程序注册为 URI 的默认处理程序之前，Android 会验证意向筛选器针对网站上的数字资产文件而标识的每个主机。 所有意向筛选器都必须通过验证，然后 Android 才能将应用建立为默认处理程序。

### <a name="creating-the-digital-assets-link-file"></a>创建数字资产链接文件

Android 6.0 应用链接要求在将应用程序设置为 URI 的默认处理程序之前，Android 验证应用程序与网站之间的关联。 首次安装应用程序时，会进行此验证。 数字资产链接  文件是由相关 Web 域托管的 JSON 文件。

> [!NOTE]
> `android:autoVerify` 属性必须由意向筛选器设置 &ndash; 否则 Android 不会执行验证。

该文件由域的网站管理员放置在 https://domain/.well-known/assetlinks.json  位置。

数字资产文件包含 Android 验证关联所需的元数据。 assetlinks.json  文件具有以下键值对：

- `namespace` &ndash; Android 应用程序的命名空间。
- `package_name` &ndash; Android 应用程序的包名称（在应用程序清单中声明）。
- `sha256_cert_fingerprints` &ndash; 签名应用程序的 SHA256 指纹。 有关如何获取应用程序的 SHA1 指纹的详细信息，请参阅指南[查找密钥存储的 MD5 或 SHA1 签名](~/android/deploy-test/signing/keystore-signature.md)。

以下片段是 assetlinks.json  的示例，其中列出了单个应用程序：

```json
[
   {
      "relation":[
         "delegate_permission/common.handle_all_urls"
      ],
      "target":{
         "namespace":"android_app",
         "package_name":"com.example",
         "sha256_cert_fingerprints":[
            "14:6D:E9:83:C5:73:06:50:D8:EE:B9:95:2F:34:FC:64:16:A0:83:42:E6:1D:BE:A8:8A:04:96:B2:3F:CF:44:E5"
         ]
      }
   }
]
```

可以注册多个 SHA256 指纹以支持不同版本或内部版本的应用程序。 下一个 assetlinks.json  文件是注册多个应用程序的示例：

```json
[
   {
      "relation":[
         "delegate_permission/common.handle_all_urls"
      ],
      "target":{
         "namespace":"android_app",
         "package_name":"example.com.puppies.app",
         "sha256_cert_fingerprints":[
            "14:6D:E9:83:C5:73:06:50:D8:EE:B9:95:2F:34:FC:64:16:A0:83:42:E6:1D:BE:A8:8A:04:96:B2:3F:CF:44:E5"
         ]
      }
   },
   {
      "relation":[
         "delegate_permission/common.handle_all_urls"
      ],
      "target":{
         "namespace":"android_app",
         "package_name":"example.com.monkeys.app",
         "sha256_cert_fingerprints":[
            "14:6D:E9:83:C5:73:06:50:D8:EE:B9:95:2F:34:FC:64:16:A0:83:42:E6:1D:BE:A8:8A:04:96:B2:3F:CF:44:E5"
         ]
      }
   }
]
```

[Google 数字资产链接网站](https://developers.google.com/digital-asset-links/tools/generator)具有可帮助创建和测试数字资产文件的联机工具。

### <a name="testing-app-links"></a>测试应用链接

实现应用链接之后，应测试各个部分以确保它们按预期方式工作。

可以使用 Google 的数字资产链接 API 来确认数字资产文件正确设置了格式并进行托管，如以下示例中所示：

```html
https://digitalassetlinks.googleapis.com/v1/statements:list?source.web.site=
  https://<WEB SITE ADDRESS>:&relation=delegate_permission/common.handle_all_urls
```

可以执行两个测试，以确保已正确配置意向筛选器以及将应用设置为 URI 的默认处理程序：

1. 数字资产文件按如上所述正确进行了托管。 第一个测试会分派 Android 应重定向到移动应用程序的意向。 Android 应用程序应启动并显示为 URL 注册的活动。 在命令提示符处键入：

    ```shell
    $ adb shell am start -a android.intent.action.VIEW \
        -c android.intent.category.BROWSABLE \
        -d "http://<domain1>/recipe/scalloped-potato"
    ```

2. 显示用于给定设备上安装的应用程序的现有链接处理策略。 以下命令会为设备上的每个用户转储链接策略列表，其中包含以下信息。 在命令提示符处，键入下列命令：

    ```shell
    $ adb shell dumpsys package domain-preferred-apps
    ```

    - `Package` &ndash; 应用程序的包名称。
    - `Domain` &ndash; 将由应用程序处理其 Web 链接的域（由空格分隔）
    - `Status` &ndash; 这是应用的当前链接处理状态。 值为 always  表示应用程序声明了 `android:autoVerify=true` 并且已通过系统验证。 它后面是一个十六进制数字，表示 Android 系统的首选项记录。

    例如：

    ```shell
    $ adb shell dumpsys package domain-preferred-apps

    App linkages for user 0:
    Package: com.android.vending
    Domains: play.google.com market.android.com
    Status: always : 200000002
    ```

## <a name="summary"></a>总结

本指南讨论了应用链接在 Android 6.0 中的工作原理。 然后介绍了如何配置 Android 6.0 应用程序以支持和响应应用链接。 还讨论了如何在 Android 应用程序中测试应用链接。

## <a name="related-links"></a>相关链接

- [查找密钥存储的 MD5 或 SHA1 签名](~/android/deploy-test/signing/keystore-signature.md)
- [AppLinks](https://developers.facebook.com/docs/applinks)
- [Google 数字资产链接](https://developers.google.com/digital-asset-links/)
- [语句列表生成器和测试器](https://developers.google.com/digital-asset-links/tools/generator)
