---
title： "使用电子书的企业应用程序模式 Xamarin.Forms " 说明： "此电子书提供了开发可自适应、可维护且可测试的企业应用程序的体系结构指南 Xamarin.Forms 。
ms-chap： xamarin assetid： 28cfed6c-6175-4223-a8cc-798d40bf0832 ms. 技术协会： xamarin 窗体作者： davidbritch： dabritch ms. 日期：08/07/2017 非 loc： [ Xamarin.Forms ， Xamarin.Essentials ]
---

# <a name="enterprise-application-patterns-using-xamarinforms-ebook"></a>《使用 Xamarin.Forms企业应用程序模式》电子书

_用于开发可自适应、可维护且可测试的 Xamarin.Forms 企业应用程序的体系结构指南_

![](images/cover-sml.png "Enterprise Application Patterns using Xamarin.Forms eBook")

此电子书提供有关如何实现模型-视图-ViewModel （MVVM）模式、依赖关系注入、导航、验证和配置管理，同时保持松散耦合的指导。 此外，还提供了有关通过 IdentityServer 执行身份验证和授权、从容器化微服务访问数据和单元测试的指南。

## <a name="preface"></a>[前言](preface.md)

本章介绍指南的目的和范围，以及其目标。

## <a name="introduction"></a>[介绍](introduction.md)

企业应用程序的开发者面临着几个难题，可以在开发过程中更改应用程序的体系结构。 因此，生成应用以便在一段时间内修改或扩展应用程序非常重要。 设计此类适应性很困难，但通常会将应用分区为分离、松散耦合的组件，这些组件可以轻松集成到应用中。

## <a name="mvvm"></a>[MVVM](mvvm.md)

模型-视图-ViewModel （MVVM）模式有助于将应用程序的业务和表示逻辑与用户界面（UI）完全分开。 在应用程序逻辑与 UI 之间保持干净分离有助于解决众多开发问题，并使应用程序更易于测试、维护和发展。 它还可以极大地提高代码重复使用机会，并使开发人员和 UI 设计人员在开发应用程序的各自部分时可以更轻松地进行协作。

## <a name="dependency-injection"></a>[Dependency Injection](dependency-injection.md)

依赖关系注入允许从依赖于这些类型的代码分离具体类型。 它通常使用容器来保存接口与抽象类型之间的注册和映射列表，以及实现或扩展这些类型的具体类型。

依赖关系注入容器可提供一种方法来实例化类实例，并基于容器的配置来管理其生存期，从而减少对象之间的耦合。 在对象创建过程中，容器将注入对象所需的任何依赖项。 如果尚未创建这些依赖关系，则容器将首先创建并解析其依赖项。

## <a name="communicating-between-loosely-coupled-components"></a>[松散耦合组件之间的通信](communicating-between-loosely-coupled-components.md)

Xamarin.Forms [`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) 类可实现发布-订阅模式，允许不便按对象和类型引用进行链接的组件之间进行基于消息的通信。 此机制使发布服务器和订阅服务器无需彼此引用即可进行通信，有助于减少组件之间的依赖关系，同时允许单独开发和测试组件。

## <a name="navigation"></a>[导航](navigation.md)

Xamarin.Forms支持页面导航，这通常是由于用户与 UI 交互导致的，或者是由于内部逻辑驱动状态更改而导致的。 但是，导航可能会很复杂，无法在使用 MVVM 模式的应用中实现。

本章介绍了一个 `NavigationService` 类，该类用于执行视图模型的视图模型优先导航。 在视图模型类中放置导航逻辑意味着可以通过自动测试实现逻辑。 此外，视图模型可以实现逻辑来控制导航，以确保强制实施某些业务规则。

## <a name="validation"></a>[验证](validation.md)

任何接受用户输入的应用都应确保输入有效。 如果没有验证，用户可以提供导致应用失败的数据。 验证将强制实施业务规则，并阻止攻击者注入恶意数据。

在 ViewModel （MVVM）模式的上下文中，视图模型或模型通常需要执行数据验证并向视图发出任何验证错误信号，以便用户可以更正这些错误。

## <a name="configuration-management"></a>[配置管理](configuration-management.md)

设置允许将配置应用行为的数据与代码分离，从而允许更改行为而无需重新生成应用。 应用程序设置是指应用程序创建和管理的数据，用户设置是应用程序的可自定义设置，它会影响应用程序的行为，而不需要频繁重新调整。

## <a name="containerized-microservices"></a>[容器化微服务](containerized-microservices.md)

微服务提供适用于应用程序开发和部署的方法，适用于现代云应用程序的灵活性、规模和可靠性要求。 微服务的主要优点之一是可以独立横向扩展，这意味着可以扩展特定功能区域，以便需要更多的处理能力或网络带宽来支持需求，而无需在未增加需求的情况下调整应用程序区域。

## <a name="authentication-and-authorization"></a>[身份验证和授权](authentication-and-authorization.md)

有多种方法可以将身份验证和授权集成到 Xamarin.Forms 与 ASP.NET MVC web 应用程序通信的应用程序中。 此处，使用 IdentityServer 4 的容器化标识微服务来执行身份验证和授权。 IdentityServer 是一个开源 OpenID Connect 和 OAuth 2.0 framework，适用于与 ASP.NET Core 标识集成以执行持有者令牌身份验证的 ASP.NET Core。

## <a name="accessing-remote-data"></a>[访问远程数据](accessing-remote-data.md)

许多基于 web 的新式解决方案都利用 web 服务器托管的 web 服务，为远程客户端应用程序提供功能。 Web 服务公开的操作构成 web API，客户端应用程序应能够利用 web API，而无需知道 API 公开的数据或操作是如何实现的。

## <a name="unit-testing"></a>[单元测试](unit-testing.md)

从 MVVM 应用程序测试模型和查看模型与测试任何其他类相同，并且可以使用相同的工具和技术。 但有些模式通常是模型和视图模型类的典型模式，这些模式可受益于特定单元测试技术。

## <a name="feedback"></a>反馈

此项目有一个社区网站，您可以在其中发布问题并提供反馈。 社区网站位于[GitHub](https://github.com/dotnet-architecture/eShopOnContainers)上。 或者，可以通过电子邮件将有关电子书的反馈发送到 [dotnet-architecture-ebooks-feedback@service.microsoft.com](mailto:dotnet-architecture-ebooks-feedback@service.microsoft.com) 。

## <a name="related-links"></a>相关链接

- [下载电子书（2Mb）](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers （GitHub）（示例）](https://github.com/dotnet-architecture/eShopOnContainers)
