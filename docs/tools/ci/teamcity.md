---
title: 使用团队城与萨马林
description: 本指南将讨论使用 TeamCity 编译移动应用程序，然后将其提交到应用中心测试所涉及的步骤。
ms.prod: xamarin
ms.assetid: AC2626CB-28A7-4808-B2A9-789D67899546
author: davidortinau
ms.author: daortin
ms.date: 04/01/2020
ms.openlocfilehash: 6ecd453180e8c392ba7d7527778617eb40950a9e
ms.sourcegitcommit: 6f3281a32017cfcebadde8a2d6e10651a277828f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/02/2020
ms.locfileid: "80587467"
---
# <a name="using-team-city-with-xamarin"></a>使用团队城与萨马林

_本指南将讨论使用 TeamCity 编译移动应用程序，然后将其提交到应用中心测试所涉及的步骤。_

如[《持续集成入门指南》](~/tools/ci/intro-to-ci.md)所述，在开发高质量的移动应用程序时，持续集成 （CI） 是一种有用的实践。 连续集成服务器软件有许多可行的选项;本指南将重点介绍捷脑团队[城](https://www.jetbrains.com/teamcity/)。

TeamCity 安装有几个不同的排列。 下面的列表描述了其中一些排列：

- **Windows 服务**– 在此方案中，团队城在 Windows 作为 Windows 服务启动时启动。 它必须与 Mac 构建主机配对才能编译任何 iOS 应用程序。

- **在 OS X 上启动守护进程**– 从概念上讲，这类似于在上一步中描述的 Windows 服务中所述的运行。 默认情况下，生成将在根帐户下运行。

- **OS X 上的用户帐户**– 可以在每次用户登录时启动的用户帐户下运行 TeamCity。

在前面的方案中，在 OS X 上的用户帐户下运行 TeamCity 是最简单、最简单的设置。

设置团队城涉及几个步骤：

- **安装团队城**– 本指南中未涵盖 TeamCity 的安装。 本指南假定 TeamCity 是在用户帐户下安装和运行的。 有关安装[TeamCity](https://confluence.jetbrains.com/display/TCD8/Installation)的说明，请参阅 JetBrains 的[TeamCity 8 文档](https://confluence.jetbrains.com/display/TCD8/TeamCity+Documentation)。

- **准备生成服务器**– 此步骤涉及安装构建移动应用程序并准备分发所需的软件、工具和证书。

- **创建生成脚本**– 此步骤并非绝对必要，但生成脚本是构建无人值守的应用程序的有用帮助。 使用生成脚本将有助于解决可能出现的生成问题，并提供一种一致、可重复的方式创建分发二进制文件，即使未实践连续集成也是如此。

- **创建团队城项目**– 完成前三个步骤后，我们必须创建一个团队城项目，该项目将包含检索源代码、编译项目并将测试提交到应用中心测试所需的所有元数据。

## <a name="requirements"></a>要求

需要使用[应用中心测试](https://docs.microsoft.com/appcenter/test-cloud/)的经验。

需要熟悉团队城 8.1。 TeamCity 的安装超出了本文档的范围。 假定 TeamCity 安装在 OS X 小牛上，并且在常规用户帐户下运行，而不是根帐户。

生成服务器应该是一台独立计算机，运行 OS X，专用于持续集成。 理想情况下，生成服务器不负责任何其他角色，如数据库服务器、Web 服务器或开发人员工作站。

> [!IMPORTANT]
> 本指南不包括 Xamarin 的"无头"安装。

[!include[](~/tools/ci/includes/firewall-information.md)]

## <a name="preparing-the-build-server"></a>准备生成服务器

配置生成服务器的一个关键步骤是安装构建移动应用程序所需的所有工具、软件和证书。 生成服务器可以编译移动解决方案并运行任何测试，这一点很重要。 为了尽量减少配置问题，软件和工具应安装在托管 TeamCity 的同一用户帐户中。 以下列表详细说明了所需的内容：

1. **适用于 Mac 的视觉工作室**– 这包括 Xamarin.iOS 和 Xamarin.安卓。
2. **登录到 Xamarin 组件存储**– 此步骤是可选的，仅当应用程序使用 Xamarin 组件存储中的组件时才是必需的。 此时主动登录到组件存储将防止 TeamCity 生成尝试编译应用程序时出现任何问题。
3. **Xcode** = Xcode 需要编译和签名 iOS 应用程序。
4. **Xcode 命令行工具**– 这在[使用 rbenv 指南的"更新 Ruby"](https://github.com/calabash/calabash-ios/wiki)安装部分的步骤 1 中对此进行了介绍。
5. **签名标识&预配配置文件**- 通过 XCode 导入证书和预配配置文件。 有关详细信息，请参阅 Apple 有关[导出签名标识和预配配置文件的](https://developer.apple.com/library/ios/recipes/xcode_help-accounts_preferences/articles/export_signing_assets.html)指南。
6. **Android 密钥存储**- 将所需的 Android 密钥存储复制到 TeamCity 用户有权访问的目录，即`~/Documents/keystores/MyAndroidApp1`。
7. **卡拉巴什**– 如果您的应用程序使用 Calabash 编写测试，则这是可选步骤。 有关详细信息，请参阅[在 OS X 小牛指南上安装卡拉巴什](https://github.com/calabash/calabash-ios/wiki)指南和使用[rbenv 更新 Ruby](https://github.com/calabash/calabash-ios/wiki)指南。

下图说明了所有这些组件：

![](teamcity-images/image1.png "This diagram illustrates all of these components")

安装所有软件后，登录到用户帐户并确认所有软件都已正确安装并正常工作。 这应该涉及编译解决方案并将应用程序提交到应用中心测试。 这可以通过运行生成脚本来简化，如下一节所述。

## <a name="create-a-build-script"></a>创建生成脚本

尽管 TeamCity 可以自行处理编译移动应用程序并将其提交到应用中心测试的所有方面;建议创建生成脚本。 生成脚本具有以下优点：

1. **文档**– 生成脚本用作有关如何构建软件的文档形式。 这将删除与部署应用程序相关的一些"魔力"，并允许开发人员专注于功能。
1. **可重复性**– 生成脚本可确保每次编译和部署应用程序时，无论工作由谁或做什么，它都会以相同的方式发生。 这种可重复的一致性可消除由于执行不正确的生成或人为错误而可能出现的任何问题或错误。
1. **版本控制**– 生成脚本可以包含在源代码管理系统中。 这意味着，如果发现错误或不准确，可以跟踪、监视和更正对生成脚本的更改。
1. **准备环境**– 生成脚本可以包含安装任何必需的第三方依赖项的逻辑。 这将确保应用程序使用适当的组件构建。

生成脚本可以像 PowerShell 文件（在 Windows 上）或 bash 脚本（在 OS X 上）一样简单。 创建生成脚本时，脚本语言有多种选择：

- [**Rake**](https://github.com/jimweirich/rake) – 这是一种基于 Ruby 构建项目的域特定语言 （DSL）。 Rake 具有普及和丰富的图书馆生态系统的优势。

- [**psake**](https://github.com/psake/psake) – 这是用于构建软件的 Windows PowerShell 库

- [**FAKE**](https://fsharp.github.io/FAKE/) – 这是基于 F# 中的 DSL，因此在必要时可以使用现有的 .NET 库。

使用哪种脚本语言取决于您的首选项和要求。

> [!NOTE]
> 可以使用基于 XML 的构建系统（如 MSBuild 或 NAnt），但这些系统缺乏专用于构建软件的 DSL 的表达性和可维护性。

### <a name="parameterizing-the-build-script"></a>参数化生成脚本

构建和测试软件的过程需要应保密的信息。 创建 APK 可能需要密钥存储和/或密钥存储中的密钥别名的密码。 同样，应用中心测试需要开发人员独有的[API 密钥](/appcenter/api-docs/)。 这些类型的值不应在生成脚本中硬编码。 相反，它们应该作为变量传递给生成脚本。

不太敏感的是识别应用中心应用于测试运行的设备的值，如 iOS 设备 ID 或 Android 设备 ID。 这些值不是需要保护的值，但它们可能会从生成更改为生成。

在生成脚本之外存储这些类型的变量还便于在组织内与开发人员共享生成脚本，例如。 开发人员可以使用与生成服务器完全相同的脚本，但可以使用自己的密钥库和[API 密钥](/appcenter/api-docs/)。

存储这些敏感值有两种可能的选项：

- **配置文件**– 为了保护[API 密钥](/appcenter/api-docs/)，不应将此值签入版本控件。 可以为每台计算机创建该文件。 如何从该文件读取值取决于所使用的脚本语言。

- **环境变量**– 这些变量可以轻松地基于每台计算机设置，并且独立于基础脚本语言。

每个选项都有优缺点。 TeamCity 很好地处理环境变量，因此本指南将在创建生成脚本时推荐此技术。

### <a name="build-steps"></a>生成步骤

生成脚本必须执行以下步骤：

- **编译应用程序**– 这包括使用正确的预配配置文件对应用程序进行签名。

- **将应用程序提交到 Xamarin 测试云**– 这包括签名和压缩 APK 与相应的密钥库对齐。

下面将更详细地解释这两个步骤。

#### <a name="compiling-a-xamarinios-application"></a>编译 Xamarin.iOS 应用程序

[!include[](~/tools/ci/includes/commandline-compile-of-xamarin-ios-ipa.md)]

#### <a name="compiling-a-xamarinandroid-application"></a>编译 Xamarin.安卓应用程序

要编译 Android 应用程序，请使用**xbuild（** 或 Windows 上的**msbuild）：**

```bash
/Library/Frameworks/Mono.framework/Commands/xbuild /t:SignAndroidPackage /p:Configuration=Release /path/to/android.csproj
```
编译 Android 应用程序**xbuild**使用项目，而 iOS 应用程序**xbuild**使用解决方案。

#### <a name="submitting-xamarinuitests-to-app-center"></a>向应用中心提交 Xamarin.UI 测试

UI测试使用[应用中心 CLI](https://github.com/microsoft/appcenter-cli)提交，如以下代码段所示：

```bash
appcenter test run uitest --app <TEAM-NAME/APP-NAME> --devices <DEVICE_SET> --token <API_KEY> --app-path <appname.APK-or-appname.IPA> --merge-nunit-xml report.xml --build-dir pathToUITestBuildDir
```

运行测试时，测试结果将以 NUnit 样式 XML 文件（称为**report.xml）** 的形式返回。 TeamCity 将在生成日志中显示信息。

有关如何向应用中心提交 UI 测试的详细信息，请参阅准备[Xamarin.Android 应用](/appcenter/test-cloud/uitest/preparing-for-upload-android)或[准备 Xamarin.iOS 应用](/appcenter/test-cloud/uitest/preparing-for-upload-ios)。

#### <a name="submitting-calabash-tests-to-app-center"></a>向应用中心提交卡拉巴什测试

卡拉巴什测试使用[应用中心 CLI](https://github.com/microsoft/appcenter-cli)提交，如以下代码段所示：

```bash
appcenter test run calabash --app <TEAM-NAME/APP-NAME> --devices <DEVICE_SET> --token <API_KEY> --app-path <appname.APK-or-appname.IPA> --project-dir pathToProjectDir
```

要将 Android 应用程序提交到应用中心测试，必须首先使用 calabash-android 重建 APK 测试服务器：

```bash
$ calabash-android build </path/to/signed/APK>
$ appcenter test run calabash --app <TEAM-NAME/APP-NAME> --devices <DEVICE_SET> --token <API_KEY> --app-path <appname.APK> --project-dir pathToProjectDir
```

有关提交卡拉巴什测试的更多信息，请参阅 Xamarin 的关于[将卡拉巴什测试提交测试云](https://github.com/calabash/calabash-ios/wiki)的指南。

## <a name="creating-a-teamcity-project"></a>创建团队城市项目

安装 TeamCity 和 Mac 的可视化工作室可以构建您的项目后，是时候在 TeamCity 中创建项目来生成项目并将其提交到应用中心。

1. 首先通过 Web 浏览器登录到 TeamCity。 导航到根项目：

    ![导航到根项目](teamcity-images/image2.png "导航到根项目")在根项目下方，创建新的子项目：

    ![导航到根项目下面的根项目，创建新的子项目](teamcity-images/image3.png "导航到根项目下面的根项目，创建新的子项目")
2. 创建子项目后，添加新的生成配置：

    ![创建子项目后，添加新的生成配置](teamcity-images/image5.png "创建子项目后，添加新的生成配置")
3. 将 VCS 项目附加到生成配置。 这通过版本控制设置屏幕完成：

    ![这是通过版本控制设置屏幕完成的](teamcity-images/image6.png "这是通过版本控制设置屏幕完成的")

    如果没有创建 VCS 项目，则可以从下面的"新 VCS 根"页创建一个项目：

    ![如果没有创建 VCS 项目，则可以从"新 VCS 根"页创建一个项目](teamcity-images/image7.png "如果没有创建 VCS 项目，则可以选择从"新 VCS 根"页创建一个项目")

    附加 VCS 根后，TeamCity 将签出项目并尝试自动检测生成步骤。 如果您熟悉 TeamCity，则可以选择检测到的生成步骤之一。 现在可以安全地忽略检测到的生成步骤。

4. 接下来，配置生成触发器。 这将在满足某些条件（例如用户将代码提交到存储库时）对生成进行排队。 以下屏幕截图演示如何添加生成触发器：

    ![此屏幕截图演示如何添加生成触发器](teamcity-images/image8.png "此屏幕截图演示如何添加生成触发器")配置生成触发器的示例可在以下屏幕截图中看到：

    ![在此屏幕截图中可以看到配置生成触发器的示例](teamcity-images/image9.png "在此屏幕截图中可以看到配置生成触发器的示例")

5. 上一节"参数化生成脚本"建议将某些值存储为环境变量。 这些变量可以通过"参数"屏幕添加到生成配置中。 添加应用中心[API 密钥](/appcenter/api-docs/)、iOS 设备 ID 和 Android 设备 ID 的变量，如下图所示：

    ![添加应用中心测试 API 密钥、iOS 设备 ID 和 Android 设备 ID 的变量](teamcity-images/image11.png "添加测试云 API 密钥、iOS 设备 ID 和 Android 设备 ID 的变量")

6. 最后一步是添加一个生成步骤，该步骤将调用生成脚本来编译应用程序并将应用程序排队到应用中心测试。 以下屏幕截图是使用 Rakefile 生成应用程序的生成步骤的示例：

    ![此屏幕截图是使用 Rakefile 构建应用程序的生成步骤的示例](teamcity-images/image12.png "此屏幕截图是使用 Rakefile 构建应用程序的生成步骤的示例")

7. 此时，生成配置已完成。 最好触发生成以确认项目配置正确。 执行此操作的一个好方法是对存储库提交一个小的、微不足道的更改。 TeamCity 应检测提交并启动生成。

8. 生成完成后，检查生成日志，并查看生成是否有任何问题或警告需要注意。

## <a name="summary"></a>总结

本指南介绍了如何使用 TeamCity 构建 Xamarin 移动应用程序，然后将其提交到应用中心测试。 我们讨论了创建生成脚本以自动执行生成过程。 生成脚本负责编译应用程序、提交到应用中心测试以及等待结果。

然后，我们介绍了如何在 TeamCity 中创建一个项目，该项目将在开发人员提交代码并调用生成脚本时对生成进行排队。

## <a name="related-links"></a>相关链接

- [准备 Xamarin.安卓应用程序](/appcenter/test-cloud/uitest/preparing-for-upload-android)
- [准备 Xamarin.iOS 应用程序](/appcenter/test-cloud/uitest/preparing-for-upload-ios)
- [安装和配置团队城](https://confluence.jetbrains.com/display/TCD8/Installing+and+Configuring+the+TeamCity+Server)
