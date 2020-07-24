---
title: 使用 CocosSharpXamarin.Forms
description: CocosSharp 可用于将精确的形状、图像和文本呈现添加到应用程序以实现高级可视化
ms.prod: xamarin
ms.assetid: E0F404D5-5C6B-4288-92EC-78996C674E4E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/03/2016
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 31b82586b47ead1a851000d59c8271deec063020
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/22/2020
ms.locfileid: "86933765"
---
# <a name="using-cocossharp-in-xamarinforms"></a>使用 CocosSharpXamarin.Forms

[![下载示例](~/media/shared/download.png) 下载示例](https://github.com/xamarin/xamarin-forms-samples/tree/master/CocosSharpForms)

_CocosSharp 可用于将精确的形状、图像和文本呈现添加到应用程序以实现高级可视化_

> [!VIDEO https://youtube.com/embed/eYCx63FeqVU]

**演化2016：科科斯 # inXamarin.Forms**

## <a name="overview"></a>概述

CocosSharp 是一种灵活、功能强大的技术，用于显示图形、读取触控输入、播放音频和管理内容。 本指南介绍如何将 CocosSharp 添加到 Xamarin.Forms 应用程序。

## <a name="what-is-cocossharp"></a>什么是 CocosSharp？

[CocosSharp](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/index.md)是一个开源游戏引擎，可在 Xamarin 平台上使用。
CocosSharp 是一个运行时高效的库，其中包含以下功能：

- 使用类的图像呈现 `CCSprite`
- 使用类的形状呈现 `CCDrawNode`
- 使用类的每帧逻辑 `CCNode.Schedule`
- 内容管理（使用来加载和卸载资源，如 .png 文件）`CCTextureCache`
- 使用类的动画 `CCAction`

CocosSharp 的主要重点是简化跨平台2D 游戏的创建;不过，它也可以是 Xamarin 窗体应用程序的极佳补充。 由于游戏通常需要有效呈现并精确控制视觉对象，CocosSharp 可用于向非游戏应用程序添加功能强大的可视化效果和效果。

Xamarin.Forms基于特定于平台的本机 UI 系统构建。 例如，在 iOS 和 Android 上以不同的[ `Button` 方式显示，](xref:Xamarin.Forms.Button)甚至不同于操作系统版本。 与此相反，CocosSharp 不使用任何特定于平台的视觉对象，因此所有平台上的所有视觉对象都是相同的。 当然，分辨率和纵横比在设备之间有所不同，这可能会影响 CocosSharp 呈现其视觉对象的方式。 这些详细信息将在本指南的后面部分进行讨论。

可在[CocosSharp 部分](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/index.md)找到更多详细信息。

## <a name="adding-the-cocossharp-nuget-packages"></a>添加 CocosSharp NuGet 包

使用 CocosSharp 之前，开发人员需要在其项目中添加一些内容 Xamarin.Forms 。
本指南假定 Xamarin.Forms 项目包含 iOS、Android 和 .NET Standard 库项目。
所有代码都将写入 .NET Standard 库项目中;但是，必须将库添加到 iOS 和 Android 项目。

CocosSharp NuGet 包包含创建 CocosSharp 对象所需的所有对象。
CocosSharp NuGet 包包含 `CocosSharpView` 用于在中承载 CocosSharp 的类 Xamarin.Forms 。
添加**CocosSharp** NuGet 和**CocosSharp**也会自动添加。
为此，请右键单击 .NET Standard 库项目中的 "**包**" 文件夹，然后选择 "**添加包 ...**"。输入搜索词 " **CocosSharp**"，选择 " ** Xamarin.Forms CocosSharp **"，然后单击 "**添加包**"。

!["添加包" 对话框](cocossharp-images/image1.png)

**CocosSharp**和**CocosSharp** NuGet 包都将添加到项目中：

![包文件夹](cocossharp-images/image2.png)

针对特定于平台的项目（如 iOS 和 Android）重复上述步骤。

## <a name="walkthrough-adding-cocossharp-to-a-xamarinforms-app"></a>演练：向 Xamarin.Forms 应用程序添加 CocosSharp

按照以下步骤将简单的 CocosSharp 视图添加到 Xamarin.Forms 应用：

1. [创建 Xamarin 窗体页](#1-creating-a-xamarin-forms-page)
1. [添加 CocosSharpView](#2-adding-a-cocossharpview)
1. [创建 GameScene](#3-creating-the-gamescene)
1. [添加圆形](#4-adding-a-circle)
1. [与 CocosSharp 交互](#5-interacting-with-cocossharp)

将 CocosSharp 视图成功添加到应用后，请 Xamarin.Forms 访问[CocosSharp 文档](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/index.md)以了解有关使用 CocosSharp 创建内容的详细信息。

### <a name="1-creating-a-xamarin-forms-page"></a>1. 创建 Xamarin 窗体页

可以在任何容器中托管 CocosSharp Xamarin.Forms 。 此页面的示例使用名为的页面 `HomePage` 。 `HomePage`以半角为分割， `Grid` 以显示 Xamarin.Forms 在同一页上同时呈现和 CocosSharp 的方式。

首先，设置页面，使其包含 `Grid` 和两个 `Button` 实例：

```csharp
public class HomePage : ContentPage
{
public HomePage ()
    {
        // This is the top-level grid, which will split our page in half
        var grid = new Grid ();
        this.Content = grid;
        grid.RowDefinitions = new RowDefinitionCollection {
            // Each half will be the same size:
            new RowDefinition{ Height = new GridLength(1, GridUnitType.Star)},
            new RowDefinition{ Height = new GridLength(1, GridUnitType.Star)},
        };
        CreateTopHalf (grid);
        CreateBottomHalf (grid);
    }
    void CreateTopHalf(Grid grid)
    {
        // We'll be adding our CocosSharpView here:
    }
    void CreateBottomHalf(Grid grid)
    {
        // We'll use a StackLayout to organize our buttons
        var stackLayout = new StackLayout();
        // The first button will move the circle to the left when it is clicked:
        var moveLeftButton = new Button {
            Text = "Move Circle Left"
        };
        stackLayout.Children.Add (moveLeftButton);

        // The second button will move the circle to the right when clicked:
        var moveCircleRight = new Button {
            Text = "Move Circle Right"
        };
        stackLayout.Children.Add (moveCircleRight);
        // The stack layout will be in the bottom half (row 1):

        grid.Children.Add (stackLayout, 0, 1);
    }
}
```

在 iOS 上，如下 `HomePage` 图所示：

![主页屏幕快照](cocossharp-images/image3.png)

### <a name="2-adding-a-cocossharpview"></a>2. 添加 CocosSharpView

`CocosSharpView`类用于将 CocosSharp 嵌入到 Xamarin.Forms 应用中。 自 `CocosSharpView` 继承[ Xamarin.Forms 。视图](xref:Xamarin.Forms.View)类，它为布局提供熟悉的界面，可在布局容器（如）中使用[ Xamarin.Forms 。网格](xref:Xamarin.Forms.Grid)。 通过完成方法，向项目中添加一个新的 `CocosSharpView` `CreateTopHalf` ：

```csharp
void CreateTopHalf(Grid grid)
{
    // This hosts our game view.
    var gameView = new CocosSharpView () {
        // Notice it has the same properties as other XamarinForms Views
        HorizontalOptions = LayoutOptions.FillAndExpand,
        VerticalOptions = LayoutOptions.FillAndExpand,
        // This gets called after CocosSharp starts up:
        ViewCreated = HandleViewCreated
    };
    // We'll add it to the top half (row 0)
    grid.Children.Add (gameView, 0, 0);
}
```

CocosSharp 初始化不是即时的，因此，在完成其创建后，为注册事件 `CocosSharpView` 。 在方法中执行此 `HandleViewCreated` 操作：

```csharp
void HandleViewCreated (object sender, EventArgs e)
{
    var gameView = sender as CCGameView;
    if (gameView != null)
    {
        // This sets the game "world" resolution to 100x100:
        gameView.DesignResolution = new CCSizeI (100, 100);
        // GameScene is the root of the CocosSharp rendering hierarchy:
        gameScene = new GameScene (gameView);
        // Starts CocosSharp:
        gameView.RunWithScene (gameScene);
    }
}
```

此 `HandleViewCreated` 方法具有两个要查看的重要详细信息。 第一个是 `GameScene` 类，将在下一节中创建该类。 请注意，在 `GameScene` 创建并解析实例引用之前，应用不会进行编译 `gameScene` 。

第二个重要的详细信息是 `DesignResolution` 属性，该属性定义游戏的 CocosSharp 对象的可见区域。 `DesignResolution`创建后，将查看该属性 `GameScene` 。

### <a name="3-creating-the-gamescene"></a>3. 创建 GameScene

`GameScene`该类继承自 CocosSharp 的 `CCScene` 。 `GameScene`是我们仅处理 CocosSharp 的第一个点。 中包含的代码 `GameScene` 在任何 CocosSharp 应用程序中都可以使用，无论它是否在 Xamarin.Forms 项目中。

`CCScene`类是所有 CocosSharp 呈现的可视化根。 任何可见的 CocosSharp 对象都必须包含在中 `CCScene` 。 更具体地说，必须将视觉对象添加到 `CCLayer` 实例，这些 `CCLayer` 实例必须添加到中 `CCScene` 。

下图可帮助可视化典型的 CocosSharp 层次结构：

![典型的 CocosSharp 层次结构](cocossharp-images/image4.png)

一次只能有一个 `CCScene` 处于活动状态。 大多数游戏使用多个 `CCLayer` 实例对内容进行排序，但我们的应用程序只使用一个。 同样，大多数游戏使用多个视觉对象，但我们的应用程序中仅有一个。 有关 CocosSharp 可视层次结构的更详细的讨论，请参阅[BouncingGame 演练](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/bouncing-game.md)。

最初，此 `GameScene` 类将为空，只需创建它来满足中的引用 `HomePage` 。 将新类添加到名为的 .NET Standard 库项目 `GameScene` 。 它应从类继承，如下所示 `CCScene` ：

```csharp
public class GameScene : CCScene
{
    public GameScene (CCGameView gameView) : base(gameView)
    {

    }
}
```

现在， `GameScene` 我们可以返回 `HomePage` 并添加一个字段：

```csharp
// Keep the GameScene at class scope
// so the button click events can access it:
GameScene gameScene;
```

现在，我们可以编译项目并运行它，以查看 CocosSharp 的运行情况。 我们未向我们添加任何内容， `GameScene,` 因此我们的页面的上半部分为黑色– CocosSharp 场景的默认颜色：

![空白 GameScene](cocossharp-images/image5.png)

### <a name="4-adding-a-circle"></a>4. 添加圆圈

应用当前具有 CocosSharp 引擎的运行实例，并显示一个空的 `CCScene` 。 接下来，我们将添加一个视觉对象：一个圆形。 `CCDrawNode`类可用于绘制各种几何形状，如[使用 CCDrawNode 的绘图几何图形](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/ccdrawnode.md)中所述。

向类添加一个圆圈 `GameScene` ，并在构造函数中对其进行实例化，如以下代码所示：

```csharp
public class GameScene : CCScene
{
    CCDrawNode circle;
    public GameScene (CCGameView gameView) : base(gameView)
    {
        var layer = new CCLayer ();
        this.AddLayer (layer);
        circle = new CCDrawNode ();
        layer.AddChild (circle);
        circle.DrawCircle (
            // The center to use when drawing the circle,
            // relative to the CCDrawNode:
            new CCPoint (0, 0),
            radius:15,
            color:CCColor4B.White);
        circle.PositionX = 20;
        circle.PositionY = 50;
    }
}
```

运行应用程序时，会在 CocosSharp 显示区域的左侧显示一个圆圈：

![GameScene 中的圆圈](cocossharp-images/image6.png)

#### <a name="understanding-designresolution"></a>了解 DesignResolution

现在会显示一个视觉对象 CocosSharp 对象，我们可以调查 `DesignResolution` 属性。

`DesignResolution`表示用于放置和调整对象大小的 CocosSharp 区域的宽度和高度。 区域的实际分辨率以*像素*为单位进行测量，而以 `DesignResolution` 世界*单位*来度量。 下图显示了在具有640x1136 像素屏幕分辨率的 iPhone 5 上显示的视图的各个部分的分辨率：

![iPhone 5s 设计解决方案](cocossharp-images/image7.png)

上图以黑色文本显示屏幕外的像素尺寸。 单元在关系图的内部以白色文本显示。 下面是上面显示的一些重要详细信息：

- CocosSharp 显示的源位于左下角。 向右移动会增大 X 值，向上移动会增加 Y 值。 请注意，Y 值相对于其他一些二维布局引擎，其中（0，0）是画布的左上角。
- CocosSharp 的默认行为是保持其视图的纵横比。 由于网格中的第一行的宽度大于高度，因此 CocosSharp 不会填充单元格的整个宽度，如点虚线白色矩形所示。 此行为可以更改，如在[CocosSharp 中处理多个解决方案中](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/resolutions.md)所述。
- 在此示例中，CocosSharp 将维护100单位宽度和高度的显示区域，而不考虑其设备的大小或纵横比。 这意味着，代码可以假定 X = 100 表示 CocosSharp 显示的最右侧边界，并在所有设备上保持布局一致。

#### <a name="ccdrawnode-details"></a>CCDrawNode 详细信息

我们的简单应用程序使用 `CCDrawNode` 类绘制一个圆圈。 此类对于业务应用非常有用，因为它提供了基于矢量的几何呈现–中缺少的功能 Xamarin.Forms 。 除了圆外，类还 `CCDrawNode` 可用于绘制矩形、样条、直线和自定义多边形。 `CCDrawNode`很容易使用，因为它不需要使用图像文件（如 .png）。 有关 CCDrawNode 的更详细讨论，请参阅[使用 CCDrawNode 指南绘制几何](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/ccdrawnode.md)。

### <a name="5-interacting-with-cocossharp"></a>5. 与 CocosSharp 交互

CocosSharp 视觉元素（如 `CCDrawNode` ）从 `CCNode` 类继承。 `CCNode`提供了两个属性，这些属性可用于相对于对象的父级定位对象： `PositionX` 和 `PositionY` 。 代码目前使用这两个属性来定位圆的中心，如以下代码片段所示：

```csharp
circle.PositionX = 20;
circle.PositionY = 50;
```

需要注意的是，CocosSharp 对象是按照显式位置值（而不是大多数视图）定位的，它们会 Xamarin.Forms 根据其父布局控件的行为自动定位。

我们将添加代码，以允许用户单击两个按钮之一将圆圈向左或向右移动10个单位（而不是像素），因为圆圈在 CocosSharp 的世界单位空间内绘制。 首先，我们将在类中创建两个公共方法 `GameScene` ：

```csharp
public void MoveCircleLeft()
{
    circle.PositionX -= 10;
}

public void MoveCircleRight()
{
    circle.PositionX += 10;
}
```

接下来，我们将向中的两个按钮添加处理程序， `HomePage` 以响应单击。 完成后，我们 `CreateBottomHalf` 的方法包含以下代码：

```csharp
void CreateBottomHalf(Grid grid)
{
    // We'll use a StackLayout to organize our buttons
    var stackLayout = new StackLayout();

    // The first button will move the circle to the left when it is clicked:
    var moveLeftButton = new Button {
        Text = "Move Circle Left"
    };
    moveLeftButton.Clicked += (sender, e) => gameScene.MoveCircleLeft ();
    stackLayout.Children.Add (moveLeftButton);

    // The second button will move the circle to the right when clicked:
    var moveCircleRight = new Button {
        Text = "Move Circle Right"
    };
    moveCircleRight.Clicked += (sender, e) => gameScene.MoveCircleRight ();
    stackLayout.Children.Add (moveCircleRight);

    // The stack layout will be in the bottom half (row 1):
    grid.Children.Add (stackLayout, 0, 1);
}
```

现在，CocosSharp 圆会移动以响应单击。 我们还可以通过将圆周向左或向右移动来清楚地查看 CocosSharp 画布的边界：

![带有移动圆圈的 GameScene](cocossharp-images/image8.png)

## <a name="summary"></a>摘要

本指南介绍如何将 CocosSharp 添加到现有 Xamarin.Forms 项目，如何创建 Xamarin.Forms 和 CocosSharp 之间的交互，并讨论在 CocosSharp 中创建布局时的各种注意事项。

CocosSharp 游戏引擎提供了许多功能和深度，因此本指南只显示了 CocosSharp 可以执行的操作的表面。 对详细了解 CocosSharp 感兴趣的开发人员可以在[CocosSharp 存档](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/)中找到很多文章。

## <a name="related-links"></a>相关链接

- [CocosSharpForms （示例）](https://github.com/xamarin/xamarin-forms-samples/tree/master/CocosSharpForms)
