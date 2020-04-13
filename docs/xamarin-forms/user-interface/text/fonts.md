---
title: Xamarin 中的字体。
description: 本文介绍如何指定在 Xamarin.Forms 应用程序中显示文本的控件上的字体信息。
ms.prod: xamarin
ms.assetid: 49DD2249-C575-41AE-AE06-08F890FD6031
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/01/2020
ms.openlocfilehash: 4f14d66e5321a0fb19078be4d97ae4df69f688c9
ms.sourcegitcommit: 6f3281a32017cfcebadde8a2d6e10651a277828f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/02/2020
ms.locfileid: "80587435"
---
# <a name="fonts-in-xamarinforms"></a>Xamarin 中的字体。

[![下载示例](~/media/shared/download.png)下载示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithfonts)

本文介绍了 Xamarin.Forms 如何允许您在显示文本的控件上指定字体属性（包括粗细和大小）。 字体信息可以在[代码中指定](#Setting_Font_in_Code)或在[XAML 中指定](#Setting_Font_in_Xaml)。 也可以使用[自定义字体](#Using_a_Custom_Font)和[显示字体图标](#display-font-icons)。

<a name="Setting_Font_in_Code" />

## <a name="set-the-font-in-code"></a>在代码中设置字体

使用显示文本的任何控件的三个与字体相关的属性：

- **字体系列**&ndash;字体`string`名称。
- **字体将**&ndash;字体大小调整为`double`。
- **Fontattributes 一**&ndash;个字符串，指定样式信息（如*斜体*和**粗体**（使用 C# 中的枚`FontAttributes`举）。

此代码演示如何创建标签并指定要显示的字体大小和粗细：

```csharp
var about = new Label
{
    FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
    FontAttributes = FontAttributes.Bold,
    Text = "Medium Bold Font"
};
```

<a name="FontSize" />

### <a name="font-size"></a>字号

属性`FontSize`可以设置为双精度值，例如：

```csharp
label.FontSize = 24;
```

大小值以与设备无关的单位度量。 有关详细信息，请参阅[度量单位](~/xamarin-forms/user-interface/controls/common-properties.md#units-of-measurement)。

Xamarin.Forms 还定义枚举中[`NamedSize`](xref:Xamarin.Forms.NamedSize)表示特定字体大小的字段。 有关命名字体大小的详细信息，请参阅[命名字体大小](#named-font-sizes)。

<a name="FontAttributes" />

### <a name="font-attributes"></a>字体属性

可以在`FontAttributes`属性上设置粗**体**和*斜体*等字体样式。 当前支持以下值：

- **无**
- **大胆**
- **斜**

枚`FontAttribute`举可以如下所示（您可以一起指定单个属性或`OR`它们）：

```csharp
label.FontAttributes = FontAttributes.Bold | FontAttributes.Italic;
```

### <a name="set-font-info-per-platform"></a>设置每个平台的字体信息

或者，`Device.RuntimePlatform`该属性可用于在每个平台上设置不同的字体名称，如以下代码所示：

```csharp
label.FontFamily = Device.RuntimePlatform == Device.iOS ? "MarkerFelt-Thin" :
   Device.RuntimePlatform == Device.Android ? "Lobster-Regular.ttf#Lobster-Regular" : "Assets/Fonts/ArimaMadurai-Black.ttf#Arima Madurai",
label.FontSize = Device.RuntimePlatform == Device.iOS ? 24 :
   Device.RuntimePlatform == Device.Android ? Device.GetNamedSize(NamedSize.Medium, label) : Device.GetNamedSize(NamedSize.Large, label);
```

iOS 的字体信息来源[iosfonts.com。](http://iosfonts.com)

<a name="Setting_Font_in_Xaml" />

## <a name="set-the-font-in-xaml"></a>在 XAML 中设置字体

显示文本的 Xamarin.Forms 控件都`FontSize`具有可在 XAML 中设置的属性。 在 XAML 中设置字体的最简单方法是使用命名的大小枚举值，如以下示例所示：

```xaml
<Label Text="Login" FontSize="Large"/>
<Label Text="Instructions" FontSize="Small"/>
```

属性有一个内置转换器`FontSize`，允许在 XAML 中将所有字体设置表示为字符串值。 此外，`FontAttributes`该属性可用于指定字体属性：

```xaml
<Label Text="Italics are supported" FontAttributes="Italic" />
<Label Text="Biggest NamedSize" FontSize="Large" />
<Label Text="Use size 72" FontSize="72" />
```

该[`Device.RuntimePlatform`](~/xamarin-forms/platform/device.md#providing-platform-specific-values)属性还可用于 XAML 中，以在每个平台上呈现不同的字体。 下面的示例在每个平台上使用不同的字体：

```xaml
<Label Text="Hello Forms with XAML">
    <Label.FontFamily>
        <OnPlatform x:TypeArguments="x:String">
                <On Platform="iOS" Value="MarkerFelt-Thin" />
                <On Platform="Android" Value="Lobster-Regular.ttf#Lobster-Regular" />
                <On Platform="UWP" Value="Assets/Fonts/ArimaMadurai-Black.ttf#Arima Madurai" />
        </OnPlatform>
    </Label.FontFamily>
</Label>
```

## <a name="named-font-sizes"></a>命名字号

Xamarin.Forms 在枚举中[`NamedSize`](xref:Xamarin.Forms.NamedSize)定义表示特定字体大小的字段。 下表显示了`NamedSize`iOS、Android 和通用 Windows 平台 （UWP） 上的成员及其默认大小：

| 成员 | iOS | Android | UWP |
| --- | --- | --- | --- |
| `Default` | 16 | 14 | 14 |
| `Micro` | 11 | 10 | 15.667 |
| `Small` | 13 | 14 | 18.667 |
| `Medium` | 16 | 17 | 22.667 |
| `Large` | 20 | 22 | 32 |
| `Body` | 17 | 16 | 14 |
| `Header` | 17 | 96 | 46 |
| `Title` | 28 | 24 | 24 |
| `Subtitle` | 22 | 16 | 20 |
| `Caption` | 12 | 12 | 12 |

大小值以与设备无关的单位度量。 有关详细信息，请参阅[度量单位](~/xamarin-forms/user-interface/controls/common-properties.md#units-of-measurement)。

命名字体大小可以通过 XAML 和代码进行设置。 此外，`Device.GetNamedSize`还可以调用 方法以返回表示命名字体`double`大小的 的 。

```csharp
label.FontSize = Device.GetNamedSize(NamedSize.Small, typeof(Label));
```

> [!NOTE]
> 在 iOS 和 Android 上，命名字体大小将根据操作系统辅助功能选项自动缩放。 此行为可以在 iOS 上使用特定于平台的 iOS 禁用。 有关详细信息，请参阅[iOS 上命名字体大小的辅助功能缩放](~/xamarin-forms/platform/ios/named-font-size-scaling.md)。

<a name="Using_a_Custom_Font" />

## <a name="use-a-custom-font"></a>使用自定义字体

使用内置字体以外的字体需要一些特定于平台的编码。 此屏幕截图显示了使用 Xamarin.Forms 呈现[的 Google 开源字体中的自定义字体](https://www.google.com/fonts)**龙虾**。

 [![iOS 和 Android 上的自定义字体](fonts-images/custom-sml.png "自定义字体示例")](fonts-images/custom.png#lightbox "自定义字体示例")

下面概述了每个平台所需的步骤。 将自定义字体文件与应用程序一起包含时，请确保验证该字体的许可证是否允许分发。

### <a name="ios"></a>iOS

可以通过首先确保加载自定义字体，然后使用 Xamarin.Forms`Font`方法按名称引用自定义字体来显示它。
按照[此博客文章](https://devblogs.microsoft.com/xamarin/custom-fonts-in-ios/)的说明：

1. 使用生成操作添加字体文件 **：捆绑资源**，以及
2. 更新**Info.plist**文件（**应用程序提供的字体**或`UIAppFonts`， 键）， 然后
3. 在 Xamarin 中定义字体时，请按名称引用它。

```csharp
new Label
{
    Text = "Hello, Forms!",
    FontFamily = Device.RuntimePlatform == Device.iOS ? "Lobster-Regular" : null // set only for iOS
}
```

### <a name="android"></a>Android

适用于 Android 的 Xamarin.Forms 可以引用已添加到项目的自定义字体，该自定义字体遵循特定的命名标准。 首先将字体文件添加到应用程序项目中**的资产**文件夹，并设置*生成操作：AndroidAsset*。 然后使用完整路径和*字体名称*分隔以哈希 （#） 作为 Xamarin.Forms 中的字体名称，如下代码段所示：

```csharp
new Label
{
  Text = "Hello, Forms!",
  FontFamily = Device.RuntimePlatform == Device.Android ? "Lobster-Regular.ttf#Lobster-Regular" : null // set only for Android
}
```

### <a name="windows"></a>Windows

适用于 Windows 平台的 Xamarin.窗体可以引用已添加到项目的自定义字体，该自定义字体遵循特定的命名标准。 首先将字体文件添加到应用程序项目中的 **/Assets/Fonts/** 文件夹，然后设置**生成操作：内容**。 然后使用完整路径和字体文件名，后跟哈希 （#） 和**字体名称**，如下代码段所示：

```csharp
new Label
{
    Text = "Hello, Forms!",
    FontFamily = Device.RuntimePlatform == Device.UWP ? "Assets/Fonts/Lobster-Regular.ttf#Lobster" : null // set only for UWP apps
}
```

> [!NOTE]
> 请注意，字体文件名和字体名称可能不同。 要在 Windows 上发现字体名称，请右键单击 .ttf 文件并选择 **"预览**"。 然后可以从预览窗口中确定字体名称。

### <a name="xaml"></a>XAML

您还可以使用[`Device.RuntimePlatform`](~/xamarin-forms/platform/device.md#interact-with-the-ui-from-background-threads)XAML 来呈现自定义字体：

```xaml
<Label Text="Hello Forms with XAML">
    <Label.FontFamily>
        <OnPlatform x:TypeArguments="x:String">
                <On Platform="iOS" Value="Lobster-Regular" />
                <On Platform="Android" Value="Lobster-Regular.ttf#Lobster-Regular" />
                <On Platform="UWP" Value="Assets/Fonts/Lobster-Regular.ttf#Lobster" />
        </OnPlatform>
    </Label.FontFamily>
</Label>
```

## <a name="use-a-custom-font-preview"></a>使用自定义字体 （预览）

自定义字体可以添加到 Xamarin.Forms 共享项目中，并由平台项目使用，而无需任何其他工作。 完成此目的的过程如下所示：

1. 将字体添加到 Xamarin.Forms 共享项目中作为嵌入式资源（**生成操作：嵌入式资源**）。
1. 使用`ExportFont`属性在**AssemblyInfo.cs**等文件中向程序集注册字体文件。 也可以指定可选别名。

> [!NOTE]
> 嵌入字体需要使用 Xamarin.窗体 4.5.0.530 或更高版本。

下面的示例显示了在程序集中注册的龙虾常规字体以及别名：

```csharp
using Xamarin.Forms;

[assembly: ExportFont("Lobster-Regular.ttf", Alias = "Lobster")]
```

> [!NOTE]
> 字体可以驻留在共享项目中的任何文件夹中，而无需在向程序集注册字体时指定文件夹名称。

然后，通过引用其名称，无需文件扩展名，可以在每个平台上使用字体：

```xaml
<!-- Use font name -->
<Label Text="Hello Xamarin.Forms"
       FontFamily="Lobster-Regular" />
```

或者，可以通过引用其别名在每个平台上使用它：

```xaml
<!-- Use font alias -->
<Label Text="Hello Xamarin.Forms"
       FontFamily="Lobster" />
```

等效 C# 代码如下：

```csharp
// Use font name
Label label1 = new Label
{
    Text = "Hello Xamarin.Forms!",
    FontFamily = "Lobster-Regular"
};

// Use font alias
Label label2 = new Label
{
    Text = "Hello Xamarin.Forms!",
    FontFamily = "Lobster"
};
```

以下屏幕截图显示了自定义字体：

[![iOS 和 Android 上的自定义字体](fonts-images/custom-sml.png "自定义字体示例")](fonts-images/custom.png#lightbox "自定义字体示例")

## <a name="display-font-icons"></a>显示字体图标

Xamarin.Forms 应用程序可以通过指定`FontImageSource`对象中的字体图标数据来显示字体图标。 此类派生自类[`ImageSource`](xref:Xamarin.Forms.ImageSource)，具有以下属性：

- `Glyph`• 字体图标的单码字符值，指定为`string`。
- `Size`•`double`指示呈现字体图标的大小（以设备无关单位）的值。 默认值为 30。 此外，此属性可以设置为命名字体大小。
- `FontFamily`•`string`表示字体图标所属的字体系列。
- `Color`• 显示[`Color`](xref:Xamarin.Forms.Color)字体图标时要使用的可选值。

此数据用于创建 PNG，该 PNG 可以由可以显示 的任何视图显示`ImageSource`。 此方法允许字体图标（如表情符号）由多个视图显示，而不是将字体图标显示限制为单个文本呈现视图（如[`Label`](xref:Xamarin.Forms.Label)）。

> [!IMPORTANT]
> 字体图标目前只能由其单码字符表示形式指定。

以下 XAML 示例有一个视图显示的[`Image`](xref:Xamarin.Forms.Image)字体图标：

```xaml
<Image BackgroundColor="#D1D1D1">
    <Image.Source>
        <FontImageSource Glyph="&#xf30c;"
                         FontFamily="{OnPlatform iOS=Ionicons, Android=ionicons.ttf#}"
                         Size="44" />
    </Image.Source>
</Image>
```

此代码在[`Image`](xref:Xamarin.Forms.Image)视图中显示来自 Ionicon 字体系列的 XBox 图标。 请注意，虽然此图标的单码字符是`\uf30c`，但它必须在 XAML 中转义，因此变为`&#xf30c;`。 等效 C# 代码如下：

```csharp
Image image = new Image { BackgroundColor = Color.FromHex("#D1D1D1") };
image.Source = new FontImageSource
{
    Glyph = "\uf30c",
    FontFamily = Device.RuntimePlatform == Device.iOS ? "Ionicons" : "ionicons.ttf#",
    Size = 44
};
```

以下屏幕截图（来自[可绑定布局示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-bindablelayouts)）显示了可绑定布局显示的几个字体图标：

![在 iOS 和 Android 上显示的字体图标的屏幕截图](fonts-images/font-image-source.png "图像视图中显示的字体图标")

## <a name="related-links"></a>相关链接

- [字体示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithfonts)
- [文本（示例）](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)
- [可绑定布局（示例）](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-bindablelayouts)
- [可绑定的布局](~/xamarin-forms/user-interface/layouts/bindable-layouts.md)
