---
title: Xamarin.表单资源字典
description: Xamarin.Forms XAML 资源是可在 Xamarin.Forms 应用程序中共享和重用的对象。
ms.prod: xamarin
ms.assetid: DF103686-4A92-40FA-9CF1-A9376293B13C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/01/2020
ms.custom: video
ms.openlocfilehash: 8dd3c7f36ddd436a812927816a1326dbb7c48341
ms.sourcegitcommit: ee9e48e2ec643915f42a6c1641077970ae20cb17
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/01/2020
ms.locfileid: "80523211"
---
# <a name="xamarinforms-resource-dictionaries"></a>Xamarin.表单资源字典

[![下载示例](~/media/shared/download.png)下载示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-resourcedictionaries)

A[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)是 Xamarin.Forms 应用程序使用的资源的存储库。 存储在`ResourceDictionary`中的典型资源包括[样式](~/xamarin-forms/user-interface/styles/index.md)、[控件模板](~/xamarin-forms/app-fundamentals/templates/control-template.md)、[数据模板](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)、颜色和转换器。

在 XAML 中，[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)可以使用`StaticResource`或`DynamicResource`标记扩展来引用 存储在 中的资源并将其应用于元素。 在 C# 中，资源也可以在`ResourceDictionary`中定义，然后通过使用基于字符串的索引器引用并应用于元素。 但是，在 C#`ResourceDictionary`中使用 没有什么好处，因为共享对象可以存储为字段或属性，并且无需首先从字典中检索它们即可直接访问。

## <a name="create-resources-in-xaml"></a>在 XAML 中创建资源

每个[`VisualElement`](xref:Xamarin.Forms.VisualElement)派生对象都有一[`Resources`](xref:Xamarin.Forms.VisualElement.Resources)个属性，该属性[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)可以包含资源。 同样，[`Application`](xref:Xamarin.Forms.Application)派生对象具有一个[`Resources`](xref:Xamarin.Forms.VisualElement.Resources)属性，该属性可以[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)包含资源。

Xamarin.Forms 应用程序仅包含派生自[`Application`](xref:Xamarin.Forms.Application)的类，但通常使用派生自[`VisualElement`](xref:Xamarin.Forms.VisualElement)的许多类，包括页面、布局和控件。 这些对象中的任何一个都可以将其`Resources`属性设置为[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)包含资源。 选择在何处放置特定`ResourceDictionary`影响，以便使用资源：

- 附加到视图`ResourceDictionary`（如[`Button`](xref:Xamarin.Forms.Button)或[`Label`](xref:Xamarin.Forms.Label)只能应用于该特定对象的）中的资源。
- `ResourceDictionary`附加到布局的资源（如[`StackLayout`](xref:Xamarin.Forms.StackLayout)或[`Grid`](xref:Xamarin.Forms.Grid)）可以应用于布局以及该布局的所有子级。
- 在页面级别`ResourceDictionary`定义的资源可以应用于页面及其所有子级。
- 在应用程序级别`ResourceDictionary`定义的资源可以应用于整个应用程序。

除了隐式样式外，资源字典中的每个资源都必须具有使用 属性`x:Key`定义的唯一字符串键。

以下 XAML 显示了在`ResourceDictionary`**App.xaml**文件中的应用程序级别中定义的资源：

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ResourceDictionaryDemo.App">
    <Application.Resources>

        <Thickness x:Key="PageMargin">20</Thickness>

        <!-- Colors -->
        <Color x:Key="AppBackgroundColor">AliceBlue</Color>
        <Color x:Key="NavigationBarColor">#1976D2</Color>
        <Color x:Key="NavigationBarTextColor">White</Color>
        <Color x:Key="NormalTextColor">Black</Color>

        <!-- Implicit styles -->
        <Style TargetType="{x:Type NavigationPage}">
            <Setter Property="BarBackgroundColor"
                    Value="{StaticResource NavigationBarColor}" />
            <Setter Property="BarTextColor"
                    Value="{StaticResource NavigationBarTextColor}" />
        </Style>

        <Style TargetType="{x:Type ContentPage}"
               ApplyToDerivedTypes="True">
            <Setter Property="BackgroundColor"
                    Value="{StaticResource AppBackgroundColor}" />
        </Style>

    </Application.Resources>
</Application>
```

在此示例中，资源字典定义资源、多个[`Thickness`](xref:Xamarin.Forms.Thickness)[`Color`](xref:Xamarin.Forms.Color)资源和两个隐式[`Style`](xref:Xamarin.Forms.Style)资源。 有关该类的详细信息，`App`请参阅[Xamarin.Forms 应用类](~/xamarin-forms/app-fundamentals/application-class.md)。

> [!NOTE]
> 将所有资源放在显式`ResourceDictionary`标记之间也有效。 但是，由于 Xamarin.窗体 3.0，`ResourceDictionary`因此不需要标记。 相反，对象`ResourceDictionary`是自动创建的，您可以直接在`Resources`属性元素标记之间插入资源。

## <a name="consume-resources-in-xaml"></a>在 XAML 中使用资源

每个资源都有一个使用`x:Key`属性指定的键，该属性将成为 中的`ResourceDictionary`字典密钥。 该键用于引用[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)具有[`StaticResource`](xref:Xamarin.Forms.Xaml.StaticResourceExtension)或[`DynamicResource`](xref:Xamarin.Forms.Xaml.DynamicResourceExtension)标记扩展的资源。

`StaticResource`标记扩展与`DynamicResource`标记扩展类似，因为两者都使用字典键引用资源字典中的值。 但是，`StaticResource`当标记扩展执行单个字典查找时，`DynamicResource`标记扩展保留指向字典键的链接。 因此，如果替换与键关联的字典条目，则更改将应用于可视元素。 这允许在应用程序中进行运行时资源更改。 有关标记扩展的详细信息，请参阅[XAML 标记扩展](~/xamarin-forms/xaml/markup-extensions/index.md)。

下面的 XAML 示例演示如何使用资源，并在 中定义其他资源[`StackLayout`](xref:Xamarin.Forms.StackLayout)：

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ResourceDictionaryDemo.HomePage"
             Title="Home Page">
    <StackLayout Margin="{StaticResource PageMargin}">
        <StackLayout.Resources>
            <!-- Implicit style -->
            <Style TargetType="Button">
                <Setter Property="FontSize" Value="Medium" />
                <Setter Property="BackgroundColor" Value="#1976D2" />
                <Setter Property="TextColor" Value="White" />
                <Setter Property="CornerRadius" Value="5" />
            </Style>
        </StackLayout.Resources>

        <Label Text="This app demonstrates consuming resources that have been defined in resource dictionaries." />
        <Button Text="Navigate"
                Clicked="OnNavigateButtonClicked" />
    </StackLayout>
</ContentPage>
```

在此示例中，[`ContentPage`](xref:Xamarin.Forms.ContentPage)对象使用应用程序级资源字典中定义的隐式样式。 对象[`StackLayout`](xref:Xamarin.Forms.StackLayout)使用应用程序级资源`PageMargin`字典中定义的资源，[`Button`](xref:Xamarin.Forms.Button)而该对象使用[`StackLayout`](xref:Xamarin.Forms.StackLayout)资源字典中定义的隐式样式。 这会导致如以下屏幕截图中所示的外观：

[![消耗资源字典资源](resource-dictionaries-images/consuming.png "使用资源字典资源")](resource-dictionaries-images/consuming-large.png#lightbox "使用资源字典资源")

> [!IMPORTANT]
> 特定于单个页面的资源不应包含在应用程序级资源字典中，因为这些资源将在应用程序启动时进行分析，而不是在页面需要时分析。 有关详细信息，请参阅[减小应用程序资源字典大小](~/xamarin-forms/deploy-test/performance.md)。

## <a name="resource-lookup-behavior"></a>资源查找行为

当使用[`StaticResource`](xref:Xamarin.Forms.Xaml.StaticResourceExtension)或[`DynamicResource`](xref:Xamarin.Forms.Xaml.DynamicResourceExtension)标记扩展引用资源时，将发生以下查找过程：

- 在资源字典中检查请求的键（如果存在）中设置该属性的元素。 如果找到请求的密钥，则返回其值并终止查找过程。
- 如果未找到匹配项，则查找过程向上搜索可视化树，检查每个父元素的资源字典。 如果找到请求的密钥，则返回其值并终止查找过程。 否则，该过程将继续向上，直到到达根元素。
- 如果在根元素上找不到匹配项，则检查应用程序级资源字典。
- 如果未找到匹配项，则引发 。 `XamlParseException`

因此，当 XAML 解析器遇到`StaticResource``DynamicResource`或 标记扩展时，它会使用它找到的第一个匹配项，通过可视化树向上移动来搜索匹配键。 如果此搜索在页面结束，并且尚未找到密钥，则 XAML 解析器将搜索`ResourceDictionary`附加到对象。 `App` 如果未找到该键，则引发异常。

## <a name="override-resources"></a>覆盖资源

当资源共享键时，在可视化树中定义较低的资源将优先于向上定义的资源。 `AppBackgroundColor`例如，将`AliceBlue`资源设置为 应用程序级别的资源将被设置为`AppBackgroundColor``Teal`的页面级别资源覆盖。 同样，控件级别资源`AppBackgroundColor`将覆盖页面级别`AppBackgroundColor`资源。

## <a name="stand-alone-resource-dictionaries"></a>独立资源字典

派生自的[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)类也可以位于单独的独立文件中。 然后，可以在应用程序之间共享结果的文件。

要创建此类文件，请向项目添加新**的内容视图**或**内容页**项（但不包括仅包含 C# 文件**的内容视图**或**内容页**）。 删除代码背后的文件，并在 XAML 文件中更改基类的名称从[`ContentView`](xref:Xamarin.Forms.ContentView)或[`ContentPage`](xref:Xamarin.Forms.ContentPage)更改为[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)。 此外，从文件的根`x:Class`标记中删除该属性。

下面的 XAML 示例显示了[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)一个名为**MyResource字典.xaml**的 ：

```xaml
<ResourceDictionary xmlns="http://xamarin.com/schemas/2014/forms"
                    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml">
    <DataTemplate x:Key="PersonDataTemplate">
        <ViewCell>
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="0.5*" />
                    <ColumnDefinition Width="0.2*" />
                    <ColumnDefinition Width="0.3*" />
                </Grid.ColumnDefinitions>
                <Label Text="{Binding Name}"
                       TextColor="{StaticResource NormalTextColor}"
                       FontAttributes="Bold" />
                <Label Grid.Column="1"
                       Text="{Binding Age}"
                       TextColor="{StaticResource NormalTextColor}" />
                <Label Grid.Column="2"
                       Text="{Binding Location}"
                       TextColor="{StaticResource NormalTextColor}"
                       HorizontalTextAlignment="End" />
            </Grid>
        </ViewCell>
    </DataTemplate>
</ResourceDictionary>
```

在此示例中，[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)包含单个资源，该资源是 类型 的对象[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)。 **MyResource字典.xaml**可以通过将其合并到另一个资源字典中来使用。

## <a name="merged-resource-dictionaries"></a>合并的资源字典

合并的资源字典将一个或多个[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)对象合并到另一个`ResourceDictionary`对象中。

### <a name="merge-local-resource-dictionaries"></a>合并本地资源字典

通过创建[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)具有资源的[`Source`](xref:Xamarin.Forms.ResourceDictionary.Source)XAML`ResourceDictionary`文件的文件`ResourceDictionary`名的对象，可以将本地文件合并到另一个文件：

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <!-- Add more resources here -->
        <ResourceDictionary Source="MyResourceDictionary.xaml" />
        <!-- Add more resources here -->
    </ContentPage.Resources>
    ...
</ContentPage>
```

此语法不会实例化类`MyResourceDictionary`。 相反，它引用 XAML 文件。 因此，在设置[`Source`](xref:Xamarin.Forms.ResourceDictionary.Source)属性时，不需要代码背后的文件，并且可以从`x:Class`**MyResource字典.xaml**文件的根标记中删除该属性。

> [!IMPORTANT]
> 只能[`Source`](xref:Xamarin.Forms.ResourceDictionary.Source)从 XAML 设置该属性。

### <a name="merge-resource-dictionaries-from-other-assemblies"></a>合并其他程序集的资源字典

还可以[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)通过将 A 添加到`ResourceDictionary`的属性[`MergedDictionaries`](xref:Xamarin.Forms.ResourceDictionary.MergedDictionaries)`ResourceDictionary`中，将其合并为另一个。 此技术允许合并资源字典，而不管它们驻留的程序集是什么。 从外部程序集合并资源字典[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)需要 将生成操作设置为**EmbeddedResource，** 具有代码后面的文件，并在文件的根标记中定义`x:Class`属性。

> [!WARNING]
> 类[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)还定义属性[`MergedWith`](xref:Xamarin.Forms.ResourceDictionary.MergedWith)。 但是，此属性已被弃用，不应再使用。

以下代码示例显示两个资源字典被添加到页面级别的[`MergedDictionaries`](xref:Xamarin.Forms.ResourceDictionary.MergedDictionaries)[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)集合中：

```xaml
<ContentPage ...
             xmlns:local="clr-namespace:ResourceDictionaryDemo"
             xmlns:theme="clr-namespace:MyThemes;assembly=MyThemes">
    <ContentPage.Resources>
        <ResourceDictionary>
            <!-- Add more resources here -->
            <ResourceDictionary.MergedDictionaries>
                <!-- Add more resource dictionaries here -->
                <local:MyResourceDictionary />
                <theme:LightTheme />
                <!-- Add more resource dictionaries here -->
            </ResourceDictionary.MergedDictionaries>
            <!-- Add more resources here -->
        </ResourceDictionary>
    </ContentPage.Resources>
    ...
</ContentPage>
```

在此示例中，来自同一程序集的资源字典和外部程序集的资源字典将合并到页面级别资源字典中。 此外，您还可以在`ResourceDictionary`[`MergedDictionaries`](xref:Xamarin.Forms.ResourceDictionary.MergedDictionaries)属性元素标记中添加其他对象，以及这些标记之外的其他资源。

> [!IMPORTANT]
> 中只能有一个`MergedDictionaries`[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)属性元素标记，但可以根据需要将尽可能多的`ResourceDictionary`对象放入其中。

当合并[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)的资源共享相同的`x:Key`属性值时，Xamarin.Forms 使用以下资源优先级：

1. 资源字典的本地资源。
1. 通过`MergedDictionaries`集合合并的资源字典中包含的资源，其顺序相反，它们列在属性中`MergedDictionaries`。

> [!NOTE]
> 如果应用程序包含多个大型资源字典，则搜索资源字典可能是计算密集型任务。 因此，为了避免不必要的搜索，应确保应用程序中的每个页面只使用适合该页的资源字典。

## <a name="related-links"></a>相关链接

- [资源字典（示例）](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-resourcedictionaries)
- [XAML 标记扩展](~/xamarin-forms/xaml/markup-extensions/index.md)
- [Xamarin.Forms 样式](~/xamarin-forms/user-interface/styles/index.md)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)

## <a name="related-video"></a>相关视频

> [!Video https://channel9.msdn.com/Shows/XamarinShow/XamarinForms-101-Application-Resources/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
