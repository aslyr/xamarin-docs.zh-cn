---
title: Xamarin.Forms RefreshView
description: Xamarin.Forms RefreshView 是一个容器控件，它为可滚动的内容提供了拉取到刷新功能。
ms.prod: xamarin
ms.assetId: 58DBD23B-ADB9-40DA-B331-4DDB6E698990
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/19/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
- RefreshView
- Universal Windows Platform
ms.openlocfilehash: 83802683aee722468acf9bcc827ba66f45c05e6b
ms.sourcegitcommit: cd0c0999b53e825b60471bfbfd4144cfcd783587
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2020
ms.locfileid: "86225476"
---
# <a name="xamarinforms-refreshview"></a>Xamarin.Forms RefreshView

[![下载示例](~/media/shared/download.png) 下载示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-refreshviewdemo/)

`RefreshView`是一个容器控件，它为可滚动的内容提供了拉取到刷新功能。 因此，的子级 `RefreshView` 必须是可滚动的控件，如 [`ScrollView`](xref:Xamarin.Forms.ScrollView) 、 [`CollectionView`](xref:Xamarin.Forms.CollectionView) 或 [`ListView`](xref:Xamarin.Forms.ListView) 。

`RefreshView` 定义以下属性:

- `Command`，类型为 `ICommand` ，在触发刷新时执行。
- `CommandParameter`，属于 `object` 类型，是传递给 `Command` 的参数。
- `IsRefreshing`，类型为 `bool` ，指示的当前状态 `RefreshView` 。
- `RefreshColor`，类型为 `Color` ，在刷新过程中显示的进度圆圈的颜色。

这些属性是由对象支持的 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) ，这意味着它们可以是数据绑定的目标和样式。

> [!NOTE]
> 在上 Universal Windows Platform ， `RefreshView` 可以使用特定于平台的对的请求方向进行设置。 有关详细信息，请参阅[ RefreshView 拉取方向](~/xamarin-forms/platform/windows/refreshview-pulldirection.md)。

## <a name="create-a-refreshview"></a>创建 RefreshView

下面的示例演示如何 `RefreshView` 在 XAML 中实例化：

```xaml
<RefreshView IsRefreshing="{Binding IsRefreshing}"
             Command="{Binding RefreshCommand}">
    <ScrollView>
        <FlexLayout Direction="Row"
                    Wrap="Wrap"
                    AlignItems="Center"
                    AlignContent="Center"
                    BindableLayout.ItemsSource="{Binding Items}"
                    BindableLayout.ItemTemplate="{StaticResource ColorItemTemplate}" />
    </ScrollView>
</RefreshView>
```

`RefreshView`也可以在代码中创建：

```csharp
RefreshView refreshView = new RefreshView();
ICommand refreshCommand = new Command(() =>
{
    // IsRefreshing is true
    // Refresh data here
    refreshView.IsRefreshing = false;
});
refreshView.Command = refreshCommand;

ScrollView scrollView = new ScrollView();
FlexLayout flexLayout = new FlexLayout { ... };
scrollView.Content = flexLayout;
refreshView.Content = scrollView;
```

在此示例中，将 `RefreshView` 向刷新功能提供对 [`ScrollView`](xref:Xamarin.Forms.ScrollView) 子级为的的请求 [`FlexLayout`](xref:Xamarin.Forms.FlexLayout) 。 `FlexLayout`通过绑定到项的集合，使用可绑定布局生成其内容，并使用设置每个项的外观 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 。 有关可绑定布局的详细信息，请参阅[中 Xamarin.Forms 的可绑定布局](~/xamarin-forms/user-interface/layouts/bindable-layouts.md)。

属性的值 `RefreshView.IsRefreshing` 指示的当前状态 `RefreshView` 。 当用户触发刷新时，此属性将自动转换为 `true` 。 刷新完成后，应将属性重置为 `false` 。

当用户启动刷新时，将 `ICommand` 执行由属性定义的 `Command` ，这会刷新正在显示的项。 刷新发生时，会显示刷新可视化效果，其中包含动画进度圆：

[![RefreshView在 iOS 和 Android 上刷新数据的屏幕截图](refreshview-images/default-progress-circle.png "[!基金.无 LOC (RefreshView) ] 正在刷新数据")](refreshview-images/default-progress-circle-large.png#lightbox "[!基金.无 LOC (RefreshView) ] 正在刷新数据")

> [!NOTE]
> 手动将 `IsRefreshing` 属性设置为 `true` 将触发刷新可视化，并将执行 `ICommand` 属性定义的 `Command` 。

## <a name="refreshview-appearance"></a>RefreshView外观

除了 `RefreshView` 从类继承的属性之外 [`VisualElement`](xref:Xamarin.Forms.VisualElement) ， `RefreshView` 还定义 `RefreshColor` 属性。 此属性可设置为定义在刷新过程中出现的进度圆圈的颜色：

```xaml
<RefreshView RefreshColor="Teal"
             ... />
```

以下屏幕截图显示了 `RefreshView` 具有 `RefreshColor` 属性集的：

[![RefreshViewIOS 和 Android 上带有蓝绿色进度圆圈的屏幕截图](refreshview-images/teal-progress-circle.png "[!基金.无位置 (RefreshView) ]，带青色的进度圆圈")](refreshview-images/teal-progress-circle-large.png#lightbox "[!基金.无位置 (RefreshView) ]，带青色的进度圆圈")

此外，还 `BackgroundColor` 可以将属性设置为 [`Color`](xref:Xamarin.Forms.Color) 表示进度圆背景色的。

> [!NOTE]
> 在 iOS 上， `BackgroundColor` 属性设置包含进度圆圈的的背景色 `UIView` 。

## <a name="disable-a-refreshview"></a>禁用RefreshView

应用程序可以输入状态，请求刷新的状态不是有效的操作。 在这种情况下， `RefreshView` 可以通过将其 `IsEnabled` 属性设置为来禁用 `false` 。 这会阻止用户触发请求刷新。

或者，在定义 `Command` 属性时， `CanExecute` 可以指定的委托 `ICommand` 来启用或禁用该命令。

## <a name="related-links"></a>相关链接

- [RefreshView (示例) ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-refreshviewdemo/)
- [可绑定布局Xamarin.Forms](~/xamarin-forms/user-interface/layouts/bindable-layouts.md)
- [RefreshView特定于平台的拉取方向](~/xamarin-forms/platform/windows/refreshview-pulldirection.md)
