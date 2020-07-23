---
title: MonoGame 游戏板参考
description: 本文档介绍了一种跨平台类，用于访问 MonoGame 中的输入设备。 本文介绍如何从游戏板读取输入，并提供示例代码。
ms.prod: xamarin
ms.assetid: 1F71F3E8-2397-4C6A-8163-6731ECFB7E03
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: 11b1cfda435e97b09f9d1eded22f11f912d1783d
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/22/2020
ms.locfileid: "86934025"
---
# <a name="monogame-gamepad-reference"></a>MonoGame 游戏板参考

_游戏板是一种标准的跨平台类，用于访问 MonoGame 中的输入设备。_

`GamePad`可用于从多个 MonoGame 平台上的输入设备读取输入。 本指南演示如何使用游戏板类。 由于每个输入设备的布局和其提供的按钮数量不同，因此本指南包括显示各种设备映射的关系图。

## <a name="gamepad-as-a-replacement-for-xbox360gamepad"></a>用于 Xbox360GamePad 的替代游戏程序

原始的输出的 API 提供了 `Xbox360GamePad` 用于从 Xbox 360 或 PC 上的游戏控制器读取输入的类。 MonoGame 已将此替换为 `GamePad` 类，因为 Xbox 360 控制器不能用于大多数 MonoGame 平台（如 iOS 或 Xbox）。 尽管名称发生更改，但类的用法 `GamePad` 与 `Xbox360GamePad` 类相似。

## <a name="reading-input-from-gamepad"></a>读取游戏板的输入

`GamePad`类提供一种在任何 MonoGame 平台上读取输入的标准化方法。 它通过以下两种方法提供信息：

- `GetState`–返回控制器的按钮、模拟杆和方向键的当前状态。
- `GetCapabilities`-返回有关硬件功能的信息，例如控制器是否具有某些按钮或是否支持振动。

### <a name="example-moving-a-character"></a>示例：移动字符

下面的代码演示如何通过设置字符的和属性来移动字符 `XVelocity` `YVelocity` 。 此代码假定 `characterInstance` 为具有和属性的对象的实例 `XVelocity` `YVelocity` ：

```csharp
// In Update, or some code called every frame:
var gamePadState = GamePad.GetState(PlayerIndex.One);
// Use gamePadState to move the character
characterInstance.XVelocity = gamePadState.ThumbSticks.Left.X * characterInstance.MaxSpeed;
characterInstance.YVelocity = gamePadState.ThumbSticks.Left.Y * characterInstance.MaxSpeed;
```

### <a name="example-detecting-pushes"></a>示例：检测推送

`GamePadState`提供有关控制器的当前状态的信息，例如是否按下某个按钮。 某些操作（例如进行字符跳转）需要检查按钮是否已被推送（不是在最后一帧上，而是在此帧上）或已释放（最后一帧未关闭，而不是按下此帧）。

若要执行这种类型的逻辑，必须创建存储前一帧 `GamePadState` 和当前帧的局部变量 `GamePadState` 。 下面的示例演示如何存储和使用上一个帧 `GamePadState` 来实现跳跃：

```csharp
// At class scope:
// Store the last frame's and this frame's GamePadStates.
// "new" them so that code doesn't have to perform null checks:
GamePadState lastFrameGamePadState = new GamePadState();
GamePadState currentGamePadState = new GamePadState();
protected override void Update(GameTime gameTime)
{
    // store off the last state before reading the new one:
    lastFrameGamePadState = currentGamePadState;
    currentGamePadState = GamePad.GetState(PlayerIndex.One);
    bool wasAButtonPushed =
currentGamePadState.Buttons.A == ButtonState.Pressed
        && lastFrameGamePadState.Buttons.A == ButtonState.Released;
    if(wasAButtonPushed)
    {
        MakeCharacterJump();
    }
...
}
```

### <a name="example-checking-for-buttons"></a>示例：检查按钮

`GetCapabilities`可用于检查控制器是否有某些硬件，如特定按钮或模拟杆。 下面的代码演示如何在需要同时存在两个按钮的游戏中检查控制器上的 B 和 Y 按钮：

```csharp
var capabilities = GamePad.GetCapabilities(PlayerIndex.One);
bool hasBButton = capabilities.HasBButton;
bool hasXButton = capabilities.HasXButton;
if(!hasBButton || !hasXButton)
{
    NotifyUserOfMissingButtons();
}
```

## <a name="ios"></a>iOS

iOS 应用支持无线游戏控制器输入。

> [!IMPORTANT]
> MonoGame 3.5 的 NuGet 包不包括对无线游戏控制器的支持。 使用 iOS 上的游戏板类需要构建 MonoGame 3.5 源或使用 MonoGame 3.6 NuGet 二进制文件。

### <a name="ios-game-controller"></a>iOS 游戏控制器

`GamePad`类返回从无线控制器读取的属性。 中的属性可 `GamePad` 为标准 iOS 控制器硬件提供良好的覆盖面，如下图所示：

![游戏板中的属性可为标准 iOS 控制器硬件提供良好的覆盖范围，如图所示](input-images/image1.png)

## <a name="apple-tv"></a>Apple TV

Apple TV 游戏可以使用 Siri 遥控器或无线游戏控制器进行输入。

### <a name="siri-remote"></a>Siri 远程

*Siri Remote*是 Apple TV 的本机输入设备。 尽管 Siri 远程的值可以通过事件读取（如[Siri 远程和蓝牙控制器指南](~/ios/tvos/platform/remote-bluetooth.md)中所示），但 `GamePad` 该类可以从 Siri 远程返回值。

请注意， `GamePad` 只能从 "播放" 按钮和触摸表面读取输入：

![请注意，游戏板只能从 "播放" 按钮和触摸表面读取输入](input-images/image2.png)

由于触摸面移动是通过属性读取的 `DPad` ，因此使用类报告移动值 `ButtonState` 。 换句话说，值仅可作为 `ButtonState.Pressed` 或 `ButtonState.Released` ，而不是数值或笔势。

### <a name="apple-tv-game-controller"></a>Apple TV 游戏控制器

Apple TV 游戏控制器的行为与适用于 iOS 应用的游戏控制器相同。 有关详细信息，请参阅[IOS 游戏控制器](#ios-game-controller)部分。 

## <a name="xbox-one"></a>Xbox One

Xbox one 控制台支持从 Xbox one 游戏控制器读取输入。

### <a name="xbox-one-game-controller"></a>Xbox One 游戏控制器

Xbox one 游戏控制器是适用于 Xbox one 的最常见输入设备。 `GamePad`类提供游戏控制器硬件的输入值。

![游戏板类提供游戏控制器硬件的输入值](input-images/image3.png)

## <a name="summary"></a>总结

本指南提供 MonoGame 的 `GamePad` 类概述、如何实现输入-读取逻辑，以及常用实现关系图 `GamePad` 。

## <a name="related-links"></a>相关链接

- [MonoGame 游戏板](http://www.monogame.net/documentation/?page=T_Microsoft_Xna_Framework_Input_GamePad)
