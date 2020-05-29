---
title: ''
description: 本文介绍如何优化 Xamarin.Forms 平板电脑的应用程序布局，而不是手机。
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 8ce5ba09f89c2bc84b7f6ba722f724ae39c0222e
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/28/2020
ms.locfileid: "84137916"
---
# <a name="layout-for-tablet-and-desktop-apps"></a>平板电脑和桌面应用的布局

Xamarin.Forms支持在受支持的平台上使用的所有设备类型，因此除了手机外，应用程序还可以在以下设备上运行：

- Ipad
- Android 平板电脑、
- Windows 平板电脑和台式计算机（运行 Windows 10）。

本页简要讨论：

- 支持的[设备类型](#Device_Types)和
- 如何[优化](#optimize)平板电脑与手机的布局。

<a name="Device_Types" />

## <a name="device-types"></a>设备类型

更大的屏幕设备可用于支持的所有平台 Xamarin.Forms 。

### <a name="ipads-ios"></a>Ipad （iOS）

该 Xamarin.Forms 模板通过将**Info.plist > 设备**设置配置为**通用**（这意味着支持 iPhone 和 iPad）来自动包含 iPad 支持。

若要提供令人愉快的启动体验，并确保在所有设备上都使用全屏分辨率，应确保提供[iPad 特定的启动屏幕](~/ios/app-fundamentals/images-icons/launch-screens.md)（使用情节提要）。 这可确保在 iPad 迷你、iPad 和 iPad Pro 设备上正确呈现应用。

在 iOS 9 之前，所有应用程序都在设备上占用整个屏幕，但有些 Ipad 现在可以执行[拆分屏幕多任务](~/ios/platform/multitasking.md)。
这意味着，你的应用程序可能会在屏幕的一侧（50% 的宽度）或整个屏幕上只占用一个超薄柱形。

[![](tablet-images/ipad-sml.png "iPad Split Screen Example")](tablet-images/ipad.png#lightbox "iPad Split Screen Example")

拆分屏幕功能意味着您应该将您的应用程序设计为在最大宽度为320像素的情况下正常工作，或者尽可能多1366像素。

### <a name="android-tablets"></a>Android 平板电脑

Android 生态系统提供多种受支持的屏幕大小（从小手机到大平板电脑）。 Xamarin.Forms可以支持所有屏幕大小，但与其他平台一样，你可能需要调整更大设备的用户界面。

支持许多不同的屏幕分辨率时，可以提供不同大小的本机映像资源来优化用户体验。
若要详细了解如何在 Android 应用程序项目中构造文件夹和文件名以在应用程序中包含优化的映像资源，请查看[android 资源](~/android/app-fundamentals/resources-in-android/index.md)文档（特别是[创建不同屏幕大小的资源](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md)）。

### <a name="windows-tablets-and-desktops"></a>Windows 平板电脑和桌面

若要支持运行 Windows 的平板电脑和台式计算机，你将需要使用[WINDOWS UWP 支持](~/xamarin-forms/platform/windows/installation/index.md)，该支持生成在 windows 10 上运行的通用应用。

除了运行全屏外，还可以将 Windows 平板电脑和桌面上运行的应用调整为任意尺寸。

[![](tablet-images/splitscreen-sml.png "Windows Split Screen Example")](tablet-images/splitscreen.png#lightbox "Windows Split Screen Example")

<a name="optimize" />

## <a name="optimizing-for-tablet-and-desktop"></a>为平板电脑和桌面优化

你可以根据 Xamarin.Forms 手机或平板电脑/桌面设备是否正在使用来调整你的用户界面。 这意味着你可以优化大屏幕设备（如平板电脑和台式计算机）的用户体验。

### <a name="deviceidiom"></a>Device。

可以使用 [`Device`](~/xamarin-forms/platform/device.md) 类更改应用或用户界面的行为。 使用 `Device.Idiom` 枚举可以

```csharp
if (Device.Idiom == TargetIdiom.Phone)
{
    HeroImage.Source = ImageSource.FromFile("hero.jpg");
} else {
    HeroImage.Source = ImageSource.FromFile("herotablet.jpg");
}
```

此方法可以进行扩展，以对单个页面布局进行重大更改，甚至在较大屏幕上呈现完全不同的页面。

### <a name="leveraging-masterdetailpage"></a>利用 MasterDetailPage

适用 [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) 于更大的屏幕，特别是在 iPad 上，它使用 [`UISplitViewController`](xref:UIKit.UISplitViewController) 提供本机 iOS 体验。

查看[此 Xamarin 博客文章](https://devblogs.microsoft.com/xamarin/bringing-xamarin-forms-apps-to-tablets/)，了解如何改编你的用户界面，以便手机使用一个布局，较大的屏幕可以使用另一个布局 `MasterDetailPage` 。

## <a name="related-links"></a>相关链接

- [Xamarin 博客](https://devblogs.microsoft.com/xamarin/bringing-xamarin-forms-apps-to-tablets/)
- [MyShoppe 示例](https://github.com/jamesmontemagno/myshoppe)
