---
title: 在应用程序中使用 Google Maps API
description: 如何在 Xamarin.Android 应用程序中实现 Google Maps API v2 功能。
ms.prod: xamarin
ms.assetid: C0589878-2D04-180E-A5B9-BB41D5AF6E02
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 09/07/2018
ms.openlocfilehash: adcfb1457742d343f87a602885566107cf327e2d
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/10/2020
ms.locfileid: "73027152"
---
# <a name="using-the-google-maps-api-in-your-application"></a>在应用程序中使用 Google Maps API

使用 Maps 应用程序的体验很棒，但有时你会希望直接在应用程序中包含地图。 除内置的地图应用程序外，Google 还提供[适用于 Android 的本机地图 API](https://developers.google.com/maps/documentation/android-sdk/intro)。
Maps API 适用于希望对地图体验保持更多控制的场景。 Maps API 可实现的功能包括：

- 以编程方式更改地图的视点。
- 添加和自定义标记。
- 使用叠加层注释地图。

与现已弃用的 Google Maps Android API v1 不同，Google Maps Android API v2 是 [Google Play Services](https://developers.google.com/android/guides/overview) 的一部分。
Xamarin.Android 应用必须先满足一些强制性先决条件，然后才能使用 Google Maps Android API。

## <a name="google-maps-api-prerequisites"></a>Google Maps API 先决条件

需要先执行多个步骤，然后才能使用 Maps API，其中包括：

- [获取 Maps API 密钥](#obtain-maps-key)
- [安装 Google Play Services SDK](#install-gps-sdk)
- [从 NuGet 安装 Xamarin.GooglePlayServices.Maps 包](#install-gpsmaps-nuget)
- [指定所需的权限](#declare-permissions)
- [（可选）使用 Google API 创建仿真器](#create-emulator-with-google-api)

### <a name="a-nameobtain-maps-key-obtain-a-google-maps-api-key"></a><a name="obtain-maps-key" />获取 Google Maps API 密钥

第一步是获取 Google Maps API 密钥（请注意，你不能重复使用旧版 Google Maps v1 API 的 API 密钥）。 有关如何通过 Xamarin.Android 获取和使用 API ​​密钥的信息，请参阅[获取 Google Maps API 密钥](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md)。

### <a name="a-nameinstall-gps-sdk--install-the-google-play-services-sdk"></a><a name="install-gps-sdk" />安装 Google Play Services SDK

Google Play Services 是 Google 的一项技术，它让 Android 应用程序能够利用各种 Google 功能，例如 Google+、In-App Billing 和 Maps。 这些功能可在 Android 设备上作为后台服务进行访问，这些服务包含在 [Google Play Services APK](https://play.google.com/store/apps/details?id=com.google.android.gms&hl=en) 中。

Android 应用程序通过 Google Play Services 客户端库与 Google Play Services 进行交互。 此库包含用于各项服务（例如 Maps）的接口和类。 下图显示 Android 应用程序与 Google Play Services 之间的关系：

![说明 Google Play Store 如何更新 Google Play Services APK 的关系图](maps-api-images/play-services-diagram.png)

Android Maps API 作为 Google Play Services 的一部分提供。
Xamarin.Android 应用程序必须先使用 [Android SDK 管理器](~/android/get-started/installation/android-sdk.md)安装 Google Play Services SDK，然后才能使用 Maps API。 以下屏幕截图显示 Android SDK 管理器中可以找到 Google Play Services 客户端的位置：

![Google Play Services 在 Android SDK 管理器的“Extras”下显示](maps-api-images/image01.png)

> [!NOTE]
> Google Play Services APK 是许可产品，并非所有设备都会提供该产品。 如果未安装，则 Google Maps 将无法在设备上运行。

### <a name="a-nameinstall-gpsmaps-nuget--install-the-xamaringoogleplayservicesmaps-package-from-nuget"></a><a name="install-gpsmaps-nuget" />从 NuGet 安装 Xamarin.GooglePlayServices.Maps 包

[Xamarin.GooglePlayServices.Maps 包](https://www.nuget.org/packages/Xamarin.GooglePlayServices.Maps)包含适用于 Google Play Services Maps API 的 Xamarin.Android 绑定。
若要添加 Google Play Services Maps 包，请在解决方案资源管理器中右键单击项目的“References”文件夹，然后单击“管理 NuGet 包...”   ：

![显示“References”下的“管理 NuGet 包”上下文菜单项的解决方案资源管理器](maps-api-images/image02.png)

此时，“NuGet 包管理器”会打开  。 单击“浏览”，然后在搜索字段中输入“Xamarin Google Play Services Maps”   。 选择“Xamarin.GooglePlayServices.Maps”，然后单击“安装”   。 （如果之前已安装此包，请单击“更新”  。）：

[![已选择 Xamarin.GooglePlayServices.Maps 包的 NuGet 包管理器](maps-api-images/image03-sml.png)](maps-api-images/image03.png#lightbox)

请注意，还安装了以下依赖项：

- **Xamarin.GooglePlayServices.Base**
- **Xamarin.GooglePlayServices.Basement**
- **Xamarin.GooglePlayServices.Tasks**

### <a name="a-namedeclare-permissions--specify-the-required-permissions"></a><a name="declare-permissions" />指定所需的权限

应用必须标识硬件和权限要求才能使用 Google Maps API。  一些权限由 Google Play Services SDK 自动授予，开发人员无需将其显式添加到 **AndroidManfest.XML** 中：

- **网络状态访问权限** &ndash;Maps API 必须能够检查其是否可以下载地图图块。

- **Internet 访问权限** &ndash;Internet 访问权限对于下载地图图块以及与 Google Play 服务器通信以获取 API 访问权限而言是必需的。

必须在 **AndroidManifest.XML** 中为 Google Maps Android API 指定以下权限和功能：

- **OpenGL ES v2** &ndash;应用程序必须声明 OpenGL ES v2 的要求。

- **Google Maps API 密钥** &ndash;API 密钥用于确认应用程序已注册并获得使用 Google Play Services 的授权。 有关此密钥的详细信息，请参阅[获取 Google Maps API 密钥](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md)。

- **请求旧版 Apache HTTP 客户端** &ndash; 面向 Android 9.0（API 级别 28）或更高版本的应用必须指定旧版 Apache HTTP 客户端是要使用的可选库。

- **访问 Google Web 服务** &ndash; 应用程序需要对支持 Android Maps API 的 Google Web 服务的访问权限。

- **Google Play Services 通知权限** &ndash; 必须授予应用程序接收来自 Google Play Services 的远程通知的权限。

- **访问位置提供程序** &ndash; 这些是可选权限。
   它们将允许 `GoogleMap` 类在地图上显示设备的位置。

此外，Android 9 已从 bootclasspath 中删除 Apache HTTP 客户端库，因此，该库不可用于面向 API 28 或更高级别的应用程序。 必须将以下行添加到 **AndroidManifest.xml** 文件的 `application` 节点，以便在面向 API 28 或更高级别的应用程序中继续使用 Apache HTTP 客户端：

```xml
<application ...>
   ...
   <uses-library android:name="org.apache.http.legacy" android:required="false" />    
</application>
```

> [!NOTE]
> 较早版本的 Google Play SDK 要求应用请求 `WRITE_EXTERNAL_STORAGE` 权限。 对于 Google Play Services 的最新 Xamarin 绑定，此要求不再有必要。

以下代码片段是必须添加到 **AndroidManifest.XML** 中的设置的示例：

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android" android:versionName="4.5" package="com.xamarin.docs.android.mapsandlocationdemo2" android:versionCode="6">
    <uses-sdk android:minSdkVersion="23" android:targetSdkVersion="28" />

    <!-- Google Maps for Android v2 requires OpenGL ES v2 -->
    <uses-feature android:glEsVersion="0x00020000" android:required="true" />

    <!-- Necessary for apps that target Android 9.0 or higher -->
    <uses-library android:name="org.apache.http.legacy" android:required="false" />

    <!-- Permission to receive remote notifications from Google Play Services -->
    <!-- Notice here that we have the package name of our application as a prefix on the permissions. -->
    <uses-permission android:name="<PACKAGE NAME>.permission.MAPS_RECEIVE" />
    <permission android:name="<PACKAGE NAME>.permission.MAPS_RECEIVE" android:protectionLevel="signature" />

    <!-- These are optional, but recommended. They will allow Maps to use the My Location provider. -->
    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />

    <application android:label="@string/app_name">
        <!-- Put your Google Maps V2 API Key here. -->
        <meta-data android:name="com.google.android.maps.v2.API_KEY" android:value="YOUR_API_KEY" />
        <meta-data android:name="com.google.android.gms.version" android:value="@integer/google_play_services_version" />
        <!-- Necessary for apps that target Android 9.0 or higher -->
        <uses-library android:name="org.apache.http.legacy" android:required="false" />
    </application>
</manifest>
```

除请求 **AndroidManifest.XML** 权限外，应用还必须对 `ACCESS_COARSE_LOCATION` 和 `ACCESS_FINE_LOCATION` 权限执行运行时权限检查。 有关执行运行时权限检查的详细信息，请参阅 [Xamarin.Android 权限](~/android/app-fundamentals/permissions.md)指南。

### <a name="a-namecreate-emulator-with-google-api-create-an-emulator-with-google-apis"></a><a name="create-emulator-with-google-api" />使用 Google API 创建仿真器

如果 Android 物理设备未安装 Google Play Services，则可以创建用于开发的仿真器映像。 有关详细信息，请参阅[设备管理器](~/android/get-started/installation/android-emulator/device-manager.md)。

## <a name="the-googlemap-class"></a>GoogleMap 类

满足先决条件后，即可开始开发应用程序并使用 Android Maps API。 [GoogleMap](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap) 类是 Xamarin.Android 应用程序将用于显示适用于 Android 的 Google Maps 并与之交互的主要 API。 此类具有以下职责：

- 与 Google Play Services 交互，从而通过 Google Web 服务对应用程序授权。

- 下载、缓存和显示地图图块。

- 向用户显示平移和缩放等 UI 控件。

- 在地图上绘制标记和几何形状。

`GoogleMap` 通过以下两种方式之一添加到 Activity：

- **MapFragment** - [MapFragment](https://developers.google.com/android/reference/com/google/android/gms/maps/MapFragment) 是专用 Fragment，充当 `GoogleMap` 对象的主机。 `MapFragment` 需要 Android API 级别 12 或更高级别。
   较旧版本的 Android 可以使用 [SupportMapFragment](https://developers.google.com/android/reference/com/google/android/gms/maps/SupportMapFragment)。  本指南将重点介绍如何使用 `MapFragment` 类。

- **MapView** - [MapView](https://developers.google.com/android/reference/com/google/android/gms/maps/MapView) 是专用 View 子类，可充当 `GoogleMap` 对象的主机。 此类的用户必须将所有 Activity 生命周期方法转发到 `MapView` 类。

其中每个容器都会公开一个 `Map` 属性，该属性返回 `GoogleMap` 的实例。 应优先考虑 [MapFragment](https://developers.google.com/android/reference/com/google/android/gms/maps/MapFragment) 类，因为它是一个较简单的 API，可减少开发人员必须手动实现的样板代码量。

### <a name="adding-a-mapfragment-to-an-activity"></a>向 Activity 添加 MapFragment

以下屏幕截图是简单的 `MapFragment` 的示例：

[![显示 Google Map 片段的设备的屏幕截图](maps-api-images/image05-sml.png)](maps-api-images/image05.png#lightbox)

与其他 Fragment 类类似，可通过两种方式向 Activity 添加 `MapFragment`：

- **以声明方式** - 可通过 XML 布局文件为 Activity 添加 `MapFragment`。 以下 XML 代码片段展示如何使用 `fragment` 元素的示例：

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <fragment xmlns:android="http://schemas.android.com/apk/res/android"
              android:id="@+id/map"
              android:layout_width="match_parent"
              android:layout_height="match_parent"
              class="com.google.android.gms.maps.MapFragment" />
    ```

- **以编程方式** - 可使用 [`MapFragment.NewInstance`](https://developers.google.com/android/reference/com/google/android/gms/maps/MapFragment.html#newInstance()) 方法以编程方式实例化 `MapFragment`，然后将其添加到 Activity。 以下代码片段演示实例化 `MapFragment` 对象并将其添加到 Activity 的最简单方法：

    ```csharp
        var mapFrag = MapFragment.NewInstance();
        activity.FragmentManager.BeginTransaction()
                                .Add(Resource.Id.map_container, mapFrag, "map_fragment")
                                .Commit();

    ```

    通过将 [`GoogleMapOptions`](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMapOptions) 对象传递到 `NewInstance`，可以配置 `MapFragment` 对象。 本指南后面的 [GoogleMap 属性](#googlemap_object)部分会对此进行讨论。

`MapFragment.GetMapAsync` 方法用于初始化片段托管的 [`GoogleMap`](#googlemap_object)，并获取对 `MapFragment` 托管的地图对象的引用。 此方法采用实现 `IOnMapReadyCallback` 接口的对象。

此接口只有一个方法 `IMapReadyCallback.OnMapReady(MapFragment map)`，当应用可能与 `GoogleMap` 对象进行交互时，将调用该方法。 以下代码片段演示 Android Activity 如何初始化 `MapFragment` 并实现 `IOnMapReadyCallback` 接口：

```csharp
public class MapWithMarkersActivity : AppCompatActivity, IOnMapReadyCallback
{
    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);
        SetContentView(Resource.Layout.MapLayout);

        var mapFragment = (MapFragment) FragmentManager.FindFragmentById(Resource.Id.map);
        mapFragment.GetMapAsync(this);

        // remainder of code omitted
    }

    public void OnMapReady(GoogleMap map)
    {
        // Do something with the map, i.e. add markers, move to a specific location, etc.
    }
}
```

### <a name="map-types"></a>地图类型

Google Maps API 提供五种不同类型的地图：

- **标准** - 这是默认地图类型。 它显示道路和重要自然景观，以及部分人造景点（例如建筑物和桥梁）。

- **卫星** - 此地图显示卫星摄影。

- **混合** - 此地图显示卫星摄影和道路图。

- **地形** - 此地图主要显示地形特征及部分道路。

- **无** - 此地图不加载任何图块，而是呈现为空网格。

下图从左到右显示三种不同类型的地图（标准、混合、地形）：

[![三个地图示例的屏幕截图：标准、混合和地形](maps-api-images/map-types-sml.png)](maps-api-images/map-types.png#lightbox)

`GoogleMap.MapType` 属性用于设置或更改显示的地图类型。 以下代码片段演示如何显示卫星地图。

```csharp
public void OnMapReady(GoogleMap map)
{
    map.MapType = GoogleMap.MapTypeHybrid;
}
```

### <a name="a-namegooglemap_object-googlemap-properties"></a><a name="googlemap_object" />GoogleMap 属性

`GoogleMap` 定义可控制地图的功能和外观的多个属性。 配置 `GoogleMap` 的初始状态的一种方法是在创建 `MapFragment` 时传递 [GoogleMapOptions](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMapOptions) 对象。 以下代码片段是在创建 `MapFragment` 时使用 `GoogleMapOptions` 对象的一个​​示例：

```csharp
GoogleMapOptions mapOptions = new GoogleMapOptions()
    .InvokeMapType(GoogleMap.MapTypeSatellite)
    .InvokeZoomControlsEnabled(false)
    .InvokeCompassEnabled(true);

FragmentTransaction fragTx = FragmentManager.BeginTransaction();
mapFragment = MapFragment.NewInstance(mapOptions);
fragTx.Add(Resource.Id.map, mapFragment, "map");
fragTx.Commit();
```

配置 `GoogleMap` 的另一种方法是操纵地图对象的 [UiSettings](https://developers.google.com/android/reference/com/google/android/gms/maps/UiSettings) 上的属性。 下一个代码示例演示如何配置 `GoogleMap` 用于显示缩放控件和指南针：

```csharp
public void OnMapReady(GoogleMap map)
{
    map.UiSettings.ZoomControlsEnabled = true;
    map.UiSettings.CompassEnabled = true;
}
```

## <a name="interacting-with-the-googlemap"></a>与 GoogleMap 交互

Android Maps API 提供一些 API，这些 API 允许 Activity 更改视点、添加标记、放置自定义叠加层或绘制几何形状。 本部分将讨论如何在 Xamarin.Android 中完成其中一些任务。

### <a name="changing-the-viewpoint"></a>更改视点

地图基于墨卡托投影建模为屏幕上的平面。 地图视图指*摄像机*在此平面上直接向下看时的视图。 可通过更改位置、缩放比例、倾斜度和方位角来控制摄像机的位置。 [CameraUpdate](https://developers.google.com/android/reference/com/google/android/gms/maps/CameraUpdate) 类用于移动摄像机位置。 `CameraUpdate` 对象不会直接实例化，相反，Maps API 会提供 [CameraUpdateFactory](https://developers.google.com/android/reference/com/google/android/gms/maps/CameraUpdateFactory) 类。

一旦创建 `CameraUpdate` 对象，它就会作为参数传递到 [GoogleMap.MoveCamera](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap#moveCamera(com.google.android.gms.maps.CameraUpdate)) 或 [GoogleMap.AnimateCamera](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap#animateCamera(com.google.android.gms.maps.CameraUpdate)) 方法。 `MoveCamera` 方法会立即更新地图，而 `AnimateCamera` 方法会提供平滑的动画过渡。

此代码片段是一个简单示例，演示如何使用 `CameraUpdateFactory` 创建 `CameraUpdate`，后者会将地图的缩放级别递增一个缩放级别：

```csharp
MapFragment mapFrag = (MapFragment) FragmentManager.FindFragmentById(Resource.Id.my_mapfragment_container);
mapFrag.GetMapAsync(this);
...

public void OnMapReady(GoogleMap map)
{   
    map.MoveCamera(CameraUpdateFactory.ZoomIn());
}
```

Maps API 提供 [CameraPosition](https://developer.android.com/reference/com/google/android/gms/maps/model/CameraPosition.html)，它将聚合摄像机位置的所有可能值。 可将此类的实例提供给 [CameraUpdateFactory.NewCameraPosition](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/CameraUpdateFactory#newCameraPosition%28com.google.android.gms.maps.model.CameraPosition%29) 方法，该方法将返回 `CameraUpdate` 对象。 Maps API 还包括 [CameraPosition.Builder](https://developer.android.com/reference/com/google/android/gms/maps/model/CameraPosition.Builder.html) 类，该类提供用于创建 `CameraPosition` 对象的 Fluent API。
以下代码片段演示从 `CameraPosition` 创建 `CameraUpdate` 并使用其更改 `GoogleMap` 上的摄像机位置的示例：

```csharp
public void OnMapReady(GoogleMap map)
{
    LatLng location = new LatLng(50.897778, 3.013333);

    CameraPosition.Builder builder = CameraPosition.InvokeBuilder();
    builder.Target(location);
    builder.Zoom(18);
    builder.Bearing(155);
    builder.Tilt(65);

    CameraPosition cameraPosition = builder.Build();

    CameraUpdate cameraUpdate = CameraUpdateFactory.NewCameraPosition(cameraPosition);

    map.MoveCamera(cameraUpdate);
}
```

在上述代码片段中，地图上的特定位置由 [LatLng](https://developers.google.com/android/reference/com/google/android/gms/maps/model/LatLng) 类表示。 缩放级别设置为 18，这是 Google Maps 使用的任意缩放度量。 方位角由指南针从北方顺时针进行测量。 倾斜度属性控制视角，并指定与垂直方向呈 25 度夹角。 以下屏幕截图显示执行上述代码后的 `GoogleMap`：

[![以倾斜视角显示指定位置的 Google Map 示例](maps-api-images/image06-sml.png)](maps-api-images/image06.png#lightbox)

### <a name="drawing-on-the-map"></a>在地图上绘制

Android Maps API 提供用于在地图上绘制以下项目的 API：

- **标记** - 这些是特殊图标，用于标识地图上的单个位置。

- **叠加层** - 这是一个图像，可用于标识地图上位置的集合或区域。

- **直线、多边形和圆形** - 这些 API 允许 Activity 向地图添加形状。

#### <a name="markers"></a>标记

Maps API 提供 [Marker](https://developers.google.com/android/reference/com/google/android/gms/maps/model/Marker) 类，该类封装有关地图上的单个位置的所有数据。 默认情况下，Marker 类使用 Google Maps 提供的标准图标。 可以自定义标记的外观以及响应用户的点击。

##### <a name="adding-a-marker"></a>添加标记

若要将标记添加到地图，必须创建新的 [MarkerOptions](https://developers.google.com/android/reference/com/google/android/gms/maps/model/MarkerOptions) 对象，然后在 `GoogleMap` 实例上调用 [AddMarker](https://developer.android.com/reference/com/google/android/gms/maps/GoogleMap.html#addMarker%28com.google.android.gms.maps.model.MarkerOptions%29) 方法。 此方法将返回 [Marker](https://developers.google.com/android/reference/com/google/android/gms/maps/model/Marker) 对象。

```csharp
public void OnMapReady(GoogleMap map)
{
    MarkerOptions markerOpt1 = new MarkerOptions();
    markerOpt1.SetPosition(new LatLng(50.379444, 2.773611));
    markerOpt1.SetTitle("Vimy Ridge");

    map.AddMarker(markerOpt1);
}
```

当用户点击标记时，标记的标题将在*信息窗口*中显示。 以下屏幕截图显示此标记的外观：

[![带有标记和 Vimy Ridge 信息窗口的 Google Map 示例](maps-api-images/image07-sml.png)](maps-api-images/image07.png#lightbox)

##### <a name="customizing-a-marker"></a>自定义标记

将标记添加到地图时，可以通过调用 `MarkerOptions.InvokeIcon` 方法来自定义标记使用的图标。
此方法采用 [BitmapDescriptor](https://developers.google.com/android/reference/com/google/android/gms/maps/model/BitmapDescriptor) 对象，该对象包含呈现图标所需的数据。 [BitmapDescriptorFactory](https://developers.google.com/android/reference/com/google/android/gms/maps/model/BitmapDescriptorFactory) 类提供一些 helper 方法，可用于简化 `BitmapDescriptor` 的创建。 以下列表介绍其中一些方法：

- `DefaultMarker(float colour)` &ndash; 使用默认 Google Maps 标记，但更改颜色。

- `FromAsset(string assetName)` &ndash; 使用 Assets 文件夹中指定文件的自定义图标。

- `FromBitmap(Bitmap image)` &ndash; 使用指定的位图作为图标。

- `FromFile(string fileName)` &ndash; 从指定路径的文件创建自定义图标。

- `FromResource(int resourceId)` &ndash; 从指定资源创建自定义图标。

以下代码片段展示创建青色默认标记的示例：

```csharp
public void OnMapReady(GoogleMap map)
{
    MarkerOptions markerOpt1 = new MarkerOptions();
    markerOpt1.SetPosition(new LatLng(50.379444, 2.773611));
    markerOpt1.SetTitle("Vimy Ridge");

    var bmDescriptor = BitmapDescriptorFactory.DefaultMarker (BitmapDescriptorFactory.HueCyan);
    markerOpt1.InvokeIcon(bmDescriptor);

    map.AddMarker(markerOpt1);
}
```

#### <a name="info-windows"></a>信息窗口

*信息窗口*是特殊窗口，当用户点击特定标记时，系统会弹出这些窗口用于向用户显示信息。 默认情况下，信息窗口将显示标记标题的内容。 如果尚未分配标题，则不会显示任何信息窗口。 一次只能显示一个信息窗口。

通过实现 [GoogleMap.IInfoWindowAdapter](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap.InfoWindowAdapter) 接口，可以自定义信息窗口。 此接口上有两种重要方法：

- `public View GetInfoWindow(Marker marker)` &ndash; 调用此方法以获取标记的自定义信息窗口。 如果返回 `null`，则使用默认的窗口呈现。 如果此方法返回 View，则该 View 会置于信息窗口框架内。

- `public View GetInfoContents(Marker marker)` &ndash; 仅当 GetInfoWindow 返回 `null` 时才会调用此方法。 如果要使用信息窗口内容的默认呈现，则此方法可能返回 `null` 值。 否则，此方法应返回具有信息窗口内容的 View。

信息窗口不是实时视图 - 相反，Android 将 View 转换为静态位图并将其显示在图像上。 这意味着信息窗口无法响应任何触控事件或手势，也不会自动进行更新。 若要更新信息窗口，必须调用 [GoogleMap.ShowInfoWindow](https://developers.google.com/android/reference/com/google/android/gms/maps/model/Marker.html#showInfoWindow()) 方法。

下图显示一些自定义信息窗口的部分示例。 左侧图像具有自定义内容，而右侧图像使用圆角自定义窗口和内容：

![墨尔本示例标记窗口，包括图标和人口。 右侧窗口具有圆角。](maps-api-images/marker-infowindows.png)

#### <a name="groundoverlays"></a>GroundOverlays

与标识地图上特定位置的标记不同，[GroundOverlay](https://developers.google.com/android/reference/com/google/android/gms/maps/model/GroundOverlay) 是用于标识地图上的位置集合或区域的图像。

##### <a name="adding-a-groundoverlay"></a>添加 GroundOverlay

在地图上添加地面叠加层与在地图上添加标记类似。 首先，创建 [GroundOverlayOptions](https://developers.google.com/android/reference/com/google/android/gms/maps/model/GroundOverlayOptions) 对象。 此对象随后作为参数传递到 [`GoogleMap.AddGroundOverlay`](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap.html#addGroundOverlay(com.google.android.gms.maps.model.GroundOverlayOptions)) 方法，后者将返回 `GroundOverlay` 对象。 此代码片段是向地图添加地面叠加层的示例：

```csharp
BitmapDescriptor image = BitmapDescriptorFactory.FromResource(Resource.Drawable.polarbear);
GroundOverlayOptions groundOverlayOptions = new GroundOverlayOptions()
    .Position(position, 150, 200)
    .InvokeImage(image);
GroundOverlay myOverlay = googleMap.AddGroundOverlay(groundOverlayOptions);
```

以下屏幕截图在地图上显示此叠加层：

[![具有北极熊重叠图像的示例地图](maps-api-images/image09-sml.png)](maps-api-images/image09.png#lightbox)

#### <a name="lines-circles-and-polygons"></a>直线、圆形和多边形

可将三种简单的几何图形添加到地图中：

- **折线** - 这是一系列连接的线段。 它可以在地图上标记路径或创建几何形状。

- **圆形** - 这将在地图上绘制一个圆。

- **多边形** - 这是一个闭合形状，用于标记地图上的区域。

##### <a name="polylines"></a>折线

[折线](https://developers.google.com/android/reference/com/google/android/gms/maps/model/Polyline)是一系列连续的 `LatLng` 对象，这些对象指定每个线段的顶点。 通过先创建 `PolylineOptions` 对象，然后将点添加到其中来创建折线。 接下来，通过调用 `AddPolyline` 方法将 `PolylineOption` 对象传递到 `GoogleMap` 对象。

```csharp
PolylineOption rectOptions = new PolylineOption();
rectOptions.Add(new LatLng(37.35, -122.0));
rectOptions.Add(new LatLng(37.45, -122.0));
rectOptions.Add(new LatLng(37.45, -122.2));
rectOptions.Add(new LatLng(37.35, -122.2));
rectOptions.Add(new LatLng(37.35, -122.0)); // close the polyline - this makes a rectangle.

googleMap.AddPolyline(rectOptions);
```

##### <a name="circles"></a>圆形

通过先实例化 [CircleOption](https://developers.google.com/android/reference/com/google/android/gms/maps/model/CircleOptions) 对象来创建圆形，该对象将以米为单位指定圆的中心和半径。 通过调用 [GoogleMap.AddCircle](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap.html#addCircle(com.google.android.gms.maps.model.CircleOptions)) 在地图上绘制圆形。
以下代码片段演示如何绘制圆形：

```csharp
CircleOptions circleOptions = new CircleOptions ();
circleOptions.InvokeCenter (new LatLng(37.4, -122.1));
circleOptions.InvokeRadius (1000);

googleMap.AddCircle (circleOptions);
```

##### <a name="polygons"></a>多边形

`Polygon` 与 `Polyline` 类似，但它们不是开放式的。 `Polygon` 是内部空间已填充的闭环。
除调用 [GoogleMap.AddPolygon](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap.html#addPolygon(com.google.android.gms.maps.model.PolygonOptions)) 方法外，`Polygon` 的创建方式与 `Polyline` 的创建方式完全相同。

与 `Polyline` 不同，`Polygon` 自动闭合。 通过绘制一条连接第一个点和最后一个点的线，可通过 `AddPolygon` 方法闭合多边形。 以下代码片段将在与 `Polyline` 示例中的之前代码片段相同的区域创建实心矩形。

```csharp
PolygonOptions rectOptions = new PolygonOptions();
rectOptions.Add(new LatLng(37.35, -122.0));
rectOptions.Add(new LatLng(37.45, -122.0));
rectOptions.Add(new LatLng(37.45, -122.2));
rectOptions.Add(new LatLng(37.35, -122.2));
// notice we don't need to close off the polygon

googleMap.AddPolygon(rectOptions);
```

## <a name="responding-to-user-events"></a>响应用户事件

用户可与地图进行三种交互：

- **标记点击** - 用户点击标记。

- **标记拖动** - 用户长按 mparger

- **信息窗口点击** - 用户点击信息窗口。

其中每个事件都将在下文中更详细地讨论。

### <a name="marker-click-events"></a>标记点击事件

用户点击标记时会引发 `MarkerClicked` 事件。 此事件接受 `GoogleMap.MarkerClickEventArgs` 对象作为参数。 此类包含两个属性：

- `GoogleMap.MarkerClickEventArgs.Handled` &ndash; 此属性应设置为 `true`，以指示事件处理程序已使用该事件。 如果将其设置为 `false`，则除事件处理程序的自定义行为外，还将发生默认行为。

- `Marker` &ndash; 此属性是对引发 `MarkerClick` 事件的标记的引用。

此代码片段演示 `MarkerClick` 的示例，该示例会将摄像机位置更改为地图上的新位置：

```csharp
void MapOnMarkerClick(object sender, GoogleMap.MarkerClickEventArgs markerClickEventArgs)
{
    markerClickEventArgs.Handled = true;

    var marker = markerClickEventArgs.Marker;
    if (marker.Id.Equals(gotMauiMarkerId))
    {
        LatLng InMaui = new LatLng(20.72110, -156.44776);

        // Move the camera to look at Maui.
        PositionPolarBearGroundOverlay(InMaui);
        googleMap.AnimateCamera(CameraUpdateFactory.NewLatLngZoom(InMaui, 13));
        gotMauiMarkerId = null;
        polarBearMarker.Remove();
        polarBearMarker = null;
    }
    else
    {
        Toast.MakeText(this, $"You clicked on Marker ID {marker.Id}", ToastLength.Short).Show();
    }
}
```

### <a name="marker-drag-events"></a>标记拖动事件

用户希望拖动标记时会引发此事件。 默认情况下，标记不可拖动。 通过将 `Marker.Draggable` 属性设置为 `true` 或通过使用 `true` 作为参数调用 `MarkerOptions.Draggable` 方法，可将标记设置为可拖动。

若要拖动标记，用户必须先长按标记，其手指随后必须停留在地图上。 当用户的手指在屏幕上四处拖动时，标记将移动。 当用户的手指离开屏幕时，标记将保留在原位。

以下列表描述将为可拖动标记引发的各种事件：

- `GoogleMap.MarkerDragStart(object sender, GoogleMap.MarkerDragStartEventArgs e)` &ndash; 在用户首次拖动标记时引发此事件。

- `GoogleMap.MarkerDrag(object sender, GoogleMap.MarkerDragEventArgs e)` &ndash; 在拖动标记时引发此事件。

- `GoogleMap.MarkerDragEnd(object sender, GoogleMap.MarkerDragEndEventArgs e)` &ndash; 在用户完成拖动标记时引发此事件。

每个 `EventArgs` 都包含一个名为 `P0` 的属性，该属性是对要拖动的 `Marker` 对象的引用。

### <a name="info-window-click-events"></a>信息窗口点击事件

一次只能显示一个信息窗口。 当用户点击地图中的信息窗口时，地图对象将引发 `InfoWindowClick` 事件。 以下代码片段演示如何将处理程序连接到事件：

```csharp
public void OnMapReady(GoogleMap map)
{
    map.InfoWindowClick += MapOnInfoWindowClick;
}

private void MapOnInfoWindowClick (object sender, GoogleMap.InfoWindowClickEventArgs e)
{
    Marker myMarker = e.Marker;
    // Do something with marker.
}
```

回想一下，信息窗口是静态 `View`，其在地图上呈现为图像。 放置在信息窗口中的任何小组件（例如按钮、复选框或文本视图）都将是静态的，无法响应任何整体用户事件。

## <a name="related-links"></a>相关链接

- [SimpleMapDemo](https://github.com/xamarin/monodroid-samples/tree/master/MapsAndLocationDemo_v3/SimpleMapDemo)
- [Google Play Services](https://developers.google.com/android/guides/overview)
- [Google Maps Android API v2](https://developers.google.com/maps/documentation/android-sdk/intro)
- [Google Play Services APK](https://play.google.com/store/apps/details?id=com.google.android.gms&hl=en)
- [获取 Google Maps API 密钥](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md)
- [uses-library](https://developer.android.com/guide/topics/manifest/uses-library-element)
- [uses-feature](https://developer.android.com/guide/topics/manifest/uses-feature-element)
