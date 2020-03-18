---
title: Lollipop 功能
description: 本文概述了 Android 5.0 (Lollipop) 中引入的新功能。 这些功能包括名为 Material Theme 的新用户界面样式，以及新的支持功能（如动画、视图阴影和可绘制着色）。 Android 5.0 还包括改进的通知、两个新的 UI 小组件、一个新的作业计划程序，以及一些对改进存储、网络、连接和多媒体功能有帮助的新 API。
ms.prod: xamarin
ms.assetid: 1CE99CFE-FAAC-49FC-AEDC-1A21FC6E946E
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: 297c7806ce8a880d65c38ef0e4672e41fee5acfe
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/10/2020
ms.locfileid: "76724440"
---
# <a name="lollipop-features"></a>Lollipop 功能

本文概述了 Android 5.0 (Lollipop) 中引入的新功能。这些功能包括名为 Material Theme 的新用户界面样式，以及新的支持功能（如动画、视图阴影和可绘制着色）。Android 5.0 还包括改进的通知、两个新的 UI 小组件、一个新的作业计划程序，以及一些对改进存储、网络、连接和多媒体功能有帮助的新 API。 

## <a name="lollipop-overview"></a>Lollipop 概述

Android 5.0 (Lollipop) 引入了一种新的设计语言 Material Design，并支持一系列新功能，使应用使用起来更轻松、更直观  。 借助 Material Design，Android 5.0 不仅使 Android 手机改头换面，还为基于 Android 的平板电脑、台式计算机、手表和智能电视提供了一组新的设计规则。 这些设计规则强调简易型和极简主义，同时使用熟悉的触觉属性（如逼真的表面和边缘提示）来帮助用户快速、直观地理解界面。

Material Theme 是这些 UI 设计原则在 Android 中的体现  。 本文首先介绍 Material Theme 的支持功能：

- **动画** &ndash;“触摸反馈”动画、“活动转换”动画、“视图状态转换”动画和“显示效果”动画     。

- **视图阴影和高度** &ndash; 视图现在具有 `elevation` 属性；`elevation` 值较大的视图在背景上投射较大的阴影。

- **配色功能** &ndash;“可绘制着色”使你可以通过更改图像资产的颜色来重复使用图像资产，而且“突出颜色提取”可帮助你基于图像中的颜色动态地为应用设置主题   。

Android 5.0 UI 体验中已经内置了许多 Material Theme 功能，而其他功能必须显式地添加到应用中。 例如，标准视图（如按钮）已经包含触摸反馈动画，而应用必须启用大多数视图阴影。

除了通过 Material Theme 带来的 UI 改进之外，Android 5.0 还包括本文中介绍的其他几个新功能：

- **改进的通知** &ndash; Android 5.0 中的通知进行了重大更新，包括新的外观、对锁屏通知的支持以及新的“浮动”通知显示格式  。

- **新的 UI 小组件** &ndash; 新的 `RecyclerView` 小组件使应用可以更轻松地传输大数据集和复杂的信息，新的 `CardView` 小组件为显示文本和图像提供了一种与卡类似的简化显示格式。

- **新的 API** &ndash; Android 5.0 增加了新的 API 以支持多个网络，改进蓝牙连接，简化存储管理，以及对多媒体播放器和照相机设备进行更灵活的控制。 新的作业计划功能可用于在计划的时间异步运行任务。 此功能有助于延长电池续航时间，例如，将任务安排在设备接通电源和充电时运行。

## <a name="requirements"></a>要求

要在基于 Xamarin 的应用中使用新的 Android 5.0 功能，需满足以下要求：

- **Xamarin.Android** &ndash; 必须安装 Xamarin.Android 4.20 或更高版本并配置 Visual Studio 或 Visual Studio for Mac。

- **Android SDK** &ndash; 必须通过 Android SDK 管理器安装 Android 5.0 (API 21) 或更高版本。

- **Java 开发人员工具包** &ndash; 如果针对 API 24 或更高级别进行开发，则 Xamarin Android 需要 [JDK 1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) 或更高版本（JDK 1.8 还支持低于 24 的 API 级别，包括 Lollipop）。 如果使用的是自定义控件或窗体预览器，则需要使用 64 位版 JDK 1.8。

如果专门针对 API 23 或更低级别进行开发，可以继续使用 [JDK 1.7](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html)。

## <a name="setting-up-an-android-50-project"></a>建立 Android 5.0 项目

要创建 Android 5.0 项目，必须安装最新的工具和 SDK 包。 请按照以下步骤建立面向 Android 5.0 的 Xamarin.Android 项目：

1. 安装 Xamarin.Android 工具并激活 Xamarin 许可证。 有关安装 Xamarin.Android 的详细信息，请参阅[设置和安装](~/android/get-started/installation/index.md)。

2. 如果使用 Visual Studio for Mac，请安装最新的 Android 5.0 更新。

3. 启动 Android SDK 管理器（在 Visual Studio for Mac 中，则使用“工具”&gt;“打开 Android SDK 管理器”&hellip;），并安装 Android SDK Tools 23.0.5 或更高版本  ：

    [![在 Android SDK 管理器中选择 Android SDK Tools](lollipop-images/android-l-tools-sml.png)](lollipop-images/android-l-tools.png#lightbox)

   同时，请安装最新的 Android 5.0 SDK 包（API 21 或更高版本）：

    [![在 Android SDK 管理器中安装 Android 5.0 SDK 包](lollipop-images/android-l-sdk-pkgs-sml.png)](lollipop-images/android-l-sdk-pkgs.png#lightbox)

   有关使用 Android SDK 管理器的详细信息，请参阅 [SDK 管理器](https://developer.android.com/tools/help/sdk-manager.html)。

4. 创建新的 Xamarin.Android 项目。 如果不熟悉使用 Xamarin 进行 Android 开发，请参阅 [Hello, Android](~/android/get-started/hello-android/index.md) 了解如何创建 Android 项目。 创建 Android 项目时，请确保为 Android 5.0 配置版本设置。
   在 Visual Studio for Mac 中，导航到“项目选项”&gt;“生成”&gt;“常规”，并将目标框架设置为 Android 5.0 (Lollipop) 或更高版本    ：

    ![将目标框架设置为 Android 5.0 Lollipop](lollipop-images/target-framework.png)

   在“项目选项”&gt;“生成”&gt;“Android 应用程序”下，将最低和目标 Android 版本设置为“自动 - 使用目标框架版本”   ：

    ![将最低和目标 Android 版本设置为自动](lollipop-images/minimum-android-version.png)

5. 配置模拟器或 Android 设备以测试应用。 如果使用的是模拟器，请参阅 [Android Emulator 设置](~/android/get-started/installation/android-emulator/index.md)，了解如何配置 Android Emulator 以与 Xamarin Studio 或 Visual Studio 配合使用。 如果使用的是 Android 设备，请参阅[设置预览 SDK](https://developer.android.com/preview/setup-sdk.html)，了解如何将设备更新为 Android 5.0。 要配置 Android 设备以运行和调试 Xamarin.Android 应用程序，请参阅[设置设备以进行开发](~/android/get-started/installation/set-up-device-for-development.md)。

注意：如果要更新面向 Android L 预览版的现有 Android 项目，则必须将“目标框架”和“Android 版本”更新为上述值   。

## <a name="important-changes"></a>重要更改

之前发布的 Android 应用可能会受到 Android 5.0 中的更改所影响。 具体而言，Android 5.0 将使用新的运行时和明显更改的通知格式。

### <a name="android-runtime"></a>Android 运行时

Android 5.0 使用新的 Android 运行时 (ART) 作为默认运行时，而不是 Dalvik。 ART 实现了几个主要的新功能：

- **预先 (AOT) 编译** &ndash; AOT 可以通过在应用首次启动之前编译应用代码来提高应用性能。 安装应用后，ART 会为目标设备生成已编译应用的可执行文件。

- **改进的垃圾回收 (GC)** &ndash; ART 中的 GC 改进也可以提高应用性能。 垃圾回收现在使用一个而非两个 GC 暂停，可以更及时地完成并发 GC 操作。

- **改进的应用调试** &ndash; ART 提供了更多的诊断细节来帮助分析异常和故障报告。

现有应用无需更改也可在 ART 下工作，但如果应用使用的是以前的 Dalvik 运行时独有的技术，则可能无法在 ART 下工作。 有关这些更改的详细信息，请参阅[在 Android 运行时 (ART) 中验证应用的行为](https://developer.android.com/guide/practices/verifying-apps-art.html)。

### <a name="notification-changes"></a>通知更改

在 Android 5.0 中，通知发生了重大更改：

- **声音和振动的处理方式不同** &ndash; 通知的声音和振动现在由 `Notification.Builder` 处理，而不是 `Ringtone`、`MediaPlayer` 和 `Vibrator`。

- **新的配色方案** &ndash; 根据 Material Theme，通知在白色或非常浅的背景上用深色文本呈现。 此外，可以通过 Android 修改通知图标中的 alpha 通道，以与系统配色方案协调。

- **锁屏通知** &ndash; 通知现在可以显示在设备锁屏上。

- **浮动** &ndash; 当设备解锁且屏幕打开时，高优先级通知现在会显示在一个小的浮动窗口（浮动通知）中。

在大多数情况下，将现有的应用通知功能移植到 Android 5.0 需执行以下步骤：

1. 将代码转换为使用 `Notification.Builder`（或 `NotificationsCompat.Builder`）来创建通知。

2. 验证现有通知资产在新的 Material Theme 配色方案中是否可见。

3. 决定在锁屏上显示时通知应具有的可见性。 如果通知是非公开的，锁屏上应显示什么内容？

4. 设置通知的类别，以便在新的 Android 5.0“请勿打扰”模式下正确处理它们  。

如果通知显示传输控件，显示媒体播放状态，使用 `RemoteControlClient` 或调用 `ActivityManager.GetRecentTasks`，请参阅[重要行为变更](https://developer.android.com/preview/api-overview.html#Behaviors)获取有关更新 Android 5.0 通知的详细信息。

有关在 Android 中创建通知的信息，请参阅[本地通知](~/android/app-fundamentals/notifications/local-notifications.md)。

## <a name="material-theme"></a>Material Theme

新的 Android 5.0 Material Theme 使 Android UI 的外观发生了巨大变化。 视觉对象元素现在使用触觉表面，这些表面具有醒目的图形、排版和色彩明艳的印刷型设计。 以下屏幕截图展示了 Material Theme 的示例：

[![Material Theme 主屏幕、“应用”屏幕“和设置”屏幕的屏幕截图](lollipop-images/android-5-gallery-labeled-sml.png)](lollipop-images/android-5-gallery-labeled.png#lightbox)

左侧的屏幕截图为主屏幕，Android 5.0 在其中向你发起问候。 中间的屏幕截图为应用列表的第一个屏幕，右侧的屏幕截图为“设置”屏幕  。 Google 的[Material Design](https://material.io/guidelines/material-design/introduction.html) 规范介绍了新 Material Theme 概念背后的基本设计规则。

你可以在应用中使用 Material Theme 内置的三种风格：`Theme.Material` 深色主题（默认）、`Theme.Material.Light` 主题和 `Theme.Material.Light.DarkActionBar` 主题：

[![深色、浅色和 DarkActionBar 主题的屏幕截图](lollipop-images/three-material-themes-sml.png)](lollipop-images/three-material-themes.png#lightbox)

有关在 Xamarin.Android 应用中使用 Material Theme 功能的详细信息，请参阅 [Material Theme](~/android/user-interface/material-theme.md)。

## <a name="animations"></a>动画

Android 5.0 提供触摸反馈动画、活动转换动画和视图状态转换动画，使应用界面使用起来更加直观。 此外，Android 5.0 应用还可使用“显示效果”动画来隐藏或显示视图  。 你可以使用“曲线运动”设置来配置动画的呈现速度（快或慢）  。

### <a name="touch-feedback-animations"></a>触摸反馈动画

触摸反馈动画在用户触摸视图时提供视觉反馈。 例如，触摸按钮现在会显示波纹效果，这是 Android 5.0 中默认的触摸反馈动画。 波纹动画由新的 `RippleDrawable` 类实现。 波纹效果可配置为在视图的边界处结束，也可配置为延伸到视图的边界之外。 例如，以下屏幕截图序列展示了触摸动画期间按钮中的波纹效果：

![按钮上波纹动画的逐帧屏幕截图](lollipop-images/touch-animation.png)

左侧第一张图像展示了最初触摸按钮时的效果，其余的序列（从左到右）展示了波纹效果如何扩散到按钮的边缘。 当波纹动画结束时，视图返回到其原始外观。 默认的波纹动画发生在一瞬间，但可将动画的长度自定义为更长或更短的时间。

有关 Android 5.0 中触摸反馈动画的详细信息，请参阅[自定义触摸反馈](https://developer.android.com/training/material/animations.html#Touch)。

### <a name="activity-transition-animations"></a>活动转换动画

活动转换动画在一个活动转换到另一个活动时为用户提供一种视觉连续的感觉。 应用可指定三种类型的转换动画：

- **进入转换** &ndash; 当活动进入场景时。

- **退出转换** &ndash; 当活动退出场景时。

- **共享元素转换** &ndash; 当两个活动共有的视图随第一个活动转换到下一个活动而更改时。

例如，以下屏幕截图序列展示了共享元素转换：

[![共享元素转换动画的逐帧屏幕截图](lollipop-images/activity-transition-sml.png)](lollipop-images/activity-transition.png#lightbox)

共享元素（毛毛虫的照片）是第一个活动中的若干视图之一；当第一个活动转换到第二个活动时，它会放大成为第二个活动中的唯一视图。

#### <a name="enter-transition-animation-types"></a>进入转换动画类型

对于进入转换，Android 5.0 提供三种类型的动画：

- **分解动画** &ndash; 从场景中心放大视图。

- **滑动动画** &ndash; 从场景的某个边缘移入视图。

- **淡化动画** &ndash; 将视图淡入场景。

#### <a name="exit-transition-animation-types"></a>退出转换动画类型

对于退出转换，Android 5.0 提供三种类型的动画：

- **分解动画** &ndash; 将视图缩小到场景中心。

- **滑动动画** &ndash; 将视图移到场景的某个边缘。

- **淡化动画** &ndash; 将视图淡出场景。

#### <a name="shared-element-transition-animation-types"></a>共享元素转换动画类型

共享元素转换支持多种类型的动画，例如：

- 更改视图的布局或剪辑边界。

- 更改视图的缩放和旋转。

- 更改视图的大小和缩放类型。

有关 Android 5.0 中活动转换动画的详细信息，请参阅[自定义活动转换](https://developer.android.com/training/material/animations.html#Transitions)。

### <a name="view-state-transition-animations"></a>视图状态转换动画

Android 5.0 允许在视图状态发生更改时运行动画。 可以使用以下技术之一对视图状态转换进行动画处理：

- 创建可绘制资源，以对与特定视图关联的状态更改进行动画处理。 新的 `AnimatedStateListDrawable` 类允许你创建在视图状态更改之间显示动画的可绘制资源。

- 定义视图状态更改时运行的动画功能。 新的 `StateListAnimator` 类允许你定义在视图状态更改时运行的动画。

有关 Android 5.0 中视图状态转换动画的详细信息，请参阅[对视图状态更改进行动画处理](https://developer.android.com/training/material/animations.html#ViewState)。

### <a name="reveal-effect"></a>显示效果

“显示效果”是一个剪辑圆，可更改半径以显示或隐藏视图  。 你可以通过设置剪辑圆的初始和最终半径来控制此效果。 以下屏幕截图序列展示了从屏幕中心发生的显示效果动画：

[![显示动画的逐帧屏幕截图](lollipop-images/reveal-center-sml.png)](lollipop-images/reveal-center.png#lightbox)

下一个序列展示了从屏幕左下角发生的显示效果动画：

[![剪辑动画的逐帧屏幕截图](lollipop-images/reveal-left-sml.png)](lollipop-images/reveal-left.png#lightbox)

显示动画可以反转，也就是说，剪辑圆可以缩小以隐藏视图，而不是放大以显示视图。

有关 Android 5.0 中显示效果的详细信息，请参阅[使用显示效果](https://developer.android.com/training/material/animations.html#Reveal)。

### <a name="curved-motion"></a>曲线运动

除了这些动画功能，Android 5.0 还提供了新的 API，使你能够指定动画的时间和运动曲线。 Android 5.0 使用这些曲线在动画期间内插时间和空间运动。 Android 5.0 中定义了三条曲线：

- **Fast\_out\_linear\_in** &ndash; 快速加速并继续加速，直到动画结束。

- **Fast\_out\_slow\_in** &ndash; 快速加速并缓慢加速，直到动画结束。

- **Linear\_out\_slow\_in** &ndash; 以峰值速度开始并缓慢减速，直到动画结束。

你可以使用新的 `PathInterpolator` 类来指定运动内插的发生方式。 `PathInterpolator` 是根据指定的控制点和运动曲线遍历动画路径的内插程序。 有关如何在 Android 5.0 中指定曲线运动设置的详细信息，请参阅[使用曲线运动](https://developer.android.com/training/material/animations.html#CurvedMotion)。

## <a name="view-shadows--elevation"></a>视图阴影和高度

在 Android 5.0 中，可以通过设置新的 `Z` 属性来指定视图的“高度”  。 较大的 `Z` 值会导致视图在背景上投射一个较大的阴影，使视图看起来漂浮在背景之上。 你可以通过在布局中配置视图的 `elevation` 属性来设置视图的初始高度。

下面的示例展示了当空 `TextView` 控件的高度属性分别设置为 2dp、4dp 和 6dp 时所投射的阴影：

[![逐渐变大的视图阴影的屏幕截图](lollipop-images/view-shadows-sml.png)](lollipop-images/view-shadows.png#lightbox)

视图阴影设置可以是静态的（如上图所示），也可以在动画中使用以使视图看起来暂时高于视图的背景。 可以使用 `ViewPropertyAnimator` 类对视图的高度进行动画处理。 视图的高度是其布局的 `elevation` 设置加上 `translationZ` 属性的总和，你可以通过 `ViewPropertyAnimator` 方法调用来设置该属性。

有关 Android 5.0 中视图阴影的详细信息，请参阅[定义阴影和剪辑视图](https://developer.android.com/training/material/shadows-clipping.html)。

## <a name="color-features"></a>配色功能

Android 5.0 提供了两个新功能来管理应用中的颜色：

- “可绘制着色”允许通过更改布局属性来更改图像资产的颜色  。

- “突出颜色提取”使你能够动态自定义应用的颜色主题，以与所显示图像的调色板协调  。

### <a name="drawable-tinting"></a>可绘制着色

Android 5.0 布局识别新的 `tint` 属性，你可以使用该属性来设置可绘制资源的颜色，而无需创建这些资产的多个版本来显示不同的颜色。 要使用此功能，可以将位图定义为 alpha 蒙板，并使用 `tint` 属性来定义资产的颜色。 这样一来，你就可以一次性创建资产，并在布局中对它们进行配色以与主题匹配。

在下面的示例中，使用了单个图像资产（带有透明背景的白色徽标）来创建色调变化：

![带有透明背景的白色 Xamarin 徽标](lollipop-images/xamarin-logo-white.png)

此徽标显示在蓝色圆形背景上方，如以下示例所示。 左侧的图像为没有 `tint` 设置时徽标的显示方式。 在中间的图像中，徽标的 `tint` 属性设置为深灰色。 在右侧的图像中，`tint` 设置为浅灰色：

![上面带有不同色调设置的徽标示例](lollipop-images/drawable-tinting.png)

有关 Android 5.0 中可绘制着色的详细信息，请参阅[可绘制着色](https://developer.android.com/training/material/drawables.html#DrawableTint)。

### <a name="prominent-color-extraction"></a>突出显示提取

新的 Android 5.0 `Palette` 类使你可从图像中提取颜色，以便可以动态地将它们应用于自定义调色板。 `Palette` 类从图像中提取六种颜色，并根据颜色饱和度和亮度的相对级别标记这些颜色：

- 明丽

- 明丽暗色

- 明丽亮色

- 柔和

- 柔和暗色

- 柔和亮色

例如，在以下屏幕截图中，照片查看应用从显示的图像中提取突出的颜色，并使用这些颜色调整应用的配色方案以与图像匹配：

[![绿色、粉色和蓝色主题颜色提取的屏幕截图](lollipop-images/prominent-color-extraction-sml.png)](lollipop-images/prominent-color-extraction.png#lightbox)

在上面的屏幕截图中，操作栏设置为提取的“明丽亮色”颜色，背景设置为提取的“明丽暗色”颜色。 在上面的每个示例中，都包含一行小的彩色正方形，以展示从图像中提取的调色板颜色。

有关 Android 5.0 中颜色提取的详细信息，请参阅[从图像中提取突出颜色](https://developer.android.com/training/material/drawables.html#ColorExtract)。

## <a name="new-ui-widgets"></a>新的 UI 小组件

Android 5.0 引入了两个新的 UI 小组件：

- `RecyclerView` &ndash; 显示可滚动项列表的视图组。

- `CardView` &ndash; 带有圆角的基本布局。

这两个小组件都包含对 Material Theme 功能的内置支持；例如，`RecyclerView` 使用动画来添加和删除视图，`CardView` 使用视图阴影来使每张卡显示为浮在背景上。 这些新小组件的示例如以下屏幕截图所示：

[![使用 RecyclerView 生成的应用的屏幕截图](lollipop-images/recyclerview-cardview-sml.png)](lollipop-images/recyclerview-cardview.png#lightbox)

左侧的屏幕截图是电子邮件应用中使用的 `RecyclerView` 示例，右侧的屏幕截图是旅游预订应用中使用的 `CardView` 示例。

### <a name="recyclerview"></a>RecyclerView

`RecyclerView` 类似于 `ListView,`，但它更适合元素动态更改的大型视图或列表集。 与 `ListView,` 一样，需指定一个适配器来访问基础数据集。 然而，与 `ListView,` 不同的是，需使用“布局管理器”来定位 `RecyclerView` 中的项  。 布局管理器还负责视图回收；它管理对用户不再可见的项目视图的重复使用。

如果使用 `RecyclerView` 小组件，则必须指定 `LayoutManager` 和适配器。 如图所示，`LayoutManager` 是适配器和 `RecyclerView` 之间的中介：

![支持 LayoutManager、适配器和数据集的 RecyclerView 图表](lollipop-images/recyclerview-diagram.png)

以下屏幕截图展示了包含 100 项（每项均由 `ImageView` 和 `TextView` 组成）的 `RecyclerView`：

[![RecyclerView 应用在图像中滚动的屏幕截图](lollipop-images/recyclerview-scroll-sml.png)](lollipop-images/recyclerview-scroll.png#lightbox)

`RecyclerView` 可轻松处理此大数据集，在本示例应用中，从列表的开头滚动到列表的结尾只需几秒钟。 `RecyclerView` 还支持动画；事实上，默认会启用添加和删除项的动画。 将项添加到 `RecyclerView` 时，它将淡入，如以下屏幕截图序列所示：

[![图片项目淡入的逐帧屏幕截图](lollipop-images/recyclerview-animation-sml.png)](lollipop-images/recyclerview-animation.png#lightbox)

有关 `RecyclerView` 的详细信息，请参阅 [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)。

### <a name="cardview"></a>CardView

`CardView` 是一个模拟带有圆角的浮动卡的简单视图。 由于 `CardView` 内置了视图阴影，因此它为你提供了一种简单的方法来增加应用的视觉深度。 以下屏幕截图展示了 `CardView` 的三个面向文本的示例：

[![使用 RecyclerView 和基于 CardView 的项的应用的示例屏幕截图](lollipop-images/recyclerview-cardview-sml.png)](lollipop-images/recyclerview-cardview.png#lightbox)

上述示例中的每个卡都包含一个 `TextView`；背景颜色通过 `cardBackgroundColor` 属性设置。

有关 `CardView` 的详细信息，请参阅 [CardView](~/android/user-interface/controls/card-view.md)。

## <a name="enhanced-notifications"></a>改进的通知

Android 5.0 中的通知系统已使用全新的视觉格式和新功能进行了重大更新。 通知在 Android 5.0 中具有新的外观。 例如，Android 5.0 中的通知现在在浅色背景上使用深色文本：

![未展开的 Android 5.0 通知示例](lollipop-images/expanded-notification-contracted.png)

当通知中显示大图标时（如上面的示例所示），Android 5.0 会将小图标作为锁屏提醒显示在大图标上。

在 Android 5.0 中，通知也可显示在设备锁屏上。
例如，以下屏幕截图展示了具有单个通知的锁屏：

[![显示在锁屏上的通知的屏幕截图](lollipop-images/lockscreen-notification-sml.png)](lollipop-images/lockscreen-notification.png#lightbox)

用户可以双击锁屏上的通知以解锁设备并跳转到发出该通知的应用，也可轻扫以消除通知。 通知具有新的“可见性”设置，用于确定锁屏上可以显示多少内容  。 用户可以选择是否允许在锁屏通知中显示敏感内容。

Android 5.0 引入了一种新的高优先级通知显示格式 -“浮动”  。 浮动通知从屏幕顶部向下滑动几秒钟，然后返回到屏幕顶部的通知阴影。 浮动通知使系统 UI 能够将重要信息放在用户面前，而不会中断当前正在运行的活动。
下面的示例展示了一个显示在应用顶部的简单浮动通知：

[![浮动通知示例](lollipop-images/heads-up-notification-sml.png)](lollipop-images/heads-up-notification.png#lightbox)

浮动通知通常用于以下事件：

- 新的下一条消息

- 来电

- 电池电量不足指示

- 警报

仅当 Android 5.0 具有高优先级或最高优先级设置时，才会以浮动格式显示通知。

在 Android 5.0 中，可以提供通知元数据来帮助 Android 更智能地对通知进行排序和显示。 Android 5.0 根据优先级、可见性和类别组织通知。
通知类别用于筛选当设备处于“请勿打扰”模式时可以显示哪些通知  。

有关使用最新的 Android 5.0 功能创建和启动通知的详细信息，请参阅[本地通知](~/android/app-fundamentals/notifications/local-notifications.md)。

## <a name="new-apis"></a>新 API

除了上述新的外观功能外，Android 5.0 还添加了新的 API，用于扩展现有多媒体、存储和无线/连接的功能。 此外，Android 5.0 还包括可为新的作业计划程序功能提供支持的新 API。

### <a name="camera"></a>照相机

Android 5.0 提供了几个新的 API 来增强照相机的功能。 新的 `Android.Hardware.Camera2` 命名空间包括可访问连接到 Android 设备的各个照相机设备的功能。 另外，`Android.Hardware.Camera2` 将每个照相机设备建模为管道：它接受捕获请求，捕获图像，然后输出结果。 这种方法使得应用可以将多个捕获请求排队到一个照相机设备。

以下 API 可以实现这些新功能：

- `CameraManager.GetCameraIdList` &ndash; 帮助你以编程方式访问照相机设备；需使用 `CameraManager.OpenCamera` 连接到特定的照相机设备。

- `CameraCaptureSession` &ndash; 从照相机设备捕获或流式处理图像。 需实现 `CameraCaptureSession.CaptureListener` 接口来处理新的图像捕获事件。

- `CaptureRequest` &ndash; 定义捕获参数。

- `CaptureResult` &ndash; 提供图像捕获操作的结果。

有关 Android 5.0 中新照相机 API 的详细信息，请参阅[媒体](https://developer.android.com/about/versions/android-5.0.html#Media)。

### <a name="audio-playback"></a>音频播放

Android 5.0 更新了 `AudioTrack` 类，以便更好地播放音频：

- `ENCODING_PCM_FLOAT` &ndash; 将 `AudioTrack` 配置为接受浮点格式的音频数据，以获得更好的动态范围、更大的空余空间和更高的质量（由于精度的提高）。 而且，浮点格式有助于避免音频剪辑。

- `ByteBuffer` &ndash; 现在可以将音频数据作为字节数组提供给 `AudioTrack`。

- `WRITE_NON_BLOCKING` &ndash; 此选项简化了某些应用的缓冲和多线程处理。

有关 Android 5.0 中 `AudioTrack` 改进的详细信息，请参阅[媒体](https://developer.android.com/about/versions/android-5.0.html#Media)。

### <a name="media-playback-control"></a>媒体播放控件

Android 5.0 引入了新的 `Android.Media.MediaController` 类，用于替换 `RemoteControlClient`。 `Android.Media.MediaController` 提供简化的传输控件 API，并提供在 UI 上下文外部播放的线程安全控件。 以下新 API 处理传输控件：

- `Android.Media.Session.MediaSession` &ndash; 处理多个控制器的媒体控制器会话。 需调用 `MediaSession.GetSessionToken` 来请求应用用于与会话交互的令牌。

- `MediaController.TransportControls` &ndash; 处理“播放”、“停止”和“跳过”等传输命令    。

此外，还可以使用新的 `Android.App.Notification.MediaStyle` 类将媒体会话与丰富的通知内容（例如提取和显示专辑封面）关联。

有关 Android 5.0 中新的媒体播放控件功能的详细信息，请参阅[媒体](https://developer.android.com/about/versions/android-5.0.html#Media)。

### <a name="storage"></a>存储

Android 5.0 更新了存储访问框架，使应用程序更容易使用目录和文档：

- 要选择目录子树，可以生成并发送 `Android.Intent.Action.OPEN_DOCUMENT_TREE` 意向。 此意向会让系统显示所有支持子树选择的提供程序实例；然后用户浏览并选择一个目录。

- 要在子树下的任何位置创建和管理新文档或目录，请使用 `DocumentsContract` 的新 `CreateDocument`、`RenameDocument` 和 `DeleteDocument` 方法。

- 要获取所有共享存储设备上媒体目录的路径，请调用新的 `Android.Content.Context.GetExternalMediaDirs` 方法。

有关 Android 5.0 中新存储 API 的详细信息，请参阅[存储](https://developer.android.com/preview/api-overview.html#Storage)。

### <a name="wireless--connectivity"></a>无线和连接

Android 5.0 为无线和连接添加了以下 API 增强功能：

- 新的多网络 API，使应用能够在建立连接之前找到并选择具有特定功能的网络  。

- 使 Android 5.0 设备可作为低能耗蓝牙外围设备的蓝牙广播功能。

- NFC 增强功能，使近场通信功能更易于与其他设备共享数据。

有关 Android 5.0 中新的无线和连接 API 的详细信息，请参阅[无线和连接](https://developer.android.com/preview/api-overview.html#Wireless)。

### <a name="job-scheduling"></a>作业计划

Android 5.0 引入了一个新的 `JobScheduler` API，它可以将某些任务安排在仅在设备接通电源和充电时运行，帮助用户最大程度减少电池电量消耗。 作业计划程序功能还可用于将任务安排在更适合该任务的条件下运行，例如在设备通过 WiFi 网络而不是按流量计费的网络进行连接时下载大文件。

有关 Android 5.0 中新的作业计划程序 API 的详细信息，请参阅[安排作业](https://developer.android.com/preview/api-overview.html#JobScheduler)。

## <a name="summary"></a>总结

本文概述了 Android 5.0 中适用于 Xamarin.Android 应用开发人员的重要新功能：

- Material Theme

- 动画

- 查看阴影和高度

- 配色功能，如可绘制着色和突出颜色提取

- 新的 `RecyclerView` 和 `CardView` 小组件

- 通知改进

- 用于照相机、音频播放、媒体控制、存储、无线/连接和作业计划的新 API

如果你不熟悉 Xamarin Android 开发，请参阅[设置和安装](~/android/get-started/installation/index.md)帮助开始使用 Xamarin.Android。
[Hello, Android](~/android/get-started/hello-android/index.md) 非常适合用于学习如何创建 Android 项目。

## <a name="related-links"></a>相关链接

- [Android L 开发人员预览版](https://developer.android.com/preview/index.html)
- [获取 Android SDK](https://developer.android.com/sdk/index.html#Other)
- [Material Design](https://developer.android.com/preview/material/index.html)
