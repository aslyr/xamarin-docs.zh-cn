---
ms.openlocfilehash: fa88f6e7844899926a194e9d0cdd455a497c2b31
ms.sourcegitcommit: bc0c1740aa0708459729c0e671ab3ff7de3e2eee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2020
ms.locfileid: "83435381"
---
# <a name="visual-studio"></a>[Visual Studio](#tab/vswin)

1. 在“MainPage.xaml”中，修改 [`Grid`](xref:Xamarin.Forms.Grid) 声明以定义列和行，并放置跨越列和行的内容  ：

    ```xaml
    <Grid Margin="20,35,20,20">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="0.5*" />
            <ColumnDefinition Width="0.5*" />
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="50" />
            <RowDefinition Height="30" />
            <RowDefinition Height="30" />
        </Grid.RowDefinitions>
        <Label Grid.ColumnSpan="2"
               Text="This text uses the ColumnSpan property to span both columns." />
        <Label Grid.Row="1"
               Grid.RowSpan="2"
               Text="This text uses the RowSpan property to span two rows." />
    </Grid>
    ```

    此代码定义 [`Grid`](xref:Xamarin.Forms.Grid) 的列和行，并将 [`Label`](xref:Xamarin.Forms.Label) 实例放置在特定的列和行中。 第一个 `Label` 设置 [`ColumnSpan`](xref:Xamarin.Forms.Grid.ColumnSpanProperty) 附加属性，使其文本跨越多列。 将 `ColumnSpan` 属性设置为 2，代表着 `Label` 将跨越的列数。 第二个 `Label` 设置 [`RowSpan`](xref:Xamarin.Forms.Grid.RowSpanProperty) 附加属性，使其文本跨越多行。 将 `RowSpan` 属性设置为 2，代表着 `Label` 将跨越的行数。

1. 在 Visual Studio 工具栏中，按“开始”按钮（类似“播放”按钮的三角形按钮），启动所选 iOS 远程模拟器或 Android Emulator 内的应用程序  ：

    [![iOS 和 Android 上内容跨多列和多行的网格屏幕截图](../images/span-columns-rows.png "内容跨多列和多行的网格")](../images/span-columns-rows-large.png#lightbox "内容跨多列和多行的网格")

    有关跨越列和行的详细信息，请参阅 [Xamarin.Forms 网格](~/xamarin-forms/user-interface/layouts/grid.md)指南中的[行和列](~/xamarin-forms/user-interface/layouts/grid.md#rows-and-columns)。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/vsmac)

1. 在“MainPage.xaml”中，修改 [`Grid`](xref:Xamarin.Forms.Grid) 声明以定义列和行，并放置跨越列和行的内容  ：

    ```xaml
    <Grid Margin="20,35,20,20">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="0.5*" />
            <ColumnDefinition Width="0.5*" />
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="50" />
            <RowDefinition Height="30" />
            <RowDefinition Height="30" />
        </Grid.RowDefinitions>
        <Label Grid.ColumnSpan="2"
               Text="This text uses the ColumnSpan property to span both columns." />
        <Label Grid.Row="1"
               Grid.RowSpan="2"
               Text="This text uses the RowSpan property to span two rows." />
    </Grid>
    ```

    此代码定义 [`Grid`](xref:Xamarin.Forms.Grid) 的列和行，并将 [`Label`](xref:Xamarin.Forms.Label) 实例放置在特定的列和行中。 第一个 `Label` 设置 [`ColumnSpan`](xref:Xamarin.Forms.Grid.ColumnSpanProperty) 附加属性，使其文本跨越多列。 将 `ColumnSpan` 属性设置为 2，代表着 `Label` 将跨越的列数。 第二个 `Label` 设置 [`RowSpan`](xref:Xamarin.Forms.Grid.RowSpanProperty) 附加属性，使其文本跨越多行。 将 `RowSpan` 属性设置为 2，代表着 `Label` 将跨越的行数。

1. 在 Visual Studio for Mac 工具栏中，按“开始”按钮（类似“播放”按钮的三角形按钮），启动所选 iOS 模拟器或 Android 模拟器内的应用程序  ：

    [![iOS 和 Android 上内容跨多列和多行的网格屏幕截图](../images/span-columns-rows.png "内容跨多列和多行的网格")](../images/span-columns-rows-large.png#lightbox "内容跨多列和多行的网格")

    有关跨越列和行的详细信息，请参阅 [Xamarin.Forms 网格](~/xamarin-forms/user-interface/layouts/grid.md)指南中的[行和列](~/xamarin-forms/user-interface/layouts/grid.md#rows-and-columns)。
