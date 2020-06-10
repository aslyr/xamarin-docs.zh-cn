---
标题： " Xamarin.Forms Web 服务身份验证" 说明： "本指南说明如何将身份验证服务集成到 Xamarin.Forms 应用程序中，使用户能够在仅有权访问自己的数据的情况下共享后端。"
ms-chap： xamarin assetid： E6FCFAE1-4F83-4F93-9190-EC5290360C54： xamarin 窗体作者： davidbritch： dabritch ms. 日期：06/27/2019 非 loc： [ Xamarin.Forms ， Xamarin.Essentials ]
---

# <a name="xamarinforms-web-service-authentication"></a>Xamarin.FormsWeb 服务身份验证

## <a name="authenticate-a-restful-web-service"></a>[对 RESTful Web 服务进行身份验证](rest.md)

HTTP 支持使用多种身份验证机制来控制对资源的访问。 基本身份验证仅向具有正确凭据的客户端提供对资源的访问权限。 本文介绍如何使用基本身份验证来保护对 RESTful web 服务资源的访问。

## <a name="authenticate-users-with-azure-active-directory-b2c"></a>[使用 Azure Active Directory B2C 对用户进行身份验证](azure-ad-b2c.md)

Azure Active Directory B2C 是一种云标识管理解决方案，适用于消费型 Web 和移动应用程序。 本文介绍如何使用 Microsoft 身份验证库（MSAL），并 Azure Active Directory B2C 将消费者标识管理集成到 Xamarin.Forms 应用程序中。

## <a name="authenticate-users-with-an-azure-cosmos-db-document-database-and-xamarinformsazure-cosmosdb-authmd"></a>[使用 Azure Cosmos DB 文档数据库对用户进行身份验证并Xamarin.Forms](azure-cosmosdb-auth.md)

Azure Cosmos DB 文档数据库支持分区集合，这些集合可以跨多个服务器和分区，同时支持无限制的存储和吞吐量。 本文介绍如何结合使用访问控制和分区集合，使用户只能在应用程序中访问他们自己的文档 Xamarin.Forms 。
