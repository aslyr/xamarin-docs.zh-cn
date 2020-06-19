---
title: Mac 平台设置
description: 本文介绍如何向项目中添加 Mac 项目 Xamarin.Forms ，该项目将生成一个应用程序，该应用程序能够在 macOS Sierra 和 MacOS El Capitan 上运行。
ms.prod: xamarin
ms.assetid: EEC549E0-F182-4F9C-B2BA-B31D19569AA5
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 05/03/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 11897d2d3b8b7ba0a62956f1dbe4d8b873352e7a
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/18/2020
ms.locfileid: "84139550"
---
# <a name="mac-platform-setup"></a>Mac 平台设置

![预览](~/media/shared/preview.png)

在开始之前，请创建（或使用现有） Xamarin.Forms 项目。 只能使用 Visual Studio for Mac 添加 Mac 应用。

> [!VIDEO https://youtube.com/embed/mvQ7jzaNseM]

**将 macOS 项目添加到 Xamarin.Forms 视频**

## <a name="adding-a-mac-app"></a>添加 Mac 应用

按照以下说明添加将在 macOS Sierra 和 macOS El Capitan 上运行的 Mac 应用：

1. 在 Visual Studio for Mac 中，右键单击现有 Xamarin.Forms 解决方案，然后选择 "**添加 > 添加新项目 ...** "

2. 在 "**新建项目**" 窗口中选择 " **Mac > 应用 > Cocoa 应用**"，然后按 "**下一步**"。

3. 键入**应用名称**（并根据需要为停靠项选择其他名称），然后按 "**下一步**"。

4. 查看配置并按 "**创建**"。 这些步骤如下所示：

    ![显示如何添加 Cocoa 应用的动画说明](mac-images/add-macos-proj.gif)

5. 在 Mac 项目中，右键单击 "**包" > "添加包 ...** " 添加 [Xamarin.Forms](https://www.nuget.org/packages/Xamarin.Forms/) NuGet。 还应将其他项目更新为使用同一版本的 Xamarin.Forms NuGet 包。

6. 在 Mac 项目中，右键单击 "**引用**" 并添加对项目的引用 Xamarin.Forms （"共享项目" 或 ".NET Standard 库项目"）。

    ![添加对 Xamarin.Forms 共享代码项目的引用](mac-images/references-sml.png)

7. 更新**Main.cs**以初始化 `AppDelegate` ：

    ```csharp
    static class MainClass
    {
        static void Main(string[] args)
        {
            NSApplication.Init();
            NSApplication.SharedApplication.Delegate = new AppDelegate(); // add this line
            NSApplication.Main(args);
        }
    }
    ```

8. 更新 `AppDelegate` 以进行初始化 Xamarin.Forms ，创建窗口，并加载 Xamarin.Forms 应用程序（记住设置适当的 `Title` ）。 _如果有其他需要初始化的依赖项，也可以在此处执行此操作。_

    ```csharp
    using Xamarin.Forms;
    using Xamarin.Forms.Platform.MacOS;
    // also add a using for the Xamarin.Forms project, if the namespace is different to this file
    ...
    [Register("AppDelegate")]
    public class AppDelegate : FormsApplicationDelegate
    {
        NSWindow window;
        public AppDelegate()
        {
            var style = NSWindowStyle.Closable | NSWindowStyle.Resizable | NSWindowStyle.Titled;

            var rect = new CoreGraphics.CGRect(200, 1000, 1024, 768);
            window = new NSWindow(rect, style, NSBackingStore.Buffered, false);
            window.Title = "Xamarin.Forms on Mac!"; // choose your own Title here
            window.TitleVisibility = NSWindowTitleVisibility.Hidden;
        }

        public override NSWindow MainWindow
        {
            get { return window; }
        }

        public override void DidFinishLaunching(NSNotification notification)
        {
            Forms.Init();
            LoadApplication(new App());
            base.DidFinishLaunching(notification);
        }
    }
    ```

9. 双击 **"Xcode" 以在**中进行编辑。 选择**窗口**并_取消选中_"**是初始控制器**" 复选框（这是因为上面的代码创建了一个窗口）：

    [![取消选中 Xcode 中的 "是初始控制器" 复选框](mac-images/xcode-init-controller-sml.png)](mac-images/xcode-init-controller.png#lightbox)

    您可以在情节提要中编辑菜单系统，以删除不需要的项目。

10. 最后，添加任何本地资源（例如 图像文件）的文件。

11. 现在，Mac 项目应 Xamarin.Forms 在 macOS 上运行你的代码！

## <a name="next-steps"></a>后续步骤

### <a name="styling"></a>“样式”

如果最近更改了 `OnPlatform` 你的，你现在可以针对任意数量的平台。 其中包括 macOS。

```xml
<Button.TextColor>
    <OnPlatform x:TypeArguments="Color">
        <On Platform="iOS" Value="White"/>
        <On Platform="macOS" Value="White"/>
        <On Platform="Android" Value="Black"/>
    </OnPlatform>
</Button.TextColor>
```

请注意，你还可以在如下所示的平台上翻倍： `<On Platform="iOS, macOS" ...>` 。

### <a name="window-size-and-position"></a>窗口大小和位置

可以在中调整窗口的初始大小和位置 `AppDelegate` ：

```csharp
var rect = new CoreGraphics.CGRect(200, 1000, 1024, 768);  // x, y, width, height
```

## <a name="known-issues"></a>已知问题

这是一种预览版，因此，你应预计并非一切都准备就绪。 向项目添加 macOS 时，你可能会遇到以下几个问题：

### <a name="not-all-nugets-are-ready-for-macos"></a>并非所有 Nuget 都已准备好 macOS

你可能会发现你使用的一些库尚不支持 macOS。 在这种情况下，你将需要向项目的 maintainer 发送请求以添加它。 在支持之前，您可能需要查找备选方案。

### <a name="missing-xamarinforms-features"></a>缺少 Xamarin.Forms 功能

Xamarin.Forms在此预览版中，并非所有功能都已完成。 有关详细信息，请参阅 GitHub 存储库中的[平台支持 MacOS 状态](https://github.com/xamarin/Xamarin.Forms/wiki/Platform-Support-macOS-Status) [Xamarin.Forms](https://github.com/xamarin/Xamarin.Forms) 。

## <a name="related-links"></a>相关链接

- [Xamarin.Mac](~/mac/index.yml)
