---
title: 在 Xamarin.Forms 中的颜色
description: Xamarin.Forms 提供了灵活的跨平台颜色类。 本文介绍了提供颜色类，以及如何使用它的功能。
ms.prod: xamarin
ms.assetid: 22288ABF-57BE-47A9-ACC3-AC604D787C46
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/18/2019
ms.openlocfilehash: 2a17b037803d1ca6e54000ea7ba3f05c8ce6034f
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/13/2020
ms.locfileid: "79305755"
---
# <a name="colors-in-xamarinforms"></a>在 Xamarin.Forms 中的颜色

[![下载示例](~/media/shared/download.png) 下载示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithcolors)

_Xamarin 提供了一个灵活的跨平台颜色类。_

本文介绍了如何在 Xamarin. Forms 中使用[`Color`](xref:Xamarin.Forms.Color)类的各种方法。

[`Color`](xref:Xamarin.Forms.Color)类提供多种方法来生成颜色实例：

- **命名颜色**-常见命名颜色的集合，包括 `Red`、`Green`和 `Blue`。
- **FromHex** -类似于 HTML 中使用的语法的字符串值，例如 "00FF00"。 可以选择将 Alpha 指定为第一对字符（"CC00FF00"）。
- **FromHsla** -色调、饱和度和明度 `double` 值，具有可选的 alpha 值（0.0-1.0）。
- **FromRgb** -红色、绿色和蓝色 `int` 值（0-255）。
- **FromRgba** （红色、绿色、蓝色和 alpha `int` 值（0-255）。
- **FromUint** -设置表示**argb**的单个 `double` 值。

下面是一些示例颜色，它们分配给某些标签使用不同形式的允许语法的 `BackgroundColor`：

```csharp
var red    = new Label { Text = "Red",   BackgroundColor = Color.Red };
var orange = new Label { Text = "Orange",BackgroundColor = Color.FromHex("FF6A00") };
var yellow = new Label { Text = "Yellow",BackgroundColor = Color.FromHsla(0.167, 1.0, 0.5, 1.0) };
var green  = new Label { Text = "Green", BackgroundColor = Color.FromRgb (38, 127, 0) };
var blue   = new Label { Text = "Blue",  BackgroundColor = Color.FromRgba(0, 38, 255, 255) };
var indigo = new Label { Text = "Indigo",BackgroundColor = Color.FromRgb (0, 72, 255) };
var violet = new Label { Text = "Violet",BackgroundColor = Color.FromHsla(0.82, 1, 0.25, 1) };

var transparent = new Label { Text = "Transparent",BackgroundColor = Color.Transparent };
var @default = new Label    { Text = "Default",    BackgroundColor = Color.Default };
var accent = new Label      { Text = "Accent",     BackgroundColor = Color.Accent };
```

以下每个平台上显示这些颜色。 请注意，最终颜色 `Accent` 是适用于 iOS 和 Android 的路子颜色;此值由 Xamarin 定义。

 [![颜色演示](colors-images/colors-sml.png "颜色演示")](colors-images/colors.png#lightbox "颜色演示")

## <a name="colordefault"></a>Color.Default

使用 `Default` 将颜色值设置（或重新设置）到平台默认值（了解对于每个属性，这表示每个平台上的基础颜色都不同）。

开发人员可以使用此值来设置 `Color` 属性，但**不**应在此实例的组件 RGB 值中查询（它们都设置为-1）。

## <a name="colortransparent"></a>Color.Transparent

设置要清除的颜色。

## <a name="coloraccent"></a>Color.Accent

IOS 和 Android 上此实例设置为颜色的对比色的默认背景上可见，但不是默认文本颜色相同。

## <a name="additional-methods"></a>其他方法

[`Color`](xref:Xamarin.Forms.Color)实例包括以下附加方法：

- **AddLuminosity** -通过提供的增量修改发光度来返回 `Color`。
- **MultiplyAlpha** -通过修改 alpha 返回 `Color`，并将其乘以提供的 alpha 值。
- **ToHex** -返回 `Color`的十六进制 `string` 表示形式。
- **WithHue** -返回 `Color`，并将色相替换为提供的值。
- **WithLuminosity** -返回 `Color`，并将发光度替换为提供的值。
- **WithSaturation** -返回 `Color`，并将饱和度替换为提供的值。

## <a name="implicit-conversions"></a>隐式转换

可以执行 `Xamarin.Forms.Color` 和 `System.Drawing.Color` 类型之间的隐式转换：

```csharp
Xamarin.Forms.Color xfColor = Xamarin.Forms.Color.FromRgb(0, 72, 255);
System.Drawing.Color sdColor = System.Drawing.Color.FromArgb(38, 127, 0);

// Implicity convert from a Xamarin.Forms.Color to a System.Drawing.Color
System.Drawing.Color sdColor2 = xfColor;

// Implicitly convert from a System.Drawing.Color to a Xamarin.Forms.Color
Xamarin.Forms.Color xfColor2 = sdColor;
```

## <a name="deviceruntimeplatform"></a>Device.RuntimePlatform

此代码片段使用 `Device.RuntimePlatform` 属性有选择地设置 `ActivityIndicator`的颜色：

```csharp
ActivityIndicator activityIndicator = new ActivityIndicator
{
    Color = Device.RuntimePlatform == Device.iOS ? Color.Black : Color.Default,
    IsRunning = true
};
```

## <a name="using-from-xaml"></a>从 XAML 使用

使用定义的颜色名称或此处显示的十六进制表示形式，还可以在 XAML 中引用颜色：

```xaml
<Label Text="Sea color" BackgroundColor="Aqua" />
<Label Text="RGB" BackgroundColor="#00FF00" />
<Label Text="Alpha plus RGB" BackgroundColor="#CC00FF00" />
<Label Text="Tiny RGB" BackgroundColor="#0F0" />
<Label Text="Tiny Alpha plus RGB" BackgroundColor="#C0F0" />
```

> [!NOTE]
> 当使用 XAML 编译，颜色名称是不区分大小写，并因此可以编写以小写形式。 有关 XAML 编译的详细信息，请参阅 [XAML 编译](~/xamarin-forms/xaml/xamlc.md)。

## <a name="related-links"></a>相关链接

- [ColorsSample](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithcolors)
- [可绑定选取器（示例）](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-bindablepicker)
