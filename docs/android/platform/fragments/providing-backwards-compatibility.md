---
title: 为 Android 支持包提供后向兼容性
ms.prod: xamarin
ms.assetid: 7511D2F8-2B4F-4200-C74E-E967153B2E8D
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/12/2017
ms.openlocfilehash: c32d666da1347b947c55209ed7c7ec31a97a70e0
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/13/2020
ms.locfileid: "73027300"
---
# <a name="providing-backwards-compatibility-with-the-android-support-package"></a>为 Android 支持包提供后向兼容性

如果低于 Android 3.0（API 级别 11）版本的设备不具备后向兼容性，Fragment 的用途将受到限制。 为了提供此功能，Google 引入了[支持库](https://developer.android.com/sdk/compatibility-library.html)（最初发布时称为“Android 兼容性库”  ），该库将一些 API 从较新的 Android 版本移植到较旧的 Android 版本。 它是支持运行 Android 1.6（API 级别 4）到 Android 2.3.3 （API 级别 10）的设备的 Android 支持包。

> [!NOTE]
> 通过 Android 支持包只能使用 `ListFragment` 和 `DialogFragment`。 Android 支持包中不支持其他任何 Fragment 子类，如 `PreferenceFragment,`。 它们将无法在低于 Android 3.0 版本的应用程序中使用。 

## <a name="adding-the-support-package"></a>添加支持包

Android 支持包不会自动添加到 Xamarin.Android 应用程序中。 Xamarin 提供了 [Android 支持库 v4 NuGet 包](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)来简化向 Xamarin.Android 应用程序添加支持库的过程。要将支持包添加到 Xamarin.Android 应用程序中，请将 [Android 支持库 v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) 组件加入 Xamarin.Android 项目中，如以下屏幕截图所示： 

[![将 Android 支持库 v4 包添加到项目中的屏幕截图](providing-backwards-compatibility-images/02-sml.png)](providing-backwards-compatibility-images/02.png#lightbox)

完成这些步骤后，便可以在早期版本的 Android 中使用 Fragment。 Fragment API 现在可以在这些早期版本中使用，但以下情况除外： 

- **更改最低 Android 版本** &ndash; 应用程序不再需要面向 Android 3.0 或更高版本，如下所示： 

    [![在“Android 清单”下设置最低 Android 目标版本的屏幕截图](providing-backwards-compatibility-images/03-sml.png)](providing-backwards-compatibility-images/03.png#lightbox)

- **扩展 FragmentActivity** &ndash; 托管 Fragment 的 Activity 现在必须继承自 `Android.Support.V4.App.FragmentActivity`，而不是继承自 `Android.App.Activity`。 

- **更新命名空间** &ndash; 继承自 `Android.App.Fragment` 的类现在必须继承自 `Android.Support.V4.App.Fragment`。 删除源代码文件顶部的 using 语句“`using Android.App;`”，然后将其替换为“`using Android.Support.V4.App`”。 

- **使用 SupportFragmentManager** &ndash; `Android.Support.V4.App.FragmentActivity` 公开 `SupportingFragmentManager` 属性，该属性必须用于获取对 `FragmentManager` 的引用。 例如： 

```csharp
FragmentTransaction fragmentTx = this.SupportingFragmentManager.BeginTransaction();
DetailsFragment detailsFrag = new DetailsFragment();
fragmentTx.Add(Resource.Id.fragment_container, detailsFrag);
fragmentTx.Commit();
```

完成这些更改后，就可以在 Android 1.6 或 2.x 以及 Honeycomb 和 Ice Cream Sandwich 上运行基于 Fragment 的应用程序了。 

## <a name="related-links"></a>相关链接

- [Android 支持库 v4 NuGet](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)
