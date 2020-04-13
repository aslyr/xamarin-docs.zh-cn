---
title: Xamarin.窗体和 Web 服务
description: 本指南介绍了如何与不同的 Web 服务通信，以便向 Xamarin.Forms 应用程序提供创建、读取、更新和删除 （CRUD） 功能。 涵盖的主题包括与 ASMX 服务、WCF 服务、REST 服务通信。
ms.prod: xamarin
ms.assetid: 8B360BDA-E4E3-4A3F-9004-0E35362F49F8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/27/2019
ms.openlocfilehash: 799a0a97f7c1a212b10dc62f8b3e7bd2cf2060c8
ms.sourcegitcommit: bf3b5925018e1bd6a00505e9f37500761b66809d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2020
ms.locfileid: "80395461"
---
# <a name="xamarinforms-and-web-services"></a>Xamarin.窗体和 Web 服务

## <a name="introduction"></a>[简介](introduction.md)

本文提供了 Xamarin.Forms 示例应用程序的演练，演示如何与不同的 Web 服务进行通信。 涵盖的主题包括应用程序的剖析、页面、数据模型和调用 Web 服务操作。

## <a name="consume-an-aspnet-web-service-asmx"></a>[使用 ASP.NET Web 服务 (ASMX)](~/xamarin-forms/data-cloud/web-services/asmx.md)

ASP.NET Web 服务 （ASMX） 提供使用简单对象访问协议 （SOAP） 通过 HTTP 生成消息的 Web 服务。 SOAP 是一个独立于平台且独立于语言的协议，用于构建和访问 Web 服务。 ASMX 服务的使用者不需要了解用于实现该服务的平台、对象模型或编程语言。 他们只需要了解如何发送和接收 SOAP 消息。 本文演示如何使用 Xamarin.Forms 应用程序中的 ASMX Web 服务。

## <a name="consume-a-windows-communication-foundation-wcf-web-service"></a>[使用 Windows 通信基础 （WCF） Web 服务](~/xamarin-forms/data-cloud/web-services/wcf.md)

WCF 是 Microsoft 构建面向服务的应用程序的统一框架。 它使开发人员能够构建安全、可靠、事务和可互操作的分布式应用程序。 ASP.NET Web 服务 （ASMX） 和 WCF 之间存在差异，但请务必了解，WCF 支持与 ASMX 提供的相同功能 – 通过 HTTP 提供 SOAP 消息。 本文演示如何使用 Xamarin.Forms 应用程序中的 WCF SOAP 服务。

## <a name="consume-a-restful-web-service"></a>[使用 RESTful Web 服务](~/xamarin-forms/data-cloud/web-services/rest.md)

表示状态传输 （REST） 是构建 Web 服务的体系结构样式。 REST 请求通过 HTTP 使用 Web 浏览器用于检索网页和将数据发送到服务器的相同 HTTP 谓词。 本文演示如何使用 Xamarin.Forms 应用程序中的 RESTful Web 服务。
