---
标题： "说明：" 中的 XAML 编译 Xamarin.Forms ： "本文介绍了如何选择性地将 xaml 编译为带有 Xamarin.Forms xaml 编译器（XAMLC）的中间语言（IL）。"
ms-chap： xamarin assetid：9A2D10A6-5DFC-485F-A75A-2F7B98314025： xamarin 窗体作者： davidbritch： dabritch ms. 日期：08/22/2018 非 loc： [ Xamarin.Forms ， Xamarin.Essentials ]
---

# <a name="xaml-compilation-in-xamarinforms"></a>XAML 编译Xamarin.Forms

_XAML 可以根据需要使用 XAML 编译器 (XAMLC) 直接编译为中间语言 (IL)。_

XAML 编译提供了很多好处：

- 它会执行 XAML 的编译时检查，从而可向用户通知任何错误。
- 它会消除 XAML 元素的某些负载和实例化时间。
- 它通过不再包含 .xaml 文件，来帮助减小最终程序集的文件大小。

默认情况下，将禁用 XAML 编译以确保向后兼容。 通过添加属性，可以在程序集和类级别启用此功能 [`XamlCompilation`](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute) 。

下面的代码示例演示如何在程序集级别启用 XAML 编译：

```csharp
using Xamarin.Forms.Xaml;
...
[assembly: XamlCompilation (XamlCompilationOptions.Compile)]
namespace PhotoApp
{
  ...
}
```

在此示例中，将对程序集内包含的所有 XAML 执行编译时检查，并在编译时（而非运行时）报告 XAML 错误。 因此， `assembly` 属性的前缀 [`XamlCompilation`](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute) 指定将该属性应用于整个程序集。

> [!NOTE]
> [`XamlCompilation`](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute)特性和 [`XamlCompilationOptions`](xref:Xamarin.Forms.Xaml.XamlCompilationOptions) 枚举位于 `Xamarin.Forms.Xaml` 命名空间中，必须导入该命名空间才能使用它们。

下面的代码示例演示如何在类级启用 XAML 编译：

```csharp
using Xamarin.Forms.Xaml;
...
[XamlCompilation (XamlCompilationOptions.Compile)]
public class HomePage : ContentPage
{
  ...
}
```

在此示例中，将执行类的 XAML 的编译时检查 `HomePage` ，并在编译过程中报告错误。

> [!NOTE]
> 可以启用编译的绑定以提高应用程序中的数据绑定性能 Xamarin.Forms 。 有关详细信息，请参阅[已编译的绑定](~/xamarin-forms/app-fundamentals/data-binding/compiled-bindings.md)。

## <a name="related-links"></a>相关链接

- [`XamlCompilation`](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute)
- [`XamlCompilationOptions`](xref:Xamarin.Forms.Xaml.XamlCompilationOptions)
