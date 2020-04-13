---
title: WPF 平台设置
description: Xamarin.Forms 现在对 WPF 平台具有预览支持
ms.prod: xamarin
ms.assetid: 650723F2-4279-4B7B-B0A1-D7F8FF26BF1E
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 04/09/2020
ms.openlocfilehash: c9ec9fec2391d7d7a24f97f2ec20208a7d69dbc1
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/09/2020
ms.locfileid: "80992388"
---
# <a name="wpf-platform-setup"></a>WPF 平台设置

![预览](~/media/shared/preview.png)

Xamarin.Forms 现在具有对 Windows 演示基础 （WPF） 的预览支持。 本文演示如何将 WPF 项目添加到 Xamarin.Forms 解决方案。

> [!IMPORTANT]
> Xamarin.表格支持WPF由社区提供。 有关详细信息，请参阅[Xamarin.窗体平台支持](https://github.com/xamarin/Xamarin.Forms/wiki/Platform-Support)。

在开始之前，在 Visual Studio 2019 中创建新的 Xamarin.Forms 解决方案，或使用现有的 Xamarin.Forms 解决方案，例如[**BoxViewClock**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-boxviewclock)。 您只能将 WPF 应用添加到 Windows 中的 Xamarin.Forms 解决方案。

## <a name="add-a-wpf-application"></a>添加 WPF 应用程序

按照以下说明添加将在 Windows 7、8 和 10 桌面上运行的 WPF 应用程序：

1. 在 Visual Studio 2019 中，右键单击**解决方案资源管理器**中的解决方案名称，然后选择"**添加>新项目..."**

2. 在"**添加新项目**"窗口中，在 **"语言**"下拉列表中选择**C#，** 在 **"平台**"下拉列表中选择**Windows，** 然后选择 **"项目类型**"下拉菜单中的**桌面**。 在项目类型列表中，选择**WPF 应用 （.NET 框架）：**

    ![添加新的 WPF 项目](wpf-images/add-project.png "添加新的 WPF 项目")

    按 **"下一步"** 按钮。

3. 在 **"配置新项目**"窗口中，键入具有**WPF**扩展名的项目的名称，例如**BoxViewClock.WPF**。 单击 **"浏览"** 按钮，选择**BoxViewClock**文件夹，然后按 **"选择文件夹"** 将 WPF 项目与解决方案中的其他项目放在同一目录中：

    ![添加新的 WPF 项目](wpf-images/configure-project.png "添加新的 WPF 项目")

    按 **"创建**"按钮以创建项目。

4. 在**解决方案资源管理器**中，右键单击新的**BoxViewClock.WPF**项目，然后选择 **"管理 NuGet 包..."。** 选择 **"浏览**"选项卡，然后搜索**Xamarin.Forms.Platform.WPF**：

    ![选择 NuGet 包](wpf-images/select-nuget-package.png "选择 NuGet 包")

    选择包，然后单击 **"安装**"按钮。

5. 右键单击**解决方案资源管理器**中的解决方案名称，然后选择 **"管理解决方案的 NuGet 包..."。** 选择"**更新**"选项卡，然后选择**Xamarin.窗体**包。 选择所有项目并将其更新为相同的 Xamarin.Forms 版本：

    ![更新 NuGet 包](wpf-images/update-nuget-package.png "更新 NuGet 包")

6. 在 WPF 项目中，右键单击 **"引用"** 并选择"**添加引用..."** 在 **"参考管理器"****对话框中，** 选择左侧的项目，然后选中与**BoxViewClock**项目相邻的复选框：

    ![引用共享项目](wpf-images/reference-shared-project.png "引用共享项目")

    按 **"确定"** 按钮。

7. 编辑 WPF 项目的**MainWindow.xaml**文件。 在标记`Window`中，为**Xamarin.Forms.Platform.WPF**程序集和命名空间添加 XML 命名空间声明：

    ```xaml
    xmlns:wpf="clr-namespace:Xamarin.Forms.Platform.WPF;assembly=Xamarin.Forms.Platform.WPF"
    ```

    现在将`Window`标记更改为`wpf:FormsApplicationPage`。 将`Title`设置更改为应用程序的名称，例如**BoxViewClock**。 已完成的 XAML 文件应如下所示：

    ```xaml
    <wpf:FormsApplicationPage x:Class="BoxViewClock.WPF.MainWindow"
            xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
            xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
            xmlns:local="clr-namespace:BoxViewClock.WPF"
            xmlns:wpf="clr-namespace:Xamarin.Forms.Platform.WPF;assembly=Xamarin.Forms.Platform.WPF"            
            mc:Ignorable="d"
            Title="BoxViewClock" Height="450" Width="800">
        <Grid>

        </Grid>
    </wpf:FormsApplicationPage>
    ```

8. 编辑 WPF 项目的**MainWindow.xaml.cs**文件。 添加两个新`using`指令：

    ```csharp
    using Xamarin.Forms;
    using Xamarin.Forms.Platform.WPF;
    ```

    将 的`MainWindow`基类从`Window``FormsApplicationPage`更改为 。 调用`InitializeComponent`后，添加以下两个语句：

    ```csharp
    Forms.Init();
    LoadApplication(new BoxViewClock.App());
    ```

    除了注释和未使用的`using`指令外，完整的**MainWindows.xaml.cs**文件应如下所示：

    ```csharp
    using Xamarin.Forms;
    using Xamarin.Forms.Platform.WPF;

    namespace BoxViewClock.WPF
    {
        public partial class MainWindow : FormsApplicationPage
        {
            public MainWindow()
            {
                InitializeComponent();

                Forms.Init();
                LoadApplication(new BoxViewClock.App());
            }
        }
    }
    ```

9. 右键单击**解决方案资源管理器**中的 WPF 项目，然后选择 **"设置为启动项目**"。 按 F5 在 Windows 桌面上使用 Visual Studio 调试器运行程序：

    ![WPF 盒视图时钟](wpf-images/wpf-boxviewclock.png "WPF 盒视图时钟" )

## <a name="platform-specifics"></a>平台细节

您可以确定 Xamarin.Forms 应用程序从代码或 XAML 运行的平台。 这允许您在 WPF 上运行程序特征时更改其特征。 在代码中，将 的值`Device.RuntimePlatform`与`Device.WPF`常量（等于字符串"WPF"）进行比较。 如果存在匹配项，则应用程序在 WPF 上运行。

在 XAML 中，`OnPlatform`可以使用标记选择特定于平台的属性值：

```xaml
<Button.TextColor>
    <OnPlatform x:TypeArguments="Color">
        <On Platform="iOS" Value="White" />
        <On Platform="macOS" Value="White" />
        <On Platform="Android" Value="Black" />
        <On Platform="WPF" Value="Blue" />
    </OnPlatform>
</Button.TextColor>
```

## <a name="window-size"></a>窗口大小

您可以在 WPF **MainWindow.xaml**文件中调整窗口的初始大小：

```xaml
Title="BoxViewClock" Height="450" Width="800"
```

## <a name="issues"></a>问题

这是预览，因此您应该期望并非所有产品都已准备就绪。 并非所有 Xamarin.Forms 的 NuGet 包都已准备好用于 WPF，并且某些功能可能未完全工作。

## <a name="related-video"></a>相关视频

> [!VIDEO https://youtube.com/embed/Fy9N6OSxK64]

**Xamarin.表格 3.0 WPF 支持视频**
