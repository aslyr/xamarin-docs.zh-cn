---
title: 使用 Xamarin.Android 对外部存储进行的文件访问
description: 本指南将讨论在 Xamarin Android 中对外部存储进行的文件访问
ms.prod: xamarin
ms.assetid: 40da10b2-a207-4f9c-a2dd-165d9b662f33
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 07/23/2018
ms.openlocfilehash: e6eb62def5aeb9e4a4a347becffcae82116c1b11
ms.sourcegitcommit: 952db1983c0bc373844c5fbe9d185e04a87d8fb4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/23/2020
ms.locfileid: "86997067"
---
# <a name="external-storage"></a>外部存储

外部存储指不在内部存储上，且不能由负责该文件的应用以独占方式访问的文件存储。 外部存储的主要用途是提供一个位置，用于放置打算在应用之间共享的文件，或是太大而无法在内部存储中容纳的文件。

从历史上来说，外部存储指诸如 SD 卡这类可移动媒体（也称为便携存储  ）上的磁盘分区。 随着 Android 设备的发展，此区别不再那么重要，许多 Android 设备不再支持可移动存储。 相反，某些设备会分配一部分内部非易失性内存，Android 可在其上执行与可移动媒体相同的功能。 这称为仿真  存储，仍被视为外部存储。 或者，某些 Android 设备可能具有多个外部存储分区。 例如，Android 平板电脑（除了其内部存储之外）可能具有仿真存储以及用于 SD 卡的一个或多个插槽。 所有这些分区都由 Android 视为外部存储。

在具有多个用户的设备上，每个用户在主外部存储分区上都有一个专用目录来用作其外部存储。 以一个用户身份运行的应用无法访问设备上其他用户的文件。 所有用户的文件仍是全局可读和全局可写；但是，Android 会对每个用户配置文件与其他配置文件之间进行沙盒处理。

在 Xamarin.Android 中，对文件的读取和写入几乎与任何其他 .NET 应用程序完全相同。 Xamarin.Android 应用会确定将操作的文件的路径，然后使用标准 .NET 习惯进行文件访问。 由于内部和外部存储的实际路径可能因设备或 Android 版本而异，因此不建议对文件路径进行硬编码。 相反，Xamarin.Android 公开了本机 Android API，可帮助确定内部和外部存储上的文件的路径。

本指南将讨论 Android 中特定于外部存储的概念和 API。

## <a name="public-and-private-files-on-external-storage"></a>外部存储上的公用和专用文件

应用可以在外部存储上保留两种不同类型的文件：

* 专用  文件 &ndash; 专用文件是特定于应用程序的文件（但仍然全局可读且全局可写）。 Android 期望专用文件存储在外部存储上的特定目录中。 尽管这些文件称为“专用”，但它们仍然可见，并且可由设备上的其他应用访问，Android 并没有对它们提供任何特殊保护。

* 公共  文件 &ndash; 这些文件不被视为特定于应用程序，可自由共享。

这些文件之间的差异主要体现在概念上。 专用文件的专用在某种意义上是因为它们被视为是应用程序的一部分，而公共文件是外部存储中存在的任何其他文件。 Android 提供两个不同的 API 来解析专用文件和公共文件的路径，但在其他方面使用相同的 .NET API 来读取和写入这些文件。 这些 API 与有关[读取和写入](~/android/platform/files/index.md#reading-or-writing-to-files-on-internal-storage)的部分中讨论的 API 相同。

### <a name="private-external-files"></a>专用外部文件

专用外部文件被视为特定于应用程序（类似于内部文件），但出于许多原因（例如，对于内部存储而言太大）而保留在外部存储上。 与内部文件类似，在用户卸载应用时会删除这些文件。

通过调用方法 `Android.Content.Context.GetExternalFilesDir(string type)`，可找到专用外部文件的主要位置。 此方法会返回一个 `Java.IO.File` 对象，它表示应用的专用外部存储目录。 将 `null` 传递到此方法会为应用程序返回用户存储目录的路径。 例如，对于包名称为 `com.companyname.app` 的应用程序，专用外部文件的“root”目录会是：

```bash
/storage/emulated/0/Android/data/com.companyname.app/files/
```

本文档将外部存储上专用文件的存储目录称为 PRIVATE\_EXTERNAL\_STORAGE  。

`GetExternalFilesDir()` 的参数是一个字符串，用于指定应用程序目录  。 这是一个目录，旨在为文件的逻辑组织提供标准位置。 可以通过 `Android.OS.Environment` 类中的常数获取字符串值：

| Android.OS.Environment | 目录 |
|-|-|
| DirectoryAlarms | PRIVATE\_EXTERNAL\_STORAGE/Alarms |
| DirectoryDcim | PRIVATE\_EXTERNAL\_STORAGE/DCIM |
| DirectoryDownloads | PRIVATE\_EXTERNAL\_STORAGE/Download |
| DirectoryDocuments | PRIVATE\_EXTERNAL\_STORAGE/Documents |
| DirectoryMovies | PRIVATE\_EXTERNAL\_STORAGE/Movies |
| DirectoryMusic | PRIVATE\_EXTERNAL\_STORAGE/Music |
| DirectoryNotifications | PRIVATE\_EXTERNAL\_STORAGE/Notifications |
| DirectoryPodcasts | PRIVATE\_EXTERNAL\_STORAGE/Podcasts |
| DirectoryRingtones | PRIVATE\_EXTERNAL\_STORAGE/Ringtones |
| DirectoryPictures | PRIVATE\_EXTERNAL\_STORAGE/Pictures |

对于具有多个外部存储分区的设备，每个分区都有一个用于专用文件的目录。 方法 `Android.Content.Context.GetExternalFilesDirs(string type)` 会返回 `Java.IO.Files` 的数组。 每个对象都表示所有共享/外部存储设备上一个特定于应用程序的专用目录，应用程序可以在其中放置它拥有的文件。

> [!IMPORTANT]
> 专用外部存储目录的确切路径可能因设备和 Android 的版本而异。 因此，应用不得对此目录的路径进行硬编码，而是使用 Xamarin.Android API，如 `Android.Content.Context.GetExternalFilesDir()`。

### <a name="public-external-files"></a>公共外部文件

公共文件是外部存储上存在的文件，它们未存储在 Android 为专用文件分配的目录中。 卸载应用时，不会删除公共文件。 Android 应用必须获得权限才能读取或写入任何公共文件。 公共文件可以存在于外部存储上的任何位置，但按照约定，Android 期望公共文件存在于由属性 `Android.OS.Environment.ExternalStorageDirectory` 标识的目录中。 此属性会返回一个 `Java.IO.File` 对象，它表示主外部存储目录。 例如，`Android.OS.Environment.ExternalStorageDirectory` 可能引用以下目录：

```bash
/storage/emulated/0/
```

本文档将外部存储上公共文件的存储目录称为 PUBLIC\_EXTERNAL\_STORAGE。

Android 还支持 PUBLIC\_EXTERNAL\_STORAGE 上的应用程序目录这一概念。 这些目录与 `PRIVATE_EXTERNAL_STORAGE` 的应用程序目录完全相同，在上一部分的表中进行了介绍。 方法 `Android.OS.Environment.GetExternalStoragePublicDirectory(string directoryType)` 会返回与公共应用程序目录对应的 `Java.IO.File` 对象。 `directoryType` 参数为必需参数，不能为 `null`。

例如，调用 `Environment.GetExternalStoragePublicDirectory(Environment.DirectoryDocuments).AbsolutePath` 会返回类似于下面这样的字符串：

```bash
/storage/emulated/0/Documents
```

> [!IMPORTANT]
> 公共外部存储目录的确切路径可能因设备和 Android 的版本而异。 因此，应用不得对此目录的路径进行硬编码，而是使用 Xamarin.Android API，如 `Android.OS.Environment.ExternalStorageDirectory`。

## <a name="working-with-external-storage"></a>使用外部存储

Xamarin.Android 应用获取文件的完整路径后，它应利用任何标准 .NET API 来创建、读取、写入或删除文件。 这可最大程度地提高应用的跨平台兼容代码数量。 但是，在尝试访问文件之前，Xamarin.Android 应用必须确保可以访问该文件。

1. 验证外部存储 &ndash; 根据外部存储的性质，它可能未装载，不可由应用使用。 所有应用都应在尝试使用外部存储之前检查其状态。
2. 执行运行时权限检查 &ndash; Android 应用必须从用户处请求获取权限才能访问外部存储。 这意味着在进行任何文件访问之前都应执行运行时权限请求。 [Xamarin.Android 中的权限](~/android/app-fundamentals/permissions.md)指南包含有关 Android 权限的更多详细信息。

下面将讨论这两个任务中的每个任务。

### <a name="verifying-that-external-storage-is-available"></a>验证外部存储是否可用

写入外部存储之前的第一步是检查它是可读或可写。 `Android.OS.Environment.ExternalStorageState` 属性保存标识外部存储状态的字符串。 此属性会返回表示状态的字符串。 此表是 `Environment.ExternalStorageState` 可能返回的 `ExternalStorageState` 值的列表：

| ExternalStorageState | 描述  |
|----------------------|---|
| MediaBadRemoval      | 突然移除了媒体，但未正确卸载。 |
| MediaChecking        | 媒体存在，但在进行磁盘检查。  |
| MediaEjecting        | 媒体正在进行卸载并弹出。  |
| MediaMounted         | 媒体已装载，可以读取或写入。  |
| MediaMountedReadOnly | 媒体已装载，但只能进行读取。 |
| MediaNofs            | 媒体存在，但不包含适用于 Android 的文件系统。 |
| MediaRemoved         | 不存在媒体。 |
| MediaShared          | 媒体存在，但未装载。 它通过 USB 与其他设备共享。|
| MediaUnknown         | 媒体的状态无法由 Android 识别。 |
| MediaUnmountable     | 媒体存在，但无法由 Android 装载。 |
| MediaUnmounted       | 媒体存在，但未装载。 |

大多数 Android 应用只需检查是否已装载外部存储。 下面的代码片段演示如何验证是否已装载外部存储来进行只读访问或读写访问：

```csharp
bool isReadonly = Environment.MediaMountedReadOnly.Equals(Environment.ExternalStorageState);
bool isWriteable = Environment.MediaMounted.Equals(Environment.ExternalStorageState);
```

## <a name="external-storage-permissions"></a>外部存储权限

Android 将外部存储视为危险权限，这通常要求用户授予其访问资源的权限。 用户可以随时撤销此权限。  这意味着在进行任何文件访问之前都应执行运行时权限请求。 应用会被自动授予读取和写入其自己的专用文件的权限。 在用户[授予了权限](~/android/app-fundamentals/permissions.md)之后，应用可以读取和写入属于其他应用的专用文件。

所有 Android 应用都必须在 AndroidManifest.xml 中为外部存储声明两个权限之一。 若要标识权限，必须将以下两个 `uses-permission` 元素之一添加到 AndroidManifest.xml：

```xml
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```

> [!NOTE]
> 如果用户授予 `WRITE_EXTERNAL_STORAGE`，则也会隐式授予 `READ_EXTERNAL_STORAGE`。 无需在 AndroidManifest.xml 中请求两个权限。

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

还可以使用解决方案属性的“Android 清单”选项卡添加权限：

![解决方案资源管理器 - Visual Studio 所需的权限](./images/required-permissions.w157.png)

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

还可以使用解决方案 Properties Pad的“Android 清单”选项卡添加权限：

[![Solution Pad - Visual Studio for Mac 的所需权限](./images/required-permissions.m752-sml.png)](./images/required-permissions.m752.png#lightbox)

-----

一般而言，所有危险权限都必须由用户进行审批。 外部存储的权限的异常之处在于此规则存在例外，具体取决于应用所运行的 Android 版本：

![外部存储权限检查的流程图](./images/external-permission-check-flowchart.png)

有关执行运行时权限请求的详细信息，请参阅 [Xamarin.Android 中的权限](~/android/app-fundamentals/permissions.md)指南。 monodroid-sample [本地文件](https://github.com/xamarin/monodroid-samples/tree/master/LocalFiles)也演示了一种执行运行时权限检查的方式。

#### <a name="granting-and-revoking-permissions-with-adb"></a>通过 ADB 授予和撤销权限

在开发 Android 应用的过程中，可能需要授予和撤销权限以测试运行时权限检查所涉及的各种工作流。 可以在命令提示符处使用 ADB 执行此操作。 以下命令行片段演示如何使用 ADB 为包名称为 com.companyname.app 的 Android 应用授予或撤销权限：

```bash
$ adb shell pm grant com.companyname.app android.permission.WRITE_EXTERNAL_STORAGE

$ adb shell pm revoke com.companyname.app android.permission.WRITE_EXTERNAL_STORAGE
```

## <a name="deleting-files"></a>删除文件

任何标准 C# API 都可以用于从外部存储中删除文件，如 [`System.IO.File.Delete`](xref:System.IO.File.Delete*)。 还可以使用 Java API，不过会牺牲代码可移植性。 例如：

```csharp
System.IO.File.Delete("/storage/emulated/0/Android/data/com.companyname.app/files/count.txt");
```

## <a name="related-links"></a>相关链接

* [monodroid-samples 上的 Xamarin.Android 本地文件示例](https://github.com/xamarin/monodroid-samples/tree/master/LocalFiles)
* [Xamarin.Android 中的权限](~/android/app-fundamentals/permissions.md)
