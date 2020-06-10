---
标题： "SearchBar 拼写检查 on Windows" 说明： "平台说明允许使用仅在特定平台上可用的功能，而无需实现自定义呈现器或效果。 本文介绍如何使用特定于 Windows 平台的，使 SearchBar 能够与拼写检查引擎进行交互。
ms-chap： xamarin assetid：7974C91F-7502-4DB3-B0E9-C45E943DDA26： xamarin 窗体作者： davidbritch： dabritch ms. 日期：10/24/2018 非 loc： [ Xamarin.Forms ， Xamarin.Essentials ]
---

# <a name="searchbar-spell-check-on-windows"></a>Windows 上的 SearchBar 拼写检查

[![下载示例](~/media/shared/download.png) 下载示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

这通用 Windows 平台平台特定，使 [`SearchBar`](xref:Xamarin.Forms.SearchBar) 能够与拼写检查引擎进行交互。 它通过将 [`SearchBar.IsSpellCheckEnabled`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.IsSpellCheckEnabledProperty) 附加属性设置为值在 XAML 中使用 `boolean` ：

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <SearchBar ... windows:SearchBar.IsSpellCheckEnabled="true" />
        ...
    </StackLayout>
</ContentPage>
```

此外，还可以使用 Fluent API 从 c # 使用该方法：

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

searchBar.On<Windows>().SetIsSpellCheckEnabled(true);
```

`SearchBar.On<Windows>`方法指定此平台特定的仅在通用 Windows 平台上运行。 [ `SearchBar.SetIsSpellCheckEnabled` ] （X： Xamarin.Forms 。PlatformConfiguration. WindowsSpecific. SearchBar. SetIsSpellCheckEnabled （ Xamarin.Forms 。IPlatformElementConfiguration { Xamarin.Forms 。PlatformConfiguration、 Xamarin.Forms 。SearchBar}，System.object）方法，在 [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) 命名空间中打开和关闭拼写检查器。 此外，该 `SearchBar.SetIsSpellCheckEnabled` 方法可用于通过调用 [ `SearchBar.GetIsSpellCheckEnabled` ] （x：）来切换拼写检查器 Xamarin.Forms 。PlatformConfiguration. WindowsSpecific. SearchBar. GetIsSpellCheckEnabled （ Xamarin.Forms 。IPlatformElementConfiguration { Xamarin.Forms 。PlatformConfiguration、 Xamarin.Forms 。SearchBar}））方法来返回是否启用拼写检查器：

```csharp
searchBar.On<Windows>().SetIsSpellCheckEnabled(!searchBar.On<Windows>().GetIsSpellCheckEnabled());
```

结果是，在中输入的文本 [`SearchBar`](xref:Xamarin.Forms.SearchBar) 可以进行拼写检查，并向用户指示不正确的拼写：

![SearchBar 拼写检查平台特定](searchbar-spell-check-images/searchbar-spellcheck.png "SearchBar 拼写检查平台特定")

> [!NOTE]
> `SearchBar`命名空间中的 [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) 类还具有 [`EnableSpellCheck`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.EnableSpellCheck*) 和 [`DisableSpellCheck`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.DisableSpellCheck*) 方法，可分别用于启用和禁用中的拼写检查器 `SearchBar` 。

## <a name="related-links"></a>相关链接

- [PlatformSpecifics （示例）](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [创建平台特定信息](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [WindowsSpecific API](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)
