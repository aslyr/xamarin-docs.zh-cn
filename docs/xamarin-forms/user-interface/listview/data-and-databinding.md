---
title: ListView 数据源
description: 此文章介绍了如何填充数据，使用 Xamarin.Forms ListView 以及如何使用 ListView 的数据绑定。
ms.prod: xamarin
ms.assetid: B5571660-1E82-4379-95C3-0725288CF5D9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/23/2020
ms.openlocfilehash: e51f0bd011750b030c0a11b9b89a2c2473f2a9ed
ms.sourcegitcommit: d83c6af42ed26947aa7c0ecfce00b9ef60f33319
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/25/2020
ms.locfileid: "80247582"
---
# <a name="listview-data-sources"></a>ListView 数据源

[![下载示例](~/media/shared/download.png) 下载示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-switchentrytwobinding)

Xamarin. Forms [`ListView`](xref:Xamarin.Forms.ListView)用于显示数据列表。 本文介绍如何使用数据填充 `ListView`，以及如何将数据绑定到所选项目。

## <a name="itemssource"></a>ItemsSource

使用[`ItemsSource`](xref:Xamarin.Forms.ItemsView`1.ItemsSource)属性填充数据的[`ListView`](xref:Xamarin.Forms.ListView) ，该属性可接受任何实现 `IEnumerable`的集合。 填充 `ListView` 的最简单方法涉及使用字符串数组：

```xaml
<ListView>
      <ListView.ItemsSource>
          <x:Array Type="{x:Type x:String}">
            <x:String>mono</x:String>
            <x:String>monodroid</x:String>
            <x:String>monotouch</x:String>
            <x:String>monorail</x:String>
            <x:String>monodevelop</x:String>
            <x:String>monotone</x:String>
            <x:String>monopoly</x:String>
            <x:String>monomodal</x:String>
            <x:String>mononucleosis</x:String>
          </x:Array>
      </ListView.ItemsSource>
</ListView>
```

等效 C# 代码如下：

```csharp
var listView = new ListView();
listView.ItemsSource = new string[]
{
  "mono",
  "monodroid",
  "monotouch",
  "monorail",
  "monodevelop",
  "monotone",
  "monopoly",
  "monomodal",
  "mononucleosis"
};
```

![](data-and-databinding-images/itemssource-simple.png "ListView Displaying List of Strings")

此方法将使用字符串列表填充 `ListView`。 默认情况下，`ListView` 将调用 `ToString` 并在每一行的 `TextCell` 中显示结果。 若要自定义数据的显示方式，请参阅[单元格外观](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md)。

由于已将 `ItemsSource` 发送到数组，因此当基础列表或数组发生变化时，内容将不会更新。 如果要在基础列表中添加、删除和更改项时，ListView 自动更新，则需要使用 `ObservableCollection`。 [`ObservableCollection`](xref:System.Collections.ObjectModel.ObservableCollection`1)在 `System.Collections.ObjectModel` 中定义，与 `List`一样，只不过它可以通知所有更改的 `ListView`：

```csharp
ObservableCollection<Employee> employees = new ObservableCollection<Employee>();
listView.ItemsSource = employees;

//Mr. Mono will be added to the ListView because it uses an ObservableCollection
employees.Add(new Employee(){ DisplayName="Mr. Mono"});
```

## <a name="data-binding"></a>数据绑定

数据绑定是将用户界面对象的属性绑定到某个 CLR 对象（如 viewmodel 中的类）的属性的 "粘附"。 数据绑定很有用，因为它简化了用户界面的开发，通过替换大量繁琐样板代码。

数据绑定的工作原理是在其绑定的值更改时保持对象的同步。 无需在每次更改控件值时都编写事件处理程序，只需在 viewmodel 中建立绑定并启用绑定。

有关数据绑定的详细信息，请参阅[数据绑定基础知识](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)，这是 Xamarin 的第四部分[。表单 XAML 基础知识文章系列](~/xamarin-forms/xaml/xaml-basics/index.md)。

### <a name="binding-cells"></a>单元格绑定

单元格（和单元格的子项）的属性可以绑定到 `ItemsSource`中的对象的属性。 例如，可以使用 `ListView` 来显示雇员列表。

Employee 类：

```csharp
public class Employee
{
    public string DisplayName {get; set;}
}
```

`ObservableCollection<Employee>` 将创建，并将其设置为 `ListView` `ItemsSource`，并使用数据填充列表：

```csharp
ObservableCollection<Employee> employees = new ObservableCollection<Employee>();
public ObservableCollection<Employee> Employees { get { return employees; }}

public EmployeeListPage()
{
    EmployeeView.ItemsSource = employees;

    // ObservableCollection allows items to be added after ItemsSource
    // is set and the UI will react to changes
    employees.Add(new Employee{ DisplayName="Rob Finnerty"});
    employees.Add(new Employee{ DisplayName="Bill Wrestler"});
    employees.Add(new Employee{ DisplayName="Dr. Geri-Beth Hooper"});
    employees.Add(new Employee{ DisplayName="Dr. Keith Joyce-Purdy"});
    employees.Add(new Employee{ DisplayName="Sheri Spruce"});
    employees.Add(new Employee{ DisplayName="Burt Indybrick"});
}
```

> [!WARNING]
> 虽然 `ListView` 将更新以响应其基础 `ObservableCollection`中的更改，但如果将不同的 `ObservableCollection` 实例分配给原始 `ObservableCollection` 引用（例如 `employees = otherObservableCollection;`），则 `ListView` 不会更新。

以下代码片段演示了一个绑定到员工列表的 `ListView`：

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:constants="clr-namespace:XamarinFormsSample;assembly=XamarinFormsXamlSample"
             x:Class="XamarinFormsXamlSample.Views.EmployeeListPage"
             Title="Employee List">
  <ListView x:Name="EmployeeView"
            ItemsSource="{Binding Employees}">
    <ListView.ItemTemplate>
      <DataTemplate>
        <TextCell Text="{Binding DisplayName}" />
      </DataTemplate>
    </ListView.ItemTemplate>
  </ListView>
</ContentPage>
```

此 XAML 示例定义包含 `ListView`的 `ContentPage`。 通过 `ItemsSource` 属性设置 `ListView` 的数据源。 `ItemsSource` 中每行的布局是在 `ListView.ItemTemplate` 元素中定义的。 这会生成以下屏幕截图：

![](data-and-databinding-images/bound-data.png "ListView using Data Binding")

> [!WARNING]
> `ObservableCollection` 不是线程安全的。 修改 `ObservableCollection` 会导致 UI 更新在执行修改的同一线程上进行。 如果该线程不是主 UI 线程，则会引发异常。

### <a name="binding-selecteditem"></a>绑定 SelectedItem

通常，您需要绑定到 `ListView`的选定项，而不是使用事件处理程序来响应更改。 若要在 XAML 中执行此操作，请绑定 `SelectedItem` 属性：

```xaml
<ListView x:Name="listView"
          SelectedItem="{Binding Source={x:Reference SomeLabel},
          Path=Text}">
 …
</ListView>
```

假定 `listView`的 `ItemsSource` 是字符串列表，`SomeLabel` 将其 `Text` 属性绑定到 `SelectedItem`。

## <a name="related-links"></a>相关链接

- [双向绑定（示例）](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-switchentrytwobinding)
