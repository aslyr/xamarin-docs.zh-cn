---
title: ''
description: 本文介绍如何使用 DataPages 开始构建简单的数据驱动页面 Xamarin.Forms 。
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 17cc67c7fcc89454ff8dcac9926617b4ed1f4b77
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/28/2020
ms.locfileid: "84134389"
---
# <a name="getting-started-with-datapages"></a>入门与 DataPages

[![下载示例](~/media/shared/download.png) 下载示例](https://github.com/xamarin/xamarin-forms-samples/tree/master/Pages/DataPagesDemo)

![](~/media/shared/preview.png "This API is currently in preview")

> [!IMPORTANT]
> DataPages 需要 Xamarin.Forms 主题引用以呈现。 这涉及到安装[ Xamarin.Forms 。主题：基本](https://www.nuget.org/packages/Xamarin.Forms.Theme.Base/)NuGet 包到你的项目中，然后是[ Xamarin.Forms 。主题浅](https://www.nuget.org/packages/Xamarin.Forms.Theme.Light/)或[ Xamarin.Forms 。主题深色](https://www.nuget.org/packages/Xamarin.Forms.Theme.Dark/)NuGet 包。

若要开始使用 DataPages 预览版构建简单的数据驱动页面，请执行以下步骤。 此演示在预览版本中使用硬编码样式（"事件"），该样式仅适用于代码中的特定 JSON 格式。

[![](get-started-images/demo-sml.png "DataPages Sample Application")](get-started-images/demo.png#lightbox "DataPages Sample Application")

## <a name="1-add-nuget-packages"></a>1. 添加 NuGet 包

将以下 NuGet 包添加到 Xamarin.Forms .NET Standard 库和应用程序项目：

- Xamarin.Forms.页
- Xamarin.Forms.主题。基本
- 主题实现 NuGet （例如 Xamarin.Forms.主题浅）

## <a name="2-add-theme-reference"></a>2. 添加主题引用

在**app.xaml**文件中，为主题添加自定义， `xmlns:mytheme` 并确保将主题合并到应用程序的资源字典中：

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms"
  xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
  xmlns:mytheme="clr-namespace:Xamarin.Forms.Themes;assembly=Xamarin.Forms.Theme.Light"
  x:Class="DataPagesDemo.App">
    <Application.Resources>
        <ResourceDictionary MergedWith="mytheme:LightThemeResources" />
    </Application.Resources>
</Application>
```

> [!IMPORTANT]
> 还应按照以下步骤通过将一些样板代码添加到 iOS 和 Android 来[加载主题程序集（见下文）](#loadtheme) `AppDelegate` `MainActivity` 。 此功能将在将来的预览版本中得到改进。

## <a name="3-add-a-xaml-page"></a>3. 添加 XAML 页

向应用程序添加新的 XAML 页 Xamarin.Forms ，并将*基类从更改* `ContentPage` 为 `Xamarin.Forms.Pages.ListDataPage` 。 这必须在 c # 和 XAML 中完成：

**C # 文件**

```csharp
public partial class SessionDataPage : Xamarin.Forms.Pages.ListDataPage // was ContentPage
{
  public SessionDataPage ()
  {
    InitializeComponent ();
  }
}
```

**XAML 文件**

除了将根元素更改为 `<p:ListDataPage>` 自定义命名空间 `xmlns:p` 还必须添加：

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<p:ListDataPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:p="clr-namespace:Xamarin.Forms.Pages;assembly=Xamarin.Forms.Pages"
             x:Class="DataPagesDemo.SessionDataPage">

    <ContentPage.Content></ContentPage.Content>

</p:ListDataPage>
```

**应用程序子类**

更改 `App` 类构造函数，以便将 `MainPage` 设置为 `NavigationPage` 包含新的 `SessionDataPage` 。 *必须*使用导航页。

```csharp
MainPage = new NavigationPage (new SessionDataPage ());
```

## <a name="3-add-the-datasource"></a>3. 添加数据源

删除 `Content` 元素并将其替换为 `p:ListDataPage.DataSource` ，以使用数据填充页面。 在下面的示例中，从 URL 加载远程 Json 数据文件。

> [!NOTE]
> 预览*需要*一个 `StyleClass` 属性以提供数据源的呈现提示。 是 `StyleClass="Events"` 指在预览中预定义的布局，其中包含与所使用的 JSON 数据源相匹配的*硬编码*样式。

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<p:ListDataPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:p="clr-namespace:Xamarin.Forms.Pages;assembly=Xamarin.Forms.Pages"
             x:Class="DataPagesDemo.SessionDataPage"
             Title="Sessions" StyleClass="Events">

    <p:ListDataPage.DataSource>
        <p:JsonDataSource Source="http://demo3143189.mockable.io/sessions" />
    </p:ListDataPage.DataSource>

</p:ListDataPage>
```

**JSON 数据**

下面显示了演示源中的 JSON 数据的示例：

```json
[{
  "end": "2016-04-27T18:00:00Z",
  "start": "2016-04-27T17:15:00Z",
  "abstract": "The new Apple TV has been released, and YOU can be one of the first developers to write apps for it. To make things even better, you can build these apps in C#! This session will introduce the basics of how to create a tvOS app with Xamarin, including: differences between tvOS and iOS APIs, TV user interface best practices, responding to user input, as well as the capabilities and limitations of building apps for a television. Grab some popcorn—this is going to be good!",
  "title": "As Seen On TV … Bringing C# to the Living Room",
  "presenter": "Matthew Soucoup",
  "biography": "Matthew is a Xamarin MVP and Certified Xamarin Developer from Madison, WI. He founded his company Code Mill Technologies and started the Madison Mobile .Net Developers Group.  Matt regularly speaks on .Net and Xamarin development at user groups, code camps and conferences throughout the Midwest. Matt gardens hot peppers, rides bikes, and loves Wisconsin micro-brews and cheese.",
  "image": "http://i.imgur.com/ASj60DP.jpg",
  "avatar": "http://i.imgur.com/ASj60DP.jpg",
  "room": "Crick"
}]
```

## <a name="4-run"></a>4. 运行！

以上步骤应会生成一个工作数据页：

[![](get-started-images/demo-sml.png "DataPages Sample Application")](get-started-images/demo.png#lightbox "DataPages Sample Application")

这是因为预建的样式 **"事件"** 存在于浅色主题 NuGet 包中，并定义了与数据源匹配的样式（例如 "title"、"image"、"呈现器"）。

生成 "事件" `StyleClass` 是为了显示控件， `ListDataPage` 其中包含 `CardView` 在中定义的自定义控件 Xamarin.Forms 。页. `CardView`控件具有三个属性： `ImageSource` 、 `Text` 和 `Detail` 。 主题进行了硬编码，以便将数据源的三个字段（从 JSON 文件）绑定到这些属性以进行显示。

## <a name="5-customize"></a>5. 自定义

通过指定模板和使用数据源绑定，可以重写继承的样式。 下面的 XAML 使用中包含的新的和语法为每一行声明一个自定义模板 `ListItemControl` `{p:DataSourceBinding}` ** Xamarin.Forms 。页面**NuGet：

```xaml
<p:ListDataPage.DefaultItemTemplate>
    <DataTemplate>
        <ViewCell>
            <p:ListItemControl
                Title="{p:DataSourceBinding title}"
                Detail="{p:DataSourceBinding room}"
                ImageSource="{p:DataSourceBinding image}"
                DataSource="{Binding Value}"
                HeightRequest="90"
            >
            </p:ListItemControl>
        </ViewCell>
    </DataTemplate>
</p:ListDataPage.DefaultItemTemplate>
```

通过提供 `DataTemplate` 此代码，将重写 `StyleClass` 并改用的默认布局 `ListItemControl` 。

[![](get-started-images/custom-sml.png "DataPages Sample Application")](get-started-images/custom.png#lightbox "DataPages Sample Application")

更倾向于 XAML 的开发人员也可以创建数据源绑定（请记住包含 `using Xamarin.Forms.Pages;` 语句）：

```csharp
SetBinding (TitleProperty, new DataSourceBinding ("title"));
```

从头开始创建主题的工作要多一些，但将来的预览版本会使此操作变得更容易。

## <a name="troubleshooting"></a>故障排除

<a name="loadtheme" />

## <a name="could-not-load-file-or-assembly-xamarinformsthemelight-or-one-of-its-dependencies"></a>未能加载文件或程序集 " Xamarin.Forms 。主题 "Light" 或其依赖项之一

在预览版本中，主题可能无法在运行时加载。 将下面显示的代码添加到相关项目以修复此错误。

**iOS**

在**AppDelegate.cs**中，将以下行添加到`LoadApplication`

```csharp
var x = typeof(Xamarin.Forms.Themes.DarkThemeResources);
x = typeof(Xamarin.Forms.Themes.LightThemeResources);
x = typeof(Xamarin.Forms.Themes.iOS.UnderlineEffect);
```

**Android**

在**MainActivity.cs**中，将以下行添加到`LoadApplication`

```csharp
var x = typeof(Xamarin.Forms.Themes.DarkThemeResources);
x = typeof(Xamarin.Forms.Themes.LightThemeResources);
x = typeof(Xamarin.Forms.Themes.Android.UnderlineEffect);
```

## <a name="related-links"></a>相关链接

- [DataPagesDemo 示例](https://github.com/xamarin/xamarin-forms-samples/tree/master/Pages/DataPagesDemo)
