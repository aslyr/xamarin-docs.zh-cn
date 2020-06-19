---
title: Xamarin.Forms和 Web 服务
description: 本指南说明如何与不同的 web 服务进行通信，以便为应用程序提供创建、读取、更新和删除（CRUD）功能 Xamarin.Forms 。 涉及的主题包括与 .ASMX 服务、WCF 服务、REST 服务通信。
ms.prod: xamarin
ms.assetid: 8B360BDA-E4E3-4A3F-9004-0E35362F49F8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/27/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 5b2613b94d2c347d9bc6a94086f869b07ab8a55b
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/18/2020
ms.locfileid: "84131880"
---
# <a name="xamarinforms-and-web-services"></a>Xamarin.Forms和 Web 服务

## <a name="introduction"></a>[简介](introduction.md)

本文提供了 Xamarin.Forms 演示如何与不同 web 服务通信的示例应用程序的演练。 涉及的主题包括应用程序解析、页面、数据模型和调用 web 服务操作。

## <a name="consume-an-aspnet-web-service-asmx"></a>[使用 ASP.NET Web 服务 (ASMX)](~/xamarin-forms/data-cloud/web-services/asmx.md)

ASP.NET Web Services （.ASMX）提供使用简单对象访问协议（SOAP）构建通过 HTTP 发送消息的 web 服务的功能。 SOAP 是一种独立于平台和语言的协议，用于生成和访问 web 服务。 对于用于实现服务的平台、对象模型或编程语言，使用该服务的使用者不需要了解有关该服务的任何信息。 它们只需要了解如何发送和接收 SOAP 消息。 本文演示如何从应用程序使用 .ASMX web 服务 Xamarin.Forms 。

## <a name="consume-a-windows-communication-foundation-wcf-web-service"></a>[使用 Windows Communication Foundation （WCF） Web 服务](~/xamarin-forms/data-cloud/web-services/wcf.md)

WCF 是 Microsoft 的统一框架，用于生成面向服务的应用程序。 它使开发人员能够构建安全、可靠、可互操作的分布式应用程序。 ASP.NET Web Services （.ASMX）和 WCF 之间存在差异，但请务必了解 WCF 支持的功能与通过 HTTP 的 SOAP 消息相同。 本文演示如何从应用程序中使用 WCF SOAP 服务 Xamarin.Forms 。

## <a name="consume-a-restful-web-service"></a>[使用 RESTful Web 服务](~/xamarin-forms/data-cloud/web-services/rest.md)

具象状态传输（REST）是用于构建 web 服务的体系结构样式。 REST 请求是通过 HTTP 使用 web 浏览器用于检索网页并将数据发送到服务器的 HTTP 谓词来完成的。 本文演示如何从应用程序使用 RESTful web 服务 Xamarin.Forms 。
