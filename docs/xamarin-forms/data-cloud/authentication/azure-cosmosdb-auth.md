---
title: 使用 Azure Cosmos DB 文档数据库和 Xamarin 验证用户。
description: 本文介绍如何将访问控制与 Azure Cosmos DB 分区集合相结合，以便用户只能在 Xamarin.Forms 应用程序中访问自己的文档。
ms.prod: xamarin
ms.assetid: 11ED4A4C-0F05-40B2-AB06-5A0F2188EF3D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2017
ms.openlocfilehash: 6e79e647d64103b6d257de7233f488899bcaff40
ms.sourcegitcommit: 2a8eb8bce427e72d4e7edd06ce432e19f17dcdd7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2020
ms.locfileid: "80388598"
---
# <a name="authenticate-users-with-an-azure-cosmos-db-document-database-and-xamarinforms"></a>使用 Azure Cosmos DB 文档数据库和 Xamarin 验证用户。

[![下载示例](~/media/shared/download.png)下载示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-tododocumentdbauth)

_Azure Cosmos DB 文档数据库支持分区集合，该集合可以跨多个服务器和分区，同时支持无限的存储和吞吐量。本文介绍如何将访问控制与分区集合相结合，以便用户只能在 Xamarin.Forms 应用程序中访问自己的文档。_

## <a name="overview"></a>概述

创建分区集合时必须指定分区键，具有相同分区键的文档将存储在同一分区中。 因此，将用户标识指定为分区键将导致分区集合仅存储该用户的文档。 这还可确保 Azure Cosmos DB 文档数据库将随着用户和项数量的增加而扩展。

必须授予任何集合访问权限，SQL API 访问控制模型定义两种类型的访问构造：

- **主密钥**允许对 Cosmos DB 帐户中的所有资源进行完全管理访问，并在创建 Cosmos DB 帐户时创建。
- **资源令牌**捕获数据库用户与用户对特定 Cosmos DB 资源（如集合或文档）的权限之间的关系。

公开主密钥会打开 Cosmos DB 帐户，使其有可能恶意或疏忽使用。 但是，Azure Cosmos DB 资源令牌提供了一种安全机制，允许客户端根据授予的权限读取、写入和删除 Azure Cosmos DB 帐户中的特定资源。

请求、生成和向移动应用程序传递资源令牌的典型方法是使用资源令牌代理。 下图显示了示例应用程序如何使用资源令牌代理来管理对文档数据库数据的访问的高级概述：

![](azure-cosmosdb-auth-images/documentdb-authentication.png "Document Database Authentication Process")

资源令牌代理是一个中端 Web API 服务，托管在 Azure 应用服务中，该服务具有 Cosmos DB 帐户的主密钥。 示例应用程序使用资源令牌代理管理对文档数据库数据的访问，如下所示：

1. 登录时，Xamarin.Forms 应用程序会与 Azure 应用服务联系以启动身份验证流。
1. Azure 应用服务使用 Facebook 执行 OAuth 身份验证流。 身份验证流完成后，Xamarin.Forms 应用程序将接收访问令牌。
1. Xamarin.Forms 应用程序使用访问令牌从资源令牌代理请求资源令牌。
1. 资源令牌代理使用访问令牌从 Facebook 请求用户的身份。 然后，用户的身份用于从 Cosmos DB 请求资源令牌，该令牌用于授予对经过身份验证的用户分区集合的读/写访问权限。
1. Xamarin.Forms 应用程序使用资源令牌直接访问 Cosmos DB 资源，并具有资源令牌定义的权限。

> [!NOTE]
> 当资源令牌过期时，后续文档数据库请求将收到 401 未经授权的异常。 此时，Xamarin.Forms 应用程序应重新建立标识并请求新的资源令牌。

有关 Cosmos DB 分区的详细信息，请参阅[如何在 Azure Cosmos DB 中分区和缩放](/azure/cosmos-db/partition-data/)。 有关 Cosmos DB 访问控制的详细信息，请参阅在 SQL API 中[保护对 Cosmos DB 数据和](/azure/cosmos-db/secure-access-to-data/)[访问控件](/rest/api/documentdb/access-control-on-documentdb-resources/)的访问。

## <a name="setup"></a>设置

将资源令牌代理集成到 Xamarin.Forms 应用程序的过程如下所示：

1. 创建将使用访问控制的 Cosmos DB 帐户。 有关详细信息，请参阅[宇宙数据库配置](#cosmosdb_configuration)。
1. 创建 Azure 应用服务以承载资源令牌代理。 有关详细信息，请参阅[Azure 应用服务配置](#app_service_configuration)。
1. 创建 Facebook 应用以执行身份验证。 有关详细信息，请参阅[Facebook 应用程序配置](#facebook_configuration)。
1. 将 Azure 应用服务配置为对 Facebook 执行简单的身份验证。 有关详细信息，请参阅[Azure 应用服务身份验证配置](#app_service_authentication_configuration)。
1. 配置 Xamarin.Forms 示例应用程序以与 Azure 应用服务和 Cosmos DB 进行通信。 有关详细信息，请参阅[Xamarin.窗体应用程序配置](#forms_application_configuration)。

> [!NOTE]
> 如果没有[Azure 订阅](/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing)，请先创建[一个免费帐户](https://aka.ms/azfree-docs-mobileapps)。然后开始。

<a name="cosmosdb_configuration" />

### <a name="azure-cosmos-db-configuration"></a>Azure 宇宙数据库配置

创建将使用访问控制的 Cosmos DB 帐户的过程如下所示：

1. 创建 Cosmos 数据库帐户。 有关详细信息，请参阅创建[Azure 宇宙数据库帐户](/azure/cosmos-db/sql-api-dotnetcore-get-started#step-1-create-an-azure-cosmos-db-account)。
1. 在 Cosmos DB 帐户中，创建名为`UserItems`的新集合，指定`/userid`的分区键。

<a name="app_service_configuration" />

### <a name="azure-app-service-configuration"></a>Azure 应用服务配置

在 Azure 应用服务中托管资源令牌代理的过程如下所示：

1. 在 Azure 门户中，创建新的应用服务 Web 应用。 有关详细信息，请参阅[在应用服务环境中创建 Web 应用](/azure/app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase/)。
1. 在 Azure 门户中，打开 Web 应用的应用设置边栏选项卡，并添加以下设置：
    - `accountUrl`• 该值应是来自 Cosmos DB 帐户的"密钥"边栏选项卡的 Cosmos DB 帐户 URL。
    - `accountKey`• 该值应是来自 Cosmos DB 帐户的 Keys 边栏选项卡的 Cosmos DB 主密钥（主密钥或辅助密钥）。
    - `databaseId`• 该值应为 Cosmos DB 数据库的名称。
    - `collectionId`• 该值应为 Cosmos DB 集合的名称（在本例中`UserItems`为 ）。
    - `hostUrl`• 该值应是应用服务帐户的"概述"选项卡中的 Web 应用的 URL。

    以下屏幕截图演示了此配置：

    [![](azure-cosmosdb-auth-images/azure-web-app-settings.png "App Service Web App Settings")](azure-cosmosdb-auth-images/azure-web-app-settings-large.png#lightbox "App Service Web App Settings")

1. 将资源令牌代理解决方案发布到 Azure 应用服务 Web 应用。

<a name="facebook_configuration" />

### <a name="facebook-app-configuration"></a>Facebook 应用程序配置

创建 Facebook 应用以执行身份验证的过程如下：

1. 创建 Facebook 应用。 有关详细信息，请参阅在 Facebook 开发人员中心[注册和配置应用](https://developers.facebook.com/docs/apps/register)。
1. 将 Facebook 登录产品添加到应用。 有关详细信息，请参阅在 Facebook 开发人员中心[将 Facebook 登录添加到您的应用或网站](https://developers.facebook.com/docs/facebook-login)。
1. 按如下方式配置 Facebook 登录：
   - 启用客户端 OAuth 登录名。
   - 启用 Web OAuth 登录。
   - 将有效 OAuth 重定向到应用服务 Web 应用的 URI，并`/.auth/login/facebook/callback`附加。

  以下屏幕截图演示了此配置：

  ![](azure-cosmosdb-auth-images/facebook-oauth-settings.png "Facebook Login OAuth Settings")

有关详细信息，请参阅在[Facebook 注册您的应用程序](/azure/app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication#a-nameregister-aregister-your-application-with-facebook)。

<a name="app_service_authentication_configuration" />

### <a name="azure-app-service-authentication-configuration"></a>Azure 应用服务身份验证配置

配置应用服务简单身份验证的过程如下：

1. 在 Azure 门户中，导航到应用服务 Web 应用。
1. 在 Azure 门户中，打开身份验证/授权边栏选项卡并执行以下配置：
    - 应用服务身份验证应打开。
    - 请求未经过身份验证时执行的操作应设置为**使用 Facebook 登录**。

    以下屏幕截图演示了此配置：

    [![](azure-cosmosdb-auth-images/app-service-authentication-settings.png "App Service Web App Authentication Settings")](azure-cosmosdb-auth-images/app-service-authentication-settings-large.png#lightbox "App Service Web App Authentication Settings")

应用服务 Web 应用也应配置为与 Facebook 应用通信，以启用身份验证流。 这可以通过选择 Facebook 标识提供商，并从 Facebook 开发人员中心的 Facebook 应用设置中输入**应用 ID** **和应用机密**值来实现。 有关详细信息，请参阅将[Facebook 信息添加到您的应用程序](/azure/app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication#a-namesecrets-aadd-facebook-information-to-your-application)。

<a name="forms_application_configuration" />

### <a name="xamarinforms-application-configuration"></a>Xamarin.窗体应用程序配置

配置 Xamarin.Forms 示例应用程序的过程如下：

1. 打开 Xamarin.窗体解决方案。
1. 打开`Constants.cs`并更新以下常量的值：
    - `EndpointUri`• 该值应是来自 Cosmos DB 帐户的"密钥"边栏选项卡的 Cosmos DB 帐户 URL。
    - `DatabaseName`• 该值应为文档数据库的名称。
    - `CollectionName`• 值应为文档数据库集合的名称（在本例中为`UserItems`）。
    - `ResourceTokenBrokerUrl`• 该值应是应用服务帐户的"概述"选项卡中资源令牌代理 Web 应用的 URL。

## <a name="initiating-login"></a>启动登录

示例应用程序通过将浏览器重定向到标识提供程序 URL 来启动登录过程，如以下示例代码所示：

```csharp
var auth = new Xamarin.Auth.WebRedirectAuthenticator(
  new Uri(Constants.ResourceTokenBrokerUrl + "/.auth/login/facebook"),
  new Uri(Constants.ResourceTokenBrokerUrl + "/.auth/login/done"));
```

这将导致在 Azure 应用服务和 Facebook 之间启动 OAuth 身份验证流，后者显示 Facebook 登录页：

![](azure-cosmosdb-auth-images/login.png "Facebook Login")

可以通过按 iOS 上的 **"取消"** 按钮或按 Android 上的 **"后退**"按钮来取消登录，在这种情况下，用户将保持未通过身份验证，并且标识提供程序用户界面将从屏幕上删除。

## <a name="obtaining-a-resource-token"></a>获取资源令牌

成功身份验证后，事件`WebRedirectAuthenticator.Completed`将触发。 以下代码示例演示了处理此事件：

```csharp
auth.Completed += async (sender, e) =>
{
  if (e.IsAuthenticated && e.Account.Properties.ContainsKey("token"))
  {
    var easyAuthResponseJson = JsonConvert.DeserializeObject<JObject>(e.Account.Properties["token"]);
    var easyAuthToken = easyAuthResponseJson.GetValue("authenticationToken").ToString();

    // Call the ResourceBroker to get the resource token
    using (var httpClient = new HttpClient())
    {
      httpClient.DefaultRequestHeaders.Add("x-zumo-auth", easyAuthToken);
      var response = await httpClient.GetAsync(Constants.ResourceTokenBrokerUrl + "/api/resourcetoken/");
      var jsonString = await response.Content.ReadAsStringAsync();
      var tokenJson = JsonConvert.DeserializeObject<JObject>(jsonString);
      resourceToken = tokenJson.GetValue("token").ToString();
      UserId = tokenJson.GetValue("userid").ToString();

      if (!string.IsNullOrWhiteSpace(resourceToken))
      {
        client = new DocumentClient(new Uri(Constants.EndpointUri), resourceToken);
        ...
      }
      ...
    }
  }
};
```

成功身份验证的结果是访问令牌，它是可用的`AuthenticatorCompletedEventArgs.Account`属性。 访问令牌被提取并用于资源令牌代理的`resourcetoken`API 的 GET 请求。

`resourcetoken` API 使用访问令牌从 Facebook 请求用户的身份，而 Facebook 又用于从 Cosmos DB 请求资源令牌。 如果文档数据库中已存在用户的有效权限文档，则检索该文档，并且包含资源令牌的 JSON 文档将返回到 Xamarin.Forms 应用程序。 如果用户不存在有效的权限文档，则在文档数据库中创建用户和权限，并从权限文档中提取资源令牌并返回到 JSON 文档中的 Xamarin.Forms 应用程序。

> [!NOTE]
> 文档数据库用户是与文档数据库关联的资源，每个数据库可能包含零个或多个用户。 文档数据库权限是与文档数据库用户关联的资源，每个用户可能包含零个或多个权限。 权限资源提供对用户在尝试访问资源（如文档）时所需的安全令牌的访问。

如果`resourcetoken`API 成功完成，它将在响应中发送 HTTP 状态代码 200 （OK）以及包含资源令牌的 JSON 文档。 以下 JSON 数据显示了典型的成功响应消息：

```csharp
{
  "id": "John Smithpermission",
  "token": "type=resource&ver=1&sig=zx6k2zzxqktzvuzuku4b7y==;a74aukk99qtwk8v5rxfrfz7ay7zzqfkbfkremrwtaapvavw2mrvia4umbi/7iiwkrrq+buqqrzkaq4pp15y6bki1u//zf7p9x/aefbvqvq3tjjqiffurfx+vexa1xarxkkv9rbua9ypfzr47xpp5vmxuvzbekkwq6txme0xxxbjhzaxbkvzaji+iru3xqjp05amvq1r1q2k+qrarurhmjzah/ha0evixazkve2xk1zu9u/jpyf1xrwbkxqpzebvqwma+hyyaazemr6qx9uz9be==;",
  "expires": 4035948,
  "userid": "John Smith"
}
```

事件`WebRedirectAuthenticator.Completed`处理程序从`resourcetoken`API 读取响应，并提取资源令牌和用户 ID。然后，资源令牌作为参数传递给`DocumentClient`构造函数，该构造函数封装了用于访问 Cosmos DB 的终结点、凭据和连接策略，并用于配置和执行针对 Cosmos DB 的请求。 资源令牌随每个请求一起发送以直接访问资源，并指示授予对经过身份验证的用户分区集合的读/写访问权限。

## <a name="retrieving-documents"></a>检索文档

通过创建包含用户 ID 作为分区键的文档查询，可以检索仅属于经过身份验证的用户的文档，并在以下代码示例中演示：

```csharp
var query = client.CreateDocumentQuery<TodoItem>(collectionLink,
                        new FeedOptions
                        {
                          MaxItemCount = -1,
                          PartitionKey = new PartitionKey(UserId)
                        })
          .Where(item => !item.Id.Contains("permission"))
          .AsDocumentQuery();
while (query.HasMoreResults)
{
  Items.AddRange(await query.ExecuteNextAsync<TodoItem>());
}
```

查询异步检索来自指定集合的所有属于经过身份验证的用户的文档，并将它们放在集合中`List<TodoItem>`以显示。

方法`CreateDocumentQuery<T>`指定一个`Uri`参数，该参数表示应查询文档和`FeedOptions`对象的集合。 该`FeedOptions`对象指定查询可以返回无限数量的项，并将用户的 ID 作为分区键返回。 这可确保在结果中仅返回用户分区集合中的文档。

> [!NOTE]
> 请注意，由资源令牌代理创建的权限文档与 Xamarin.Forms 应用程序创建的文档存储在相同的文档集合中。 因此，文档查询包含一个`Where`子句，该子句将筛选谓词应用于针对文档集合的查询。 此子句可确保不会从文档集合返回权限文档。

有关从文档集合检索文档的详细信息，请参阅[检索文档集合文档](~/xamarin-forms/data-cloud/azure-services/azure-cosmosdb.md#document_query)。

## <a name="inserting-documents"></a>插入文档

在将文档插入到文档集合中之前，应使用`TodoItem.UserId`用作分区键的值更新该属性，如下代码示例所示：

```csharp
item.UserId = UserId;
await client.CreateDocumentAsync(collectionLink, item);
```

这可确保将文档插入到用户的分区集合中。

有关将文档插入文档集合的详细信息，请参阅[将文档插入文档集合](~/xamarin-forms/data-cloud/azure-services/azure-cosmosdb.md#inserting_document)。

## <a name="deleting-documents"></a>删除文档

从分区集合中删除文档时必须指定分区键值，如下代码示例所示：

```csharp
await client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(Constants.DatabaseName, Constants.CollectionName, id),
                 new RequestOptions
                 {
                   PartitionKey = new PartitionKey(UserId)
                 });
```

这可确保 Cosmos DB 知道要从哪个分区集合中删除文档。

有关从文档集合中删除文档的详细信息，请参阅[从文档集合中删除文档](~/xamarin-forms/data-cloud/azure-services/azure-cosmosdb.md#deleting_document)。

## <a name="summary"></a>总结

本文介绍了如何将访问控制与分区集合相结合，以便用户只能在 Xamarin.Forms 应用程序中访问自己的文档数据库文档。 将用户标识指定为分区键可确保分区集合只能为该用户存储文档。

## <a name="related-links"></a>相关链接

- [Do Azure 宇宙 DB Auth（示例）](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-tododocumentdbauth)
- [使用 Azure Cosmos DB 文档数据库](~/xamarin-forms/data-cloud/azure-services/azure-cosmosdb.md)
- [保护对 Azure Cosmos DB 数据的访问](/azure/cosmos-db/secure-access-to-data/)
- [SQL API 中的访问控制](/rest/api/documentdb/access-control-on-documentdb-resources/)。
- [如何在 Azure Cosmos DB 中分区和缩放](/azure/cosmos-db/partition-data/)
- [Azure 宇宙数据库客户端库](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core)
- [Azure Cosmos DB API](https://msdn.microsoft.com/library/azure/dn948556.aspx)
