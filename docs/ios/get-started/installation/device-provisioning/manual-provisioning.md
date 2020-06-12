---
title: Xamarin.iOS 的手动预配
description: Xamarin.iOS 成功安装后，iOS 开发的下一步是 iOS 设备预配。 本指南介绍如何使用手动预配设置开发证书和配置文件。
ms.prod: xamarin
ms.assetid: E26ACC94-F4A5-4FF5-B7D4-BE596745A665
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/06/2020
ms.openlocfilehash: 9333750432395d008a5454e293648f4e594ae112
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/09/2020
ms.locfileid: "84571683"
---
# <a name="manual-provisioning-for-xamarinios"></a>Xamarin.iOS 的手动预配

Xamarin.iOS 成功安装后，iOS 开发的下一步是预配 iOS 设备。_本指南介绍如何使用手动预配设置开发证书和配置文件。_

> [!NOTE]
> 此页上的说明与付费访问 Apple 开发者计划的开发人员相关。 如果你有一个免费帐户，请参阅[免费预配](~/ios/get-started/installation/device-provisioning/free-provisioning.md)指南，详细了解基于设备的测试。

## <a name="create-a-development-certificate"></a>创建开发证书

设置开发设备的第一步是创建签名证书。 签名证书包含以下两项：

- 开发证书
- 私钥

开发证书和关联[密钥](#understanding-certificate-key-pairs)对 iOS 开发者至关重要：它们生成了你的 Apple 标识，并将你与给定设备和配置文件相关联以用于开发，类似于将你的数字签名放入应用程序。 Apple 会检查证书以控制对你可以部署的设备的访问。

可通过访问 Apple Members Center 的[证书、标识符和配置文件](https://developer.apple.com/account/resources/certificates/list)（要求登录）部分管理开发团队、证书和配置文件。 Apple 要求你拥有签名标识才能为设备或模拟器生成代码。  

> [!IMPORTANT]
> 请务必注意，任何时候，一次只能有两个 iOS 开发证书。 如果需要创建更多证书，需要撤销现有证书。 使用已撤销证书的计算机不能对应用进行签名。

在开始手动预配过程之前，应确保在 Visual Studio 中添加了 Apple 开发者帐户，如 [Apple 帐户管理](~/cross-platform/macios/apple-account-management.md)指南中所述。 添加 Apple 开发者帐户后，请执行以下操作以生成签名证书：

1. 转到 Visual Studio 中的 Apple 开发者帐户窗口。
    1. Mac：“Visual Studio”>“首选项”>“Apple 开发者帐户” 
    2. Windows：“工具”>“选项”>“Xamarin”>“Apple 帐户” 

2. 选择团队，并单击“查看详细信息…” 
3. 单击“创建证书”，然后选择“Apple 开发”或“iOS 开发”    。 如果你拥有正确的权限，几秒钟后系统将显示新的签名标识。

### <a name="understanding-certificate-key-pairs"></a>了解证书密钥对

开发人员配置文件包含证书、关联密钥，以及任何与帐户关联的设置配置文件。 实际上有两个版本的开发者配置文件 — 一个在开发者门户中，另一个在本地 Mac 上。 两者之间的区别在于它们所包含的密钥类型：_门户上的配置文件包含与证书关联的所有公钥，而本地 Mac 上的副本包含所有私钥_。 要使证书生效，密钥对必须匹配。

> [!WARNING]
> 丢失证书和关联密钥会很麻烦，因为需要撤销现有证书并重新预配任何关联的设备，包括为临时部署注册的设备。 成功设置开发证书后，导出备份副本并将其存储在安全位置。 有关如何执行此操作的详细信息，请参阅 Apple 文档中[维护证书](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/MaintainingCertificates/MaintainingCertificates.html)指南的“导出与导入证书和配置文件”部分。

<a name="provisioning"></a>

## <a name="provision-an-ios-device-for-development"></a>预配 iOS 设备进行开发

现在你已经生成 Apple 标识，并且已具有开发证书，现在必须设置预配配置文件和所需实体，以便将应用部署到 Apple 设备。 设备必须运行 Xcode 支持的 iOS 版本 - 可能需要更新设备和/或 Xcode。

<a name="adddevice"></a>

## <a name="add-a-device"></a>添加设备

创建用于开发的设置配置文件时，必须声明哪些设备可以运行应用程序。 为了实现此目的，开发者门户每个日历年可以添加最多 100 台设备，还可以从这里选择要添加到特定设置配置文件的设备。 请按照以下步骤在 Mac 上将设备添加到开发人员门户

1. 使用提供的 USB 电缆连接要设置到 Mac 的设备。
2. 打开 Xcode 并转到“窗口”>“设备和模拟器”  。
3. 在“设备”选项卡下，从左侧的菜单中选择设备  。
4. 突出显示“标识符”  字符串并将其复制到剪贴板：

   ![Xcode 设备和模拟器窗口，突出显示 iOS 标识符字符串位置。](manual-provisioning-images/xcode-devices.png)

5. 在 Web 浏览器中，转到[开发人员门户中的设备部分](https://developer.apple.com/account/resources/devices/list)，然后单击 + 按钮  ：

   ![Apple 开发人员网站上“设备”页面的屏幕截图，其中突出显示了添加按钮。](manual-provisioning-images/developer-portal-devices.png)

6. 设置正确的平台，并为新设备提供名称  。 将先前复制的标识符粘贴到“设备 ID”字段中  ：

    ![新设备注册页的屏幕截图，其中的字段已正确填充。](manual-provisioning-images/new-device-info.png)

7. 单击 **“继续”** 。
8. 查看信息，然后单击“注册”  。

对将用于测试或调试 Xamarin.iOS 应用程序的任何 iOS 设备重复上述步骤。

<a name="provisioningprofile"></a>

## <a name="create-a-development-provisioning-profile"></a>创建开发预配配置文件

将设备添加到开发人员门户后，需要创建设置配置文件并向其添加设备。 

创建预配配置文件以前，应先创建应用 ID  。 应用 ID 是反向 DNS 样式字符串，用于唯一标识应用程序。 以下步骤演示如何创建通配符应用 ID  ，以用于生成和安装大多数应用程序。 “显式应用 ID”  只允许安装一个应用程序（具有匹配的捆绑 ID），且通常用于某些 iOS 功能，如 Apple Pay 和 HealthKit。 有关创建“显式应用 ID”的信息，请参阅[使用功能](~/ios/deploy-test/provisioning/capabilities/index.md)指南。

### <a name="new-wildcard-app-id"></a>新建通配符应用 ID

1. 转到[开发人员门户中的标识符部分](https://developer.apple.com/account/resources/identifiers/list)，然后单击 + 按钮  。
2. 选择“应用 ID”，然后单击“继续”   。
3. 提供说明  。 然后将“捆绑 ID”设置为“通配符”，并以 `com.[DomainName].*` 格式输入 ID   ：

   ![新应用 ID 注册页的屏幕截图，其中必填字段已填充。](manual-provisioning-images/new-app-id.png)

4. 单击 **“继续”** 。
5. 查看信息，然后单击“注册”  。

### <a name="new-provisioning-profile"></a>新建预配配置文件

创建好应用 ID 后，即可创建预配配置文件。 此配置文件包含以下信息：与此配置文件相关的应用（如果是通配符应用 ID，则为多个应用），谁可以使用配置文件（具体取决于添加了哪些开发人员证书），以及哪些设备可以安装应用    。

若要手动创建用于开发的预配配置文件，请执行以下操作：

1. 转到[开发人员门户中的配置文件部分](https://developer.apple.com/account/resources/profiles/list)，然后单击 + 按钮  。

2. 在“开发”下，选择“iOS 应用开发”，然后单击“继续”    。

3. 从下拉菜单中选择要使用的应用 ID，然后单击“继续”  。

4. 选择要在预配配置文件中包含的一个或多个证书，然后单击“继续”  。

5. 选择将安装应用的所有设备，然后单击“继续”  。

6. 提供预配配置文件名，然后单击“生成”   。

7. 你可以根据需要单击下一页上的“下载”，将预配配置文件下载到 Mac 上  。

<a name="download"></a>

## <a name="download-provisioning-profiles-in-visual-studio"></a>在 Visual Studio 中下载预配配置文件

在 Apple 开发人员门户中创建新的预配配置文件后，使用 Visual Studio 下载它，以便可在应用中将它用于捆绑包签名。

1. 转到 Visual Studio 中的 Apple 开发者帐户窗口。
    1. Mac：“Visual Studio”>“首选项”>“Apple 开发者帐户” 
    2. Windows：“工具”>“选项”>“Xamarin”>“Apple 帐户” 

2. 选择团队，然后单击“查看详细信息…” 
3. 验证新配置文件是否出现在“预配配置文件”列表中  。 可能需要重启 Visual Studio 以刷新该列表。 
4. 单击“下载所有配置文件”  。

Visual Studio 中现在将提供新的预配配置文件，并且它可供使用。

## <a name="deploy-to-a-device"></a>部署到设备

这时，设置应已完成，且应用已准备好部署到设备。 为此，请执行以下步骤：

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

1. 将设备连接到 Mac。
2. 打开“Info.plist”，确保“捆绑包标识符”与前面创建的应用 ID 匹配（除非应用 ID 是通配符）   。
3. 在“签名”部分中，选择“手动预配”作为“方案”    ：

    ![Visual Studio for Mac 中 Info.plist 的屏幕截图，其中已选定“手动预配”](manual-provisioning-images/vsm-info-plist.png)

4. 单击“捆绑包签名选项…” 
5. 确保生成配置设置为“调试”和“iPhone”  。 打开“签名标识”和“预配配置文件”下拉菜单，以验证列出的证书和预配配置文件是否正确   ： 

   ![iOS 捆绑包签名属性页，其中的“预配配置文件”下拉菜单已打开，列出了应用的所有可用预配配置文件。](manual-provisioning-images/vsm-bundle-signing.png)

6. 选择要使用的特定标识和配置文件，或将其保留为“自动”  。 设置为“自动”时，Visual Studio for Mac 将基于“Info.plist”中的“捆绑包标识符”选择标识和配置文件    。 
7. 单击 **“确定”** 。
8. 单击“运行”将应用部署到设备  。


# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. 将设备连接到 Mac 生成主机。
2. 打开“Info.plist”，确保“捆绑包标识符”与前面创建的应用 ID 匹配（除非应用 ID 是通配符）   。
3. 在“解决方案资源管理器”中，右键单击 iOS 项目名称，选择“属性”，然后导航到“iOS 捆绑包签名”选项卡    。
4. 确保生成配置设置为“调试”和“iPhone”  。 在“捆绑包签名”下，选择“手动预配”作为“方案”    ：

    ![Visual Studio for Mac 中 Info.plist 的屏幕截图，其中已选定“手动预配”](manual-provisioning-images/vs-bundle-signing.png)

5. 打开“签名标识”和“预配配置文件”下拉菜单，以验证列出的证书和预配配置文件是否正确   。
6. 选择要使用的特定标识和配置文件，或将其保留为“自动”  。 设置为“自动”时，Visual Studio 将基于“Info.plist”中的“捆绑包标识符”选择标识和配置文件    。 
7. 单击“运行”将应用部署到设备  。

-----

## <a name="provisioning-for-application-services"></a>对应用程序服务进行预配

Apple 提供一系列精选的特殊应用服务（亦称为“功能”），可以为 Xamarin.iOS 应用激活这些服务。 必须同时在 iOS 设置门户（创建“应用 ID”  时）和“Entitlements.plist”  文件（它是 Xamarin.iOS 应用程序项目的一部分）上配置这些应用程序服务。 若要了解如何将应用程序服务添加到应用，请参阅[功能简介](~/ios/deploy-test/provisioning/capabilities/index.md)指南和[使用授权](~/ios/deploy-test/provisioning/entitlements.md)指南。

- 创建具备所需应用服务的应用 ID。
- 创建含有此应用 ID 的新[配置文件](#provisioningprofile)。
- 在 Xamarin.iOS 项目中设置权利

## <a name="related-links"></a>相关链接

- [可用设置](~/ios/get-started/installation/device-provisioning/free-provisioning.md)
- [应用分发](~/ios/deploy-test/app-distribution/index.md)
- [疑难解答](~/ios/deploy-test/troubleshooting.md)
- [Apple - 应用分发指南](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html)
