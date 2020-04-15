---
title: 更智能的 Xamarin Android 支持 v4 / v13 NuGet 包
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: FE66A82A-6C05-4646-BC52-E806F5DC606C
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/09/2018
ms.openlocfilehash: c74cac6a6d669385855999a565711a3fdc85f3b7
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/13/2020
ms.locfileid: "73019540"
---
# <a name="smarter-xamarin-android-support-v4--v13-nuget-packages"></a>更智能的 Xamarin Android 支持 v4 / v13 NuGet 包

## <a name="about-the-android-support-libraries"></a>关于 Android 支持库

Google 创建了支持库，以便新功能可以用于旧版 Android。 通常，支持库的名称中会有一个版本号，指明所能兼容的最低 Android API 级别（例如：Support-v4 只能在 API 级别 4 及更高级别上使用。 有关详细信息，请参阅此 [Stack Overflow 讨论](https://stackoverflow.com/questions/9926403/android-support-package-compatibility-library-use-v4-or-v13)）。 

两个支持库（`Support-v4` 和 `Support-v13`）不能同时用于同一个应用；也就是说，它们是互斥的。 这是因为，`Support-v13` 实际上包含 `Support-v4` 的所有类型和实现。 如果尝试在同一项目中引用这两者，则会遇到“类型重复”错误。

## <a name="problems-with-referencing"></a>引用问题

由于 `Support-v4` 已经非常普遍，因此许多第三方库现在都依赖它。 它们本可以选择依赖 Support-v13，但更常见的是依赖 v4  ，因为这可便于任何使用第三方库的应用支持最低为 4 的 API 级别。

如果 Xamarin 第三方库引用绑定到 `Support-v4` 的 `Xamarin.Android.Support.v4.dll`，那么任何使用此库的应用也必须引用 `Xamarin.Android.Support.v4.dll`。 当同一个应用还希望使用绑定到 `Support-v13` 的 `Xamarin.Android.Support.v13.dll` 中的某些功能时，这就成了问题。 如果同时引用这两个绑定，则会遇到“类型重复”错误。

## <a name="type-forwarded-v4-binding-assembly"></a>类型转发 v4 绑定程序集

为了解决此问题，我们创建了特殊 `Xamarin.Android.Support.v4.dll` 程序集，它没有实现，而只有 `[assembly: TypeForwardedTo (..)]` 属性，这些属性将所有 `Support-v4` 类型都转发到 `Xamarin.Android.Support.v13.dll` 程序集内的实现。

也就是说，开发人员可以在应用中引用此类型转发  程序集，这会满足任何第三方库对 `Xamarin.Android.Support.v4.dll` 的引用，同时仍然允许在应用中使用 `Xamarin.Android.Support.v13.dll`。

## <a name="nuget-assistance"></a>NuGet 帮助

虽然开发人员可以手动添加所需的正确引用，但在安装 NuGet 包时，可以使用 NuGet 来帮助选择正确的程序集（常规 v4  绑定或类型转发 v4  程序集）。

因此，`Xamarin.Android.Support.v4` NuGet 包现在包含以下逻辑：

如果应用定目标到 API 级别 13 (Gingerbread 3.2) 或更高级别：

* `Xamarin.Android.Support.v13` NuGet 会自动添加为依赖项
* 将在项目中引用类型转发  `Xamarin.Android.Support.v4.dll`

如果应用定目标到低于 API 级别 13 的任何级别，则将在项目中引用常规 `Xamarin.Android.Support.v4.dll` 绑定。

## <a name="do-i-have-to-use-support-v13"></a>我是否必须使用 Support-v13？

如果应用定目标到 API 级别 13 或更高级别，并且你选择使用 `Xamarin Android Support-v4` NuGet 包，则 `Xamarin Android Support v13` NuGet 包是必需的依赖项。

我们认为，相对于它所带来的兼容性和易用性，足以无视应用大小的轻微增加（两个 .jar 文件相差 17kb）。

如果你坚持要在定目标到 API 级别 13 或更高级别的应用中使用 `Support-v4`，可以始终手动下载 `.nupkg`，解压缩它，并引用程序集。
