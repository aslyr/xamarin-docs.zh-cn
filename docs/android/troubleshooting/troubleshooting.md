---
title: 疑难解答指南
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 56137ACA-4811-B312-6860-E16D0FA123F7
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/15/2018
ms.openlocfilehash: 6d83afa47c459633506736b2497a82c444352c90
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/13/2020
ms.locfileid: "79303515"
---
# <a name="troubleshooting-tips"></a>疑难解答指南

## <a name="getting-diagnostic-information"></a>获取诊断信息

在追踪各种 bug 时，需要在 Xamarin.Android 中查看以下几项信息。
这些方法包括：

1. MSBuild 诊断输出。
2. 设备部署日志。
3. Android 调试日志输出。

<a name="Diagnostic_MSBuild_Output" />

## <a name="diagnostic-msbuild-output"></a>MSBuild 诊断输出

MSBuild 诊断输出可以包含与包生成相关的额外信息，也可以包含某些包部署信息。

若要在 Visual Studio 中启用诊断 MSBuild 输出：

1. 单击“工具”>“选项…” 
2. 在左侧树状视图中，选择“项目和解决方案”>“生成并运行” 
3. 在右侧面板中，将“MSBuild 生成输出详细程度”下拉列表设置为“诊断”
4. 单击“确定” 
5. 清除并重新生成包。
6. 诊断输出显示在“输出”面板中。

在 Visual Studio for Mac/OS X 中启用 MSBuild 诊断输出：

1. 单击“Visual Studio for Mac”>“首选项…” 
2. 在左侧树状视图中，选择“项目”>“生成” 
3. 在右侧面板中，将“日志详细级别”下拉列表设置为“诊断”
4. 单击“确定” 
5. 重启 Visual Studio for Mac
6. 清除并重新生成包。
7. 单击“生成输出”按钮，即可在 Errors Pad（“视图”>“栏”>“错误”）中看到诊断输出  。

## <a name="device-deployment-logs"></a>设备部署日志

在 Visual Studio 中启用设备部署日志记录功能：

1. “工具”>“选项…”> 
2. 在左侧树状视图中，选择“Xamarin”>“Android 设置” 
3. 在右侧面板中，勾选 [X]“扩展调试日志(将 monodroid.log 写入桌面)”复选框  。
4. 日志消息写入桌面上的 monodroid.log 文件。

Visual Studio for Mac 始终编写设备部署日志。 找到这些日志稍微困难些；每次（日期 + 时间）发生部署，系统都会创建一个 AndroidUtils 日志文件，例如  ：**AndroidTools-2012-10-24_12-35-45.log**。

- 在 Windows 上，日志文件写入 `%LOCALAPPDATA%\XamarinStudio-{VERSION}\Logs`。
- 在 OS X 上，日志文件写入 `$HOME/Library/Logs/XamarinStudio-{VERSION}`。

## <a name="android-debug-log-output"></a>Android 调试日志输出

Android 将向 [Android 调试日志](~/android/deploy-test/debugging/android-debug-log.md)写入多条消息。
Xamarin.Android 使用 Android 系统属性来控制 Android 调试日志中额外的消息的生成。 可以通过 [Android Debug Bridge (adb)](https://developer.android.com/guide/developing/tools/adb.html) 中的setprop 命令设置 Android 系统属性  ：

```shell
adb shell setprop PROPERTY_NAME PROPERTY_VALUE
```

系统在进程启动期间读取系统属性，因此必须在应用程序启动之前设置这些属性，否则更改系统属性后必须重启应用程序。

### <a name="xamarinandroid-system-properties"></a>Xamarin.Android 系统属性

Xamarin.Android 支持以下系统属性：

- debug.mono.debug  ：如果为非空字符串，则它等效于 `*mono-debug*`。

- debug.mono.env  ：在初始化 mono 之前，要在应用程序启动期间导出的用竖线 (|) 分隔的环境变量列表   。 这允许设置控制 mono 日志记录的环境变量。

  > [!NOTE]
  > 由于使用 | 分隔值，因此值必须拥有一级额外的引号，因为 \`adb shell\` 命令会删除一组引号   。

  > [!NOTE]
  > Android 系统属性值的长度不能超过 92 个字符。

  示例：

  ```
  adb shell setprop debug.mono.env "'MONO_LOG_LEVEL=info|MONO_LOG_MASK=asm'"
  ```

- debug.mono.log  ：以逗号 (,) 分隔的组件列表，该列表将向 Android 调试日志打印额外的消息  。 默认情况下，不设置任何内容。 组件包括：

  - *all*：打印所有消息
  - *gc*：打印 GC 相关消息。
  - *gref*：打印（弱全局）引用分配和解除分配消息。
  - *lref*：打印本地引用分配和解除分配消息。

  > [!NOTE]
  > 这些消息非常详细  。 除非确实需要，否则不要启用。

- debug.mono.trace  ：允许设置 [mono--trace](http://docs.go-mono.com/?link=man%3amono(1))`=PROPERTY_VALUE` 设置。

## <a name="deleting-bin-and-obj"></a>删除 `bin` 和 `obj`

Xamarin.Android 过去曾遭遇过如下情况：

- 遇到异常生成或运行时错误。
- `Clean`、`Rebuild`或手动删除 `bin` 和 `obj` 目录。
- 问题就解决了。

由于这些问题对开发人员生产效率的影响，我们对解决这些问题给予了大量投入。

如果出现这样的问题：

1. 在脑海里思考一下。 使项目进入此状态的最后一个操作是什么？
1. 保存当前的生成日志。 再次尝试生成，并记录[诊断生成日志](#diagnostic-msbuild-output)。
1. 提交 [bug 报告][bug]。

在删除 `bin` 和 `obj` 目录之前，根据需要对它们进行压缩和保存，以备以后诊断使用。 可能只需要`Clean` Xamarin.Android 应用程序项目就可以让工作恢复正常。

[bug]: https://github.com/xamarin/xamarin-android/wiki/Submitting-Bugs,-Feature-Requests,-and-Pull-Requests

## <a name="xamarinandroid-cannot-resolve-systemvaluetuple"></a>Xamarin.Android 无法解析 System.ValueTuple

发生此错误的原因是与 Visual Studio 不兼容。

- **Visual Studio 2017 Update 1**（15.1 或以下版本）仅与 System.ValueTuple NuGet 4.3.0（或以下版本）兼容  。

- **Visual Studio 2017 Update 2**（15.2 或以上版本）仅与 System.ValueTuple NuGet 4.3.1（或以上版本）兼容  。

请选择与 Visual Studio 2017 安装相对应的正确 System.ValueTuple NuGet。

## <a name="gc-messages"></a>GC 消息

将 debug.mono.log 系统属性设置为包含 gc 的值，即可查看 GC 组件消息。

每当 GC 执行并提供有关 GC 执行了多少工作的信息时，系统生成 GC 消息：

```shell
I/monodroid-gc(12331): GC cleanup summary: 81 objects tested - resurrecting 21.
```

将 `MONO_LOG_LEVEL` 环境变量设置为 `debug`，即可生成其他 GC 信息（例如计时信息）：

```shell
adb shell setprop debug.mono.env MONO_LOG_LEVEL=debug
```

这将产生（大量）额外的 Mono 消息，包括以下三个结果：

```shell
D/Mono (15723): GC_BRIDGE num-objects 1 num_hash_entries 81226 sccs size 81223 init 0.00ms df1 285.36ms sort 38.56ms dfs2 50.04ms setup-cb 9.95ms free-data 106.54ms user-cb 20.12ms clenanup 0.05ms links 5523436/5523436/5523096/1 dfs passes 1104 6883/11046605
D/Mono (15723): GC_MINOR: (Nursery full) pause 2.01ms, total 287.45ms, bridge 225.60 promoted 0K major 325184K los 1816K
D/Mono ( 2073): GC_MAJOR: (user request) pause 2.17ms, total 2.47ms, bridge 28.77 major 576K/576K los 0K/16K
```

在 `GC_BRIDGE` 消息中，`num-objects` 是此次传递考虑使用的桥对象数，`num_hash_entries` 是此次桥代码调用期间处理的对象数。

在 `GC_MINOR` 和 `GC_MAJOR` 消息中，`total` 是环境暂停的时间量（没有线程正在执行），而 `bridge` 是桥处理代码（处理 Java VM）所用的时间量。 发生桥处理时，环境未暂停  。

 通常，`num_hash_entries` 的值越大，`bridge` 集合花费的时间就越多，收集所花费的 `total` 时间就越多  。

## <a name="global-reference-messages"></a>全局引用消息

若要启用全局引用 (GREF) 日志记录，debug.mono.log 系统属性必须包含 gref，例如   ：

```shell
adb shell setprop debug.mono.log gref
```

Xamarin.Android 使用 Android 全局引用来提供 Java 实例和相关托管实例之间的映射，就像调用 Java 方法时，需要向 Java 提供 Java 实例一样。

遗憾的是，Android 仿真器一次只允许使用 2000 个全局引用。 硬件具有的全局引用限制高得多，是 52000 个。 在仿真器上运行应用程序时，限制较低会出现问题，因此知道实例的来源非常有用  。

> [!NOTE]
> 全局引用计数限于 Xamarin.Android 内部，不包括（也不能包括）由加载到进程中的其他本机库提取出的全局引用。 将全局引用计数用作估计值。

```shell
I/monodroid-gref(12405): +g+ grefc 108 gwrefc 0 obj-handle 0x40517468/L -> new-handle 0x40517468/L from    at Java.Lang.Object.RegisterInstance(IJavaObject instance, IntPtr value, JniHandleOwnership transfer)
I/monodroid-gref(12405):    at Java.Lang.Object.SetHandle(IntPtr value, JniHandleOwnership transfer)
I/monodroid-gref(12405):    at Java.Lang.Object..ctor(IntPtr handle, JniHandleOwnership transfer)
I/monodroid-gref(12405):    at Java.Lang.Thread+RunnableImplementor..ctor(System.Action handler, Boolean removable)
I/monodroid-gref(12405):    at Java.Lang.Thread+RunnableImplementor..ctor(System.Action handler)
I/monodroid-gref(12405):    at Android.App.Activity.RunOnUiThread(System.Action action)
I/monodroid-gref(12405):    at Mono.Samples.Hello.HelloActivity.UseLotsOfMemory(Android.Widget.TextView textview)
I/monodroid-gref(12405):    at Mono.Samples.Hello.HelloActivity.<OnCreate>m__3(System.Object o)
I/monodroid-gref(12405): handle 0x40517468; key_handle 0x40517468: Java Type: `mono/java/lang/RunnableImplementor`; MCW type: `Java.Lang.Thread+RunnableImplementor`
I/monodroid-gref(12405): Disposing handle 0x40517468
I/monodroid-gref(12405): -g- grefc 107 gwrefc 0 handle 0x40517468/L from    at Java.Lang.Object.Dispose(System.Object instance, IntPtr handle, IntPtr key_handle, JObjectRefType handle_type)
I/monodroid-gref(12405):    at Java.Lang.Object.Dispose()
I/monodroid-gref(12405):    at Java.Lang.Thread+RunnableImplementor.Run()
I/monodroid-gref(12405):    at Java.Lang.IRunnableInvoker.n_Run(IntPtr jnienv, IntPtr native__this)
I/monodroid-gref(12405):    at System.Object.c200fe6f-ac33-441b-a3a0-47659e3f6750(IntPtr , IntPtr )
I/monodroid-gref(27679): +w+ grefc 1916 gwrefc 296 obj-handle 0x406b2b98/G -> new-handle 0xde68f4bf/W from take_weak_global_ref_jni
I/monodroid-gref(27679): -w- grefc 1915 gwrefc 294 handle 0xde691aaf/W from take_global_ref_jni
```

有四个重要的消息：

- 全局引用创建：这些消息行以 +g+ 开头，将为创建代码路径提供堆栈跟踪  。
- 全局引用析构：这些消息行以 -g- 开头，可能为全局引用的代码路径处理提供堆栈跟踪  。 如果 GC 正在处理 gref，则不会提供堆栈跟踪。
- 弱全局引用创建：这些行以 +w+ 开头  。
- 弱全局引用析构：这些行以 -w- 开头  。

在所有消息中，grefc 值是 Xamarin.Android 创建的全局引用计数，grefwc 值是 Xamarin.Android 创建的弱全局引用计数   。 而 handle 或 obj-handle 值是 JNI 句柄值，“/”后面的字符是句柄值的类型：/L 表示本地引用，/G 表示全局引用，/W 表示弱全局引用       。

在 GC 过程中，系统将全局引用 (+g+) 转换为弱全局引用（得到 +w+ 和 -g-），逐出 Java 端的 GC，然后检查是否收集了弱全局引用。 如果仍然存在，则围绕弱引用（+g+、-w-）创建一个新的 gref，否则弱引用将被销毁 (-w)。

## <a name="java-instance-is-created-and-wrapped-by-a-mcw"></a>Java 实例由 MCW 创建和包装

```shell
I/monodroid-gref(27679): +g+ grefc 2211 gwrefc 0 obj-handle 0x4066df10/L -> new-handle 0x4066df10/L from ...
I/monodroid-gref(27679): handle 0x4066df10; key_handle 0x4066df10: Java Type: `android/graphics/drawable/TransitionDrawable`; MCW type: `Android.Graphics.Drawables.TransitionDrawable`
```

## <a name="a-gc-is-being-performed"></a>正在执行 GC…

```shell
I/monodroid-gref(27679): +w+ grefc 1953 gwrefc 259 obj-handle 0x4066df10/G -> new-handle 0xde68f95f/W from take_weak_global_ref_jni
I/monodroid-gref(27679): -g- grefc 1952 gwrefc 259 handle 0x4066df10/G from take_weak_global_ref_jni
```

## <a name="object-is-still-alive-as-handle--null"></a>对象仍存在，因为句柄 != null
## <a name="wref-turned-back-into-a-gref"></a>wref 恢复为 gref

```shell
I/monodroid-gref(27679): *try_take_global obj=0x4976f080 -> wref=0xde68f95f handle=0x4066df10
I/monodroid-gref(27679): +g+ grefc 1930 gwrefc 39 obj-handle 0xde68f95f/W -> new-handle 0x4066df10/G from take_global_ref_jni
I/monodroid-gref(27679): -w- grefc 1930 gwrefc 38 handle 0xde68f95f/W from take_global_ref_jni
```

## <a name="object-is-dead-as-handle--null"></a>对象不存在，因为句柄 == null
## <a name="wref-is-freed-no-new-gref-created"></a>wref 已释放，未创建任何新 gref

```shell
I/monodroid-gref(27679): *try_take_global obj=0x4976f080 -> wref=0xde68f95f handle=0x0
I/monodroid-gref(27679): -w- grefc 1914 gwrefc 296 handle 0xde68f95f/W from take_global_ref_jni
```

有一个有趣的窍门：在运行 Android 4.0 之前版本的目标上，gref 值等于 Java 对象在 Android 运行时内存中的地址。 （也就是说，GC 是非移动的、保守的收集器，它提供对这些对象的直接引用。）因此，在 +g+、+w+、-g-、+g+、-w- 序列之后，得到的 gref 值将与原始 gref 值相同。 这使检索整个日志变得非常简单。

不过，Android 4.0 具有移动的收集器，不再提供对 Android 运行时 VM 对象的直接引用。 于是，在 a+g+、+w+、-g-、+g+、-w- 序列之后，gref 值将变得不同  。 如果对象在经过多次 GC 后仍然存在，它将有几个 gref 值，这就很难确定分配实例的实际位置。

### <a name="querying-programmatically"></a>以编程方式查询

可以通过查询 `JniRuntime` 对象来查询 GREF 和 WREF 计数。

`Java.Interop.JniRuntime.CurrentRuntime.GlobalReferenceCount` - 全局引用计数

`Java.Interop.JniRuntime.CurrentRuntime.WeakGlobalReferenceCount` - 弱引用计数

## <a name="android-debug-logs"></a>Android 调试日志

[Android 调试日志](~/android/deploy-test/debugging/android-debug-log.md)可能会提供有关你看到的任何运行时错误的其他上下文。

## <a name="floating-point-performance-is-terrible"></a>浮点性能非常糟糕！

或者说，“我的应用在调试版本中的运行速度比发布版本快 10 倍！”

Xamarin.Android 支持多个设备 ABI：armeabi、armeabi-v7a 和 x86    。 可以在“项目属性”>“应用程序选项卡”>“支持的体系结构”中指定设备 ABI  。

调试版本使用提供所有 ABI 的 Android 包，因此将为目标设备使用最快的 ABI。

发布版本只包含在“项目属性”选项卡中选择的 ABI。可以选择多个。

“armeabi”是默认的 ABI，具有最广泛的设备支持  。 然而，armeabi 不支持多 CPU 设备和硬件浮点等  。 因此，使用 armeabi 发布运行时的应用将与单个核心绑定，并将使用软浮动实现。 这两种情况都会显著降低应用的性能。

如果应用需要良好的浮点性能（例如游戏），你应该启用 armeabi-v7a ABI  。 你可能只想支持 armeabi-v7a 运行时，尽管这意味着只支持 armeabi 的旧设备将无法运行你的应用   。

## <a name="could-not-locate-android-sdk"></a>找不到 Android SDK

Google 提供了 2 个适用于 Windows 的 Android SDK 下载。
如果选择 .exe 安装程序，它将编写注册表项，告诉 Xamarin.Android 安装位置。 如果选择 .zip 文件并自行解压缩，Xamarin.Android 不知道在何处查找 SDK。 通过转到“工具”>“选项”>“Xamarin”>“Android 设置”来告诉 Xamarin.Android SDK 在 Visual Studio 中的位置  ：

[![Xamarin Android 设置中的 Android SDK 位置](troubleshooting-images/01.png)](troubleshooting-images/01.png#lightbox)

## <a name="ide-does-not-display-target-device"></a>IDE 不显示目标设备

有时，你会尝试将应用程序部署到设备，但要部署到的设备未显示在“选择设备”对话框中。 Android Debug Bridge 出现异常时，可能会发生这种情况。

若要诊断此问题，请找到 [adb 程序](~/android/deploy-test/debugging/android-debug-log.md)，然后运行：

```shell
adb devices
```

如果设备不存在，则需要重启 Android Debug Bridge 服务器，以便可以找到设备：

```shell
adb kill-server
adb start-server
```

HTC 同步软件可能会阻止“adb start-server”正常运行  。 如果“adb start-server”命令没有打印出它启动所用的端口，请退出 HTC 同步软件并尝试重启 adb 服务器  。

## <a name="the-specified-task-executable-keytool-could-not-be-run"></a>无法运行指定的任务可执行文件“keytool”

这意味着，路径不包含 Java SDK 的 bin 目录所在的目录。 检查是否遵循了[安装](~/android/get-started/installation/index.md)指南中的步骤。

## <a name="monodroidexe-or-aresgenexe-exited-with-code-1"></a>monodroid.exe 或 aresgen.exe 已退出，显示错误代码 1

若要帮助调试此问题，请转到 Visual Studio 并更改 MSBuild 详细程度。若要执行此操作，请选择：“工具”>“选项”>“项目和解决方案”>“生成并运行”>“MSBuild 项目生成输出详细程度”，并将此值设置为“正常”     。

重新生成，并检查 Visual Studio 的输出窗格，该窗格应包含完整错误。

## <a name="there-is-not-enough-storage-space-on-the-device-to-deploy-the-package"></a>设备上没有足够的存储空间用于部署包

如果不从 Visual Studio 内启动仿真器，就会发生这种情况。 在 Visual Studio 之外启动仿真器时，需要传递 `-partition-size 512` 选项，例如

```shell
emulator -partition-size 512 -avd MonoDroid
```

确保使用正确的仿真器名，即[配置仿真器时使用的名称](~/android/get-started/installation/windows.md#device)。

## <a name="install_failed_invalid_apk-when-installing-a-package"></a>安装包时，系统提示 INSTALL\_FAILED\_INVALID\_APK

Android 包名称必须包含句点（“.”）   。 编辑包名称，使其包含句点。

- 在 Visual Studio 中：
  - 右键单击“项目”>“属性”
  - 单击左侧的“Android 清单”选项卡。
  - 更新“包名称”字段。
    - 如果看到消息&ldquo;找不到 AndroidManifest.xml。 单击添加一个。&rdquo;，单击链接，然后更新包名称字段。
- 在 Visual Studio for Mac 中：
  - 右键单击“项目”>“选项”
  - 导航到“生成/Android 应用程序”部分。
  - 将包名称字段更改为包含“.”。

## <a name="install_failed_missing_shared_library-when-installing-a-package"></a>安装包时，系统提示 INSTALL\_FAILED\_MISSING\_SHARED\_LIBRARY

此上下文中的“共享库”不是本机共享库 (libfoo.so) 文件；而是必须单独安装在目标设备上的库，如 Google Maps   。

Android 包 `<uses-library/>` 元素指定需要哪些共享库。 如果目标设备上没有所需的库（例如 `//uses-library/@android:required` 为 true，这是默认值），则包安装将失败，系统显示错误消息 INSTALL\_FAILED\_MISSING\_SHARED\_LIBRARY    。

若要确定所需的共享库，请查看生成的 AndroidManifest.xml 文件（例如obj\\Debug\\android\\AndroidManifest.xml），并查找 `<uses-library/>` 元素    。 可以在项目的 Properties\\AndroidManifest.xml 文件中，通过 [UsesLibraryAttribute 自定义属性](xref:Android.App.UsesLibraryAttribute)手动添加 `<uses-library/>` 元素  。

例如，添加对 Mono.Android.Google Maps.dll 的程序集引用将隐式地为 Google Maps 共享库添加 `<uses-library/>` 。

## <a name="install_failed_update_incompatible-when-installing-a-package"></a>安装包时，系统提示 INSTALL\_FAILED\_UPDATE\_INCOMPATIBLE

Android 包有三个要求：

- 它们必须包含“.”（请参阅上一项）
- 它们必须拥有唯一的字符串包名称（也就是在 Android 应用名中所用的反向 TLD 约定，例如为 Chrome 应用使用名称 com.android.chrome）
- 升级包时，包必须具有相同的签名密钥。

因此，请设想以下情况：

1. 你将应用生成并部署为调试应用
2. 更改签名密钥，例如用作发布应用（或因你不喜欢默认提供的调试签名密钥）
3. 安装应用时不首先将其删除，例如在 Visual Studio 内点击“调试”>“开始执行(不调试)”

发生这种情况时，包安装将失败，并指示 INSTALL\_FAILED\_UPDATE\_INCOMPATIBLE 错误，因为签名密钥更改时包名称没有更改。 [Android 调试日志](~/android/deploy-test/debugging/android-debug-log.md)还将包含类似于以下内容的消息：

```shell
E/PackageManager(  146): Package [PackageName] signatures do not match the previously installed version; ignoring!
```

若要修复此错误，请在重新安装之前从设备中完全删除应用程序。

## <a name="install_failed_uid_changed-when-installing-a-package"></a>安装包时，系统提示 INSTALL\_FAILED\_UID\_CHANGED

安装 Android 包时，系统会为其分配一个“用户 ID”(UID)  。
有时，由于当前未知的原因，在已安装的应用上安装包时，安装将失败，系统提示 `INSTALL_FAILED_UID_CHANGED` ：

```shell
ERROR [2015-03-23 11:19:01Z]: ANDROID: Deployment failed
Mono.AndroidTools.InstallFailedException: Failure [INSTALL_FAILED_UID_CHANGED]
   at Mono.AndroidTools.Internal.AdbOutputParsing.CheckInstallSuccess(String output, String packageName)
   at Mono.AndroidTools.AndroidDevice.<>c__DisplayClass2c.<InstallPackage>b__2b(Task`1 t)
   at System.Threading.Tasks.ContinuationTaskFromResultTask`1.InnerInvoke()
   at System.Threading.Tasks.Task.Execute()
```

若要暂时避开此问题，请完全卸载 Android 包，方法是从 Android 目标的 GUI 安装应用，或者使用 `adb` ：

```shell
$ adb uninstall @PACKAGE_NAME@
```

请勿使用 `adb uninstall -k`，因为这将保留应用程序数据，从而在目标设备上保留冲突的 UID   。

## <a name="release-apps-fail-to-launch-on-device"></a>无法在设备上启动发布应用

Android 调试日志输出是否包含类似于以下内容的消息：

```shell
D/AndroidRuntime( 1710): Shutting down VM
W/dalvikvm( 1710): threadid=1: thread exiting with uncaught exception (group=0xb412f180)
E/AndroidRuntime( 1710): FATAL EXCEPTION: main
E/AndroidRuntime( 1710): java.lang.UnsatisfiedLinkError: Couldn't load monodroid: findLibrary returned null
E/AndroidRuntime( 1710):        at java.lang.Runtime.loadLibrary(Runtime.java:365)
```

如果是，则有两个可能的原因：

1. .apk 不提供目标设备支持的 ABI。
    例如，.apk 只包含 armeabi-v7a 二进制文件，而目标设备只支持 armeabi。

2. [Android bug](https://code.google.com/p/android/issues/detail?id=21670)。 如果是这种情况，请卸载应用，祈祷好运，然后重新安装应用。

若要修复 (1)，请编辑项目选项/属性，然后[向支持的 ABI 列表中添加对所需 ABI 的支持](~/android/app-fundamentals/cpu-architectures.md)。 若要确定需要添加到的 ABI，请对目标设备运行以下 adb 命令：

```shell
adb shell getprop ro.product.cpu.abi
adb shell getprop ro.product.cpu.abi2
```

输出将包含主 ABI（并且根据需要包含辅助 ABI）。

```shell
$ adb shell getprop | grep ro.product.cpu
[ro.product.cpu.abi2]: [armeabi]
[ro.product.cpu.abi]: [armeabi-v7a]
```

## <a name="the-outpath-property-is-not-set-for-project-ldquomyappcsprojrdquo"></a>没有为项目 &ldquo;MyApp.csproj&rdquo; 设置 OutPath 属性

这通常表示你有一个 HP 计算机，并且环境变量 &ldquo;Platform&rdquo; 已设置为类似 MCD 或 HPD 的值。 这与通常设置为“任意 CPU”或“x86”的MSBuild Platform 属性冲突&ldquo;&rdquo;&ldquo;&rdquo;。 在 MSBuild 可以正常运行之前，你需要从计算机中删除此环境变量：

- 控制面板 > 系统 > 高级 > 环境变量

重启 Visual Studio 或 Visual Studio for Mac 并尝试重新生成。 现在，一切应当会按预期工作。

## <a name="javalangclasscastexception-monoandroidruntimejavaobject-cannot-be-cast-to"></a>无法将 java.lang.ClassCastException: mono.android.runtime.JavaObject 强制转换为...

Xamarin.Android 4.x 未能正确地封送嵌套泛型类型。 例如，请考虑使用以下使用 [SimpleExpandableListAdapter](xref:Android.Widget.SimpleExpandableListAdapter) 的 C\# 代码：

```csharp
// BAD CODE; DO NOT USE
var groupData = new List<IDictionary<string, object>> () {
        new Dictionary<string, object> {
                { "NAME", "Group 1" },
                { "IS_EVEN", "This group is odd" },
        },
};
var childData = new List<IList<IDictionary<string, object>>> () {
        new List<IDictionary<string, object>> {
                new Dictionary<string, object> {
                        { "NAME", "Child 1" },
                        { "IS_EVEN", "This group is odd" },
                },
        },
};
mAdapter = new SimpleExpandableListAdapter (
        this,
        groupData,
        Android.Resource.Layout.SimpleExpandableListItem1,
        new string[] { "NAME", "IS_EVEN" },
        new int[] { Android.Resource.Id.Text1, Android.Resource.Id.Text2 },
        childData,
        Android.Resource.Layout.SimpleExpandableListItem2,
        new string[] { "NAME", "IS_EVEN" },
        new int[] { Android.Resource.Id.Text1, Android.Resource.Id.Text2 }
);
```

问题在于，Xamarin.Android 错误地封送嵌套泛型类型。 `List<IDictionary<string, object>>` 封送至 [java.lang.ArrrayList](xref:Java.Util.ArrayList)，但 `ArrayList` 包含 `mono.android.runtime.JavaObject` 实例（它引用 `Dictionary<string, object>` 实例），而不是实现 [java.util.Map](xref:Java.Util.IMap) 的实例，从而导致以下异常：

```shell
E/AndroidRuntime( 2991): FATAL EXCEPTION: main
E/AndroidRuntime( 2991): java.lang.ClassCastException: mono.android.runtime.JavaObject cannot be cast to java.util.Map
E/AndroidRuntime( 2991):        at android.widget.SimpleExpandableListAdapter.getGroupView(SimpleExpandableListAdapter.java:278)
E/AndroidRuntime( 2991):        at android.widget.ExpandableListConnector.getView(ExpandableListConnector.java:446)
E/AndroidRuntime( 2991):        at android.widget.AbsListView.obtainView(AbsListView.java:2271)
E/AndroidRuntime( 2991):        at android.widget.ListView.makeAndAddView(ListView.java:1769)
E/AndroidRuntime( 2991):        at android.widget.ListView.fillDown(ListView.java:672)
E/AndroidRuntime( 2991):        at android.widget.ListView.fillFromTop(ListView.java:733)
E/AndroidRuntime( 2991):        at android.widget.ListView.layoutChildren(ListView.java:1622)
```

变通方法是使用提供的 [Java 集合类型](~/android/internals/api-design.md)，而不是用作&ldquo;内部&rdquo;类型的 `System.Collections.Generic` 类型。 这将在封送实例时得到适当的 Java 类型。 （为了缩短 gref 的生存期，以下代码比需要的更复杂。 如果不考虑 gref 的生存期，可以简化为通过 `s/List/JavaList/g` 和 `s/Dictionary/JavaDictionary/g` 更改原始代码。）

```csharp
// insert good code here
using (var groupData = new JavaList<IDictionary<string, object>> ()) {
    using (var groupEntry = new JavaDictionary<string, object> ()) {
        groupEntry.Add ("NAME", "Group 1");
        groupEntry.Add ("IS_EVEN", "This group is odd");
        groupData.Add (groupEntry);
    }
    using (var childData = new JavaList<IList<IDictionary<string, object>>> ()) {
        using (var childEntry = new JavaList<IDictionary<string, object>> ())
        using (var childEntryDict = new JavaDictionary<string, object> ()) {
            childEntryDict.Add ("NAME", "Child 1");
            childEntryDict.Add ("IS_EVEN", "This child is odd.");
            childEntry.Add (childEntryDict);
            childData.Add (childEntry);
        }
        mAdapter = new SimpleExpandableListAdapter (
            this,
            groupData,
            Android.Resource.Layout.SimpleExpandableListItem1,
            new string[] { "NAME", "IS_EVEN" },
            new int[] { Android.Resource.Id.Text1, Android.Resource.Id.Text2 },
            childData,
            Android.Resource.Layout.SimpleExpandableListItem2,
            new string[] { "NAME", "IS_EVEN" },
            new int[] { Android.Resource.Id.Text1, Android.Resource.Id.Text2 }
        );
    }
}
```

[此问题将在未来版本中得到解决](https://bugzilla.xamarin.com/show_bug.cgi?id=5401)。

## <a name="unexpected-nullreferenceexceptions"></a>意外的 NullReferenceExceptions

[Android 调试日志](~/android/deploy-test/debugging/android-debug-log.md)偶尔会在应用停止前提到&ldquo; 不会发生&rdquo;或来自 Mono for Android 运行时代码的 NullReferenceExceptions：

```shell
E/mono(15202): Unhandled Exception: System.NullReferenceException: Object reference not set to an instance of an object
E/mono(15202):   at Java.Lang.Object.GetObject (IntPtr handle, System.Type type, Boolean owned)
E/mono(15202):   at Java.Lang.Object._GetObject[IOnTouchListener] (IntPtr handle, Boolean owned)
E/mono(15202):   at Java.Lang.Object.GetObject[IOnTouchListener] (IntPtr handle, Boolean owned)
E/mono(15202):   at Android.Views.View+IOnTouchListenerAdapter.n_OnTouch_Landroid_view_View_Landroid_view_MotionEvent_(IntPtr jnienv, IntPtr native__this, IntPtr native_v, IntPtr native_e)
E/mono(15202):   at (wrapper dynamic-method) object:b039cbb0-15e9-4f47-87ce-442060701362 (intptr,intptr,intptr,intptr)
```

or

```shell
E/mono    ( 4176): Unhandled Exception:
E/mono    ( 4176): System.NullReferenceException: Object reference not set to an instance of an object
E/mono    ( 4176): at Android.Runtime.JNIEnv.NewString (string)
E/mono    ( 4176): at Android.Util.Log.Info (string,string)
```

Android 运行时决定中止进程时可能会发生这种情况，出现这种情况的原因有很多，包括达到目标 GREF 限制或使用 JNI 执行了&ldquo;错误&rdquo;的操作。

若要了解是否属于这种情况，请查看 Android 调试日志是否含有来自进程的消息与以下消息相似：

```shell
E/dalvikvm(  123): VM aborting
```

## <a name="abort-due-to-global-reference-exhaustion"></a>因全局引用耗尽而中止

Android 运行时的 JNI 层只支持限定数量的 JNI 对象引用在任何给定的时间点有效。 超过此限制时，就会出现中断。

仿真器中的 GREF（全局引用）限制是 2000 个引用，硬件上的限制是 52000 个  。

在 Android 调试日志中看到如下消息时，就知道创建了太多的 GREF：

```shell
D/dalvikvm(  602): GREF has increased to 1801
```

达到 GREF 限制时，系统将提示如下消息：

```shell
D/dalvikvm(  602): GREF has increased to 2001
W/dalvikvm(  602): Last 10 entries in JNI global reference table:
W/dalvikvm(  602):  1991: 0x4057eff8 cls=Landroid/graphics/Point; (20 bytes)
W/dalvikvm(  602):  1992: 0x4057f010 cls=Landroid/graphics/Point; (28 bytes)
W/dalvikvm(  602):  1993: 0x40698e70 cls=Landroid/graphics/Point; (20 bytes)
W/dalvikvm(  602):  1994: 0x40698e88 cls=Landroid/graphics/Point; (20 bytes)
W/dalvikvm(  602):  1995: 0x40698ea0 cls=Landroid/graphics/Point; (28 bytes)
W/dalvikvm(  602):  1996: 0x406981f0 cls=Landroid/graphics/Point; (20 bytes)
W/dalvikvm(  602):  1997: 0x40698208 cls=Landroid/graphics/Point; (20 bytes)
W/dalvikvm(  602):  1998: 0x40698220 cls=Landroid/graphics/Point; (28 bytes)
W/dalvikvm(  602):  1999: 0x406956a8 cls=Landroid/graphics/Point; (20 bytes)
W/dalvikvm(  602):  2000: 0x406956c0 cls=Landroid/graphics/Point; (20 bytes)
W/dalvikvm(  602): JNI global reference table summary (2001 entries):
W/dalvikvm(  602):    51 of Ljava/lang/Class; 164B (41 unique)
W/dalvikvm(  602):    46 of Ljava/lang/Class; 188B (17 unique)
W/dalvikvm(  602):     6 of Ljava/lang/Class; 212B (6 unique)
W/dalvikvm(  602):    11 of Ljava/lang/Class; 236B (7 unique)
W/dalvikvm(  602):     3 of Ljava/lang/Class; 260B (3 unique)
W/dalvikvm(  602):     4 of Ljava/lang/Class; 284B (2 unique)
W/dalvikvm(  602):     8 of Ljava/lang/Class; 308B (6 unique)
W/dalvikvm(  602):     1 of Ljava/lang/Class; 316B
W/dalvikvm(  602):     4 of Ljava/lang/Class; 332B (3 unique)
W/dalvikvm(  602):     1 of Ljava/lang/Class; 356B
W/dalvikvm(  602):     2 of Ljava/lang/Class; 380B (1 unique)
W/dalvikvm(  602):     1 of Ljava/lang/Class; 428B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 452B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 476B
W/dalvikvm(  602):     2 of Ljava/lang/Class; 500B (1 unique)
W/dalvikvm(  602):     1 of Ljava/lang/Class; 548B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 572B
W/dalvikvm(  602):     2 of Ljava/lang/Class; 596B (2 unique)
W/dalvikvm(  602):     1 of Ljava/lang/Class; 692B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 956B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 1004B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 1148B
W/dalvikvm(  602):     2 of Ljava/lang/Class; 1172B (1 unique)
W/dalvikvm(  602):     1 of Ljava/lang/Class; 1316B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 3428B
W/dalvikvm(  602):     1 of Ljava/lang/Class; 3452B
W/dalvikvm(  602):     1 of Ljava/lang/String; 28B
W/dalvikvm(  602):     2 of Ldalvik/system/VMRuntime; 12B (1 unique)
W/dalvikvm(  602):    10 of Ljava/lang/ref/WeakReference; 28B (10 unique)
W/dalvikvm(  602):     1 of Ldalvik/system/PathClassLoader; 44B
W/dalvikvm(  602):  1553 of Landroid/graphics/Point; 20B (1553 unique)
W/dalvikvm(  602):   261 of Landroid/graphics/Point; 28B (261 unique)
W/dalvikvm(  602):     1 of Landroid/view/MotionEvent; 100B
W/dalvikvm(  602):     1 of Landroid/app/ActivityThread$ApplicationThread; 28B
W/dalvikvm(  602):     1 of Landroid/content/ContentProvider$Transport; 28B
W/dalvikvm(  602):     1 of Landroid/view/Surface$CompatibleCanvas; 44B
W/dalvikvm(  602):     1 of Landroid/view/inputmethod/InputMethodManager$ControlledInputConnectionWrapper; 36B
W/dalvikvm(  602):     1 of Landroid/view/ViewRoot$1; 12B
W/dalvikvm(  602):     1 of Landroid/view/ViewRoot$W; 28B
W/dalvikvm(  602):     1 of Landroid/view/inputmethod/InputMethodManager$1; 28B
W/dalvikvm(  602):     1 of Landroid/view/accessibility/AccessibilityManager$1; 28B
W/dalvikvm(  602):     1 of Landroid/widget/LinearLayout$LayoutParams; 44B
W/dalvikvm(  602):     1 of Landroid/widget/LinearLayout; 332B
W/dalvikvm(  602):     2 of Lorg/apache/harmony/xnet/provider/jsse/TrustManagerImpl; 28B (1 unique)
W/dalvikvm(  602):     1 of Landroid/view/SurfaceView$MyWindow; 36B
W/dalvikvm(  602):     1 of Ltouchtest/RenderThread; 92B
W/dalvikvm(  602):     1 of Landroid/view/SurfaceView$3; 12B
W/dalvikvm(  602):     1 of Ltouchtest/DrawingView; 412B
W/dalvikvm(  602):     1 of Ltouchtest/Activity1; 180B
W/dalvikvm(  602): Memory held directly by tracked refs is 75624 bytes
E/dalvikvm(  602): Excessive JNI global references (2001)
E/dalvikvm(  602): VM aborting
```

在上面的例子中（顺便说一下，这个例子来自 [bug 685215](https://bugzilla.novell.com/show_bug.cgi?id=685215)），问题在于创建了太多的 Android.Graphics.Point 实例；请参阅[注释 \#2](https://bugzilla.novell.com/show_bug.cgi?id=685215#c2)，了解此项特定错误的一系列修复措施。

有用的解决方案通常是找到分配了太多实例的类型（上面的转储文件中的 Android.Graphics.Point），然后在源代码中找到创建它们的位置并适当地处理它们（以便缩短其 Java 对象生存期）。 虽然这种做法并不始终适用（\# 685215 是多线程的，这种繁琐的解决方案避免了 Dispose 调用），却是首先要考虑的做法。

可以启用 [GREF 日志记录](~/android/troubleshooting/index.md)，查看 GREF 的创建时间以及存在的数量。

## <a name="abort-due-to-jni-type-mismatch"></a>因 JNI 类型不匹配而中止

如果手动滚动 JNI 代码，类型有可能匹配错误，例如，如果尝试对不实现 `java.lang.Runnable` 的类型调用 `java.lang.Runnable.run`。 出现这种情况时，Android 调试日志中将显示类似的消息：

```shell
W/dalvikvm( 123): JNI WARNING: can't call Ljava/Type;;.method on instance of Lanother/java/Type;
W/dalvikvm( 123):              in Lmono/java/lang/RunnableImplementor;.n_run:()V (CallVoidMethodA)
...
E/dalvikvm( 123): VM aborting
```

## <a name="dynamic-code-support"></a>动态代码支持

### <a name="dynamic-code-does-not-compile"></a>动态代码不能编译

若要在应用程序或库中使用 C\# 动态代码，必须将 System.Core.dll、Microsoft.CSharp.dll 和 Mono.CSharp.dll 添加到项目中。

### <a name="in-release-build-missingmethodexception-occurs-for-dynamic-code-at-run-time"></a>在发布版本中，动态代码在运行时发生 MissingMethodException。

- 应用程序项目可能没有对 System.Core.dll、Microsoft.CSharp.dll 或 Mono.CSharp.dll 的引用。 请确保引用了这些程序集。

  - 请记住，动态代码始终会产生费用。 如果需要有效的代码，请考虑不要使用动态代码。

- 在第一个预览中，排除了这些程序集，除非应用程序代码显式使用每个程序集中的类型。 请查看以下网页，了解变通方法：[http://lists.ximian.com/pipermail/mo...il/009798.html](http://lists.ximian.com/pipermail/monodroid/2012-April/009798.html)

## <a name="projects-built-with-aotllvm-crash-on-x86-devices"></a>在 x86 设备上使用 AOT+LLVM 生成的项目出现故障

在基于 x86 的设备上部署使用 [AOT+LLVM](~/android/deploy-test/release-prep/index.md) 生成的应用时，可能会看到类似于以下内容的异常错误消息：

```shell
Assertion: should not be reached at /Users/.../external/mono/mono/mini/tramp-x86.c:124
Fatal signal 6 (SIGABRT), code -6 in tid 4051 (Xamarin.bug56111)
```

这是一个已知问题 - 变通方法是禁用 LLVM。
