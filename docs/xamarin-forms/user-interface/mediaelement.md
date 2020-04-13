---
title: Xamarin.forms 媒体元素
description: 本文介绍如何使用 MediaElement 在 Xamarin.Forms 应用程序中播放视频和音频。
ms.prod: xamarin
ms.assetid: e65f1e56-a80d-46c7-9ff4-7ae6650a3165
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/18/2020
ms.openlocfilehash: 6f6c51c428de569ceb09ed6a26cfc36881c86dc5
ms.sourcegitcommit: b93754b220fca3d6e3d131341e3cfbe233d10f84
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/03/2020
ms.locfileid: "80628331"
---
# <a name="xamarinforms-mediaelement"></a>Xamarin.forms 媒体元素

![](~/media/shared/preview.png "This API is currently pre-release")

[![下载示例](~/media/shared/download.png)下载示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-mediaelementdemos/)

[`MediaElement`](xref:Xamarin.Forms.MediaElement)是播放视频和音频的视图。 基础平台支持的介质可以从以下来源播放：

- 使用 URI（HTTP 或 HTTPS）的 Web。
- 使用`ms-appx:///`URI 方案嵌入到平台应用程序中的资源。
- 来自应用的本地和临时数据文件夹中的文件，使用`ms-appdata:///`URI 方案。
- 设备的库。

[`MediaElement`](xref:Xamarin.Forms.MediaElement)可以使用平台播放控件，称为传输控件。 但是，默认情况下，它们被禁用，并可以替换为您自己的传输控件。 以下屏幕截图显示`MediaElement`使用平台传输控件播放视频：

[![在 iOS 和安卓系统播放视频的媒体元素的屏幕截图](mediaelement-images/playback-controls.png "媒体元素播放视频")](mediaelement-images/playback-controls-large.png#lightbox "媒体元素播放视频")

[`MediaElement`](xref:Xamarin.Forms.MediaElement)有 Xamarin.表格 4.5 中的可用。 但是，它当前是实验性的，只能通过将以下代码行添加到*App.xaml.cs*文件来使用：

```csharp
Device.SetFlags(new string[]{ "MediaElement_Experimental" });
```

> [!NOTE]
> [`MediaElement`](xref:Xamarin.Forms.MediaElement)可在 iOS、Android、通用 Windows 平台 （UWP）、macOS、Windows 演示基金会和 Tizen 上提供。

[`MediaElement`](xref:Xamarin.Forms.MediaElement)定义以下属性：

- [`Aspect`](xref:Xamarin.Forms.MediaElement.Aspect)的类型[`Aspect`](xref:Xamarin.Forms.Aspect)，确定如何缩放媒体以适合显示区域。 此属性的默认值为 `AspectFit`。
- [`AutoPlay`](xref:Xamarin.Forms.MediaElement.AutoPlay)类型`bool`， 指示在设置[`Source`](xref:Xamarin.Forms.MediaElement.Source)属性时是否自动开始媒体播放。 此属性的默认值为 `true`。
- [`BufferingProgress`](xref:Xamarin.Forms.MediaElement.BufferingProgress)类型`double`， 指示当前缓冲进度。 此属性的默认值为 0.0。
- [`CanSeek`](xref:Xamarin.Forms.MediaElement.CanSeek)类型`bool`， 指示是否可以通过设置[`Position`](xref:Xamarin.Forms.MediaElement.Position)属性的值来重新定位媒体。 这是只读属性。
- [`CurrentState`](xref:Xamarin.Forms.MediaElement.CurrentState)的类型[`MediaElementState`](xref:Xamarin.Forms.MediaElementState)，指示控件的当前状态。 这是一个只读属性，其默认值为`MediaElementState.Closed`。
- [`Duration`](xref:Xamarin.Forms.MediaElement.Duration)的类型`TimeSpan?`，表示当前打开的媒体的持续时间。 这是一个只读属性，其默认值为`null`。
- [`IsLooping`](xref:Xamarin.Forms.MediaElement.IsLooping)的类型`bool`， 描述当前加载的媒体源是否应在到达其末尾后从头开始恢复播放。 此属性的默认值为 `false`。
- [`KeepScreenOn`](xref:Xamarin.Forms.MediaElement.KeepScreenOn)的类型`bool`，确定设备屏幕是否应在媒体播放期间保持打开状态。 此属性的默认值为 `false`。
- [`Position`](xref:Xamarin.Forms.MediaElement.Position)的类型`TimeSpan`，通过媒体的播放时间描述当前进度。 此属性的默认值为 `TimeSpan.Zero`。
- [`ShowsPlaybackControls`](xref:Xamarin.Forms.MediaElement.ShowsPlaybackControls)的类型`bool`，确定是否显示平台播放控件。 此属性的默认值为 `false`。
- [`Source`](xref:Xamarin.Forms.MediaElement.Source)的类型[`MediaSource`](xref:Xamarin.Forms.MediaSource)， 指示加载到控件中的介质的源。
- [`VideoHeight`](xref:Xamarin.Forms.MediaElement.VideoHeight)的类型`int`，指示控件的高度。 这是只读属性。
- [`VideoWidth`](xref:Xamarin.Forms.MediaElement.VideoWidth)的类型`int`， 表示控件的宽度。 这是只读属性。
- [`Volume`](xref:Xamarin.Forms.MediaElement.Volume)类型`double`， 确定媒体的体积， 以 0 和 1 之间的线性比例表示。 此属性使用`TwoWay`绑定，其默认值为 1。

这些属性（属性除外`CanSeek`）由[`BindableProperty`](xref:Xamarin.Forms.BindableProperty)对象支持，这意味着它们可以成为数据绑定的目标和样式。

该[`MediaElement`](xref:Xamarin.Forms.MediaElement)类还定义了四个事件：

- [`MediaOpened`](xref:Xamarin.Forms.MediaElement.MediaOpened)在验证并打开媒体流时触发。
- [`MediaEnded`](xref:Xamarin.Forms.MediaElement.MediaEnded)在`MediaElement`播放其媒体后触发。
- [`MediaFailed`](xref:Xamarin.Forms.MediaElement.MediaFailed)当存在与媒体源关联的错误时，将触发。
- [`SeekCompleted`](xref:Xamarin.Forms.MediaElement.SeekCompleted)当请求的寻用操作的搜索点已准备好进行播放时，将触发。

此外，[`MediaElement`](xref:Xamarin.Forms.MediaElement)包括[`Play`](xref:Xamarin.Forms.MediaElement.Play)和[`Pause`](xref:Xamarin.Forms.MediaElement.Pause)[`Stop`](xref:Xamarin.Forms.MediaElement.Stop)

有关 Android 上受支持的媒体格式的信息，请参阅 developer.android.com[上支持的媒体格式](https://developer.android.com/guide/topics/media/media-formats)。 有关通用 Windows 平台 （UWP） 上受支持的媒体格式的信息，请参阅[支持编解码器](/windows/uwp/audio-video-camera/supported-codecs)。

## <a name="play-remote-media"></a>播放远程媒体

A[`MediaElement`](xref:Xamarin.Forms.MediaElement)可以使用 HTTP 和 HTTPS URI 方案播放远程媒体文件。 这是通过将[`Source`](xref:Xamarin.Forms.MediaElement.Source)属性设置为媒体文件的 URI 来实现的：

```xaml
<MediaElement Source="https://sec.ch9.ms/ch9/5d93/a1eab4bf-3288-4faf-81c4-294402a85d93/XamarinShow_mid.mp4"
              ShowsPlaybackControls="True" />
```

默认情况下，[`Source`](xref:Xamarin.Forms.MediaElement.Source)属性定义的媒体在打开媒体后立即播放。 要禁止自动媒体播放，将[`AutoPlay`](xref:Xamarin.Forms.MediaElement.AutoPlay)属性设置为`false`。

默认情况下禁用媒体播放控件，并通过将[`ShowsPlaybackControls`](xref:Xamarin.Forms.MediaElement.ShowsPlaybackControls)属性设置为`true`启用。 [`MediaElement`](xref:Xamarin.Forms.MediaElement)然后将使用平台播放控件。

## <a name="play-local-media"></a>播放本地媒体

可以从以下来源播放本地媒体：

- 使用`ms-appx:///`URI 方案嵌入到平台应用程序中的资源。
- 来自应用的本地和临时数据文件夹中的文件，使用`ms-appdata:///`URI 方案。
- 设备的库。

有关这些 URI 方案的详细信息，请参阅[URI 方案](/windows/uwp/app-resources/uri-schemes)。

### <a name="play-media-embedded-in-the-app-package"></a>播放嵌入在应用包中的媒体

A[`MediaElement`](xref:Xamarin.Forms.MediaElement)可以使用`ms-appx:///`URI 方案播放嵌入在应用包中的媒体文件。 媒体文件通过将它们放置在平台项目中而嵌入到应用包中。

在平台项目中存储媒体文件对于每个平台是不同的：

- 在 iOS 上，媒体文件必须存储在 **"资源"** 文件夹或 **"资源"** 文件夹的子文件夹中。 媒体文件必须具有 的`Build Action``BundleResource`。
- 在 Android 上，媒体文件必须**存储在名为****raw**的资源的子文件夹中。 “raw”文件夹不能包含子文件夹****。 媒体文件必须具有 的`Build Action``AndroidResource`。
- 在 UWP 上，媒体文件可以存储在项目中的任何文件夹中。 媒体文件必须具有 的`BuildAction``Content`。

然后可以使用 URI 方案播放满足这些标准的媒体`ms-appx:///`文件：

```xaml
<MediaElement Source="ms-appx:///XamarinForms101UsingEmbeddedImages.mp4"
              ShowsPlaybackControls="True" />
```

使用数据绑定时，可以使用值转换器应用此 URI 方案：

```csharp
public class VideoSourceConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        if (value == null)
            return null;

        if (string.IsNullOrWhiteSpace(value.ToString()))
            return null;

        if (Device.RuntimePlatform == Device.UWP)
            return new Uri($"ms-appx:///Assets/{value}");
        else
            return new Uri($"ms-appx:///{value}");
    }
    // ...
}
```

然后，可以使用`VideoSourceConverter`的实例将`ms-appx:///`URI 方案应用于嵌入式媒体文件：

```xaml
<MediaElement Source="{Binding MediaSource, Converter={StaticResource VideoSourceConverter}}"
              ShowsPlaybackControls="True" />
```

有关 ms-appx URI 方案的详细信息，请参阅[ms-appx 和 ms-appx-web](/windows/uwp/app-resources/uri-schemes#ms-appx-and-ms-appx-web)。

### <a name="play-media-from-the-apps-local-and-temporary-folders"></a>从应用的本地和临时文件夹中播放媒体

可以[`MediaElement`](xref:Xamarin.Forms.MediaElement)使用`ms-appdata:///`URI 方案播放复制到应用的本地或临时数据文件夹中的媒体文件。

下面的示例显示设置为存储在[`Source`](xref:Xamarin.Forms.MediaElement.Source)应用本地数据文件夹中的媒体文件的属性：

```xaml
<MediaElement Source="ms-appdata:///local/XamarinVideo.mp4"
              ShowsPlaybackControls="True" />
```

下面的示例向[`Source`](xref:Xamarin.Forms.MediaElement.Source)存储在应用的临时数据文件夹中的媒体文件显示该属性：

```xaml
<MediaElement Source="ms-appdata:///temp/XamarinVideo.mp4"
              ShowsPlaybackControls="True" />
```

> [!IMPORTANT]
> 除了播放存储在应用的本地或临时数据文件夹中的媒体文件外，UWP 还可以播放位于应用漫游文件夹中的媒体文件。 这可以通过将媒体文件的前缀与`ms-appdata:///roaming/`来实现。

使用数据绑定时，可以使用值转换器应用此 URI 方案：

```csharp
public class VideoSourceConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        if (value == null)
            return null;

        if (string.IsNullOrWhiteSpace(value.ToString()))
            return null;

        return new Uri($"ms-appdata:///{value}");
    }
    // ...
}
```

然后，可以使用`VideoSourceConverter`的 实例将`ms-appdata:///`URI 方案应用于应用的本地或临时数据文件夹中的媒体文件：

```xaml
<MediaElement Source="{Binding MediaSource, Converter={StaticResource VideoSourceConverter}}"
              ShowsPlaybackControls="True" />
```

有关 ms-appdata URI 方案的详细信息，请参阅[ms-appdata](/windows/uwp/app-resources/uri-schemes#ms-appdata)。

#### <a name="copying-a-media-file-to-the-apps-local-or-temporary-data-folder"></a>将媒体文件复制到应用的本地或临时数据文件夹

播放存储在应用的本地或临时数据文件夹中的媒体文件需要应用将媒体文件复制到该文件夹中。 例如，可以通过从应用包复制媒体文件来实现：

```csharp
// This method copies the video from the app package to the app data
// directory for your app. To copy the video to the temp directory
// for your app, comment out the first line of code, and uncomment
// the second line of code.
public static async Task CopyVideoIfNotExists(string filename)
{
    string folder = FileSystem.AppDataDirectory;
    //string folder = Path.GetTempPath();
    string videoFile = Path.Combine(folder, "XamarinVideo.mp4");

    if (!File.Exists(videoFile))
    {
        using (Stream inputStream = await FileSystem.OpenAppPackageFileAsync(filename))
        {
            using (FileStream outputStream = File.Create(videoFile))
            {
                await inputStream.CopyToAsync(outputStream);
            }
        }
    }
}
```

> [!NOTE]
> 上面的代码示例使用 Xamarin 中包含的`FileSystem`类。 有关详细信息，请参阅[Xamarin.要点：文件系统帮助器](~/essentials/file-system-helpers.md?context=xamarin%2Fxamarin-forms&tabs=android)。

### <a name="play-media-from-the-device-library"></a>从设备库中播放媒体

大多数现代移动设备和台式计算机都能够使用设备的摄像头和麦克风录制视频和音频。 然后，创建的介质将存储为设备上的文件。 这些文件可以从库中检索并由 播放[`MediaElement`](xref:Xamarin.Forms.MediaElement)。

每个平台都包含一个允许用户从设备库中选择媒体的功能。 在 Xamarin.Forms 中，平台项目可以调用此功能，并且可以通过[`DependencyService`](xref:Xamarin.Forms.DependencyService)类调用它。

示例应用程序中使用的视频选取依赖项服务与[从图片库中选取照片](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md)时定义的服务非常相似，只不过选取器返回文件名而不是`Stream`对象。 共享代码项目定义名为 的`IVideoPicker`接口，用于定义名为 的`GetVideoFileAsync`单个方法。 然后，每个平台在`VideoPicker`类中实现此接口。

以下代码示例演示如何从设备库中检索媒体文件：

```csharp
string filename = await DependencyService.Get<IVideoPicker>().GetVideoFileAsync();
if (!string.IsNullOrWhiteSpace(filename))
{
    mediaElement.Source = new FileMediaSource
    {
        File = filename
    };
}
```

通过调用`DependencyService.Get`方法获取平台项目中`IVideoPicker`接口的实现来调用视频选取依赖项服务。 `GetVideoFileAsync`然后，该方法将调用该实例，返回的文件名用于创建[`FileMediaSource`](xref:Xamarin.Forms.FileMediaSource)对象并将其设置为[`Source`](xref:Xamarin.Forms.MediaElement.Source)的属性[`MediaElement`](xref:Xamarin.Forms.MediaElement)。

## <a name="change-video-aspect-ratio"></a>更改视频纵横比

该[`Aspect`](xref:Xamarin.Forms.MediaElement.Aspect)属性确定如何缩放视频媒体以适合显示区域。 默认情况下，此属性设置为`AspectFit`枚举成员，但可以将其设置为任何[`Aspect`](xref:Xamarin.Forms.Aspect)枚举成员：

- `AspectFit`指示视频将信箱（如果需要）以适合显示区域，同时保持纵横比。
- `AspectFill`表示将剪切视频以填充显示区域，同时保持纵横比。
- `Fill`表示视频将被拉伸以填充显示区域。

## <a name="poll-for-position-data"></a>职位数据的轮询

可[`Position`](xref:Xamarin.Forms.MediaElement.Position)绑定属性的属性更改通知仅在播放开始和结束等关键时刻触发，并发生暂停。 因此，绑定到`Position`属性的数据不会生成准确的位置数据。 相反，您必须设置计时器并轮询属性。

执行此操作的好地方是在`OnAppearing`播放媒体时需要位置数据的页面的重写中：

```csharp
bool polling = true;

protected override void OnAppearing()
{
    base.OnAppearing();

    Device.StartTimer(TimeSpan.FromMilliseconds(1000), () =>
    {
        Device.BeginInvokeOnMainThread(() =>
        {
            positionLabel.Text = mediaElement.Position.ToString("hh\\:mm\\:ss");
        });
        return polling;
    });
}

protected override void OnDisappearing()
{
    base.OnDisappearing();
    polling = false;
}
```

在此示例中，`OnAppearing`重写将启动每秒`positionLabel`更新该值的`Position`计时器。 计时器回调每秒调用一次，直到回调返回`false`。 当发生页面导航时`OnDisappearing`，将执行重写，从而停止调用计时器回调。

## <a name="understand-mediasource-types"></a>了解媒体来源类型

通过将[`MediaElement`](xref:Xamarin.Forms.MediaElement)属性[`Source`](xref:Xamarin.Forms.MediaElement.Source)设置为远程或本地媒体文件，可以播放媒体。 属性`Source`的类型为[`MediaSource`](xref:Xamarin.Forms.MediaSource)，并且此类定义两个静态方法：

- [`FromFile`](xref:Xamarin.Forms.MediaSource.FromFile*)从`string`参数[`MediaSource`](xref:Xamarin.Forms.MediaSource)返回实例。
- [`FromUri`](xref:Xamarin.Forms.MediaSource.FromUri*)从`Uri`参数[`MediaSource`](xref:Xamarin.Forms.MediaSource)返回实例。

此外，[`MediaSource`](xref:Xamarin.Forms.MediaSource)类还具有从`MediaSource``string`和`Uri`返回实例的隐式运算符。

> [!NOTE]
> 在[`Source`](xref:Xamarin.Forms.MediaElement.Source)XAML 中设置该属性时，将调用类型转换器从[`MediaSource`](xref:Xamarin.Forms.MediaSource)`string`或`Uri`返回实例。

该[`MediaSource`](xref:Xamarin.Forms.MediaSource)类还有两个派生类：

- [`UriMediaSource`](xref:Xamarin.Forms.UriMediaSource)，用于从 URI 指定远程媒体文件。 此类具有[`Uri`](xref:Xamarin.Forms.UriMediaSource.Uri)可设置为 的属性`Uri`。
- [`FileMediaSource`](xref:Xamarin.Forms.FileMediaSource)用于指定 的本地媒体文件`string`。 此类具有[`File`](xref:Xamarin.Forms.FileMediaSource.File)可设置为 的属性`string`。 此外，类具有`string`将 转换为`FileMediaSource`对象的隐式运算符，并将`FileMediaSource`对象转换为 。 `string`

> [!NOTE]
> 在[`FileMediaSource`](xref:Xamarin.Forms.FileMediaSource)XAML 中创建对象时，将调用类型转换器从 返回[`FileMediaSource`](xref:Xamarin.Forms.FileMediaSource)`string`实例。

## <a name="determine-mediaelement-status"></a>确定媒体元素状态

类[`MediaElement`](xref:Xamarin.Forms.MediaElement)定义类型 为[`CurrentState`](xref:Xamarin.Forms.MediaElement.CurrentState)的只读可绑定属性， 类型[`MediaElementState`](xref:Xamarin.Forms.MediaElementState)。 此属性指示控件的当前状态，例如媒体是正在播放还是暂停，或者它是否尚未准备好播放媒体。

枚[`MediaElementState`](xref:Xamarin.Forms.MediaElementState)举定义以下成员：

- `Closed`表示 不包含`MediaElement`任何介质。
- `Opening`指示 正在`MediaElement`验证 并尝试加载指定的源。
- `Buffering`指示 正在`MediaElement`加载媒体以进行播放。 其[`Position`](xref:Xamarin.Forms.MediaElement.Position)属性在此状态期间不会前进。 `MediaElement`如果 正在播放视频，则它将继续显示最后显示的帧。
- `Playing`指示 正在播放`MediaElement`的媒体源。
- `Paused`表示 不`MediaElement`预支其[`Position`](xref:Xamarin.Forms.MediaElement.Position)属性。 `MediaElement`如果 正在播放视频，则它将继续显示当前帧。
- `Stopped`指示`MediaElement`包含的媒体，但它未播放或暂停。 其[`Position`](xref:Xamarin.Forms.MediaElement.Position)属性为 0，不前进。 如果加载的媒体是视频，则`MediaElement`显示第一帧。

使用传输控件时，[`CurrentState`](xref:Xamarin.Forms.MediaElement.CurrentState)[`MediaElement`](xref:Xamarin.Forms.MediaElement)通常不需要检查属性。 但是，在实现自己的传输控件时，此属性变得非常重要。

## <a name="implement-custom-transport-controls"></a>实施自定义传输控件

媒体播放器的传输控件包括执行**功能"播放**"、"**暂停**"和 **"停止"** 的按钮。 这些按钮通常使用熟悉的图标而非文本来标识，且“播放”和“暂停”功能通常合并成一个按钮********。

默认情况下，[`MediaElement`](xref:Xamarin.Forms.MediaElement)播放控件被禁用。 这使您能够以编程方式控制`MediaElement`，或者通过提供您自己的传输控件来控制。 为此，`MediaElement`包括[`Play`](xref:Xamarin.Forms.MediaElement.Play)，[`Pause`](xref:Xamarin.Forms.MediaElement.Pause)和 方法。 [`Stop`](xref:Xamarin.Forms.MediaElement.Stop)

下面的 XAML 示例显示包含[`MediaElement`](xref:Xamarin.Forms.MediaElement)和 自定义传输控件的页面：

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="MediaElementDemos.CustomTransportPage"
             Title="Custom transport">
    <Grid>
        ...
        <MediaElement x:Name="mediaElement"
                      AutoPlay="False"
                      ... />
        <StackLayout BindingContext="{x:Reference mediaElement}"
                     ...>
            <Button Text="&#x25B6;&#xFE0F; Play"
                    HorizontalOptions="CenterAndExpand"
                    Clicked="OnPlayPauseButtonClicked">
                <Button.Triggers>
                    <DataTrigger TargetType="Button"
                                 Binding="{Binding CurrentState}"
                                 Value="{x:Static MediaElementState.Playing}">
                        <Setter Property="Text"
                                Value="&#x23F8; Pause" />
                    </DataTrigger>
                    <DataTrigger TargetType="Button"
                                 Binding="{Binding CurrentState}"
                                 Value="{x:Static MediaElementState.Buffering}">
                        <Setter Property="IsEnabled"
                                Value="False" />
                    </DataTrigger>
                </Button.Triggers>
            </Button>
            <Button Text="&#x23F9; Stop"
                    HorizontalOptions="CenterAndExpand"
                    Clicked="OnStopButtonClicked">
                <Button.Triggers>
                    <DataTrigger TargetType="Button"
                                 Binding="{Binding CurrentState}"
                                 Value="{x:Static MediaElementState.Stopped}">
                        <Setter Property="IsEnabled"
                                Value="False" />
                    </DataTrigger>
                </Button.Triggers>
            </Button>
        </StackLayout>
    </Grid>
</ContentPage>
```

在此示例中，自定义传输控件定义为[`Button`](xref:Xamarin.Forms.Button)对象。 `Button`但是，只有两个对象，第一个`Button`表示**播放**和**暂停**，第二`Button`个表示**停止**。 [`DataTrigger`](xref:Xamarin.Forms.DataTrigger)对象用于启用和禁用按钮，并在 **"播放"** 和 **"暂停**"之间切换第一个按钮。 有关数据触发器的详细信息，请参阅[Xamarin.窗体触发器](~/xamarin-forms/app-fundamentals/triggers.md)。

代码背后的文件具有[`Clicked`](xref:Xamarin.Forms.Button.Clicked)事件的处理程序：

```csharp
void OnPlayPauseButtonClicked(object sender, EventArgs args)
{
    if (mediaElement.CurrentState == MediaElementState.Stopped ||
        mediaElement.CurrentState == MediaElementState.Paused)
    {
        mediaElement.Play();
    }
    else if (mediaElement.CurrentState == MediaElementState.Playing)
    {
        mediaElement.Pause();
    }
}

void OnStopButtonClicked(object sender, EventArgs args)
{
    mediaElement.Stop();
}
```

启用"**播放"** 按钮后，可以按该按钮开始播放：

[![在 iOS 和 Android 上使用自定义传输控件的 MediaElement 的屏幕截图](mediaelement-images/custom-transport-playback.png "媒体元素播放视频")](mediaelement-images/custom-transport-playback-large.png#lightbox "媒体元素播放视频")

按下 **"暂停"** 按钮会导致播放暂停：

[![在 iOS 和 Android 上暂停播放的媒体元素的屏幕截图](mediaelement-images/custom-transport-paused.png "带暂停视频的媒体元素")](mediaelement-images/custom-transport-paused-large.png#lightbox "带暂停视频的媒体元素")

按下 **"停止"** 按钮将停止播放，并将媒体文件的位置返回到开头。

## <a name="implement-a-custom-position-bar"></a>实现自定义位置栏

由每个平台实现的传输控件都有一个定位条。 此条形类似于滑块，并显示媒体在其总持续时间内的当前位置。 此外，您可以操作位置栏以向前或向后移动到视频中的新位置。

实现自定义位置栏需要了解媒体的持续时间和当前播放位置。 此数据在[`Duration`](xref:Xamarin.Forms.MediaElement.Duration)和[`Position`](xref:Xamarin.Forms.MediaElement.Position)属性中可用。

> [!IMPORTANT]
> [`Position`](xref:Xamarin.Forms.MediaElement.Position)必须轮询以获取准确的位置数据。 有关详细信息，请参阅[轮询位置数据](#poll-for-position-data)。

可以使用 实现自定义位置栏[`Slider`](xref:Xamarin.Forms.Slider)，如以下示例所示：

```csharp
public class PositionSlider : Slider
{
    public static readonly BindableProperty DurationProperty =
        BindableProperty.Create(nameof(Duration), typeof(TimeSpan), typeof(PositionSlider), new TimeSpan(1),
                                propertyChanged: (bindable, oldValue, newValue) =>
                                {
                                    ((PositionSlider)bindable).SetTimeToEnd();
                                    double seconds = ((TimeSpan)newValue).TotalSeconds;
                                    ((Slider)bindable).Maximum = seconds <= 0 ? 1 : seconds;
                                });

    public TimeSpan Duration
    {
        get { return (TimeSpan)GetValue(DurationProperty); }
        set { SetValue(DurationProperty, value); }
    }

    public static readonly BindableProperty PositionProperty =
        BindableProperty.Create(nameof(Position), typeof(TimeSpan), typeof(PositionSlider), new TimeSpan(0),
                                propertyChanged: (bindable, oldValue, newValue) => ((PositionSlider)bindable).SetTimeToEnd());

    public TimeSpan Position
    {
        get { return (TimeSpan)GetValue(PositionProperty); }
        set { SetValue(PositionProperty, value); }
    }

    static readonly BindablePropertyKey TimeToEndPropertyKey =
        BindableProperty.CreateReadOnly(nameof(TimeToEnd), typeof(TimeSpan), typeof(PositionSlider), new TimeSpan());

    public static readonly BindableProperty TimeToEndProperty = TimeToEndPropertyKey.BindableProperty;

    public TimeSpan TimeToEnd
    {
        get { return (TimeSpan)GetValue(TimeToEndProperty); }
        private set { SetValue(TimeToEndPropertyKey, value); }
    }

    public PositionSlider()
    {
        PropertyChanged += (sender, args) =>
        {
            if (args.PropertyName == "Value")
            {
                TimeSpan newPosition = TimeSpan.FromSeconds(Value);
                if (Math.Abs(newPosition.TotalSeconds - Position.TotalSeconds) / Duration.TotalSeconds > 0.01)
                {
                    Position = newPosition;
                }
            }
        };
    }

    void SetTimeToEnd()
    {
        TimeToEnd = Duration - Position;
    }
}
```

类`PositionSlider`定义其自己的`Duration`可`Position`绑定属性和可`TimeToEnd`绑定属性。 所有三个属性都是类型的`TimeSpan`。 `Duration`属性的属性`Maximum`更改处理程序将 的属性[`Slider`](xref:Xamarin.Forms.Slider)设置到 值`TotalSeconds`的属性的属性。 `TimeSpan` 属性`TimeToEnd`根据 对`Duration`和`Position`属性的更改计算，从媒体的持续时间开始，随着播放的进行，该属性将减少到零。

`PositionSlider`移动时[`Slider`](xref:Xamarin.Forms.Slider)`Slider`从基础更新，以指示介质应前进或反转到新位置。 这在`PropertyChanged``PositionSlider`构造函数中的处理程序中检测到。 处理程序检查 `Value` 属性中的更改，并且如果它与 `Position` 属性不同，那么 `Position` 属性将从 `Value` 属性进行设置。 有关使用[`Slider`](xref:Xamarin.Forms.Slider)请参阅的详细信息[，Xamarin.窗体滑块](~/xamarin-forms/user-interface/slider.md)

> [!NOTE]
> 在 Android[`Slider`](xref:Xamarin.Forms.Slider)上，只有 1000 个离散步骤`Minimum`，`Maximum`而不考虑 和 设置。 如果媒体长度大于 1000 秒，则两个不同的`Position`值将对应于`Value`的相同`Slider`。 这就是为什么上面的代码检查新职位和现有职位是否大于总持续时间的百分之一。

下面的示例显示了`PositionSlider`在页面上使用：

```xaml
<controls:PositionSlider x:Name="positionSlider"
                         BindingContext="{x:Reference mediaElement}"
                         Duration="{Binding Duration}"
                         ValueChanged="OnPositionSliderValueChanged">
    <controls:PositionSlider.Triggers>
        <DataTrigger TargetType="controls:PositionSlider"
                     Binding="{Binding CurrentState}"
                     Value="{x:Static MediaElementState.Buffering}">
            <Setter Property="IsEnabled" Value="False" />
        </DataTrigger>
    </controls:PositionSlider.Triggers>
</controls:PositionSlider>
```

在此示例中，`Duration`属性`PositionSlider`的数据绑定到[`Duration`](xref:Xamarin.Forms.MediaElement.Duration)的属性。 [`MediaElement`](xref:Xamarin.Forms.MediaElement) 更改的属性[`Value`](xref:Xamarin.Forms.Slider.Value)[`Slider`](xref:Xamarin.Forms.Slider)时，`ValueChanged`事件将触发并`OnPositionSliderValueChanged`执行处理程序。 此处理程序将[`MediaElement.Position`](xref:Xamarin.Forms.MediaElement.Position)属性设置到`PositionSlider.Position`属性的值。 因此，拖动`Slider`媒体播放位置的结果将发生变化：

[![在 iOS 和 Android 上使用自定义位置栏的 MediaElement 的屏幕截图](mediaelement-images/custom-position-bar.png "带有自定义位置栏的媒体元素")](mediaelement-images/custom-position-bar-large.png#lightbox "带有自定义位置栏的媒体元素")

此外，对象[`DataTrigger`](xref:Xamarin.Forms.DataTrigger)用于禁用媒体缓冲`PositionSlider`时的 。 有关数据触发器的详细信息，请参阅[Xamarin.窗体触发器](~/xamarin-forms/app-fundamentals/triggers.md)。

## <a name="implement-a-custom-volume-control"></a>实现自定义音量控制

每个平台实现的媒体播放控件包括一个音量栏。 此条形类似于滑块，显示介质的音量。 此外，您可以操作音量条以增加或减少音量。

可以使用 实现自定义卷栏[`Slider`](xref:Xamarin.Forms.Slider)，如以下示例所示：

```xaml
<StackLayout>
    <MediaElement AutoPlay="False"
                  Source="{StaticResource AdvancedAsync}" />
    <Slider Maximum="1.0"
            Minimum="0.0"
            Value="{Binding Volume}"
            Rotation="270"
            WidthRequest="100" />
</StackLayout>
```

在此示例中，[`Slider`](xref:Xamarin.Forms.Slider)数据将其`Value`属性绑定到[`Volume`](xref:Xamarin.Forms.MediaElement.Volume)的属性。 [`MediaElement`](xref:Xamarin.Forms.MediaElement) 这是可能的，`Volume`因为属性使用`TwoWay`绑定。 因此，更改属性`Value`将导致`Volume`属性更改。

> [!NOTE]
> 属性[`Volume`](xref:Xamarin.Forms.MediaElement.Volume)具有 vlidation 回调，可确保其值大于或等于 0.0，小于或等于 1.0。

有关使用[`Slider`](xref:Xamarin.Forms.Slider)请参阅的详细信息[，Xamarin.窗体滑块](~/xamarin-forms/user-interface/slider.md)

## <a name="related-links"></a>相关链接

- [媒体元素演示（示例）](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-mediaelementdemos/)
- [URI 方案](/windows/uwp/app-resources/uri-schemes)
- [Xamarin.Forms 触发器](~/xamarin-forms/app-fundamentals/triggers.md)
- [Xamarin.窗体滑块](~/xamarin-forms/user-interface/slider.md)
- [安卓：支持的媒体格式](https://developer.android.com/guide/topics/media/media-formats)
- [UWP：支持的编解码器](/windows/uwp/audio-video-camera/supported-codecs)
