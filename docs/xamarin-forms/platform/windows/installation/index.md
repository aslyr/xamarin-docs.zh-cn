---
title: 设置 Windows 项目
description: 旧 Xamarin.Forms 解决方案（或在 macOS 上创建的解决方案）将不通用 Windows 平台项目，因此本文介绍如何向现有解决方案添加新的 UWP 项目 Xamarin.Forms 。
ms.prod: xamarin
ms.assetid: A0774D2E-6994-4D91-84E8-DAB66FC92320
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/10/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 06f7a138ee27fe095b99a55917267aaa6e01998e
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/22/2020
ms.locfileid: "86939017"
---
# <a name="setup-windows-projects"></a>设置 Windows 项目

_向现有解决方案中添加新的 Windows 项目 Xamarin.Forms_

旧 Xamarin.Forms 解决方案（或在 macOS 上创建的解决方案）将不包含通用 Windows 平台（UWP）应用项目。 因此，您需要手动添加 UWP 项目来构建 Windows 10 （UWP）应用。

## <a name="add-a-universal-windows-platform-app"></a>添加通用 Windows 平台应用

建议使用**Windows 10**上的**Visual STUDIO 2019**生成 UWP 应用。 有关通用 Windows 平台的详细信息，请参阅[通用 Windows 平台简介](/windows/uwp/get-started/universal-application-platform-guide/)。

UWP 在 Xamarin.Forms 2.1 和更高版本以及和更高版本中可用 Xamarin.Forms 。Xamarin.Forms2.2 及更高版本支持映射。

请查看<a href="#troubleshooting">故障排除</a>部分，了解相关帮助。

按照以下说明添加将在 Windows 10 手机、平板电脑和台式机上运行的 UWP 应用：

 2. 右键单击该解决方案，然后选择 "**添加" > "新建项目 ...** " 并添加**空白应用（通用 Windows）** 项目：

  ![“添加新项目”对话框](universal-images/add-wu.png)

 pps-2. 在 "**新建通用 Windows 平台项目**" 对话框中，选择应用将在其上运行的 Windows 10 的最低版本和目标版本：

  !["新建通用 Windows 平台项目" 对话框](universal-images/target-version.png)

 三维空间. 右键单击 "UWP" 项目，然后选择 "**管理 NuGet 包 ...** " 并添加 **Xamarin.Forms** 包。 确保解决方案中的其他项目也更新为同一版本的 Xamarin.Forms 包。

 4. 请确保在 "**生成 >" Configuration Manager** "窗口中生成新的 UWP 项目（默认情况下可能不会发生这种情况）。 勾选通用项目的 "**生成**" 和 "**部署**" 框：

  [![Configuration Manager 窗口](universal-images/configuration-sml.png)](universal-images/configuration.png#lightbox "Configuration Manager 窗口")

 5. 右键单击该项目，然后选择 "**添加 > 引用**"，然后创建对 Xamarin.Forms 应用程序项目的引用（.NET Standard 或共享项目）。

  ![引用管理器对话框](universal-images/addref-sml.png)

 共. 在 UWP 项目中，编辑**App.xaml.cs** ，使其在 `Init` 方法内包含 `OnLaunched` 第52行的方法调用：

```csharp
// under this line
rootFrame.NavigationFailed += OnNavigationFailed;
// add this line
Xamarin.Forms.Forms.Init (e); // requires the `e` parameter
```

 全天候. 在 UWP 项目中，通过**MainPage.xaml**删除 `Grid` 元素中包含的来编辑 MainPage。 `Page`

 8. 在**MainPage**中，为添加新 `xmlns` 条目 `Xamarin.Forms.Platform.UWP` ：

```csharp
xmlns:forms="using:Xamarin.Forms.Platform.UWP"
```

 900. 在**MainPage**中，将根元素更改 `<Page` 为 `<forms:WindowsPage` ：

```xaml
<forms:WindowsPage
...
   xmlns:forms="using:Xamarin.Forms.Platform.UWP"
...
</forms:WindowsPage>
```

 万. 在 UWP 项目中，编辑**MainPage.xaml.cs**以删除 `: Page` 类名的继承说明符（因为在 `WindowsPage` 上一步中所做的更改，它现在将从继承）：

```csharp
public sealed partial class MainPage  // REMOVE ": Page"
```

 11x17. 在**MainPage.xaml.cs**中，在 `LoadApplication` 构造函数中添加调用 `MainPage` 以启动 Xamarin.Forms 应用程序：

```csharp
// below this existing line
this.InitializeComponent();
// add this line
LoadApplication(new YOUR_NAMESPACE.App());
```

> [!NOTE]
> 方法的参数 `LoadApplication` 是 `Xamarin.Forms.Application` 在 .net standard 项目中定义的实例。

<!--
11 . Double-click **Package.appxmanifest** to set these capabilities
  that are often required:

  Capabilities set:

  * Internet (Client)
  * Location
-->

10. 添加任何本地资源（例如 图像文件）的文件。

## <a name="troubleshooting"></a>疑难解答

### <a name="target-invocation-exception-when-using-compile-with-net-native-tool-chain"></a>使用 "使用 .NET Native 工具链编译" 时的 "目标调用异常"

如果 UWP 应用引用多个程序集（例如，第三方控件库，或应用本身拆分为多个库），则 Xamarin.Forms 可能无法从这些程序集（如自定义呈现器）加载对象。

使用**带有 .NET Native 工具链的编译**时可能会发生这种情况，这是在项目的 "**生成 > 常规**" 窗口的 > 属性中的 UWP 应用的选项。

您可以使用 App.xaml.cs 中的调用的 UWP 特定重载来解决此问题， `Forms.Init` 如下面的代码所示（应将替换为您的**App.xaml.cs** `ClassInOtherAssembly` 代码引用的实际类）：

```csharp
// You'll need to add `using System.Reflection;`
List<Assembly> assembliesToInclude = new List<Assembly>();

// Now, add in all the assemblies your app uses
assembliesToInclude.Add(typeof (ClassInOtherAssembly).GetTypeInfo().Assembly);

// Also do this for all your other 3rd party libraries
Xamarin.Forms.Forms.Init(e, assembliesToInclude);
// replaces Xamarin.Forms.Forms.Init(e);
```

通过直接引用或 NuGet，为已添加为解决方案资源管理器中的引用的每个程序集添加一个条目。

#### <a name="dependency-services-and-net-native-compilation"></a>依赖项服务和 .NET Native 编译

使用 .NET Native 编译的发布版本可能无法解析在主应用可执行文件（例如，单独的项目或库）外部定义的依赖项服务。

使用 `DependencyService.Register<T>()` 方法可手动注册依赖项服务类。 根据上面的示例，添加 register 方法，如下所示：

```csharp
Xamarin.Forms.Forms.Init(e, assembliesToInclude);
Xamarin.Forms.DependencyService.Register<ClassInOtherAssembly>(); // add this
```
