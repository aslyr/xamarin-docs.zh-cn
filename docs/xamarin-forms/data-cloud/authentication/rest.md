---
title: 验证 RESTful Web 服务
description: 基本身份验证仅向具有正确凭据的客户端提供对资源的访问权限。 本文介绍如何使用基本身份验证来保护对 RESTful Web 服务资源的访问。
ms.prod: xamarin
ms.assetid: 7B5FFDC4-F2AA-4B12-A30A-1DACC7FECBF1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/22/2018
ms.openlocfilehash: 65606b72c3944809a09b8c70b9cdbb5524e0f856
ms.sourcegitcommit: 2a8eb8bce427e72d4e7edd06ce432e19f17dcdd7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2020
ms.locfileid: "80388585"
---
# <a name="authenticate-a-restful-web-service"></a>验证 RESTful Web 服务

_HTTP 支持使用多种身份验证机制来控制对资源的访问。基本身份验证仅向具有正确凭据的客户端提供对资源的访问权限。本文演示如何使用基本身份验证来保护对 RESTful Web 服务资源的访问。_

> [!NOTE]
> 在 iOS 9 及更高级别中，应用传输安全 （ATS） 强制 Internet 资源（如应用的后端服务器）与应用之间的安全连接，从而防止敏感信息意外泄露。 由于默认情况下，在为 iOS 9 构建的应用中启用了 ATS，因此所有连接都必须遵守 ATS 安全要求。 如果连接不符合这些要求，它们将失败，但有一个例外。
> 如果无法使用该协议和安全通信来获取互联网资源，`HTTPS`则可以选择退出 ATS。 这可以通过更新应用的**Info.plist**文件来实现。 有关详细信息，请参阅[应用传输安全](~/ios/app-fundamentals/ats.md)。

## <a name="authenticating-users-over-http"></a>通过 HTTP 对用户进行身份验证

基本身份验证是 HTTP 支持的最简单的身份验证机制，涉及客户端将用户名和密码作为未加密的 base64 编码文本发送。 此功能的工作原理如下：

- 如果 Web 服务收到受保护资源的请求，它将使用 HTTP 状态代码 401（访问被拒绝）拒绝请求，并设置 WWW-身份验证响应标头，如下图所示：

![](rest-images/basic-authentication-fail.png "Basic Authentication Failing")

- 如果 Web 服务收到受保护资源的请求，并且`Authorization`标头设置正确，则 Web 服务会使用 HTTP 状态代码 200 进行响应，该状态代码 200 指示请求成功，并且请求的信息在响应中。 此方案显示在下图中：

![](rest-images/basic-authentication-success.png "Basic Authentication Succeeding")

> [!NOTE]
> 基本身份验证应仅在 HTTPS 连接上使用。 当通过 HTTP 连接使用时`Authorization`，如果攻击者捕获了 HTTP 流量，则可以轻松解码标头。

## <a name="specifying-basic-authentication-in-a-web-request"></a>在 Web 请求中指定基本身份验证

基本身份验证的使用指定如下：

1. 字符串"基本"将添加到请求的`Authorization`标头中。
1. 用户名和密码合并到格式为"用户名：密码"的字符串中，然后对 base64 进行编码并添加到请求`Authorization`的标头中。

因此，使用"XamarinUser"的用户名和"XamarinPassword"的密码，标头变为：

```csharp
Authorization: Basic WGFtYXJpblVzZXI6WGFtYXJpblBhc3N3b3Jk
```

类`HttpClient`可以在`Authorization``HttpClient.DefaultRequestHeaders.Authorization`属性上设置标头值。 由于`HttpClient`实例存在于多个请求中，`Authorization`因此标头只需设置一次，而不是在发出每个请求时设置一次，如以下代码示例所示：

```csharp
public class RestService : IRestService
{
  HttpClient _client;
  ...

  public RestService ()
  {
    var authData = string.Format ("{0}:{1}", Constants.Username, Constants.Password);
    var authHeaderValue = Convert.ToBase64String (Encoding.UTF8.GetBytes (authData));

    _client = new HttpClient ();
    _client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue ("Basic", authHeaderValue);
  }
  ...
}
```

然后，当请求 Web 服务操作时，请求用`Authorization`标头签名，指示用户是否具有调用该操作的权限。

> [!IMPORTANT]
> 虽然此代码将凭据存储为常量，但它们不应以不安全的格式存储在已发布的应用程序中。

## <a name="processing-the-authorization-header-server-side"></a>处理授权标头服务器端

REST 服务应使用 属性`[BasicAuthentication]`修饰每个操作。 此属性用于分析`Authorization`标头，并通过将 base64 编码凭据与存储在*Web.config*中的值进行比较来确定其是否有效。虽然此方法适用于示例服务，但它需要扩展面向公众的 Web 服务。

在 IIS 使用的基本身份验证模块中，用户会根据 Windows 凭据进行身份验证。 因此，用户必须在服务器的域上具有帐户。 但是，基本身份验证模型可以配置为允许自定义身份验证，其中用户帐户对外部源（如数据库）进行身份验证。 有关详细信息，请参阅ASP.NET网站上[ASP.NET Web API 中的基本身份验证](https://www.asp.net/web-api/overview/security/basic-authentication)。

> [!NOTE]
> 基本身份验证不是用于管理注销。因此，注销的标准基本身份验证方法是结束会话。

## <a name="related-links"></a>相关链接

- [使用 RESTful Web 服务](~/xamarin-forms/data-cloud/web-services/rest.md)
- [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)
