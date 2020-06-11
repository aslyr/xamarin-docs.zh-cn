---
标题： "添加 AppCompat 和材料设计" 说明： "本文介绍了如何转换现有 Xamarin.Forms Android 应用程序以使用 AppCompat 和材料设计。"
ms-chap： xamarin assetid： 045FBCDF-4D45-48BB-9911-BD3938C87D58 毫秒： xamarin 窗体作者： davidbritch 毫秒. 作者： dabritch 毫秒。日期：06/27/2017 非 loc： [ Xamarin.Forms ， Xamarin.Essentials ]
---

# <a name="adding-appcompat-and-material-design"></a>添加 AppCompat 和材料设计

_请按照以下步骤将现有 Xamarin.Forms Android 应用转换为使用 AppCompat 和材料设计_

<!-- source https://gist.github.com/jassmith/a3b2a543f99126782936
https://blog.xamarin.com/material-design-for-your-xamarin-forms-android-apps/ -->

## <a name="overview"></a>概述

这些说明解释了如何更新现有 Xamarin.Forms Android 应用程序以使用 AppCompat 库，并在 Android 版本的应用中启用材料设计 Xamarin.Forms 。

### <a name="1-update-xamarinforms"></a>1. 更新Xamarin.Forms

确保解决方案使用 Xamarin.Forms 2.0 或更高版本。 更新Xamarin.Forms
  NuGet 包到2.0 （如果需要）。

### <a name="2-check-android-version"></a>2. 检查 Android 版本

确保 Android 项目的目标框架为 Android 6.0 （Marshmallow）。 检查**Android 项目 > 选项 "> 生成 > 常规**设置"，确保选择 corrent 框架：

 ![](appcompat-images/target-android-6-sml.png "Android General Build Configuration")

### <a name="3-add-new-themes-to-support-material-design"></a>3. 添加新主题以支持材料设计

在 Android 项目中创建以下三个文件，并粘贴以下内容。 Google 提供了[样式指南](https://www.google.com/design/spec/style/color.html#color-color-palette)和[调色板生成器](https://www.materialpalette.com/)，可帮助你为指定的配色方案选择一个替代配色方案。

**资源/值/colors.xml**

```xml
<resources>
  <color name="primary">#2196F3</color>
  <color name="primaryDark">#1976D2</color>
  <color name="accent">#FFC107</color>
  <color name="window_background">#F5F5F5</color>
</resources>
```

**资源/值/style.xml**

```xml
<resources>
  <style name="MyTheme" parent="MyTheme.Base">
  </style>
  <style name="MyTheme.Base" parent="Theme.AppCompat.Light.NoActionBar">
    <item name="colorPrimary">@color/primary</item>
    <item name="colorPrimaryDark">@color/primaryDark</item>
    <item name="colorAccent">@color/accent</item>
    <item name="android:windowBackground">@color/window_background</item>
    <item name="windowActionModeOverlay">true</item>
  </style>
</resources>
```

在 Android 棒糖和更高版本上运行时，必须在**v21**文件夹中包含附加样式，以应用特定属性。

**资源/值-v21/style.xml**

```xml
<resources>
  <style name="MyTheme" parent="MyTheme.Base">
    <!--If you are using MasterDetailPage you will want to set these, else you can leave them out-->
    <!--<item name="android:windowDrawsSystemBarBackgrounds">true</item>
    <item name="android:statusBarColor">@android:color/transparent</item>-->
  </style>
</resources>
```

### <a name="4-update-androidmanifestxml"></a>4. 更新 AndroidManifest.xml

若要确保使用这一新的主题信息，请在**androidmanifest.xml**文件中添加 "主题"，方法是添加 `android:theme="@style/MyTheme"` （保留 XML 的其余部分）。

**Properties/AndroidManifest.xml**

```xml
...
<application android:label="AppName" android:icon="@drawable/icon"
  android:theme="@style/MyTheme">
...
```

### <a name="5-provide-toolbar-and-tab-layouts"></a>5. 提供工具栏和选项卡布局

在**资源/布局**目录中创建**选项卡栏 main.axml**和**main.axml**文件，并粘贴以下内容：

**Resources/layout/选项卡栏. main.axml**

```xml
<android.support.design.widget.TabLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/sliding_tabs"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:background="?attr/colorPrimary"
    android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
    app:tabIndicatorColor="@android:color/white"
    app:tabGravity="fill"
    app:tabMode="fixed" />
```

已设置选项卡的几个属性，其中包括 tab 的重力到 `fill` 和模式 `fixed` 。
如果有很多选项卡，你可能想要将此选项卡切换到可滚动-通过 Android [TabLayout 文档](https://developer.android.com/reference/android/support/design/widget/TabLayout.html)阅读，以了解详细信息。

**资源/布局/工具栏. main.axml**

```xml
<android.support.v7.widget.Toolbar
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/toolbar"
    android:layout_width="match_parent"
    android:layout_height="?attr/actionBarSize"
    android:minHeight="?attr/actionBarSize"
    android:background="?attr/colorPrimary"
    android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
    app:popupTheme="@style/ThemeOverlay.AppCompat.Light"
    app:layout_scrollFlags="scroll|enterAlways" />
```

在这些文件中，我们将为你的应用程序的工具栏创建特定主题。
请参阅[Hello 工具栏](https://blog.xamarin.com/android-tips-hello-toolbar-goodbye-action-bar/)博客文章了解详细信息。

### <a name="6-update-the-mainactivity"></a>6. 更新`MainActivity`

在现有 Xamarin.Forms 应用中， **MainActivity.cs**类将从继承 `FormsApplicationActivity` 。 必须将其替换 `FormsAppCompatActivity` 为才能启用新功能。

**MainActivity.cs**

```csharp
public class MainActivity : FormsAppCompatActivity  // was FormsApplicationActivity
```

最后，将方法中的步骤5的新布局 "连接到" `OnCreate` ，如下所示：

```csharp
protected override void OnCreate(Bundle bundle)
{
  // set the layout resources first
  FormsAppCompatActivity.ToolbarResource = Resource.Layout.Toolbar;
  FormsAppCompatActivity.TabLayoutResource = Resource.Layout.Tabbar;

  // then call base.OnCreate and the Xamarin.Forms methods
  base.OnCreate(bundle);
  Forms.Init(this, bundle);
  LoadApplication(new App());
}
```
