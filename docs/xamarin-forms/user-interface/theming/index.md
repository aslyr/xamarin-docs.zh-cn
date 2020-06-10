---
标题： "主题 a Xamarin.Forms 应用程序" 说明： " Xamarin.Forms 应用程序通过为每个主题创建 ResourceDictionary 并使用 DynamicResource 标记扩展加载资源来支持主题。"
ms-chap： xamarin assetId： BF92AEDD-EF23-4D08-A972-B089066E75F9： xamarin 窗体作者： davidbritch： dabritch ms. 日期：04/22/2020 非 loc： [ Xamarin.Forms ， Xamarin.Essentials ]
---

# <a name="theming-a-xamarinforms-application"></a>主题 Xamarin.Forms 应用程序

## <a name="theme-an-application"></a>[主题应用程序](theming.md)

可以 Xamarin.Forms 通过 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 为每个主题创建，然后使用标记扩展加载资源，在应用程序中实现主题 `DynamicResource` 。

## <a name="respond-to-system-theme-changes"></a>[响应系统主题更改](system-theme-changes.md)

设备通常包括浅色和深色主题，它们分别引用可在操作系统级别设置的各种外观首选项。 应用程序应遵守这些系统主题，并在系统主题发生更改时立即做出响应。
