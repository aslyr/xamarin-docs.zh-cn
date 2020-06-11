---
title： "Windows 上的 InputView 读取顺序" 说明： "平台详细信息允许使用仅在特定平台上可用的功能，而无需实现自定义呈现器或效果。 本文介绍如何使用特定于 Windows 平台的，以便能够动态检测双向文本的读取顺序。
ms-chap： xamarin assetid： E61BAEE0-C8B7-4F33-8DDC-FA1B9CA8E81D： xamarin 窗体作者： davidbritch： dabritch ms. 日期：10/24/2018 非 loc： [ Xamarin.Forms ， Xamarin.Essentials ]
---

# <a name="inputview-reading-order-on-windows"></a>Windows 上的 InputView 读取顺序

[![下载示例](~/media/shared/download.png) 下载示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

此通用 Windows 平台平台特定，可在 [`Entry`](xref:Xamarin.Forms.Entry) 、 [`Editor`](xref:Xamarin.Forms.Editor) 和实例中动态检测双向文本的读取顺序（从左到右或从右到左） [`Label`](xref:Xamarin.Forms.Label) 。 它通过将 [`InputView.DetectReadingOrderFromContent`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.InputView.DetectReadingOrderFromContentProperty) （for `Entry` 和 `Editor` 实例）或 [`Label.DetectReadingOrderFromContent`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.Label.DetectReadingOrderFromContentProperty) 附加属性设置为一个 `boolean` 值，在 XAML 中使用：

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <Editor ... windows:InputView.DetectReadingOrderFromContent="true" />
        ...
    </StackLayout>
</ContentPage>
```

此外，还可以使用 Fluent API 从 c # 使用该方法：

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

editor.On<Windows>().SetDetectReadingOrderFromContent(true);
```

`Editor.On<Windows>`方法指定此平台特定的仅在通用 Windows 平台上运行。 [ `InputView.SetDetectReadingOrderFromContent` ] （X： Xamarin.Forms 。PlatformConfiguration. WindowsSpecific. InputView. SetDetectReadingOrderFromContent （ Xamarin.Forms 。IPlatformElementConfiguration { Xamarin.Forms 。PlatformConfiguration、 Xamarin.Forms 。InputView}，System.object）方法， [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) 用于控制是否从中的内容中检测到读取顺序 [`InputView`](xref:Xamarin.Forms.InputView) 。 此外， `InputView.SetDetectReadingOrderFromContent` 通过调用 [ `InputView.GetDetectReadingOrderFromContent` ] （x：），该方法可用于切换是否从内容中检测到读取顺序 Xamarin.Forms 。PlatformConfiguration. WindowsSpecific. InputView. GetDetectReadingOrderFromContent （ Xamarin.Forms 。IPlatformElementConfiguration { Xamarin.Forms 。PlatformConfiguration、 Xamarin.Forms 。InputView}））方法返回当前值：

```csharp
editor.On<Windows>().SetDetectReadingOrderFromContent(!editor.On<Windows>().GetDetectReadingOrderFromContent());
```

因此，、 [`Entry`](xref:Xamarin.Forms.Entry) [`Editor`](xref:Xamarin.Forms.Editor) 和 [`Label`](xref:Xamarin.Forms.Label) 实例可以具有其内容的读取顺序动态检测：

[![InputView 检测内容平台特定的读取顺序](inputview-reading-order-images/editor-readingorder.png "InputView 检测内容平台特定的读取顺序")](inputview-reading-order-images/editor-readingorder-large.png#lightbox "InputView 检测内容平台特定的读取顺序")

> [!NOTE]
> 与设置 [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) 属性不同，从其文本内容中检测到读取顺序的视图的逻辑不会影响视图中文本的对齐方式。 相反，它会调整双向文本块的布局顺序。

## <a name="related-links"></a>相关链接

- [PlatformSpecifics （示例）](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [创建平台特定信息](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [WindowsSpecific API](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)
