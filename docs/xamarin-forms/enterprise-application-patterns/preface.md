---
title: 到企业应用开发
description: 本章使用来提供企业应用程序模式的前缀 Xamarin.Forms 。
ms.prod: xamarin
ms.assetid: fbf32a44-1d33-4e16-a904-dc7ee5991e7c
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 6ea63fc483025fc6f9b0c7f379b6dfdc6ca30de8
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/18/2020
ms.locfileid: "84198088"
---
# <a name="preface-to-enterprise-app-development"></a>到企业应用开发

此电子书提供有关使用生成跨平台企业应用程序的指导 Xamarin.Forms 。 Xamarin.Forms是一个跨平台 UI 工具包，使开发人员能够轻松创建可在各种平台（包括 iOS、Android 和通用 Windows 平台（UWP））之间共享的本机用户界面布局。 它为企业到员工（B2E）、企业到企业（B2B）和企业对消费者（B2C）应用提供综合性解决方案，提供跨所有目标平台共享代码的功能，并有助于降低总拥有成本（TCO）。

本指南提供了开发可自适应、可维护且可测试的企业应用程序的体系结构指南 Xamarin.Forms 。 本指南提供了如何实现 MVVM、依赖关系注入、导航、验证和配置管理，同时保持松散耦合。 此外，还提供了有关通过 IdentityServer 执行身份验证和授权、从容器化微服务访问数据和单元测试的指南。

本指南附带了[eShopOnContainers 移动应用程序](https://github.com/dotnet-architecture/eShopOnContainers/tree/master/src/Mobile)的源代码和[eShopOnContainers reference 应用程序](https://github.com/dotnet-architecture/eShopOnContainers)的源代码。 EShopOnContainers 移动应用是使用开发的跨平台企业应用 Xamarin.Forms ，它连接到一系列称为 eShopOnContainers reference 应用的容器化微服务。 但是，可以将 eShopOnContainers 移动应用配置为使用模拟服务中的数据，以避免部署容器化微服务。

## <a name="whats-left-out-of-this-guides-scope"></a>本指南范围的剩余内容

本指南面向已熟悉的读者 Xamarin.Forms 。 有关的详细介绍 Xamarin.Forms ，请参阅[ Xamarin.Forms 文档](~/xamarin-forms/index.yml)和[ Xamarin.Forms 使用创建移动应用](https://aka.ms/xamformsebook)。

本指南是[针对容器化 .Net 应用程序的 .Net 微服务体系结构](https://aka.ms/microservicesebook)的补充，重点介绍如何开发和部署容器化微服务。 其他一些需要阅读的指南包括：利用[ASP.NET Core 和 Microsoft Azure 构建和开发新式 Web 应用](https://aka.ms/WebAppEbook)程序，以及[Microsoft 平台和工具的容器化 Docker 应用程序生命周期](https://aka.ms/dockerlifecycleebook)，以及[用于移动应用程序开发的 microsoft 平台和工具](https://aka.ms/MobAppDev/StndPDF)。

## <a name="who-should-use-this-guide"></a>应使用本指南的人员

本指南的受众主要是希望了解如何使用构建和实现跨平台企业应用程序的开发人员和架构师 Xamarin.Forms 。

辅助受众是技术决策者，他们想要在决定使用哪种方法来进行跨平台的企业应用开发之前获得体系结构和技术概述 Xamarin.Forms 。

## <a name="how-to-use-this-guide"></a>本指南的使用方式

本指南重点介绍如何使用生成跨平台的企业应用 Xamarin.Forms 。 因此，应将其完整地读入，以便为理解此类应用及其技术注意事项打下基础。 指南及其示例应用还可用作创建新的企业应用的起点或参考。 使用关联的示例应用作为新应用的模板，或了解如何组织应用的组件部件。 然后，请返回本指南了解体系结构指南。

欢迎将本指南转发给团队成员，以帮助确保对使用的跨平台企业应用程序开发的共同了解 Xamarin.Forms 。 让每个人都在使用一组通用的术语和基础原则时，将有助于确保一致地应用体系结构模式和实践。

## <a name="related-links"></a>相关链接

- [下载电子书（2Mb）](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers （GitHub）（示例）](https://github.com/dotnet-architecture/eShopOnContainers)
