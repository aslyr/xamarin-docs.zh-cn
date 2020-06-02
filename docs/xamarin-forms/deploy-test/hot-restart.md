---
title: ''
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 45b7d0d20c43aa22ebde3a17552f10ceea77a48b
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/28/2020
ms.locfileid: "84139134"
---
# <a name="xamarin-hot-restart-preview"></a>Xamarin 热重启（预览版）

![预览版功能](~/media/shared/preview.png)

Xamarin 热重启使你可以在开发过程中快速测试对应用进行的更改，包括多文件代码编辑、资源和引用。 它将新更改推送到调试目标上的现有应用捆绑包，这会使生成和部署周期快得多。

> [!IMPORTANT]
> Xamarin 热重启当前在 Visual Studio 2019 16.5 版中提供，并支持使用 Xamarin.Forms 的 iOS 应用。 对 Visual Studio for Mac 和非 Xamarin.Forms 应用的支持在计划之中。

## <a name="requirements"></a>要求

- Visual Studio 2019 版本 16.5
- iTunes（64 位）
- Apple 开发人员帐户和付费的 [Apple 开发人员计划](https://developer.apple.com/programs)合约


## <a name="initial-setup"></a>初始设置

> [!NOTE]
> Xamarin 热重启在预览状态下默认处于禁用状态。 可以在“工具”>“选项”>“环境”>“预览功能”>“启用 Xamarin 热重启”下启用它。

1. 确保将 iOS 项目设置为启动项目，并将生成配置设置为“调试|iPhone”。

   1. 如果这是现有项目，请转到“生成”>“配置管理器...” 并确保为 iOS 项目启用“部署”。

2. 在工具栏中选择并单击“本地设备”，以启动设置向导：

    [![](hot-restart-images/toolbar.png "Screenshot of the Visual Studio toolbar with local device set as the debug target.")](hot-restart-images/toolbar.png)

3. 如果未安装 iTunes，请单击“下载 iTunes”以下载安装程序。 iTunes 安装完成时单击“下一步”。

4. 将 iOS 设备连接到计算机。 如果设备已接入电源，请断开电源，然后重新接入。 检测到后，设备名称会显示在向导中。 单击 **“下一步”** 。

5. 输入 Apple 开发人员帐户凭据，然后单击“下一步”。

6. 使用下拉菜单选择开发团队，以便在项目中启用[自动预配](~/ios/get-started/installation/device-provisioning/automatic-provisioning.md)。 单击 **“完成”** 。

> [!NOTE]
> 建议使用自动预配，以便可以轻松地为部署配置其他 iOS 设备。 但是，如果存在正确的预配配置文件，则可以禁用它并继续使用手动预配。

## <a name="use-xamarin-hot-restart"></a>使用 Xamarin 热重启
初始设置之后，连接的设备会出现在调试目标下拉菜单中。 若要调试应用，请在下拉菜单中选择你的设备，然后单击“运行”按钮。 你可能会在 Visual Studio 中看到一条消息，指示你在设备上手动启动应用以启动调试会话。

可以在调试时编辑代码文件，然后按调试工具栏中的“重启”按钮，或使用 Ctrl+Shift+F5 重启调试会话，并应用新更改：

[![](hot-restart-images/restart.png "Screenshot of the debug toolbar with the restart button highlighted.")](hot-restart-images/toolbar.png)

还可以使用 `HOTRESTART` 预处理器符号来防止在使用 Xamarin 热重启进行调试时执行某些代码。

## <a name="limitations"></a>限制

- 当前仅支持使用 Xamarin.Forms 和 iOS 设备生成的 iOS 应用。
- 仅支持 64 位 iOS 设备。 从 iOS 11 开始，Apple 不再支持在 32 位体系结构（早于 iPhone 5s 的设备）上运行 iOS 应用。
- 不支持情节提要和 XIB 文件，如果应用尝试在运行时加载这些文件，则应用可能会崩溃。 使用 `HOTRESTART` 预处理器符号可防止执行此代码。
- 不支持静态 iOS 库和框架，如果应用尝试加载它们，则可能会看到运行时错误或崩溃。 使用 `HOTRESTART` 预处理器符号可防止执行此代码。 支持动态 iOS 库。
- 无法使用 Xamarin 热重启创建应用捆绑包进行发布。 仍需要 Mac 计算机进行完整编译、签名和部署，以便将应用程序投入生产。

## <a name="troubleshoot"></a>疑难解答

- 如果 iTunes 是通过 Microsoft Store 安装，则设置向导不会检测到 iTunes。 首先需要卸载该版本，然后[从 Apple下载安装程序](https://go.microsoft.com/fwlink/?linkid=2101014)。
- 存在一个已知问题，即启用设备特定的生成会阻止应用进入调试模式。 解决方法是在“属性”>“iOS 版本”下禁用此项，然后重新尝试调试。 此问题将在未来版本中得到解决。
- 如果设备上已存在该应用，尝试使用热重启进行部署可能会失败，并出现 `AMDeviceStartHouseArrestService` 错误。 解决方法是在设备上卸载该应用，然后再次部署。
- 输入不属于 Apple 开发人员计划的 Apple ID 可能导致以下错误：`Authentication Error. Xcode 7.3 or later is required to continue developing with your Apple ID`。 必须具有有效的 Apple 开发人员帐户才能在 iOS 设备上使用 Xamarin 热重启。 

若要报告其他问题，请在[“帮助”>“发送反馈”>“报告问题”](/visualstudio/ide/feedback-options?view=vs-2019#report-a-problem)处使用反馈工具。
