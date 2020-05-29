---
title: ''
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 943148a642a1dec7e84f74ae96983c21f30319ab
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/28/2020
ms.locfileid: "84139264"
---
# <a name="configuration-management"></a>配置管理

设置允许将配置应用行为的数据与代码分离，从而允许更改行为而无需重新生成应用。 有两种类型的设置： "应用设置" 和 "用户设置"。

应用程序设置是指应用程序创建和管理的数据。 它可以包含固定 web 服务终结点、API 密钥和运行时状态等数据。 应用设置与应用的存在相关，仅对该应用有意义。

用户设置是应用程序的可自定义设置，它会影响应用程序的行为，而不需要频繁重新调整。 例如，应用可能会让用户指定从中检索数据的位置，以及如何在屏幕上显示数据。

Xamarin.Forms包含一个可用于存储设置数据的持久字典。 可以使用属性访问此字典 [`Application.Current.Properties`](xref:Xamarin.Forms.Application.Properties) ，当应用进入睡眠状态时，将保存其中的任何数据，并在应用恢复或重新启动时还原。 此外， [`Application`](xref:Xamarin.Forms.Application) 类还提供了一 [`SavePropertiesAsync`](xref:Xamarin.Forms.Application.SavePropertiesAsync) 种方法，允许应用在需要时保存其设置。 有关此字典的详细信息，请参阅[属性字典](~/xamarin-forms/app-fundamentals/application-class.md#Properties_Dictionary)。

使用持久性字典存储数据的一个缺点 Xamarin.Forms 是它不容易被绑定到数据。 因此，eShopOnContainers 移动应用使用的是可从[NuGet](https://www.nuget.org/packages/Xam.Plugins.Settings/)获得的库。 此库提供一致的类型安全跨平台方法，用于保留和检索应用和用户设置，同时使用每个平台提供的本机设置管理。 此外，使用数据绑定访问由库公开的设置数据非常简单。

> [!NOTE]
> 虽然 Xam 插件库可以存储应用和用户设置，但两者之间不会有任何区别。

## <a name="creating-a-settings-class"></a>创建设置类

使用 Xam 类库时，应创建一个静态类，该静态类将包含应用所需的应用和用户设置。 下面的代码示例显示了 eShopOnContainers 移动应用中的 Settings 类：

```csharp
public static class Settings  
{  
    private static ISettings AppSettings  
    {  
        get  
        {  
            return CrossSettings.Current;  
        }  
    }  
    ...  
}
```

可以通过 `ISettings` 由 Xam 提供的 API 来读取和写入设置。 此库提供了一个可用于访问 API 的单一实例， `CrossSettings.Current` 应用的 settings 类应通过属性公开此单一实例 `ISettings` 。

> [!NOTE]
> 使用插件的指令。设置和插件命名空间应添加到需要访问 Xam. Settings 库类型的类。

## <a name="adding-a-setting"></a>添加设置

每个设置都包含一个键、一个默认值和一个属性。 下面的代码示例显示了一个用户设置的所有三个项，它表示 eShopOnContainers 移动应用程序连接到的联机服务的基 URL：

```csharp
public static class Settings  
{  
    ...  
    private const string IdUrlBase = "url_base";  
    private static readonly string UrlBaseDefault = GlobalSetting.Instance.BaseEndpoint;  
    ...  

    public static string UrlBase  
    {  
        get  
        {  
            return AppSettings.GetValueOrDefault<string>(IdUrlBase, UrlBaseDefault);  
        }  
        set  
        {  
            AppSettings.AddOrUpdateValue<string>(IdUrlBase, value);  
        }  
    }  
}
```

该键始终是一个定义密钥名称的常量字符串，其默认值为该设置是所需类型的静态 readonly 值。 如果检索到未设置的设置，提供默认值可确保有效的值可用。

`UrlBase`静态属性使用 API 中的两个方法 `ISettings` 来读取或写入设置值。 `ISettings.GetValueOrDefault`方法用于从平台特定的存储检索设置的值。 如果没有为设置定义值，则改为检索其默认值。 同样， `ISettings.AddOrUpdateValue` 使用方法将设置的值保存到平台特定的存储。

而是在类中定义一个默认值 `Settings` ，该 `UrlBaseDefault` 字符串从类中获取其值 `GlobalSetting` 。 下面的代码示例演示了 `BaseEndpoint` `UpdateEndpoint` 此类中的属性和方法：

```csharp
public class GlobalSetting  
{  
    ...  
    public string BaseEndpoint  
    {  
        get { return _baseEndpoint; }  
        set  
        {  
            _baseEndpoint = value;  
            UpdateEndpoint(_baseEndpoint);  
        }  
    }  
    ...  

    private void UpdateEndpoint(string baseEndpoint)  
    {  
        RegisterWebsite = string.Format("{0}:5105/Account/Register", baseEndpoint);  
        CatalogEndpoint = string.Format("{0}:5101", baseEndpoint);  
        OrdersEndpoint = string.Format("{0}:5102", baseEndpoint);  
        BasketEndpoint = string.Format("{0}:5103", baseEndpoint);  
        IdentityEndpoint = string.Format("{0}:5105/connect/authorize", baseEndpoint);  
        UserInfoEndpoint = string.Format("{0}:5105/connect/userinfo", baseEndpoint);  
        TokenEndpoint = string.Format("{0}:5105/connect/token", baseEndpoint);  
        LogoutEndpoint = string.Format("{0}:5105/connect/endsession", baseEndpoint);  
        IdentityCallback = string.Format("{0}:5105/xamarincallback", baseEndpoint);  
        LogoutCallback = string.Format("{0}:5105/Account/Redirecting", baseEndpoint);  
    }  
}
```

每次 `BaseEndpoint` 设置此属性时，都将 `UpdateEndpoint` 调用方法。 此方法更新一系列属性，这些属性都基于 `UrlBase` 类提供的用户设置， `Settings` 表示 eShopOnContainers 移动应用程序连接到的不同终结点。

## <a name="data-binding-to-user-settings"></a>数据绑定到用户设置

在 eShopOnContainers 移动应用中，会 `SettingsView` 公开两个用户设置。 这些设置允许配置应用是否应从部署为 Docker 容器的微服务中检索数据，或应用是否应从无需 internet 连接的 mock 服务中检索数据。 选择从容器化的微服务中检索数据时，必须指定微服务的基本终结点 URL。 图7-1 显示 `SettingsView` 用户选择从容器化微服务检索数据的时间。

![](configuration-management-images/settings-endpoint.png "User settings exposed by the eShopOnContainers mobile app")

**图 7-1**： eShopOnContainers 移动应用公开的用户设置

数据绑定可用于检索和设置由类公开的设置 `Settings` 。 这是通过视图绑定上的控件来实现的，它可以查看模型属性，这些属性又可以访问类中的属性 `Settings` ，并且在设置值更改时引发属性更改通知。 有关 eShopOnContainers 移动应用如何构造视图模型并将其与视图相关联的信息，请参阅[自动创建具有视图模型定位器的视图模型](~/xamarin-forms/enterprise-application-patterns/mvvm.md#automatically_creating_a_view_model_with_a_view_model_locator)。

下面的代码示例演示 [`Entry`](xref:Xamarin.Forms.Entry) 了中的控件 `SettingsView` ，该控件允许用户为容器化的微服务输入基终结点 URL：

```xaml
<Entry Text="{Binding Endpoint, Mode=TwoWay}" />
```

此 [`Entry`](xref:Xamarin.Forms.Entry) 控件 `Endpoint` `SettingsViewModel` 使用双向绑定绑定到类的属性。 下面的代码示例演示终结点属性：

```csharp
public string Endpoint  
{  
    get { return _endpoint; }  
    set  
    {  
        _endpoint = value;  

        if(!string.IsNullOrEmpty(_endpoint))  
        {  
            UpdateEndpoint(_endpoint);  
        }  

        RaisePropertyChanged(() => Endpoint);  
    }  
}
```

`Endpoint`设置属性时 `UpdateEndpoint` ，如果提供的值有效，则调用方法，并引发属性更改通知。 下面的代码示例说明 `UpdateEndpoint` 方法：

```csharp
private void UpdateEndpoint(string endpoint)  
{  
    Settings.UrlBase = endpoint;  
}
```

此方法 `UrlBase` `Settings` 使用用户输入的基本终结点 URL 值更新类中的属性，这会使该 URL 保持到特定于平台的存储中。

当 `SettingsView` 导航到时，将 `InitializeAsync` 执行类中的方法 `SettingsViewModel` 。 下面的代码示例演示此方法：

```csharp
public override Task InitializeAsync(object navigationData)  
{  
    ...  
    Endpoint = Settings.UrlBase;  
    ...  
}
```

方法将属性设置 `Endpoint` 为 `UrlBase` 类中的属性的值 `Settings` 。 访问 `UrlBase` 属性会导致在特定于平台的存储中检索设置值。 有关如何 `InitializeAsync` 调用方法的信息，请参阅[在导航过程中传递参数](~/xamarin-forms/enterprise-application-patterns/navigation.md#passing_parameters_during_navigation)。

此机制可确保每次用户导航到 SettingsView 时，都会从特定于平台的存储检索用户设置，并通过数据绑定来显示用户设置。 然后，如果用户更改设置值，数据绑定将确保它们立即保留回平台特定的存储。

## <a name="summary"></a>总结

设置允许将配置应用行为的数据与代码分离，从而允许更改行为而无需重新生成应用。 应用程序设置是指应用程序创建和管理的数据，用户设置是应用程序的可自定义设置，它会影响应用程序的行为，而不需要频繁重新调整。

"Xam" 设置库提供了一致的类型安全跨平台方法用于保存和检索应用和用户设置，数据绑定可用于访问使用库创建的设置。

## <a name="related-links"></a>相关链接

- [下载电子书（2Mb）](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers （GitHub）（示例）](https://github.com/dotnet-architecture/eShopOnContainers)
