---
title: Xamarin.Essentials:应用主题
description: 本文档介绍了 Xamarin.Essentials 中请求的应用主题 API，它提供了系统为正在运行的应用请求什么主题样式的信息。
ms.assetid: F6F6D496-A8A9-4B9A-AF1A-370D937E5073
author: jamesmontemagno
ms.author: jamont
ms.date: 01/06/2020
ms.openlocfilehash: e27f43ae17fcdaf7bb40b75907a7595c5c20988b
ms.sourcegitcommit: 9c517f2c5af21aab0775f91286f42e919ff52292
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/01/2020
ms.locfileid: "82619863"
---
# <a name="xamarinessentials-app-theme"></a>Xamarin.Essentials:应用主题

RequestedTheme  API 属于 [`AppInfo`](app-information.md) 类，它提供了系统为正在运行的应用请求什么主题的信息。

## <a name="get-started"></a>入门

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-requestedtheme"></a>使用 RequestedTheme

在你的类中添加对 Xamarin.Essentials 的引用：

```csharp
using Xamarin.Essentials;
```

## <a name="obtaining-theme-information"></a>获取主题信息

可以使用下面的 API 来检测请求的应用主题：

```csharp
AppTheme appTheme = AppInfo.RequestedTheme;

```

这会提供系统当前为应用请求的主题。 返回下列值之一：

* 未指定
* 浅
* 深

如果操作系统没有要请求的特定用户界面样式，则返回“Unspecified”。 例如，设备在版本低于 13.0 的 iOS 上运行。


## <a name="platform-implementation-specifics"></a>平台实现细节

# <a name="android"></a>[Android](#tab/android)

Android 使用配置模式来指定要向用户请求的主题类型。 它可以由用户更改，也可以在节电模式启用时更改，具体视 Android 版本而定。

有关详细信息，请参阅官方的 [Android 深色主题文档](https://developer.android.com/guide/topics/ui/look-and-feel/darktheme)。


# <a name="ios"></a>[iOS](#tab/ios)

在版本低于 13.0 的 iOS 上，始终返回“Unspecified” 


# <a name="uwp"></a>[UWP](#tab/uwp)

必须在 UI 线程上调用 `RequestedTheme`，否则将引发异常。

UWP 应用程序将在 RUWP App.xaml 中的 RequestedTheme 下反映出你的设置  。 如果将它设置为特定主题，则 Xamarin.Essentials 将始终返回此设置。 要使用操作系统的动态主题，请从应用程序中删除此节点，然后在应用运行时，它将返回用户在 Windows 设置中（“设置”>“个性化”>“颜色”>“选择默认应用模式”）设置的主题  。

有关详细信息，请参阅 [UWP RequestedTheme 文档](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.requestedtheme)。

--------------

## <a name="api"></a>API

- [AppInfo 源代码](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/AppInfo)
- [AppInfo API 文档](xref:Xamarin.Essentials.AppInfo)
