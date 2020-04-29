---
title: Xamarin Map 引脚
description: 本文介绍如何在 Xamarin. Forms 地图上创建 pin。
ms.prod: xamarin
ms.assetid: F8FC081B-A811-4FBB-B8F8-30D6FD36BD40
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/23/2019
ms.openlocfilehash: 3df78a7c8eaf12306ade182f134f8d294d203af5
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "82517587"
---
# <a name="xamarinforms-map-pins"></a>Xamarin Map 引脚

[![下载示例](~/media/shared/download.png)下载示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

Xamarin [`Map`](xref:Xamarin.Forms.Maps.Map)控件允许将位置标记为[`Pin`](xref:Xamarin.Forms.Maps.Pin)对象。 `Pin`是在点击时打开信息窗口的地图标记：

[![IOS 和 Android 上的地图 pin 及其信息窗口的屏幕截图](pins-images/pin-and-information-window.png "用信息窗口映射 pin")](pins-images/pin-and-information-window-large.png#lightbox "用信息窗口映射 pin")

将[`Pin`](xref:Xamarin.Forms.Maps.Pin)对象添加到[`Map.Pins`](xref:Xamarin.Forms.Maps.Pin)集合时，会在地图上呈现该固定。

[`Pin`](xref:Xamarin.Forms.Maps.Pin)类具有以下属性：

- [`Address`](xref:Xamarin.Forms.Maps.Pin.Address)，类型`string`为，通常表示 pin 位置的地址。 但是，它可以是任何`string`内容，而不仅仅是地址。
- [`Label`](xref:Xamarin.Forms.Maps.Pin.Label)，类型`string`为，通常表示 pin 标题。
- [`Position`](xref:Xamarin.Forms.Maps.Pin.Position)，类型[`Position`](xref:Xamarin.Forms.Maps.Position)为，表示 pin 的纬度和经度。
- [`Type`](xref:Xamarin.Forms.Maps.Pin.Type)，类型[`PinType`](xref:Xamarin.Forms.Maps.PinType)为，表示 pin 类型。

这些属性是由[`BindableProperty`](xref:Xamarin.Forms.BindableProperty)对象支持的，这意味着`Pin`可以是数据绑定的目标。 有关数据绑定`Pin`对象的详细信息，请参阅[显示固定的集合](#display-a-pin-collection)。

此外， [`Pin`](xref:Xamarin.Forms.Maps.Pin)类还定义`MarkerClicked`和`InfoWindowClicked`事件。 点击`MarkerClicked`某个 pin 时，将触发该事件，并在`InfoWindowClicked`点击信息窗口时激发该事件。 同时`PinClickedEventArgs`提供两个事件的对象具有类型`HideInfoWindow` `bool`为的单个属性。

## <a name="display-a-pin"></a>显示 pin

[`Pin`](xref:Xamarin.Forms.Maps.Pin)可以[`Map`](xref:Xamarin.Forms.Maps.Map)在 XAML 中将添加到：

```xaml
<ContentPage ...
             xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps">
     <maps:Map x:Name="map"
               IsShowingUser="True"
               MoveToLastRegionOnLayoutChange="False">
         <x:Arguments>
             <maps:MapSpan>
                 <x:Arguments>
                     <maps:Position>
                         <x:Arguments>
                             <x:Double>36.9628066</x:Double>
                             <x:Double>-122.0194722</x:Double>
                         </x:Arguments>
                     </maps:Position>
                     <x:Double>0.01</x:Double>
                     <x:Double>0.01</x:Double>
                 </x:Arguments>
             </maps:MapSpan>
         </x:Arguments>
         <maps:Map.Pins>
             <maps:Pin Label="Santa Cruz"
                       Address="The city with a boardwalk"
                       Type="Place">
                 <maps:Pin.Position>
                     <maps:Position>
                         <x:Arguments>
                             <x:Double>36.9628066</x:Double>
                             <x:Double>-122.0194722</x:Double>
                         </x:Arguments>
                     </maps:Position>
                 </maps:Pin.Position>
             </maps:Pin>
         </maps:Map.Pins>
     </maps:Map>
</ContentPage>
```

此 XAML 将创建[`Map`](xref:Xamarin.Forms.Maps.Map)一个对象，该对象显示[`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan)对象指定的区域。 `MapSpan`对象在由[`Position`](xref:Xamarin.Forms.Maps.Position)对象表示的纬度和经度上居中，这会延长0.01 的纬度和经度度。 [`Pin`](xref:Xamarin.Forms.Maps.Pin)对象将添加到[`Map.Pins`](xref:Xamarin.Forms.Maps.Pin)集合中，并在上`Map`的[`Position`](xref:Xamarin.Forms.Maps.Pin.Position)属性所指定的位置绘制。 有关[`Position`](xref:Xamarin.Forms.Maps.Position)结构的信息，请参阅[地图位置和距离](position-distance.md)。 有关将 XAML 中的参数传递给缺少默认构造函数的对象的信息，请参阅[在 xaml 中传递参数](~/xamarin-forms/xaml/passing-arguments.md)。

等效 C# 代码如下：

```csharp
using Xamarin.Forms.Maps;
// ...
Map map = new Map
{
  // ...
};
Pin pin = new Pin
{
  Label = "Santa Cruz",
  Address = "The city with a boardwalk",
  Type = PinType.Place,
  Position = new Position(36.9628066, -122.0194722)
};
map.Pins.Add(pin);
```

> [!WARNING]
> 如果将[`Pin.Label`](xref:Xamarin.Forms.Maps.Pin.Label)属性设置为，则在将`ArgumentException`添加到时[`Pin`](xref:Xamarin.Forms.Maps.Pin) ，将导致引发[`Map`](xref:Xamarin.Forms.Maps.Map)。

此示例代码会导致在地图上呈现一个插针：

[![IOS 和 Android 上的地图 pin 的屏幕截图](pins-images/pin-only.png "地图 pin")](pins-images/pin-only-large.png#lightbox "地图 pin")

## <a name="interact-with-a-pin"></a>与 pin 交互

默认情况下，当[`Pin`](xref:Xamarin.Forms.Maps.Pin)点击其信息窗口时，将显示：

[![IOS 和 Android 上的地图 pin 及其信息窗口的屏幕截图](pins-images/pin-and-information-window.png "用信息窗口映射 pin")](pins-images/pin-and-information-window-large.png#lightbox "用信息窗口映射 pin")

点击地图上的其他位置会关闭信息窗口。

[`Pin`](xref:Xamarin.Forms.Maps.Pin)类定义一个`MarkerClicked`事件，该事件在点击时`Pin`被触发。 不需要处理此事件以显示信息窗口。 相反，当需要通知已点击特定 pin 时，应处理此事件。

[`Pin`](xref:Xamarin.Forms.Maps.Pin)类还定义在点击`InfoWindowClicked`信息窗口时触发的事件。 当需要通知已点击特定信息窗口时，应处理此事件。

下面的代码显示了处理这些事件的示例：

```csharp
using Xamarin.Forms.Maps;
// ...
Pin boardwalkPin = new Pin
{
    Position = new Position(36.9641949, -122.0177232),
    Label = "Boardwalk",
    Address = "Santa Cruz",
    Type = PinType.Place
};
boardwalkPin.MarkerClicked += async (s, args) =>
{
    args.HideInfoWindow = true;
    string pinName = ((Pin)s).Label;
    await DisplayAlert("Pin Clicked", $"{pinName} was clicked.", "Ok");
};

Pin wharfPin = new Pin
{
    Position = new Position(36.9571571, -122.0173544),
    Label = "Wharf",
    Address = "Santa Cruz",
    Type = PinType.Place
};
wharfPin.InfoWindowClicked += async (s, args) =>
{
    string pinName = ((Pin)s).Label;
    await DisplayAlert("Info Window Clicked", $"The info window was clicked for {pinName}.", "Ok");
};
```

同时`PinClickedEventArgs`提供两个事件的对象具有类型`HideInfoWindow` `bool`为的单个属性。 如果将此属性设置为`true`事件处理程序内部，将隐藏信息窗口。

## <a name="pin-types"></a>固定类型

[`Pin`](xref:Xamarin.Forms.Maps.Pin)对象包括类型[`Type`](xref:Xamarin.Forms.Maps.Pin.Type) [`PinType`](xref:Xamarin.Forms.Maps.PinType)为的属性，表示 pin 类型。 `PinType` 枚举定义下列成员：

- `Generic`表示泛型 pin。
- `Place`表示位置的 pin。
- `SavedPin`表示保存位置的 pin。
- `SearchResult`表示搜索结果的 pin。

但是，将[`Pin.Type`](xref:Xamarin.Forms.Maps.Pin.Type)属性设置为任何[`PinType`](xref:Xamarin.Forms.Maps.PinType)成员不会更改呈现的 pin 的外观。 相反，您必须创建自定义呈现器来自定义 pin 外观。 有关详细信息，请参阅[自定义地图 pin](~/xamarin-forms/app-fundamentals/custom-renderer/map-pin.md)。

## <a name="display-a-pin-collection"></a>显示固定集合

[`Map`](xref:Xamarin.Forms.Maps.Map)类定义以下属性：

- [`ItemsSource`](xref:Xamarin.Forms.Maps.Map.ItemsSource)，类型`IEnumerable`为，它指定要显示的`IEnumerable`项的集合。
- [`ItemTemplate`](xref:Xamarin.Forms.Maps.Map.ItemTemplate)，类型[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)为，它指定要[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)应用于显示的项集合中的每个项的。
- `ItemTemplateSelector`，类型[`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector)为，它指定[`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector)将用于在运行时[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)为项选择。

> [!IMPORTANT]
> 当[`ItemTemplate`](xref:Xamarin.Forms.Maps.Map.ItemTemplate)设置了`ItemTemplate`和`ItemTemplateSelector`属性时，属性将优先。

[`Map`](xref:Xamarin.Forms.Maps.Map)可以通过使用数据绑定将其[`ItemsSource`](xref:Xamarin.Forms.Maps.Map.ItemsSource)属性绑定到`IEnumerable`集合来填充 pin：

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps"
             x:Class="WorkingWithMaps.PinItemsSourcePage">
    <Grid>
        ...
        <maps:Map x:Name="map"
                  ItemsSource="{Binding Locations}">
            <maps:Map.ItemTemplate>
                <DataTemplate>
                    <maps:Pin Position="{Binding Position}"
                              Address="{Binding Address}"
                              Label="{Binding Description}" />
                </DataTemplate>
            </maps:Map.ItemTemplate>
        </maps:Map>
        ...
    </Grid>
</ContentPage>
```

[`ItemsSource`](xref:Xamarin.Forms.Maps.Map.ItemsSource)属性数据绑定到连接的`Locations` viewmodel 的属性，该属性返回对象`ObservableCollection`的`Location` ，这是一个自定义类型。 每`Location`个对象`Address`都`Description`定义类型的和`string`属性，并`Position`定义类型[`Position`](xref:Xamarin.Forms.Maps.Position)的属性和属性。

`IEnumerable`通过将[`ItemTemplate`](xref:Xamarin.Forms.Maps.Map.ItemTemplate)属性设置为[`DataTemplate`](xref:Xamarin.Forms.DataTemplate) ，可以定义集合中每个项的外观，方法是将[`Pin`](xref:Xamarin.Forms.Maps.Pin)属性设置为，其中包含数据绑定到相应属性的对象。

以下屏幕截图显示了[`Map`](xref:Xamarin.Forms.Maps.Map)使用数据[`Pin`](xref:Xamarin.Forms.Maps.Pin)绑定显示集合的内容：

[![在 iOS 和 Android 上带有数据绑定插针的地图屏幕截图](pins-images/pins-itemsource.png "具有数据绑定插针的映射")](pins-images/pins-itemsource-large.png#lightbox "具有数据绑定插针的映射")

### <a name="choose-item-appearance-at-runtime"></a>在运行时选择项外观

可以在运行时根据项值`IEnumerable`选择集合中每个项的外观，方法是将`ItemTemplateSelector`属性设置为： [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector)

```xaml
<ContentPage ...
             xmlns:local="clr-namespace:WorkingWithMaps"
             xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps">
    <ContentPage.Resources>
        <local:MapItemTemplateSelector x:Key="MapItemTemplateSelector">
            <local:MapItemTemplateSelector.DefaultTemplate>
                <DataTemplate>
                    <maps:Pin Position="{Binding Position}"
                              Address="{Binding Address}"
                              Label="{Binding Description}" />
                </DataTemplate>
            </local:MapItemTemplateSelector.DefaultTemplate>
            <local:MapItemTemplateSelector.XamarinTemplate>
                <DataTemplate>
                    <!-- Change the property values, or the properties that are bound to. -->
                    <maps:Pin Position="{Binding Position}"
                              Address="{Binding Address}"
                              Label="Xamarin!" />
                </DataTemplate>
            </local:MapItemTemplateSelector.XamarinTemplate>    
        </local:MapItemTemplateSelector>
    </ContentPage.Resources>

    <Grid>
        ...
        <maps:Map x:Name="map"
                  ItemsSource="{Binding Locations}"
                  ItemTemplateSelector="{StaticResource MapItemTemplateSelector}" />
        ...
    </Grid>
</ContentPage>
```

下面的示例演示了`MapItemTemplateSelector`类：

```csharp
public class MapItemTemplateSelector : DataTemplateSelector
{
    public DataTemplate DefaultTemplate { get; set; }
    public DataTemplate XamarinTemplate { get; set; }

    protected override DataTemplate OnSelectTemplate(object item, BindableObject container)
    {
        return ((Location)item).Address.Contains("San Francisco") ? XamarinTemplate : DefaultTemplate;
    }
}
```

`MapItemTemplateSelector`类定义`DefaultTemplate`了并`XamarinTemplate` [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)将属性设置为不同的数据模板。 `OnSelectTemplate`方法返回`XamarinTemplate`，当在点击时将 "Xamarin" 作为标签`Pin`显示，当项的地址包含 "旧金山" 时。 如果项没有包含 "旧金山" 的地址，则该`OnSelectTemplate`方法将返回。 `DefaultTemplate`

> [!NOTE]
> 此功能的一个用例是基于子类型将子归类[`Pin`](xref:Xamarin.Forms.Maps.Pin)对象的属性绑定到不同的`Pin`属性。

有关数据模板选择器的详细信息，请参阅[创建 Xamarin。窗体并重](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)。

## <a name="related-links"></a>相关链接

- [地图示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)
- [映射自定义呈现器](~/xamarin-forms/app-fundamentals/custom-renderer/map-pin.md)
- [在 XAML 中传递参数](~/xamarin-forms/xaml/passing-arguments.md)
- [创建 Xamarin.Forms DataTemplateSelector](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
