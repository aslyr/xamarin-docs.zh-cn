---
title: Xamarin 中的字体
description: 本文介绍如何在 Xamarin. Forms 应用程序中指定显示文本的控件的字体信息。
ms.prod: xamarin
ms.assetid: 49DD2249-C575-41AE-AE06-08F890FD6031
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/01/2020
ms.openlocfilehash: 160cbbfa99114d74fa5fdaa5f92b8397ea7d3367
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "82516483"
---
# <a name="fonts-in-xamarinforms"></a>Xamarin 中的字体

[![下载示例](~/media/shared/download.png)下载示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithfonts)

本文介绍如何通过 Xamarin 来指定显示文本的控件的字体特性（包括权重和大小）。 字体信息可以[在代码中指定](#Setting_Font_in_Code)，也可以[在 XAML 中指定](#Setting_Font_in_Xaml)。 它还可以使用[自定义字体](#use-a-custom-font)并[显示字体图标](#display-font-icons)。

<a name="Setting_Font_in_Code" />

## <a name="set-the-font-in-code"></a>设置代码中的字体

使用显示文本的任何控件的三个与字体相关的属性：

- **FontFamily** &ndash; FontFamily `string`字体名称。
- 将字体大小**FontSize** &ndash;为`double`。
- **FontAttributes** &ndash;指定样式信息（如*斜体*和**粗体**）的字符串（ `FontAttributes`使用 c # 中的枚举）。

此代码演示如何创建标签，并指定要显示的字体大小和粗细：

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

`FontSize`属性可以设置为双精度值，例如：

```csharp
label.FontSize = 24;
```

大小值以与设备无关的单位来度量。 有关详细信息，请参阅[测量单位](~/xamarin-forms/user-interface/controls/common-properties.md#units-of-measurement)。

Xamarin 还会在[`NamedSize`](xref:Xamarin.Forms.NamedSize)枚举中定义表示特定字体大小的字段。 有关命名字体大小的详细信息，请参阅[命名字体大小](#named-font-sizes)。

<a name="FontAttributes" />

### <a name="font-attributes"></a>字体特性

可以在`FontAttributes`属性上设置**粗体**和*斜体*等字体样式。 目前支持以下值：

- **无**
- **Bold**
- **字体**

可以`FontAttribute`按如下所示使用枚举（可以指定单个属性，也`OR`可以将它们一起指定）：

```csharp
label.FontAttributes = FontAttributes.Bold | FontAttributes.Italic;
```

### <a name="set-font-info-per-platform"></a>为每个平台设置字体信息

或者，可以`Device.RuntimePlatform`使用属性在每个平台上设置不同的字体名称，如以下代码所示：

```csharp
label.FontFamily = Device.RuntimePlatform == Device.iOS ? "MarkerFelt-Thin" :
   Device.RuntimePlatform == Device.Android ? "Lobster-Regular.ttf#Lobster-Regular" : "Assets/Fonts/ArimaMadurai-Black.ttf#Arima Madurai",
label.FontSize = Device.RuntimePlatform == Device.iOS ? 24 :
   Device.RuntimePlatform == Device.Android ? Device.GetNamedSize(NamedSize.Medium, label) : Device.GetNamedSize(NamedSize.Large, label);
```

IOS 的字体信息的良好源是[iosfonts.com](http://iosfonts.com)。

<a name="Setting_Font_in_Xaml" />

## <a name="set-the-font-in-xaml"></a>设置 XAML 中的字体

用于显示文本的窗体控件都具有可`FontSize`在 XAML 中设置的属性。 若要在 XAML 中设置字体，最简单的方法是使用已命名大小的枚举值，如以下示例中所示：

```xaml
<Label Text="Login" FontSize="Large"/>
<Label Text="Instructions" FontSize="Small"/>
```

有一个用于`FontSize`属性的内置转换器，该转换器允许在 XAML 中将所有字体设置表示为字符串值。 此外， `FontAttributes`属性可用于指定字体属性：

```xaml
<Label Text="Italics are supported" FontAttributes="Italic" />
<Label Text="Biggest NamedSize" FontSize="Large" />
<Label Text="Use size 72" FontSize="72" />
```

[`Device.RuntimePlatform`](~/xamarin-forms/platform/device.md#provide-platform-specific-values)属性还可以在 XAML 中用于在每个平台上呈现不同的字体。 以下示例在每个平台上使用不同的字体：

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

Xamarin。 Forms 定义[`NamedSize`](xref:Xamarin.Forms.NamedSize)枚举中的字段，这些字段表示特定字体大小。 下表显示了成员`NamedSize`以及它们在 IOS、Android 和通用 WINDOWS 平台（UWP）上的默认大小：

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

大小值以与设备无关的单位来度量。 有关详细信息，请参阅[测量单位](~/xamarin-forms/user-interface/controls/common-properties.md#units-of-measurement)。

可以通过 XAML 和代码设置命名字体大小。 此外，还可以`Device.GetNamedSize`调用方法返回`double`表示命名字体大小的：

```csharp
label.FontSize = Device.GetNamedSize(NamedSize.Small, typeof(Label));
```

> [!NOTE]
> 在 iOS 和 Android 上，基于操作系统的辅助功能选项，命名字体大小将自动缩放。 可以在使用特定于平台的 iOS 上禁用此行为。 有关详细信息，请参阅[iOS 上命名字体大小的辅助功能缩放](~/xamarin-forms/platform/ios/named-font-size-scaling.md)。

## <a name="use-a-custom-font"></a>使用自定义字体

自定义字体可以添加到 Xamarin 共享项目中，并可由平台项目使用，无需任何其他操作。 完成此目的的过程如下所示：

1. 将该字体作为嵌入资源添加到 Xamarin. Forms 共享项目中（**生成操作： EmbeddedResource**）。
1. 使用`ExportFont`属性在文件（如**AssemblyInfo.cs**）中将字体文件注册到程序集。 还可以指定可选别名。

> [!IMPORTANT]
> 嵌入字体需要使用 Xamarin. Forms 4.5.0.530 或更高版本。

下面的示例显示了向程序集注册的 Lobster 常规字体以及别名：

```csharp
using Xamarin.Forms;

[assembly: ExportFont("Lobster-Regular.ttf", Alias = "Lobster")]
```

> [!NOTE]
> 该字体可以驻留在共享项目的任何文件夹中，而无需在向程序集注册字体时指定文件夹名称。

然后，可以在每个平台上使用该字体，只需引用其名称，无需文件扩展名：

```xaml
<!-- Use font name -->
<Label Text="Hello Xamarin.Forms"
       FontFamily="Lobster-Regular" />
```

或者，通过引用其别名，可以在每个平台上使用它：

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

以下屏幕截图显示自定义字体：

[![IOS 和 Android 上的自定义字体](fonts-images/custom-sml.png "自定义字体示例")](fonts-images/custom.png#lightbox "自定义字体示例")

> [!IMPORTANT]
> 在 Windows 上，字体文件的名称和字体名称可能不同。 若要在 Windows 上发现字体名称，请右键单击 .ttf 文件并选择 "**预览**"。 然后，可以从预览窗口确定字体名称。

## <a name="display-font-icons"></a>显示字体图标

Xamarin 可以通过在`FontImageSource`对象中指定字体图标数据来显示字体图标。 此类派生自[`ImageSource`](xref:Xamarin.Forms.ImageSource)类，具有以下属性：

- `Glyph`–指定为的`string`字体图标的 unicode 字符值。
- `Size`–一个`double`值，该值指示呈现的字体图标的大小（以与设备无关的单位表示）。 默认值为 30。 此外，此属性可以设置为命名字体大小。
- `FontFamily`– `string`表示字体图标所属的字体系列的。
- `Color`–显示字体[`Color`](xref:Xamarin.Forms.Color)图标时要使用的可选值。

此数据用于创建一个 PNG，可以通过任何可显示的视图来显示该 PNG `ImageSource`。 此方法允许多个视图显示字体图标（如表情符号），而不是将字体图标显示限制为单个文本显示视图，例如[`Label`](xref:Xamarin.Forms.Label)。

> [!IMPORTANT]
> 字体图标当前只能由其 unicode 字符表示形式指定。

下面的 XAML 示例显示一个由[`Image`](xref:Xamarin.Forms.Image)视图显示的字体图标：

```xaml
<Image BackgroundColor="#D1D1D1">
    <Image.Source>
        <FontImageSource Glyph="&#xf30c;"
                         FontFamily="{OnPlatform iOS=Ionicons, Android=ionicons.ttf#}"
                         Size="44" />
    </Image.Source>
</Image>
```

此代码在[`Image`](xref:Xamarin.Forms.Image)视图中显示 Ionicons 字体系列的 XBox 图标。 请注意，虽然此图标的 unicode 字符是`\uf30c`，但它必须在 XAML 中进行转义，因此`&#xf30c;`会变得如此。 等效 C# 代码如下：

```csharp
Image image = new Image { BackgroundColor = Color.FromHex("#D1D1D1") };
image.Source = new FontImageSource
{
    Glyph = "\uf30c",
    FontFamily = Device.RuntimePlatform == Device.iOS ? "Ionicons" : "ionicons.ttf#",
    Size = 44
};
```

下面的屏幕截图中，从 "可[绑定](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-bindablelayouts)布局" 示例中，显示了多个字体图标，可通过可绑定布局显示：

![在 iOS 和 Android 上显示的字体图标屏幕截图](fonts-images/font-image-source.png "在图像视图中显示的字体图标")

## <a name="related-links"></a>相关链接

- [FontsSample](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithfonts)
- [文本（示例）](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)
- [可绑定布局（示例）](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-bindablelayouts)
- [可绑定的布局](~/xamarin-forms/user-interface/layouts/bindable-layouts.md)
