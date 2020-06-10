---
标题： "说明：" 中的 "布局选项" Xamarin.Forms ：每个 Xamarin.Forms 视图都有 HorizontalOptions 和 VerticalOptions 属性，其类型为 LayoutOptions。 本文说明每个 LayoutOptions 值对视图对齐和展开的影响。
ms-chap： xamarin assetid：7CAB5631-5153-4DEF-8AD7-C6011CE44307： xamarin 窗体作者： davidbritch： dabritch ms. 日期：02/10/2017 非 loc： [ Xamarin.Forms ， Xamarin.Essentials ]
---

# <a name="layout-options-in-xamarinforms"></a>布局选项Xamarin.Forms

[![下载示例](~/media/shared/download.png) 下载示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layoutoptions)

_每个 Xamarin.Forms 视图都具有类型为 LayoutOptions 的 HorizontalOptions 和 VerticalOptions 属性。本文说明每个 LayoutOptions 值对视图对齐和展开的影响。_

## <a name="overview"></a>概述

此 [`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions) 结构封装两个布局首选项：

- **对齐方式**–视图的首选对齐方式，用于确定其在其父布局中的位置和大小。
- "**扩展**" –仅用于 [`StackLayout`](xref:Xamarin.Forms.StackLayout) ，并指示视图是否应使用额外空间（如果可用）。

[`View`](xref:Xamarin.Forms.View)通过将 [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) 的或属性设置 `View` 为结构中的一个公共字段 [`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions) ，可将这些布局首选项应用于相对于其父级的。 公共字段如下所示：

- [`Start`](xref:Xamarin.Forms.LayoutOptions.Start)
- [`Center`](xref:Xamarin.Forms.LayoutOptions.Center)
- [`End`](xref:Xamarin.Forms.LayoutOptions.End)
- [`Fill`](xref:Xamarin.Forms.LayoutOptions.Fill)
- [`StartAndExpand`](xref:Xamarin.Forms.LayoutOptions.StartAndExpand)
- [`CenterAndExpand`](xref:Xamarin.Forms.LayoutOptions.CenterAndExpand)
- [`EndAndExpand`](xref:Xamarin.Forms.LayoutOptions.EndAndExpand)
- [`FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand)

`Start`、、 `Center` `End` 和 `Fill` 字段用于定义视图在父布局中的对齐方式：

- 对于水平对齐方式， [`Start`](xref:Xamarin.Forms.LayoutOptions.Start) 将定位在 [`View`](xref:Xamarin.Forms.View) 父布局的左侧，对于垂直对齐，它将定位在 `View` 父布局的顶部。
- 对于水平对齐和垂直对齐， [`Center`](xref:Xamarin.Forms.LayoutOptions.Center) 水平或垂直居中对齐 [`View`](xref:Xamarin.Forms.View) 。
- 对于水平对齐方式， [`End`](xref:Xamarin.Forms.LayoutOptions.End) 将定位在 [`View`](xref:Xamarin.Forms.View) 父布局的右侧，对于垂直对齐，它将定位在 `View` 父布局的底部。
- 对于水平对齐方式， [`Fill`](xref:Xamarin.Forms.LayoutOptions.Fill) 可确保 [`View`](xref:Xamarin.Forms.View) 填充父布局的宽度，对于垂直对齐，它可确保 `View` 填充父布局的高度。

`StartAndExpand`、、 `CenterAndExpand` `EndAndExpand` 和 `FillAndExpand` 值用于定义对齐首选项，以及视图是否会占用父对象中的更多空间 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 。

> [!NOTE]
> 视图的 [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) 和 [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) 属性的默认值为 [`LayoutOptions.Fill`](xref:Xamarin.Forms.LayoutOptions.Fill)。

## <a name="alignment"></a>对齐方式

对齐方式控制当父布局包含未使用空间时，视图在其父布局中的定位方式（即，父布局大于其所有子项的总大小）。

[`StackLayout`](xref:Xamarin.Forms.StackLayout)仅对 `Start` `Center` `End` `Fill` [`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions) 与方向相反的子视图具有、、和字段的情况 `StackLayout` 。 因此，垂直方向上的子视图 `StackLayout` 可以将它们 [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) 的属性设置为 `Start` 、、 `Center` `End` 或字段之一 `Fill` 。 同样，水平方向上的子视图 `StackLayout` 可以将它们 [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) 的属性设置为 `Start` 、、 `Center` `End` 或字段之一 `Fill` 。

不 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 遵循 `Start` `Center` `End` `Fill` [`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions) 与方向方向相同的子视图上的、、和字段 `StackLayout` 。 因此， `StackLayout` `Start` `Center` `End` 如果将、、或 `Fill` 字段设置为 [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) 子视图的属性，则该垂直方向将忽略、、或字段。 同样， `StackLayout` `Start` 如果在 `Center` `End` `Fill` 子视图的属性上设置、、或字段 [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) ，则水平方向将忽略、、或字段。

> [!NOTE]
> [`LayoutOptions.Fill`](xref:Xamarin.Forms.LayoutOptions.Fill)通常会重写使用和属性指定的大小请求 [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) 。

下面的 XAML 代码示例演示了一个垂直方向 [`StackLayout`](xref:Xamarin.Forms.StackLayout) ，其中每个子元素会 [`Label`](xref:Xamarin.Forms.Label) 将其 [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) 属性设置为结构中的四个对齐字段之一 [`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions) ：

```xaml
<StackLayout Margin="0,20,0,0">
  ...
  <Label Text="Start" BackgroundColor="Gray" HorizontalOptions="Start" />
  <Label Text="Center" BackgroundColor="Gray" HorizontalOptions="Center" />
  <Label Text="End" BackgroundColor="Gray" HorizontalOptions="End" />
  <Label Text="Fill" BackgroundColor="Gray" HorizontalOptions="Fill" />
</StackLayout>
```

下面显示了等效的 c # 代码：

```csharp
Content = new StackLayout
{
  Margin = new Thickness(0, 20, 0, 0),
  Children = {
    ...
    new Label { Text = "Start", BackgroundColor = Color.Gray, HorizontalOptions = LayoutOptions.Start },
    new Label { Text = "Center", BackgroundColor = Color.Gray, HorizontalOptions = LayoutOptions.Center },
    new Label { Text = "End", BackgroundColor = Color.Gray, HorizontalOptions = LayoutOptions.End },
    new Label { Text = "Fill", BackgroundColor = Color.Gray, HorizontalOptions = LayoutOptions.Fill }
  }
};
```

此代码将生成如下屏幕截图所示的布局：

[![](layout-options-images/alignment.png "Alignment Layout Options")](layout-options-images/alignment-large.png#lightbox "Alignment Layout Options")

## <a name="expansion"></a>扩展

展开控制在中，视图是否会占用更多空间（如果可用） [`StackLayout`](xref:Xamarin.Forms.StackLayout) 。 如果 `StackLayout` 包含未使用的空间（即，大于 `StackLayout` 其所有子项的组合大小），则通过将请求扩展的所有子视图的 [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) 或 [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) 属性设置为 [`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions) 使用该后缀的字段，可平均共享未使用的空间 `AndExpand` 。 请注意，当使用中的所有空格时 `StackLayout` ，扩展选项不起作用。

[`StackLayout`](xref:Xamarin.Forms.StackLayout) 只能按照其方向展开子视图。 因此， `StackLayout` [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) `StartAndExpand` `CenterAndExpand` `EndAndExpand` `FillAndExpand` 如果 `StackLayout` 包含未使用的空间，则垂直方向可以展开将其属性设置为、、或字段之一的子视图。 同样， `StackLayout` [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) `StartAndExpand` `CenterAndExpand` `EndAndExpand` `FillAndExpand` 如果 `StackLayout` 包含未使用的空间，则水平方向可以展开将其属性设置为、、或字段之一的子视图。

[`StackLayout`](xref:Xamarin.Forms.StackLayout)不能以相对于其方向的方向展开子视图。 因此，在垂直方向上 `StackLayout` ，将 [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) 子视图上的属性设置为 [`StartAndExpand`](xref:Xamarin.Forms.LayoutOptions.StartAndExpand) 具有与将属性设置为相同的效果 [`Start`](xref:Xamarin.Forms.LayoutOptions.Start) 。

> [!NOTE]
> 请注意，启用扩展不会更改视图的大小，除非它使用 [`LayoutOptions.FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand) 。

下面的 XAML 代码示例演示了一个垂直方向 [`StackLayout`](xref:Xamarin.Forms.StackLayout) ，其中每个子元素会 [`Label`](xref:Xamarin.Forms.Label) 将其 [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) 属性设置为结构中的四个扩展字段之一 [`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions) ：

```xaml
<StackLayout Margin="0,20,0,0">
  ...
  <BoxView BackgroundColor="Red" HeightRequest="1" />
  <Label Text="Start" BackgroundColor="Gray" VerticalOptions="StartAndExpand" />
  <BoxView BackgroundColor="Red" HeightRequest="1" />
  <Label Text="Center" BackgroundColor="Gray" VerticalOptions="CenterAndExpand" />
  <BoxView BackgroundColor="Red" HeightRequest="1" />
  <Label Text="End" BackgroundColor="Gray" VerticalOptions="EndAndExpand" />
  <BoxView BackgroundColor="Red" HeightRequest="1" />
  <Label Text="Fill" BackgroundColor="Gray" VerticalOptions="FillAndExpand" />
  <BoxView BackgroundColor="Red" HeightRequest="1" />
</StackLayout>
```

下面显示了等效的 c # 代码：

```csharp
Content = new StackLayout
{
  Margin = new Thickness(0, 20, 0, 0),
  Children = {
    ...
    new BoxView { BackgroundColor = Color.Red, HeightRequest = 1 },
    new Label { Text = "StartAndExpand", BackgroundColor = Color.Gray, VerticalOptions = LayoutOptions.StartAndExpand },
    new BoxView { BackgroundColor = Color.Red, HeightRequest = 1 },
    new Label { Text = "CenterAndExpand", BackgroundColor = Color.Gray, VerticalOptions = LayoutOptions.CenterAndExpand },
    new BoxView { BackgroundColor = Color.Red, HeightRequest = 1 },
    new Label { Text = "EndAndExpand", BackgroundColor = Color.Gray, VerticalOptions = LayoutOptions.EndAndExpand },
    new BoxView { BackgroundColor = Color.Red, HeightRequest = 1 },
    new Label { Text = "FillAndExpand", BackgroundColor = Color.Gray, VerticalOptions = LayoutOptions.FillAndExpand },
    new BoxView { BackgroundColor = Color.Red, HeightRequest = 1 }
  }
};
```

此代码将生成如下屏幕截图所示的布局：

[![](layout-options-images/expansion.png "Expansion Layout Options")](layout-options-images/expansion-large.png#lightbox "Expansion Layout Options")

每个在 [`Label`](xref:Xamarin.Forms.Label) 中占据相同的空间量 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 。 但是，只有最后一个 `Label`（可将 [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) 属性设置为 [`FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand)）具有不同的大小。 此外，每个 `Label` 都由一个小红色分隔 [`BoxView`](xref:Xamarin.Forms.BoxView) ，使占有的空间 `Label` 易于查看。

## <a name="summary"></a>总结

本文介绍了每个 [`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions) 结构值相对于其父级的对齐和展开的影响。 `Start`、、 `Center` `End` 和 `Fill` 字段用于定义视图在父布局中的对齐方式， `StartAndExpand` 、、 `CenterAndExpand` `EndAndExpand` 和 `FillAndExpand` 字段用于定义对齐首选项，并用于确定视图是否在中占用更多空间（如果有） [`StackLayout`](xref:Xamarin.Forms.StackLayout) 。

## <a name="related-links"></a>相关链接

- [LayoutOptions （示例）](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layoutoptions)
- [LayoutOptions](xref:Xamarin.Forms.LayoutOptions)
