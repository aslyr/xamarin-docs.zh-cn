---
title: Xamarin.窗体 Web 服务身份验证
description: 本指南介绍如何将身份验证服务集成到 Xamarin.Forms 应用程序中，使用户能够共享后端，同时仅有权访问自己的数据。
ms.prod: xamarin
ms.assetid: E6FCFAE1-4F83-4F93-9190-EC5290360C54
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/27/2019
ms.openlocfilehash: c3ad061953b08ef12bea7b6a63b7c39bbd89a5f6
ms.sourcegitcommit: 2a8eb8bce427e72d4e7edd06ce432e19f17dcdd7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2020
ms.locfileid: "80388572"
---
# <a name="xamarinforms-web-service-authentication"></a>Xamarin.窗体 Web 服务身份验证

## <a name="authenticate-a-restful-web-service"></a>[验证 RESTful Web 服务](rest.md)

HTTP 支持使用多种身份验证机制来控制对资源的访问。 基本身份验证仅向具有正确凭据的客户端提供对资源的访问权限。 本文介绍如何使用基本身份验证来保护对 RESTful Web 服务资源的访问。

## <a name="authenticate-users-with-azure-active-directory-b2c"></a>[使用 Azure Active Directory B2C 对用户进行身份验证](azure-ad-b2c.md)

Azure Active Directory B2C 是一种云标识管理解决方案，适用于消费型 Web 和移动应用程序。 本文介绍如何使用 Microsoft 身份验证库 （MSAL） 和 Azure 活动目录 B2C 将使用者标识管理集成到 Xamarin.Forms 应用程序中。

## <a name="authenticate-users-with-an-azure-cosmos-db-document-database-and-xamarinforms"></a>[使用 Azure Cosmos DB 文档数据库和 Xamarin 验证用户。](azure-cosmosdb-auth.md)

Azure Cosmos DB 文档数据库支持分区集合，该集合可以跨多个服务器和分区，同时支持无限的存储和吞吐量。 本文介绍如何将访问控制与分区集合相结合，以便用户只能在 Xamarin.Forms 应用程序中访问自己的文档。
