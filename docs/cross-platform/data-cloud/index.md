---
title: 微软 Azure 和 Xamarin
description: 本文档链接到有关 Mac、Azure 移动应用、活动目录身份验证和 WebAPI 可视化工作室中已连接服务的文档。
ms.prod: xamarin
ms.assetid: 7b9aa8d9-c181-4c33-8ab0-2f56e4dbfc04
author: davidortinau
ms.author: daortin
ms.date: 10/09/2017
ms.openlocfilehash: 2b6dfeb0de0fac59556280609dbf870c23a9298b
ms.sourcegitcommit: 2a8eb8bce427e72d4e7edd06ce432e19f17dcdd7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2020
ms.locfileid: "80388520"
---
# <a name="microsoft-azure-and-xamarin"></a>微软 Azure 和 Xamarin

[![](images/evolve-mikej-azure-sml.png "Azure App Services features are easy to add to Xamarin apps, including cloud data storage and cross-platform push notifications")](https://evolve.xamarin.com/session/56ec886fde91c6253c277bc6)

[2016 年发展：使用 Azure 和 Xamarin 开发互联应用](https://evolve.xamarin.com/session/56ec886fde91c6253c277bc6)

## <a name="connected-services-in-visual-studio-for-mac"></a>Visual Studio for Mac 中连接的服务

适用于 Mac 的 Visual Studio 新的[互联服务](connected-services.md)功能可帮助开发人员从 IDE 中快速轻松地将 Azure 功能添加到移动应用程序。 目前可在 Alpha 通道中进行测试。

## <a name="azure-app-services"></a>Azure 应用服务

有一组[Azure 移动应用文档](~/cross-platform/data-cloud/mobile-apps.md)，用于指导您完成实现[Azure 移动客户端](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)的过程。
Xamarin 还为[iOS](https://www.nuget.org/packages/Xamarin.Azure.NotificationHubs.iOS/)和[Android](https://www.nuget.org/packages/Xamarin.Azure.NotificationHubs.Android/)提供了 Azure 消息传递 NuGet 包，以帮助跨平台实现推送通知。

在[Azure 应用服务门户](https://portal.azure.com/)上配置应用以访问移动应用、Web API、存储等。 了解[应用程序服务是如何不同的](https://azure.microsoft.com/updates/whats-new-with-azure-app-service/)，并观看[这些视频从微软](https://azure.microsoft.com/campaigns/azure-march-announcement/)。

## <a name="active-directory-authentication"></a>Active Directory 身份验证

[Azure 活动目录](~/cross-platform/data-cloud/active-directory/index.md)可用于将用户登录到 Xamarin 应用。 然后，这些应用可以访问 Office 365 等其他服务。

## <a name="webapi"></a>WebAPI

微软的 Web API 公开了一个类似 REST 的界面，Xamarin 应用程序可以轻松使用该接口。
您可以轻松地启动[Azure 网站](https://trywebsites.azurewebsites.net/)并构建基于 WebAPI 的应用以连接到 Xamarin 应用。

### <a name="introduction-to-web-services"></a>[网络服务简介](~/cross-platform/data-cloud/web-services/index.md)

本教程介绍如何将 REST、WCF 和 SOAP Web 服务技术与 Xamarin 移动应用程序集成。 它检查各种服务实现，评估用于集成它们的可用工具和库，并提供用于使用服务数据的示例模式。 最后，它提供了使用 Xamarin 移动应用程序创建用于消费的 RESTful Web 服务的基本概述。

## <a name="samples"></a>示例

除了[文档示例](https://github.com/xamarin/mobile-samples/tree/master/Azure)之外，以下完整应用程序演示了集成到 Xamarin 应用程序中的各种 Azure 功能：

- [体育](https://github.com/xamarin/Sport)– 友好的体育联赛跟踪应用程序，使用数据存储&推送通知。
- [时刻](https://github.com/pierceboggan/Moments)– 使用 Azure 存储的图像的即时照片共享。
- [Xamarin CRM](https://github.com/xamarin/app-crm) = 使用 Web API 作为后端。
- [MyShoppe](https://github.com/jamesmontemagno/MyShoppe) - Azure 移动应用。

- [eShop](https://github.com/dotnet-architecture/eShopOnContainers) –[体系结构系列](https://www.microsoft.com/net/learn/architecture)电子书的示例。
- [MyDriving](https://azure.microsoft.com/campaigns/mydriving/) = Azure + IoT 示例来自 2016 年生成。

## <a name="related-links"></a>相关链接

- [Azure PCL 示例@paulbatum（按 ） （示例）](https://github.com/paulbatum/mobile-services-xamarin-pcl)
- [Azure 门户](https://azure.microsoft.com/)
- [Xamarin 的移动客户端 （NuGet）](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
