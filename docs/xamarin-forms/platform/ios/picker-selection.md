---
标题： "在 iOS 上选择选取器项" 说明： "平台说明允许使用仅在特定平台上可用的功能，而无需实现自定义呈现器或效果。 本文介绍如何使用特定于 iOS 平台的来控制选取器中发生项目选择的时间。
ms-chap： xamarin assetid：26B0604A-BD30-49FD-83A6-F0EDFBB0524B： xamarin 窗体作者： davidbritch： dabritch ms. 日期：10/24/2018 非 loc： [ Xamarin.Forms ， Xamarin.Essentials ]
---

# <a name="picker-item-selection-on-ios"></a>IOS 上的选取器项选择

[![下载示例](~/media/shared/download.png) 下载示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

此 iOS 平台特定的控件：当项在中发生选择时 [`Picker`](xref:Xamarin.Forms.Picker) ，允许用户指定在浏览控件中的项时或在按 "**完成**" 按钮后所做的选择。 它通过将 `Picker.UpdateMode` 附加属性设置为枚举的值，在 XAML 中使用 `UpdateMode` ：

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        <Picker ... Title="Select a monkey" ios:Picker.UpdateMode="WhenFinished">
          ...
        </Picker>
        ...
    </StackLayout>
</ContentPage>
```

此外，还可以使用 Fluent API 从 c # 使用该方法：

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

picker.On<iOS>().SetUpdateMode(UpdateMode.WhenFinished);
```

`Picker.On<iOS>`方法指定此平台特定的仅在 iOS 上运行。 `Picker.SetUpdateMode`命名空间中的方法 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 用于控制项的选择发生的时间， `UpdateMode` 枚举提供了两个可能的值：

- `Immediately`–当用户浏览中的项时，将发生项选择 [`Picker`](xref:Xamarin.Forms.Picker) 。 这是中的默认行为 Xamarin.Forms 。
- `WhenFinished`-仅当用户按下了 "**完成**" 按钮后，才会进行项目选择 [`Picker`](xref:Xamarin.Forms.Picker) 。

此外，该 `SetUpdateMode` 方法可用于通过调用返回当前的方法来切换枚举值 `UpdateMode` `UpdateMode` ：

```csharp
switch (picker.On<iOS>().UpdateMode())
{
    case UpdateMode.Immediately:
        picker.On<iOS>().SetUpdateMode(UpdateMode.WhenFinished);
        break;
    case UpdateMode.WhenFinished:
        picker.On<iOS>().SetUpdateMode(UpdateMode.Immediately);
        break;
}
```

结果是，将指定 `UpdateMode` 应用于 [`Picker`](xref:Xamarin.Forms.Picker) ，后者控制何时发生项选择：

[![](picker-selection-images/picker-updatemode.png "Picker UpdateMode Platform-Specific")](picker-selection-images/picker-updatemode-large.png#lightbox "Picker UpdateMode Platform-Specific")

## <a name="related-links"></a>相关链接

- [PlatformSpecifics （示例）](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [创建平台特定信息](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
