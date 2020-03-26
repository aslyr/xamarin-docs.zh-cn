---
title: 采用 XAML 的本机视图
description: 从 iOS、 Android 和通用 Windows 平台的本机视图可以直接引用 Xamarin.Forms XAML 文件中。 上的本机视图，可以设置属性和事件处理程序，它们可以与 Xamarin.Forms 视图进行交互。 本文演示如何使用 Xamarin.Forms XAML 文件中的本机视图。
ms.prod: xamarin
ms.assetid: 7A856D31-B300-409E-9AEB-F8A4DB99B37E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/23/2019
ms.openlocfilehash: 6d3954f44cd769ed02535eb260b9952e81e67c98
ms.sourcegitcommit: d83c6af42ed26947aa7c0ecfce00b9ef60f33319
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/25/2020
ms.locfileid: "80247621"
---
# <a name="native-views-in-xaml"></a>采用 XAML 的本机视图

[![下载示例](~/media/shared/download.png) 下载示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-nativeviews-nativeswitch)

_可以直接从 Xamarin、Android 和通用 Windows 平台中引用 iOS、Android 和的本机视图。可以在本机视图上设置属性和事件处理程序，并且可以与 Xamarin. Forms 视图进行交互。本文演示如何从 Xamarin XAML 文件使用本机视图。_

本文讨论以下主题：

- [使用本机视图](#consuming)–从 XAML 使用本机视图的过程。
- [使用本机绑定](#native_bindings)–与本机视图的属性之间的数据绑定。
- [将自变量传递给本机](#passing_arguments)视图–将参数传递给本机视图构造函数并调用本机视图工厂方法。
- [从代码引用本机视图](#native_view_code)–从代码隐藏文件检索在 XAML 文件中声明的本机视图实例。
- [子类本机视图](#subclassing)–为定义 XAML 友好 API 定义 XAML 本机视图。  

<a name="overview" />

## <a name="overview"></a>概述

若要嵌入到 Xamarin.Forms XAML 文件的本机视图：

1. 为包含本机视图的命名空间在 XAML 文件中添加 `xmlns` 命名空间声明。
1. 在 XAML 文件中创建本机视图的实例。

> [!IMPORTANT]
> 对于任何使用本机视图的 XAML 页，必须禁用已编译的 XAML。 这可以通过使用 `[XamlCompilation(XamlCompilationOptions.Skip)]` 特性修饰 XAML 页的代码隐藏类来实现。 有关 XAML 编译的详细信息，请参阅[Xamarin 中的 XAML 编译](~/xamarin-forms/xaml/xamlc.md)。

若要从代码隐藏文件引用本机视图，必须使用共享资产项目 (SAP)，并使用条件编译指令将特定于平台的代码包装。 有关详细信息，请参阅[从代码引用本机视图](#native_view_code)。

<a name="consuming" />

## <a name="consuming-native-views"></a>使用本机视图

下面的代码示例演示如何将每个平台的本机视图用于 Xamarin。 Forms [`ContentPage`](xref:Xamarin.Forms.ContentPage)：

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
        xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
        xmlns:ios="clr-namespace:UIKit;assembly=Xamarin.iOS;targetPlatform=iOS"
        xmlns:androidWidget="clr-namespace:Android.Widget;assembly=Mono.Android;targetPlatform=Android"
        xmlns:androidLocal="clr-namespace:SimpleColorPicker.Droid;assembly=SimpleColorPicker.Droid;targetPlatform=Android"
        xmlns:win="clr-namespace:Windows.UI.Xaml.Controls;assembly=Windows, Version=255.255.255.255,
            Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        x:Class="NativeViews.NativeViewDemo">
    <StackLayout Margin="20">
        <ios:UILabel Text="Hello World" TextColor="{x:Static ios:UIColor.Red}" View.HorizontalOptions="Start" />
        <androidWidget:TextView Text="Hello World" x:Arguments="{x:Static androidLocal:MainActivity.Instance}" />
        <win:TextBlock Text="Hello World" />
    </StackLayout>
</ContentPage>
```

还必须指定 `targetPlatform`，同时指定本机视图命名空间的 `clr-namespace` 和 `assembly`。 应将此设置为 `iOS`、`Android`、`UWP`、`Windows` （等效于 `UWP`）、`macOS`、`GTK`、`Tizen`或 `WPF`。 在运行时，XAML 分析器将忽略任何与运行应用程序的平台不匹配的 `targetPlatform` 的 XML 命名空间前缀。

每个命名空间声明可以用于引用来自指定命名空间的任何类或结构。 例如，可以使用 `ios` 命名空间声明来引用 iOS `UIKit` 命名空间中的任何类或结构。 可以通过 XAML，设置本机视图的属性，但属性和对象类型必须匹配。 例如，使用 `x:Static` 标记扩展和 `ios` 命名空间，将 `UILabel.TextColor` 属性设置为 `UIColor.Red`。

通过使用 `Class.BindableProperty="value"` 语法，还可以在本机视图上设置可绑定属性和附加的可绑定属性。 每个本机视图都包装在特定于平台的 `NativeViewWrapper` 实例中，该实例从[`Xamarin.Forms.View`](xref:Xamarin.Forms.View)类派生。 本机视图上设置可绑定属性或附加可绑定属性传输到包装器的属性值。 例如，可以通过在本机视图上设置 `View.HorizontalOptions="Center"` 来指定居中的水平布局。

> [!NOTE]
> 请注意，样式不能用于本机视图，因为样式只能定位由 `BindableProperty` 对象支持的属性。

Android 小组件构造函数通常要求将 Android `Context` 对象作为参数，并且通过 `MainActivity` 类中的静态属性可实现此功能。 因此，在 XAML 中创建 Android 小组件时，通常必须使用具有 `x:Static` 标记扩展的 `x:Arguments` 属性将 `Context` 对象传递给小组件的构造函数。 有关详细信息，请参阅[将参数传递给本机视图](#passing_arguments)。

> [!NOTE]
> 请注意，在 .NET Standard 库项目或共享资产项目（SAP）中，不能将本机视图命名为 `x:Name`。 执行此操作将生成的变量的本机类型，这将导致编译错误。 但是，本机视图可以包装在 `ContentView` 实例中并在代码隐藏文件中检索，前提是使用了 SAP。 有关详细信息，请参阅[从代码引用本机视图](#native_view_code)。

<a name="native_bindings" />

## <a name="native-bindings"></a>本机绑定

数据绑定用来同步其数据源，使用 UI，并简化 Xamarin.Forms 应用程序显示的方式，并使用其数据进行交互。 如果源对象实现 `INotifyPropertyChanged` 接口，则*源*对象中的更改会由绑定框架自动推送到*目标*对象，并且可以选择将*目标*对象中的更改推送到*源*对象。

本机视图的属性还可以使用数据绑定。 下面的代码示例演示如何使用本机视图的属性的数据绑定：

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
        xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
        xmlns:ios="clr-namespace:UIKit;assembly=Xamarin.iOS;targetPlatform=iOS"
        xmlns:androidWidget="clr-namespace:Android.Widget;assembly=Mono.Android;targetPlatform=Android"
        xmlns:androidLocal="clr-namespace:SimpleColorPicker.Droid;assembly=SimpleColorPicker.Droid;targetPlatform=Android"
        xmlns:win="clr-namespace:Windows.UI.Xaml.Controls;assembly=Windows, Version=255.255.255.255,
            Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        xmlns:local="clr-namespace:NativeSwitch"
        x:Class="NativeSwitch.NativeSwitchPage">
    <StackLayout Margin="20">
        <Label Text="Native Views Demo" FontAttributes="Bold" HorizontalOptions="Center" />
        <Entry Placeholder="This Entry is bound to the native switch" IsEnabled="{Binding IsSwitchOn}" />
        <ios:UISwitch On="{Binding Path=IsSwitchOn, Mode=TwoWay, UpdateSourceEventName=ValueChanged}"
            OnTintColor="{x:Static ios:UIColor.Red}"
            ThumbTintColor="{x:Static ios:UIColor.Blue}" />
        <androidWidget:Switch x:Arguments="{x:Static androidLocal:MainActivity.Instance}"
            Checked="{Binding Path=IsSwitchOn, Mode=TwoWay, UpdateSourceEventName=CheckedChange}"
            Text="Enable Entry?" />
        <win:ToggleSwitch Header="Enable Entry?"
            OffContent="No"
            OnContent="Yes"
            IsOn="{Binding IsSwitchOn, Mode=TwoWay, UpdateSourceEventName=Toggled}" />
    </StackLayout>
</ContentPage>

```

页面包含一个[`Entry`](xref:Xamarin.Forms.Entry) ，其[`IsEnabled`](xref:Xamarin.Forms.VisualElement.IsEnabled)属性绑定到 `NativeSwitchPageViewModel.IsSwitchOn` 属性。 页面的[`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)设置为代码隐藏文件中 `NativeSwitchPageViewModel` 类的新实例，并具有实现 `INotifyPropertyChanged` 接口的 ViewModel 类。

此页还包含用于每个平台的本机交换机。 每个本机开关使用[`TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay)绑定来更新 `NativeSwitchPageViewModel.IsSwitchOn` 属性的值。 因此，当开关处于关闭状态时，`Entry` 处于禁用状态，并且当开关处于开启状态时，将启用 `Entry`。 下面的屏幕截图显示了每个平台上的此功能：

![](xaml-images/native-switch-disabled.png "Native Switch Disabled")
![](xaml-images/native-switch-enabled.png "Native Switch Enabled")

如果本机属性实现 `INotifyPropertyChanged`，或在 iOS 上支持键值观察（KVO），或者它是 UWP `DependencyProperty`，则自动支持双向绑定。 但是，许多本机视图不支持属性更改通知。 对于这些视图，可以指定[`UpdateSourceEventName`](xref:Xamarin.Forms.Binding.UpdateSourceEventName)属性值作为绑定表达式的一部分。 此属性应设置为目标属性已更改时发出信号的本机视图中的事件的名称。 然后，当本机开关的值发生更改时，会通知 `Binding` 类用户已更改开关值，并且 `NativeSwitchPageViewModel.IsSwitchOn` 属性值已更新。

<a name="passing_arguments" />

## <a name="passing-arguments-to-native-views"></a>传递自变量到本机视图

构造函数参数可以通过使用 `x:Static` 标记扩展的 `x:Arguments` 特性传递到本机视图。 此外，可以通过使用 `x:FactoryMethod` 属性指定方法的名称，并使用 `x:Arguments` 特性来指定方法的名称，使用本机视图工厂方法（`public static` 方法返回与定义方法的类或结构相同的类型的对象或值）。

下面的代码示例演示了这两种技术：

```xaml
<ContentPage ...
        xmlns:ios="clr-namespace:UIKit;assembly=Xamarin.iOS;targetPlatform=iOS"
        xmlns:androidWidget="clr-namespace:Android.Widget;assembly=Mono.Android;targetPlatform=Android"
        xmlns:androidGraphics="clr-namespace:Android.Graphics;assembly=Mono.Android;targetPlatform=Android"
        xmlns:androidLocal="clr-namespace:SimpleColorPicker.Droid;assembly=SimpleColorPicker.Droid;targetPlatform=Android"
        xmlns:winControls="clr-namespace:Windows.UI.Xaml.Controls;assembly=Windows, Version=255.255.255.255, Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        xmlns:winMedia="clr-namespace:Windows.UI.Xaml.Media;assembly=Windows, Version=255.255.255.255, Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        xmlns:winText="clr-namespace:Windows.UI.Text;assembly=Windows, Version=255.255.255.255, Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        xmlns:winui="clr-namespace:Windows.UI;assembly=Windows, Version=255.255.255.255, Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows">
        ...
        <ios:UILabel Text="Simple Native Color Picker" View.HorizontalOptions="Center">
            <ios:UILabel.Font>
                <ios:UIFont x:FactoryMethod="FromName">
                    <x:Arguments>
                        <x:String>Papyrus</x:String>
                        <x:Single>24</x:Single>
                    </x:Arguments>
                </ios:UIFont>
            </ios:UILabel.Font>
        </ios:UILabel>
        <androidWidget:TextView x:Arguments="{x:Static androidLocal:MainActivity.Instance}"
                    Text="Simple Native Color Picker"
                    TextSize="24"
                    View.HorizontalOptions="Center">
            <androidWidget:TextView.Typeface>
                <androidGraphics:Typeface x:FactoryMethod="Create">
                    <x:Arguments>
                        <x:String>cursive</x:String>
                        <androidGraphics:TypefaceStyle>Normal</androidGraphics:TypefaceStyle>
                    </x:Arguments>
                </androidGraphics:Typeface>
            </androidWidget:TextView.Typeface>
        </androidWidget:TextView>
        <winControls:TextBlock Text="Simple Native Color Picker"
                    FontSize="20"
                    FontStyle="{x:Static winText:FontStyle.Italic}"
                    View.HorizontalOptions="Center">
            <winControls:TextBlock.FontFamily>
                <winMedia:FontFamily>
                    <x:Arguments>
                        <x:String>Georgia</x:String>
                    </x:Arguments>
                </winMedia:FontFamily>
            </winControls:TextBlock.FontFamily>
        </winControls:TextBlock>
        ...
</ContentPage>
```

[`UIFont.FromName`](xref:UIKit.UIFont.FromName*)工厂方法用于将[`UILabel.Font`](xref:UIKit.UILabel.Font)属性设置为 iOS 上的新[`UIFont`](xref:UIKit.UIFont) 。 `UIFont` 名称和大小由作为 `x:Arguments` 特性的子级的方法参数指定。

[`Typeface.Create`](xref:Android.Graphics.Typeface.Create*)工厂方法用于将[`TextView.Typeface`](xref:Android.Widget.TextView.Typeface)属性设置为 Android 上的新[`Typeface`](xref:Android.Graphics.Typeface) 。 `Typeface` 系列名称和样式由作为 `x:Arguments` 属性的子级的方法参数指定。

[`FontFamily`](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.fontfamily)构造函数用于将[`TextBlock.FontFamily`](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.fontfamily)属性设置为通用 Windows 平台（UWP）上的新 `FontFamily`。 `FontFamily` 名称由作为 `x:Arguments` 属性的子元素的方法参数指定。

> [!NOTE]
> 参数必须与构造函数或工厂方法所需的类型匹配。

以下屏幕截图显示指定工厂方法和构造函数参数，若要将字体设置不同的本机视图上的结果：

![](xaml-images/passing-arguments.png "Setting Fonts on Native Views")

有关在 XAML 中传递参数的详细信息，请参阅[在 xaml 中传递参数](~/xamarin-forms/xaml/passing-arguments.md)。

<a name="native_view_code" />

## <a name="referring-to-native-views-from-code"></a>从代码中引用的本机视图

尽管不能使用 `x:Name` 特性来命名本机视图，但可以从共享访问项目中的代码隐藏文件中检索在 XAML 文件中声明的本机视图实例，前提是本机视图是指定 `x:Name` 属性值的[`ContentView`](xref:Xamarin.Forms.ContentView)的子实例。 然后，在代码隐藏文件中的条件编译指令内您应该：

1. 检索[`ContentView.Content`](xref:Xamarin.Forms.ContentView.Content)属性值，并将其转换为特定于平台的 `NativeViewWrapper` 类型。
1. 检索 `NativeViewWrapper.NativeElement` 属性，并将其转换为本机视图类型。

然后执行所需的操作的本机视图调用本机 API。 此方法还提供了一项好处，即不同平台的多个 XAML 本机视图可以是同一[`ContentView`](xref:Xamarin.Forms.ContentView)的子视图。 下面的代码示例演示了此种方法：

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
        xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
        xmlns:ios="clr-namespace:UIKit;assembly=Xamarin.iOS;targetPlatform=iOS"
        xmlns:androidWidget="clr-namespace:Android.Widget;assembly=Mono.Android;targetPlatform=Android"
        xmlns:androidLocal="clr-namespace:SimpleColorPicker.Droid;assembly=SimpleColorPicker.Droid;targetPlatform=Android"
        xmlns:winControls="clr-namespace:Windows.UI.Xaml.Controls;assembly=Windows, Version=255.255.255.255,
            Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        xmlns:local="clr-namespace:NativeViewInsideContentView"
        x:Class="NativeViewInsideContentView.NativeViewInsideContentViewPage">
    <StackLayout Margin="20">
        <ContentView x:Name="contentViewTextParent" HorizontalOptions="Center" VerticalOptions="CenterAndExpand">
            <ios:UILabel Text="Text in a UILabel" TextColor="{x:Static ios:UIColor.Red}" />
            <androidWidget:TextView x:Arguments="{x:Static androidLocal:MainActivity.Instance}"
                Text="Text in a TextView" />
              <winControls:TextBlock Text="Text in a TextBlock" />
        </ContentView>
        <ContentView x:Name="contentViewButtonParent" HorizontalOptions="Center" VerticalOptions="EndAndExpand">
            <ios:UIButton TouchUpInside="OnButtonTap" View.HorizontalOptions="Center" View.VerticalOptions="Center" />
            <androidWidget:Button x:Arguments="{x:Static androidLocal:MainActivity.Instance}"
                Text="Scale and Rotate Text"
                Click="OnButtonTap" />
            <winControls:Button Content="Scale and Rotate Text" />
        </ContentView>
    </StackLayout>
</ContentPage>
```

在上面的示例中，每个平台的本机视图都是[`ContentView`](xref:Xamarin.Forms.ContentView)控件的子控件，其 `x:Name` 特性值用于检索代码隐藏中的 `ContentView`：

```csharp
public partial class NativeViewInsideContentViewPage : ContentPage
{
    public NativeViewInsideContentViewPage()
    {
        InitializeComponent();

#if __IOS__
        var wrapper = (Xamarin.Forms.Platform.iOS.NativeViewWrapper)contentViewButtonParent.Content;
        var button = (UIKit.UIButton)wrapper.NativeView;
        button.SetTitle("Scale and Rotate Text", UIKit.UIControlState.Normal);
        button.SetTitleColor(UIKit.UIColor.Black, UIKit.UIControlState.Normal);
#endif
#if __ANDROID__
        var wrapper = (Xamarin.Forms.Platform.Android.NativeViewWrapper)contentViewTextParent.Content;
        var textView = (Android.Widget.TextView)wrapper.NativeView;
        textView.SetTextColor(Android.Graphics.Color.Red);
#endif
#if WINDOWS_UWP
        var textWrapper = (Xamarin.Forms.Platform.UWP.NativeViewWrapper)contentViewTextParent.Content;
        var textBlock = (Windows.UI.Xaml.Controls.TextBlock)textWrapper.NativeElement;
        textBlock.Foreground = new Windows.UI.Xaml.Media.SolidColorBrush(Windows.UI.Colors.Red);
        var buttonWrapper = (Xamarin.Forms.Platform.UWP.NativeViewWrapper)contentViewButtonParent.Content;
        var button = (Windows.UI.Xaml.Controls.Button)buttonWrapper.NativeElement;
        button.Click += (sender, args) => OnButtonTap(sender, EventArgs.Empty);
#endif
    }

    async void OnButtonTap(object sender, EventArgs e)
    {
        contentViewButtonParent.Content.IsEnabled = false;
        contentViewTextParent.Content.ScaleTo(2, 2000);
        await contentViewTextParent.Content.RotateTo(360, 2000);
        contentViewTextParent.Content.ScaleTo(1, 2000);
        await contentViewTextParent.Content.RelRotateTo(360, 2000);
        contentViewButtonParent.Content.IsEnabled = true;
    }
}
```

访问[`ContentView.Content`](xref:Xamarin.Forms.ContentView.Content)属性可将包装的本机视图作为特定于平台的 `NativeViewWrapper` 实例检索。 然后，将访问 `NativeViewWrapper.NativeElement` 属性以检索本机视图的本机类型。 然后调用本机视图 API 来执行所需的操作。

IOS 和 Android 本机按钮共享同一 `OnButtonTap` 事件处理程序，因为每个本机按钮都使用 `EventHandler` 委托来响应触控事件。 但是，通用 Windows 平台（UWP）使用单独的 `RoutedEventHandler`，而在本示例中，将使用 `OnButtonTap` 事件处理程序。 因此，当单击 "本机" 按钮时，`OnButtonTap` 事件处理程序将执行，该事件处理程序将缩放和旋转包含在名为 `contentViewTextParent`的[`ContentView`](xref:Xamarin.Forms.ContentView)中的本机控件。 下面的屏幕截图演示了此每个平台上发生：

![](xaml-images/contentview.png "ContentView Containing a Native Control")

<a name="subclassing" />

## <a name="subclassing-native-views"></a>子类化本机视图

许多 iOS 和 Android 的本机视图并不适合因为它们使用的方法，而不是属性，若要设置该控件在 XAML 中实例化。 此问题的解决方案是为子类中定义多个 XAML 友好 API，使用属性来设置该控件，并使用独立于平台的事件的包装器的本机视图。 已包装的本机视图可放置在共享资产项目 (SAP) 和使用条件编译指令括住或置于特定于平台的项目中和从 XAML 引用.NET Standard 库项目中。

下面的代码示例演示了使用 Xamarin.Forms 页面创建子类的本机视图：

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
        xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
        xmlns:ios="clr-namespace:UIKit;assembly=Xamarin.iOS;targetPlatform=iOS"
        xmlns:iosLocal="clr-namespace:SubclassedNativeControls.iOS;assembly=SubclassedNativeControls.iOS;targetPlatform=iOS"
        xmlns:android="clr-namespace:Android.Widget;assembly=Mono.Android;targetPlatform=Android"
        xmlns:androidLocal="clr-namespace:SimpleColorPicker.Droid;assembly=SimpleColorPicker.Droid;targetPlatform=Android"
        xmlns:androidLocal="clr-namespace:SubclassedNativeControls.Droid;assembly=SubclassedNativeControls.Droid;targetPlatform=Android"
        xmlns:winControls="clr-namespace:Windows.UI.Xaml.Controls;assembly=Windows, Version=255.255.255.255,
            Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        xmlns:local="clr-namespace:SubclassedNativeControls"
        x:Class="SubclassedNativeControls.SubclassedNativeControlsPage">
    <StackLayout Margin="20">
        <Label Text="Subclassed Native Views Demo" FontAttributes="Bold" HorizontalOptions="Center" />
        <StackLayout Orientation="Horizontal">
          <Label Text="You have chosen:" />
          <Label Text="{Binding SelectedFruit}" />      
        </StackLayout>
        <iosLocal:MyUIPickerView ItemsSource="{Binding Fruits}"
            SelectedItem="{Binding SelectedFruit, Mode=TwoWay, UpdateSourceEventName=SelectedItemChanged}" />
        <androidLocal:MySpinner x:Arguments="{x:Static androidLocal:MainActivity.Instance}"
            ItemsSource="{Binding Fruits}"
            SelectedObject="{Binding SelectedFruit, Mode=TwoWay, UpdateSourceEventName=ItemSelected}" />
        <winControls:ComboBox ItemsSource="{Binding Fruits}"
            SelectedItem="{Binding SelectedFruit, Mode=TwoWay, UpdateSourceEventName=SelectionChanged}" />
    </StackLayout>
</ContentPage>
```

页面包含一个[`Label`](xref:Xamarin.Forms.Label) ，它显示用户从本机控件选择的水果。 `Label` 绑定到 `SubclassedNativeControlsPageViewModel.SelectedFruit` 属性。 页面的[`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)设置为代码隐藏文件中 `SubclassedNativeControlsPageViewModel` 类的新实例，并具有实现 `INotifyPropertyChanged` 接口的 ViewModel 类。

该页还包含每个平台的本机选取器视图。 每个本机视图都通过将其 `ItemSource` 属性绑定到 `SubclassedNativeControlsPageViewModel.Fruits` 集合来显示水果的集合。 这允许用户选取水果，如以下屏幕截图中所示：

![](xaml-images/sub-classed.png "Subclassed Native Views")

IOS 和 Android 上本机选取器使用方法设置这些控件。 因此，这些选取器必须子类化来公开属性，以使它们适合于 XAML 应用。 在通用 Windows 平台（UWP）上，`ComboBox` 已是 XAML 友好的，因此不需要进行子类化。

### <a name="ios"></a>iOS

IOS 实现为[`UIPickerView`](xref:UIKit.UIPickerView)视图提供子类，并公开可从 XAML 轻松使用的属性和事件：

```csharp
public class MyUIPickerView : UIPickerView
{
    public event EventHandler<EventArgs> SelectedItemChanged;

    public MyUIPickerView()
    {
        var model = new PickerModel();
        model.ItemChanged += (sender, e) =>
        {
            if (SelectedItemChanged != null)
            {
                SelectedItemChanged.Invoke(this, e);
            }
        };
        Model = model;
    }

    public IList<string> ItemsSource
    {
        get
        {
            var pickerModel = Model as PickerModel;
            return (pickerModel != null) ? pickerModel.Items : null;
        }
        set
        {
            var model = Model as PickerModel;
            if (model != null)
            {
                model.Items = value;
            }
        }
    }

    public string SelectedItem
    {
        get { return (Model as PickerModel).SelectedItem; }
        set { }
    }
}
```

`MyUIPickerView` 类公开 `ItemsSource` 和 `SelectedItem` 属性以及一个 `SelectedItemChanged` 事件。 [`UIPickerView`](xref:UIKit.UIPickerView)需要基础[`UIPickerViewModel`](xref:UIKit.UIPickerViewModel)数据模型，该模型由 `MyUIPickerView` 属性和事件访问。 `UIPickerViewModel` 数据模型由 `PickerModel` 类提供：

```csharp
class PickerModel : UIPickerViewModel
{
    int selectedIndex = 0;
    public event EventHandler<EventArgs> ItemChanged;
    public IList<string> Items { get; set; }

    public string SelectedItem
    {
        get
        {
            return Items != null && selectedIndex >= 0 && selectedIndex < Items.Count ? Items[selectedIndex] : null;
        }
    }

    public override nint GetRowsInComponent(UIPickerView pickerView, nint component)
    {
        return Items != null ? Items.Count : 0;
    }

    public override string GetTitle(UIPickerView pickerView, nint row, nint component)
    {
        return Items != null && Items.Count > row ? Items[(int)row] : null;
    }

    public override nint GetComponentCount(UIPickerView pickerView)
    {
        return 1;
    }

    public override void Selected(UIPickerView pickerView, nint row, nint component)
    {
        selectedIndex = (int)row;
        if (ItemChanged != null)
        {
            ItemChanged.Invoke(this, new EventArgs());
        }
    }
}
```

`PickerModel` 类通过 `Items` 属性为 `MyUIPickerView` 类提供基础存储。 每当 `MyUIPickerView` 中的选定项发生更改时，都会执行[`Selected`](xref:UIKit.UIPickerViewModel.Selected*)方法，该方法将更新选定的索引并激发 `ItemChanged` 事件。 这可确保 `SelectedItem` 属性将始终返回用户选取的最后一项。 此外，`PickerModel` 类将重写用于设置 `MyUIPickerView` 实例的方法。

### <a name="android"></a>Android

Android 实现为[`Spinner`](xref:Android.Widget.Spinner)视图提供子类，并公开可从 XAML 轻松使用的属性和事件：

```csharp
class MySpinner : Spinner
{
    ArrayAdapter adapter;
    IList<string> items;

    public IList<string> ItemsSource
    {
        get { return items; }
        set
        {
            if (items != value)
            {
                items = value;
                adapter.Clear();

                foreach (string str in items)
                {
                    adapter.Add(str);
                }
            }
        }
    }

    public string SelectedObject
    {
        get { return (string)GetItemAtPosition(SelectedItemPosition); }
        set
        {
            if (items != null)
            {
                int index = items.IndexOf(value);
                if (index != -1)
                {
                    SetSelection(index);
                }
            }
        }
    }

    public MySpinner(Context context) : base(context)
    {
        ItemSelected += OnBindableSpinnerItemSelected;

        adapter = new ArrayAdapter(context, Android.Resource.Layout.SimpleSpinnerItem);
        adapter.SetDropDownViewResource(Android.Resource.Layout.SimpleSpinnerDropDownItem);
        Adapter = adapter;
    }

    void OnBindableSpinnerItemSelected(object sender, ItemSelectedEventArgs args)
    {
        SelectedObject = (string)GetItemAtPosition(args.Position);
    }
}
```

`MySpinner` 类公开 `ItemsSource` 和 `SelectedObject` 属性以及一个 `ItemSelected` 事件。 `MySpinner` 类显示的项由与视图关联的[`Adapter`](xref:Android.Widget.Adapter)提供，在首次设置 `ItemsSource` 属性时，项将填充到 `Adapter` 中。 每当 `MySpinner` 类中的选定项发生更改时，`OnBindableSpinnerItemSelected` 事件处理程序就会更新 `SelectedObject` 属性。

## <a name="summary"></a>总结

本文演示了如何使用 Xamarin.Forms XAML 文件中的本机视图。 上的本机视图，可以设置属性和事件处理程序，它们可以与 Xamarin.Forms 视图进行交互。

## <a name="related-links"></a>相关链接

- [NativeSwitch （示例）](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-nativeviews-nativeswitch)
- [Forms2Native （示例）](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/forms2native)
- [NativeViewInsideContentView （示例）](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-nativeviews-nativeviewinsidecontentview)
- [SubclassedNativeControls （示例）](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-nativeviews-subclassednativecontrols)
- [本机窗体](~/xamarin-forms/platform/native-forms.md)
- [在 XAML 中传递参数](~/xamarin-forms/xaml/passing-arguments.md)
