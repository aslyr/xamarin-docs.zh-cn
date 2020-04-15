---
title: 如何手动安装 Xamarin.Android.Support 包所需的 Android 支持库？
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: A9CB8CA8-8A6D-405E-B84C-A16CE452C0F7
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: 99571e0b62592597bb1fffdc8d3ed8336fe050b2
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/13/2020
ms.locfileid: "73026927"
---
# <a name="how-can-i-manually-install-the-android-support-libraries-required-by-the-xamarinandroidsupport-packages"></a>如何手动安装 Xamarin.Android.Support 包所需的 Android 支持库？

## <a name="example-steps-for-xamarinandroidsupportv4"></a>Xamarin.Android.Support.v4 的示例步骤 

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

下载所需的 Xamarin.Android.Support NuGet 包（例如，通过使用 NuGet 软件包管理器进行安装）。

使用 `ildasm` 检查 NuGet 包需要哪个版本的 android_m2repository.zip  ：

```cmd
ildasm /caverbal /text /item:Xamarin.Android.Support.v4 packages\Xamarin.Android.Support.v4.23.4.0.1\lib\MonoAndroid403\Xamarin.Android.Support.v4.dll | findstr SourceUrl
```

示例输出：

```cmd
property string 'SourceUrl' = string('https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip')
property string 'SourceUrl' = string('https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip')
property string 'SourceUrl' = string('https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip')
```

使用 ildasm  返回的 URL 从 Google 下载 android\_m2repository.zip  。 或者，可以检查当前在 Android SDK 管理器中安装的 Android 支持存储库  的版本：

![“Android SDK 管理器显示已安装 Android 支持存储库版本 32”](install-android-support-library-images/sdk-extras.png)

如果版本与 NuGet 包所需的版本匹配，则无需下载任何新内容。 可以改为在 SDK 路径  （如 Android SDK 管理器窗口顶部所示）中的 extras\\android  下重新压缩现有的 m2repository  目录。

计算从 ildasm  返回的 URL 的 MD5 哈希。 格式化结果字符串以使用所有大写字母且不包含空格。 例如，根据需要调整 `$url` 变量，然后在 PowerShell 中运行以下两行（基于[来自 Xamarin.Android 的原始 C# 代码](https://github.com/xamarin/xamarin-android/blob/8e8a4dd90f26eb39172876cc52181b6639e20524/src/Xamarin.Android.Build.Tasks/Tasks/GetAdditionalResourcesFromAssemblies.cs#L208)）：

```powershell
$url = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip"
(([System.Security.Cryptography.MD5]::Create()).ComputeHash([System.Text.Encoding]::UTF8.GetBytes($url)) | %{ $_.ToString("X02") }) -join ""
```

示例输出：

```powershell
F16A3455987DBAE5783F058F19F7FCDF
```

将 android\_m2repository.zip  复制到 %LOCALAPPDATA%\\Xamarin\\zips\\  文件夹中。 重命名该文件以使用上一个 MD5 哈希计算步骤中的 MD5 哈希。 例如：

**%LOCALAPPDATA%\\Xamarin\\zips\\F16A3455987DBAE5783F058F19F7FCDF.zip**

（可选）将文件解压缩到 %LOCALAPPDATA%\\Xamarin\\Xamarin.Android.Support.v4\\23.4.0.0\\content\\  （创建 content\\m2repository  子目录）。 如果跳过此步骤，则使用该库的第一个版本将花费较长时间，因为它将需要完成此步骤。
子目录的版本号（在此示例中为 23.4.0.0  ）与 NuGet 包的版本号不太相同。 可以使用 `ildasm` 来查找正确的版本号：

```cmd
ildasm /caverbal /text /item:Xamarin.Android.Support.v4 packages\Xamarin.Android.Support.v4.23.4.0.1\lib\MonoAndroid403\Xamarin.Android.Support.v4.dll | findstr /C:"string 'Version'"
```

示例输出：

```cmd
property string 'Version' = string('23.4.0.0')}
property string 'Version' = string('23.4.0.0')}
property string 'Version' = string('23.4.0.0')}
```

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

下载所需的 Xamarin.Android.Support NuGet 包（例如，通过使用 NuGet 软件包管理器进行安装）。

在 Visual Studio for Mac 中的 Android 项目的“引用”  部分下，双击“Xamarin.Android.Support.v4”  程序集，以在程序集浏览器中打开该程序集。 确保将“语言”  下拉菜单设置为“C#”  ，然后从程序集浏览器导航树中选择顶级“Xamarin.Android.Support.v4”  程序集。 在 `IncludeAndroidResourcesFrom` 或 `JavaLibraryReference` 属性之一下找到 `SourceUrl` 属性：

```csharp
[assembly: IncludeAndroidResourcesFrom ("./", PackageName = "Xamarin.Android.Support.v4", SourceUrl = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip", EmbeddedArchive = "m2repository/com/android/support/support-v4/23.4.0/support-v4-23.4.0.aar", Version = "23.4.0.0")]
```

使用从 ildasm  返回的 `SourceUrl` 从 Google 下载 android\_m2repository.zip  。 或者，可以检查当前在 Android SDK 管理器中安装的 Android 支持存储库  的版本：

![“Android SDK 管理器显示已安装 Android 支持存储库版本 32”](install-android-support-library-images/sdk-extras.png)

如果版本与 NuGet 包所需的版本匹配，则无需下载任何新内容。 可以改为在 SDK 路径  （如 Android SDK 管理器窗口顶部所示）中的 extras/android  下重新压缩现有的 m2repository  目录。

计算从 ildasm  返回的 URL 的 MD5 哈希。 格式化结果字符串以使用所有大写字母且不包含空格。 例如，根据需要调整 URL 字符串，然后在 Terminal.app  命令提示符下运行以下命令：

```bash
echo -n "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip" | md5 | tr '[:lower:]' '[:upper:]'
```

另一个方法是使用 `csharp` 解释器来运行 [Xamarin.Android 本身使用的相同 C# 代码](https://github.com/xamarin/xamarin-android/blob/8e8a4dd90f26eb39172876cc52181b6639e20524/src/Xamarin.Android.Build.Tasks/Tasks/GetAdditionalResourcesFromAssemblies.cs#L208)。
为此，请根据需要调整 `url` 变量，然后在 Terminal.app  命令提示符下运行以下命令：

```bash
csharp -e 'var url = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip"; string.Concat((System.Security.Cryptography.MD5.Create().ComputeHash(System.Text.Encoding.UTF8.GetBytes(url))).Select(b => b.ToString("X02")))'
```

示例输出：

```bash
F16A3455987DBAE5783F058F19F7FCDF
```

将 android\_m2repository.zip  复制到 $HOME/.local/share/Xamarin/zips/  文件夹。 重命名该文件以使用上一个 MD5 哈希计算步骤中的 MD5 哈希。 例如：

**$HOME/.local/share/Xamarin/zips/F16A3455987DBAE5783F058F19F7FCDF.zip**

（可选）将文件解压缩到： 

**$HOME/.local/share/Xamarin/Xamarin.Android.Support.v4/23.4.0.0/content/**

（创建 content/m2repository  子目录）。 如果跳过此步骤，则使用该库的第一个版本将花费较长时间，因为它将需要完成此步骤。

子目录的版本号（在此示例中为 23.4.0.0  ）与 NuGet 包的版本号不太相同。 如前面的 ildasm  步骤中所示，可以使用 Visual Studio for Mac 中的程序集浏览器来查找正确的版本号。 在 `IncludeAndroidResourcesFrom` 或 `JavaLibraryReference` 属性之一下查找 `Version` 属性：

```csharp
[assembly: IncludeAndroidResourcesFrom ("./", PackageName = "Xamarin.Android.Support.v4", SourceUrl = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip", EmbeddedArchive = "m2repository/com/android/support/support-v4/23.4.0/support-v4-23.4.0.aar", Version = "23.4.0.0")]
```

-----

## <a name="additional-references"></a>其他参考

- [Bug 43245](https://bugzilla.xamarin.com/show_bug.cgi?id=43245) – 不正确的“下载失败。 请下载 {0}，并将其置于 {1} 目录。” 以及与 Xamarin.Android.Support 包有关的“请安装包: SDK 安装程序提供的‘{0}’”错误消息

### <a name="next-steps"></a>后续步骤

本文档将讨论截至 2016 年 8 月的当前行为。 本文档中所述的技术不是 Xamarin 稳定测试套件的一部分，因此将来可能会出现问题。

如需进一步的帮助，请与我们联系，或者，如果在使用上述信息后仍然存在此问题，请参阅 [Xamarin 有哪些可用的支持选项？](~/cross-platform/troubleshooting/support-options.md)，了解有关联系人选项、建议以及如何在需要时提交新 Bug 的信息。
