---
title: 绑定 Eclipse 库项目
description: 本演练介绍了如何使用 Xamarin.Android 项目模板来绑定 Eclipse Android 库项目。
ms.prod: xamarin
ms.assetid: CEE90F8A-164B-4155-813A-7537A665A7E7
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/01/2018
ms.openlocfilehash: 2ac402bf423c9f3fe136d1ba31622d915d2e2eef
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/10/2020
ms.locfileid: "73027730"
---
# <a name="binding-an-eclipse-library-project"></a>绑定 Eclipse 库项目

本演练介绍了如何使用 Xamarin.Android 项目模板来绑定 Eclipse Android 库项目。 

## <a name="overview"></a>概述

尽管 .AAR 文件正日益成为 Android 库分发的规范，但在某些情况下，仍有必要为 Android 库项目  创建绑定。 Android 库项目是特殊的 Android 项目，其中包含可由 Android 应用程序项目引用的可共享代码和资源。 通常，在 Eclipse IDE 中创建库时，需要绑定到 Android 库项目。
本演练举例说明了如何从 Eclipse 项目的目录结构创建 Android 库项目 .ZIP。

Android 库项目与常规 Android 项目的不同之处在于，前者没有编译到 APK 中，也不能自行部署到设备上。 相反，Android 库项目应由 Android 应用程序项目引用。 生成 Android 应用程序项目时，首先编译的是 Android 库项目。 然后，Android 应用程序项目会被并入已编译的 Android 库项目中，并将代码和资源添加到 APK 中以供分发。 正是由于有这种差异，为 Android 库项目创建绑定与为 Java .JAR 或 .AAR 文件创建绑定略有不同。

## <a name="walkthrough"></a>演练

若要在 Xamarin.Android Java 绑定项目中使用 Android 库项目，首先需要在 Eclipse 中生成 Android 库项目。 下面的屏幕截图展示了一个编译后 Android 库项目示例： 

[![Eclipse 中的示例库项目](binding-a-library-project-images/build-lib-in-eclipse.png)](binding-a-library-project-images/build-lib-in-eclipse.png#lightbox)

请注意，Android 库项目中的源代码已编译到临时 .JAR 文件 android-mapviewballoons.jar  中，且资源已复制到 bin/res/crunch  文件夹中。 

在 Eclipse 中编译完 Android 库项目后，便可以使用 Xamarin.Androidn Java 绑定项目来绑定它。 首先，必须创建 .ZIP 文件，其中包含 Android 库项目的 bin  和 res  文件夹。 请务必删除干预 crunch  子目录，让资源驻留在 bin/res  中。下面的屏幕截图展示了一个此类 .ZIP 文件的内容： 

[![Android 库项目 .zip 的内容](binding-a-library-project-images/contents-of-zip-file.png)](binding-a-library-project-images/contents-of-zip-file.png#lightbox)

然后，将此 .ZIP 文件添加到 Xamarin.Android Java 绑定项目中，如下面的屏幕截图所示：

[![添加到 Java 绑定项目的 Zip 文件](binding-a-library-project-images/zip-in-binding-project.png)](binding-a-library-project-images/zip-in-binding-project.png#lightbox)

请注意，.ZIP 文件的“生成操作”已自动设置为“LibraryProjectZip”  。

若有 Android 库项目需要的任何 .JAR 文件，应将它们添加到 Java 绑定库项目的 Jar  文件夹中，并将“生成操作”  设置为“ReferenceJar”  。 下面的屏幕截图中举例说明了这种情况： 

[![设置为“ReferenceJar”的“生成操作”](binding-a-library-project-images/set-to-referencejar.png)](binding-a-library-project-images/set-to-referencejar.png#lightbox)

完成这些步骤后，可以按本文档前面所述，使用 Xamarin.Android Java 绑定项目。

> [!NOTE]
> 暂不支持在其他 IDE 中编译 Android 库项目。 其他 IDE 可能不会在 bin  文件夹中创建与 Eclipse 相同的目录结构或文件。 

## <a name="summary"></a>总结

本文逐步介绍了绑定 Android 库项目的过程。 我们先是在 Eclipse 中生成了 Android 库项目，然后从 Android 库项目的 bin  和 res  文件夹创建了 zip 文件。 接下来，我们使用此 zip 创建了 Xamarin.Android Java 绑定项目。 
