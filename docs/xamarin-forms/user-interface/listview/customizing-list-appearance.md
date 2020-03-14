---
title: ListView 外观
description: 本文介绍如何自定义 Listview Xamarin.Forms 应用程序中使用标头、 页脚、 组和高度不同的单元格。
ms.prod: xamarin
ms.assetid: DC8009B0-4371-4D60-885A-5362FC7EE3E5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/13/2018
ms.openlocfilehash: 90b0e0f3802ce766decb802c9406d72b5966360e
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/13/2020
ms.locfileid: "79305647"
---
# <a name="listview-appearance"></a>ListView 外观

[![下载示例](~/media/shared/download.png) 下载示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-grouping)

Xamarin [`ListView`](xref:Xamarin.Forms.ListView)允许自定义列表的显示，以及列表中每一行的[`ViewCell`](xref:Xamarin.Forms.ViewCell)实例。

## <a name="grouping"></a>分组

在连续滚动列表中显示大量数据时，可能会变得难以使用。 启用分组可以更好地组织内容和激活轻松导航数据的特定于平台的控件通过提高在这些情况下的用户体验。

为 `ListView`激活分组后，将为每个组添加一个标题行。

若要启用分组：

- 创建列表的列表 （组的列表，每个组正在元素的列表）。
- 将 `ListView`的 `ItemsSource` 设置为该列表。
- 将 `IsGroupingEnabled` 设置为 true。
- 将[`GroupDisplayBinding`](xref:Xamarin.Forms.ListView.GroupDisplayBinding)设置为绑定到用作组标题的组的属性。
- 可有可无将[`GroupShortNameBinding`](xref:Xamarin.Forms.ListView.GroupShortNameBinding)设置为绑定到用作组的短名称的组的属性。 短名称用于跳转列表 （在 iOS 上的右侧列）。

首先创建组的类：

```csharp
public class PageTypeGroup : List<PageModel>
    {
        public string Title { get; set; }
        public string ShortName { get; set; } //will be used for jump lists
        public string Subtitle { get; set; }
        private PageTypeGroup(string title, string shortName)
        {
            Title = title;
            ShortName = shortName;
        }

        public static IList<PageTypeGroup> All { private set; get; }
    }
```

在上面的代码中，`All` 是将作为绑定源提供给我们的 ListView 的列表。 `Title` 和 `ShortName` 是将用于组标题的属性。

在此阶段，`All` 为空列表。 添加静态构造函数，使程序启动时将填充列表：

```csharp
static PageTypeGroup()
{
    List<PageTypeGroup> Groups = new List<PageTypeGroup> {
            new PageTypeGroup ("Alpha", "A"){
                new PageModel("Amelia", "Cedar", new switchCellPage(),""),
                new PageModel("Alfie", "Spruce", new switchCellPage(), "grapefruit.jpg"),
                new PageModel("Ava", "Pine", new switchCellPage(), "grapefruit.jpg"),
                new PageModel("Archie", "Maple", new switchCellPage(), "grapefruit.jpg")
            },
            new PageTypeGroup ("Bravo", "B"){
                new PageModel("Brooke", "Lumia", new switchCellPage(),""),
                new PageModel("Bobby", "Xperia", new switchCellPage(), "grapefruit.jpg"),
                new PageModel("Bella", "Desire", new switchCellPage(), "grapefruit.jpg"),
                new PageModel("Ben", "Chocolate", new switchCellPage(), "grapefruit.jpg")
            }
        };
        All = Groups; //set the publicly accessible list
}
```

在上面的代码中，还可以对 `Groups`的元素调用 `Add`，这是 `PageTypeGroup`类型的实例。 此方法之所以可行，是因为 `PageTypeGroup` 继承自 `List<PageModel>`。

下面是用于显示分组的列表 XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DemoListView.GroupingViewPage"
    <ContentPage.Content>
        <ListView  x:Name="GroupedView"
                   GroupDisplayBinding="{Binding Title}"
                   GroupShortNameBinding="{Binding ShortName}"
                   IsGroupingEnabled="true">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <TextCell Text="{Binding Title}"
                              Detail="{Binding Subtitle}" />
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </ContentPage.Content>
</ContentPage>
```

此 XAML 执行以下操作：

- 将 `GroupShortNameBinding` 设置为在组类中定义的 `ShortName` 属性
- 将 `GroupDisplayBinding` 设置为在组类中定义的 `Title` 属性
- 将 `IsGroupingEnabled` 设置为 true
- 已将 `ListView`的 `ItemsSource` 更改为分组列表

下面的屏幕截图显示了生成的 UI：

![](customizing-list-appearance-images/grouping-depth.png "ListView Grouping Example")

### <a name="customizing-grouping"></a>自定义分组

如果在列表中已启用分组，也可以定制的组标头。

与 `ListView` 如何 `ItemTemplate` 来定义行的显示方式相似，`ListView` 具有 `GroupHeaderTemplate`。

自定义 XAML 中的组标头的示例如下所示：

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DemoListView.GroupingViewPage">
    <ContentPage.Content>
        <ListView x:Name="GroupedView"
                  GroupDisplayBinding="{Binding Title}"
                  GroupShortNameBinding="{Binding ShortName}"
                  IsGroupingEnabled="true">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <TextCell Text="{Binding Title}"
                              Detail="{Binding Subtitle}"
                              TextColor="#f35e20"
                              DetailColor="#503026" />
                </DataTemplate>
            </ListView.ItemTemplate>
            <!-- Group Header Customization-->
            <ListView.GroupHeaderTemplate>
                <DataTemplate>
                    <TextCell Text="{Binding Title}"
                              Detail="{Binding ShortName}"
                              TextColor="#f35e20"
                              DetailColor="#503026" />
                </DataTemplate>
            </ListView.GroupHeaderTemplate>
            <!-- End Group Header Customization -->
        </ListView>
    </ContentPage.Content>
</ContentPage>
```

## <a name="headers-and-footers"></a>页眉和页脚

很可能 ListView 显示页眉和页脚的向下滚动列表中的元素。 页眉和页脚可以是文本字符串或更复杂的布局。 此行为与[节组](#grouping)不同。

您可以将 `Header` 和/或 `Footer` 设置为 `string` 值，也可以将其设置为更复杂的布局。 还有 `HeaderTemplate` 和 `FooterTemplate` 属性，使您可以为支持数据绑定的页眉和页脚创建更复杂的布局。

若要创建基本页眉/页脚，只需将 "页眉" 或 "页脚" 属性设置为要显示的文本。 在代码中：

```csharp
ListView HeaderList = new ListView()
{
    Header = "Header",
    Footer = "Footer"
};
```

在 XAML:

```xaml
<ListView x:Name="HeaderList" 
          Header="Header"
          Footer="Footer">
    ...
</ListView>
```

![](customizing-list-appearance-images/header-default.png "ListView with Header and Footer")

若要创建自定义的页眉和页脚，请定义页眉和页脚视图：

```xaml
<ListView.Header>
    <StackLayout Orientation="Horizontal">
        <Label Text="Header"
               TextColor="Olive"
               BackgroundColor="Red" />
    </StackLayout>
</ListView.Header>
<ListView.Footer>
    <StackLayout Orientation="Horizontal">
        <Label Text="Footer"
               TextColor="Gray"
               BackgroundColor="Blue" />
    </StackLayout>
</ListView.Footer>
```

![](customizing-list-appearance-images/header-custom.png "ListView with Customized Header and Footer")

## <a name="scrollbar-visibility"></a>滚动条可见性

[`ListView`](xref:Xamarin.Forms.ListView)类具有 `HorizontalScrollBarVisibility` 和 `VerticalScrollBarVisibility` 属性，这些属性可获取或设置一个[`ScrollBarVisibility`](xref:Xamarin.Forms.ScrollBarVisibility)值，该值表示水平或垂直滚动条何时可见。 这两个属性都可以设置为以下值：

- [`Default`](xref:Xamarin.Forms.ScrollBarVisibility)指示平台的默认滚动条行为，是 `HorizontalScrollBarVisibility` 和 `VerticalScrollBarVisibility` 属性的默认值。
- [`Always`](xref:Xamarin.Forms.ScrollBarVisibility)指示滚动条可见，即使在视图中显示了内容也是如此。
- [`Never`](xref:Xamarin.Forms.ScrollBarVisibility)指示滚动条将不可见，即使内容无法在视图中显示也是如此。

## <a name="row-separators"></a>行分隔符

默认情况下，默认情况下，在 iOS 和 Android 上显示分隔符线 `ListView` 元素。 如果希望隐藏 iOS 和 Android 上的分隔线，请在 ListView 上设置 "`SeparatorVisibility`" 属性。 `SeparatorVisibility` 的选项如下：

- **默认值**-显示 IOS 和 Android 上的分隔线。
- **无**-隐藏所有平台上的分隔符。

默认可见性：

C#：

```csharp
SeparatorDemoListView.SeparatorVisibility = SeparatorVisibility.Default;
```

XAML:

```xaml
<ListView x:Name="SeparatorDemoListView" SeparatorVisibility="Default" />
```

![](customizing-list-appearance-images/separator-default.png "ListView with Default Row Separators")

“无”：

C#：

```csharp
SeparatorDemoListView.SeparatorVisibility = SeparatorVisibility.None;
```

XAML:

```xaml
<ListView x:Name="SeparatorDemoListView" SeparatorVisibility="None" />
```

![](customizing-list-appearance-images/separator-none.png "ListView without Row Separators")

还可以通过 `SeparatorColor` 属性设置分隔线的颜色：

C#：

```csharp
SeparatorDemoListView.SeparatorColor = Color.Green;
```

XAML:

```xaml
<ListView x:Name="SeparatorDemoListView" SeparatorColor="Green" />
```

![](customizing-list-appearance-images/separator-custom.png "ListView with Green Row Separators")

> [!NOTE]
> 在加载 `ListView` 后，在 Android 上设置这些属性中的任何一个会导致性能大幅下降。

## <a name="row-height"></a>行高

默认情况下，ListView 中的所有行都具有相同的高度。 ListView 有可用于更改该行为的两个属性：

- `HasUnevenRows` &ndash; `true`/`false` 值，如果设置为 `true`，则行具有不同的高度。 默认为 `false`。
- `RowHeight` &ndash; 在 `false``HasUnevenRows` 时设置每行的高度。

通过设置 `ListView`上的 `RowHeight` 属性，可以设置所有行的高度。

### <a name="custom-fixed-row-height"></a>自定义固定行高

C#：

```csharp
RowHeightDemoListView.RowHeight = 100;
```

XAML:

```xaml
<ListView x:Name="RowHeightDemoListView" RowHeight="100" />
```

![](customizing-list-appearance-images/height-custom.png "ListView with Fixed Row Height")

### <a name="uneven-rows"></a>非对称行

如果希望各个行具有不同的高度，可以将 `HasUnevenRows` 属性设置为 `true`。 一旦将 `HasUnevenRows` 设置为 `true`，就不必手动设置行高度，因为 Xamarin 会自动计算行高。

C#：

```csharp
RowHeightDemoListView.HasUnevenRows = true;
```

XAML:

```xaml
<ListView x:Name="RowHeightDemoListView" HasUnevenRows="true" />
```

![](customizing-list-appearance-images/height-uneven.png "ListView with Uneven Rows")

### <a name="resize-rows-at-runtime"></a>在运行时调整行的大小

可以在运行时以编程方式调整单个 `ListView` 行的大小，前提是 `HasUnevenRows` 属性设置为 `true`。 即使当前不可见， [`Cell.ForceUpdateSize`](xref:Xamarin.Forms.Cell.ForceUpdateSize)方法也会更新单元格的大小，如下面的代码示例所示：

```csharp
void OnImageTapped (object sender, EventArgs args)
{
    var image = sender as Image;
    var viewCell = image.Parent.Parent as ViewCell;

    if (image.HeightRequest < 250) {
        image.HeightRequest = image.Height + 100;
        viewCell.ForceUpdateSize ();
    }
}
```

`OnImageTapped` 事件处理程序是为了响应正在点击的单元格中的[`Image`](xref:Xamarin.Forms.Image)而执行的，它增加了显示在单元格中的 `Image` 大小，以便于查看。

![](customizing-list-appearance-images/dynamic-row-resizing.png "ListView with Runtime Row Resizing")

> [!WARNING]
> 过度运行时行调整大小可能会导致性能下降。

## <a name="related-links"></a>相关链接

- [分组（示例）](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-grouping)
- [自定义呈现器视图（示例）](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistviewnative)
- [动态调整行的大小（示例）](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-dynamicunevenlistcells)
- [1.4 发行说明](https://forums.xamarin.com/discussion/35451/xamarin-forms-1-4-0-released/)
- [1.3 发行说明](https://forums.xamarin.com/discussion/29934/xamarin-forms-1-3-0-released/)
