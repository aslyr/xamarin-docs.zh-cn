---
标题： "iOS 上的 TabbedPage 半透明选项卡栏" 说明： "平台细节使你可以使用仅在特定平台上可用的功能，而无需实现自定义呈现器或效果。 本文介绍如何使用 iOS 平台特定的来设置 TabbedPage 上选项卡栏的半透明度模式。
ms-chap： xamarin assetid：9581AE81-9557-47AD-8B07-25A1AC5F055B： xamarin 窗体作者： davidbritch： dabritch ms. 日期：01/16/2020 非 loc： [ Xamarin.Forms ， Xamarin.Essentials ]
---

# <a name="tabbedpage-translucent-tab-bar-on-ios"></a>IOS 上的 TabbedPage 半透明选项卡栏

[![下载示例](~/media/shared/download.png) 下载示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

此 iOS 平台特定用于设置上选项卡栏的半透明度模式 [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) 。 它通过将 `TabbedPage.TranslucencyMode` 可绑定的属性设置为一个枚举值在 XAML 中使用 `TranslucencyMode` ：

```xaml
<TabbedPage ...
            xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
            ios:TabbedPage.TranslucencyMode="Opaque">
    ...
</TabbedPage>
```

此外，还可以使用 Fluent API 从 c # 使用该方法：

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

On<iOS>().SetTranslucencyMode(TranslucencyMode.Opaque);
```

`TabbedPage.On<iOS>`方法指定此平台特定的仅在 iOS 上运行。 `TabbedPage.SetTranslucencyMode`命名空间中的方法 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 可 [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) 通过指定以下枚举值之一，在上设置选项卡栏的半透明度模式 `TranslucencyMode` ：

- `Default`，它将选项卡栏设置为其默认的半透明度模式。 这是 `TabbedPage.TranslucencyMode` 属性的默认值。
- `Translucent`，它将选项卡栏设置为半透明。
- `Opaque`，它将选项卡栏设置为不透明。

此外， `GetTranslucencyMode` 方法可用于检索 `TranslucencyMode` 应用于的枚举的当前值 [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) 。

结果就是可以设置上的选项卡栏的半透明度模式 [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) ：

![IOS 上半透明和不透明选项卡栏的屏幕截图](tabbedpage-translucent-tabbar-images/translucencymodes.png "透明且不透明的选项卡")

## <a name="related-links"></a>相关链接

- [PlatformSpecifics （示例）](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [创建平台特定信息](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
