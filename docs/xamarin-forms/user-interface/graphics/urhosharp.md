---
title: 使用 UrhoSharpXamarin.Forms
description: 本文介绍如何使用 UrhoSharp 将三维图形添加到 Xamarin.Forms 应用程序中，以便进行高级可视化。
ms.prod: xamarin
ms.assetid: 0646B98E-CC04-4537-9715-9F82338FD7FF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/11/2016
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: fbf717092da7f77e265803fae87efb5bf0e9876f
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/18/2020
ms.locfileid: "84574361"
---
# <a name="using-urhosharp-in-xamarinforms"></a>使用 UrhoSharpXamarin.Forms

[![下载示例](~/media/shared/download.png) 下载示例](https://github.com/xamarin/urho-samples/tree/master/FormsSample)

## <a name="what-is-urhosharp"></a>什么是 UrhoSharp？

[UrhoSharp](~/graphics-games/urhosharp/index.md)是一个功能强大的3d 引擎，适用于 Xamarin 和 .net 开发人员。 [简介](~/graphics-games/urhosharp/introduction.md)介绍了有关 UrhoSharp 库的详细信息，[这些说明](~/graphics-games/urhosharp/using.md)介绍了如何对场景和操作进行编程。

UrhoSharp 可用于在应用程序中呈现图形 Xamarin.Forms 。
此[示例](https://github.com/xamarin/urho-samples/tree/master/FormsSample)演示如何使用 UrhoSharp 构造交互式三维图表：

![](urhosharp-images/ios-animation.gif "UrhoSharp 3D Interactive Chart on iOS")
![](urhosharp-images/android-animation.gif "UrhoSharp 3D Interactive Chart on Android")

## <a name="adding-the-urhosharp-nuget-packages"></a>添加 UrhoSharp NuGet 包

使用 UrhoSharp 之前，开发人员需要将 UrhoSharp NuGet 包添加到其解决方案。 本指南假定 Xamarin.Forms 项目包含 iOS、Android 和 .NET Standard 库项目。 所有代码都将写入 .NET Standard 库项目中;但是，还必须将 UrhoSharp NuGet 添加到 iOS 和 Android 项目。

UrhoSharp NuGet 包包含创建 UrhoSharp 对象所需的所有对象。 UrhoSharp NuGet 包包含 `UrhoSurface` 用于在中承载 UrhoSharp 的类 Xamarin.Forms 。
若要开始，请右键单击 .NET Standard 库项目中的 "**包**" 文件夹，然后选择 "**添加包 ...**"。输入搜索词 " **UrhoSharp**"，选择 " ** Xamarin.Forms UrhoSharp **"，然后单击 "**添加包**"。

[![](urhosharp-images/add-package-sml.png "Add Packages Dialog")](urhosharp-images/add-package.png#lightbox "Add Packages Dialog")

将 UrhoSharp NuGet 包添加到项目中：

![](urhosharp-images/packages.png "Packages Folder")

针对特定于平台的项目（如 iOS 和 Android）重复上述步骤。

## <a name="walkthrough-adding-urhosharp-to-a-xamarinforms-app"></a>演练：向 Xamarin.Forms 应用程序添加 UrhoSharp

以下步骤介绍了 UrhoSharp 示例中的代码 Xamarin.Forms ：

1. [创建 Xamarin 窗体页](#1-create-a-xamarin-forms-page)
2. [添加 UrhoSurface](#2-add-the-urhosurface)
3. [构建 Urho 应用程序](#3-build-an-urho-application)
4. [将图表类添加到 UrhoSurface](#4-add-the-charts-class-to-the-urhosurface)
5. [与 UrhoSharp 交互](#5-interacting-with-urhosharp)

请注意，此示例使用 c # 6 功能，并且可能无法在较旧版本的 Visual Studio 中进行编译。

### <a name="1-create-a-xamarin-forms-page"></a>1. 创建 Xamarin 窗体页

下面的代码显示在 Xamarin.Forms `UrhoPage` 添加了任何 Urho 相关代码之前的一页：

```csharp
public class UrhoPage : ContentPage
{
  Slider selectedBarSlider, rotationSlider;

  public UrhoPage()
  {
    // we'll add Urho later

    rotationSlider = new Slider(0, 500, 250);

    selectedBarSlider = new Slider(0, 5, 2.5);

    Title = " UrhoSharp + Xamarin.Forms";
    Content = new StackLayout {
      Padding = new Thickness (12, 12, 12, 40),
      VerticalOptions = LayoutOptions.FillAndExpand,
      Children = {
        rotationSlider,
        new Label { Text = "SELECTED VALUE:" },
        selectedBarSlider,
      }
    };
  }
```

### <a name="2-add-the-urhosurface"></a>2. 添加 UrhoSurface

UrhoSharp 可以在 `ContentPage` 类似于其他控件中承载 Xamarin.Forms 。
下面的代码片段演示了 `UrhoSurface` 如何添加到 Xamarin.Forms 页面：

```csharp
using Urho;
using Urho.Forms;
...
public class UrhoPage : ContentPage
{
  UrhoSurface urhoSurface;

  public UrhoPage()
  {
    urhoSurface = new UrhoSurface();
    urhoSurface.VerticalOptions = LayoutOptions.FillAndExpand;
...
    Content = new StackLayout {
    Padding = new Thickness (12, 12, 12, 40),
    VerticalOptions = LayoutOptions.FillAndExpand,
    Children = {
      urhoSurface,  // added
      new Label { Text = "ROTATION:" },
      rotationSlider,
      new Label { Text = "SELECTED VALUE:" },
      selectedBarSlider,
    }
  };
```

### <a name="3-build-an-urho-application"></a>3. 生成 Urho 应用程序

`Charts`有关本示例中使用的 Urho 3d 图形的实现，请参阅类。 下面显示了基本代码大纲-请注意，类实现的类与 `Urho.Application` `Xamarin.Forms.Application` 在**App.cs**中实现的类不同。

```csharp
using Urho;
using Urho.Actions;
using Urho.Gui;
using Urho.Shapes;

namespace FormsSample
{
    public class Charts : Urho.Application
    {
    public Charts (ApplicationOptions options = null) : base(options) { }
    protected override void Start ()
    {
      ...
    }
    protected override void OnUpdate(float timeStep)
    {
      ...
    }
```

[UrhoSharp 文档](~/graphics-games/urhosharp/index.md)包含有关如何生成3d 场景和操作的详细信息。

### <a name="4-add-the-charts-class-to-the-urhosurface"></a>4. 将图表类添加到 UrhoSurface

使用 `UrhoSurface.Show<T>` 泛型方法将 Urho 应用程序添加到 Xamarin.Forms 页面。 下面的代码片段显示了创建类所需的附加代码 `Charts` ：

```csharp
public class UrhoPage : ContentPage
{
  Charts urhoApp;
  ...
  protected override async void OnAppearing ()
  {
    urhoApp = await urhoSurface.Show<Charts> (new ApplicationOptions(assetsFolder: null)
      { Orientation = ApplicationOptions.OrientationType.Portrait });
  }
```

注意：此 `Show<T>` 方法是异步的，应用关键字调用 `await` 。

### <a name="5-interacting-with-urhosharp"></a>5. 与 UrhoSharp 交互

该示例允许选择和修改图表栏。 `Charts`类公开 `Bars` 和 `SelectedBar` 以启用此交互。

`Charts`通过遍历公开的集合，每个 "bar" 都在类呈现后添加了选择事件处理程序 `Bars` ：

```csharp
protected override async void OnAppearing ()
{
  urhoApp = await urhoSurface.Show<Charts>(new ApplicationOptions(assetsFolder: null) { Orientation = ApplicationOptions.OrientationType.Portrait });
  foreach (var bar in urhoApp.Bars)
  {
    bar.Selected += OnBarSelection;
  }
}
```

事件处理程序使用控件的值 Xamarin.Forms `Slider` 来调整给定条形的高度：

```csharp
private void OnBarSelection(Bar bar)
{
  //reset value
  selectedBarSlider.ValueChanged -= OnValuesSliderValueChanged;
  selectedBarSlider.Value = bar.Value;
  selectedBarSlider.ValueChanged += OnValuesSliderValueChanged;
}

void OnValuesSliderValueChanged(object sender, ValueChangedEventArgs e)
{  // C# 6
  if (urhoApp?.SelectedBar != null)
  {
    urhoApp.SelectedBar.Value = (float)e.NewValue;
  }
}
```

最后，将两个控件连接在一起， `Slider` 以便在其值改变时，UrhoSharp 画布会受到影响。 第一个滑块旋转三维图表图像，第二个滑块调整所选条形的高度。

```csharp
rotationSlider = new Slider(0, 500, 250);
rotationSlider.ValueChanged += (s, e) => urhoApp?.Rotate((float)(e.NewValue - e.OldValue));

selectedBarSlider = new Slider(0, 5, 2.5);
selectedBarSlider.ValueChanged += OnValuesSliderValueChanged;
```

[页面顶部](#what-is-urhosharp)的动画显示运行的示例。

## <a name="summary"></a>摘要

此页说明如何使用 UrhoSharp 将三维数据可视化添加到 Xamarin.Forms 。 请参阅[UrhoSharp 文档](~/graphics-games/urhosharp/index.md)，了解有关如何 Xamarin.Forms 使用上面所示的方法生成可包括在应用中的 Urho 场景的详细信息。

## <a name="related-links"></a>相关链接

- [图表示例（c # 6）](https://github.com/xamarin/urho-samples/tree/master/FormsSample)
- [UrhoSharp](~/graphics-games/urhosharp/index.md)
