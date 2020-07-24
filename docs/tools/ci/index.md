---
title: 与 Xamarin 持续集成
description: 本文档链接到介绍与 Xamarin 持续集成的指南。 链接内容提供持续集成的概述，并讨论 App Center Build、TeamCity 和 Jenkins。
ms.prod: xamarin
ms.assetid: 99484E96-DC69-4697-8BBB-1B44C5CBB5ED
author: davidortinau
ms.author: daortin
ms.date: 10/23/2018
ms.openlocfilehash: 9c87a65481ca58b2861c40a420459d629852f6b6
ms.sourcegitcommit: 952db1983c0bc373844c5fbe9d185e04a87d8fb4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/23/2020
ms.locfileid: "86997418"
---
# <a name="continuous-integration-with-xamarin"></a>与 Xamarin 持续集成

> [!Video https://youtube.com/embed/wXgnh2Q7Uv8]

## <a name="introduction-to-continuous-integration"></a>[持续集成简介](~/tools/ci/intro-to-ci.md)

本部分介绍持续集成和它们之间的关系所涉及的不同组件。 它概述了下面特定部分中讨论的持续集成环境。

## <a name="devops-with-xamarin"></a>[带 Xamarin 的 DevOps](~/tools/ci/devops.md)

本部分介绍 Azure 和 Visual Studio 中的哪些 DevOps 功能，你可以正常使用 Xamarin 项目。

## <a name="working-with-continuous-integration-environments"></a>使用持续集成环境

### <a name="build-xamarin-apps-with-azure-pipelines"></a>[生成包含 Azure Pipelines 的 Xamarin 应用](https://docs.microsoft.com/azure/devops/pipelines/languages/xamarin/)

使用 Azure Pipelines 自动生成适用于 Android 和 iOS 的 Xamarin 应用。

### <a name="build-xamarin-apps-using-app-center"></a>[使用 App Center 生成 Xamarin 应用](https://docs.microsoft.com/appcenter/build/xamarin/)

通过 App Center 直接从 GitHub、Azure DevOps 或 Bitbucket 生成 Xamarin 和 Xamarin Android 解决方案。

### <a name="build-xamarin-apps-with-teamcity"></a>[通过 TeamCity 生成 Xamarin 应用](~/tools/ci/teamcity.md)

本指南讨论了使用 TeamCity 编译移动应用，并将其提交到 App Center 测试所涉及的步骤。

### <a name="build-xamarin-apps-with-jenkins"></a>[通过 Jenkins 生成 Xamarin 应用](~/tools/ci/jenkins-walkthrough.md)

本指南说明如何将 Jenkins 设置为持续集成服务器，并自动编译使用 Xamarin 创建的移动应用。 它介绍了如何在 OS X 上安装 Jenkins，如何对其进行配置，以及如何设置在将更改提交到版本控制系统时编译 Xamarin 和 Xamarin Android 应用程序的作业。
