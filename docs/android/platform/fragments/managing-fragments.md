---
title: 管理片段
ms.prod: xamarin
ms.assetid: 02C5E8F0-32EF-4FD9-DC8B-04650E20722C
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/07/2018
ms.openlocfilehash: 3733ed37abe9604e77529db4864f601d2b473280
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/13/2020
ms.locfileid: "73027321"
---
# <a name="managing-fragments"></a>管理片段

为了帮助管理片段，Android 提供了 `FragmentManager` 类。 每个活动都有一个 `Android.App.FragmentManager` 实例，它会查找或动态更改自己的片段。 每一组更改都称为“事务”  ，并通过使用类 `Android.App.FragmentTransation`（由 `FragmentManager` 托管）中包含的 API 之一来执行。 活动可能会启动事务，如下所示：

```csharp
FragmentTransaction fragmentTx = this.FragmentManager.BeginTransaction();
```

这些对片段的更改是使用 `Add()`、`Remove(),` 和 `Replace().` 等方法在 `FragmentTransaction` 实例中执行。然后，使用 `Commit()` 应用更改。 事务中的更改不会立即执行。
相反，将它们计划为在活动的 UI 线程上尽快运行。

下面的示例展示了如何向现有容器添加片段：

```csharp
// Create a new fragment and a transaction.
FragmentTransaction fragmentTx = this.FragmentManager.BeginTransaction();
DetailsFragment aDifferentDetailsFrag = new DetailsFragment();

// The fragment will have the ID of Resource.Id.fragment_container.
fragmentTx.Add(Resource.Id.fragment_container, aDifferentDetailsFrag);

// Commit the transaction.
fragmentTx.Commit();
```

如果事务提交晚于 `Activity.OnSaveInstanceState()` 调用，就会导致异常抛出。 这是因为，当活动保存它的状态时，Android 也会保存任何托管片段的状态。 如果在此之后提交任何片段事务，这些事务的状态就会在活动还原时丢失。

可以通过调用 `FragmentTransaction.AddToBackStack()`，将片段事务保存到活动的[返回栈](https://developer.android.com/guide/topics/fundamentals/tasks-and-back-stack.html)。 这样，用户可以在按下“返回”  按钮时，向后导航浏览片段更改。 如果不调用此方法，删除的片段会被销毁，并在用户通过活动导航回来时不可用。

下面的示例展示了如何使用 `FragmentTransaction` 的 `AddToBackStack` 方法来替换一个片段，同时暂留返回栈上第一个片段的状态：

```csharp
// Create a new fragment and a transaction.
FragmentTransaction fragmentTx = this.FragmentManager.BeginTransaction();
DetailsFragment aDifferentDetailsFrag = new DetailsFragment();

// Replace the fragment that is in the View fragment_container (if applicable).
fragmentTx.Replace(Resource.Id.fragment_container, aDifferentDetailsFrag);

// Add the transaction to the back stack.
fragmentTx.AddToBackStack(null);

// Commit the transaction.
fragmentTx.Commit();
```

## <a name="communicating-with-fragments"></a>与片段通信

FragmentManager  了解所有附加到活动的片段，并提供两种方法来帮助查找这些片段：

- **FindFragmentById** &ndash; 此方法使用在布局文件中指定的 ID 或在将片段添加为事务的一部分时指定的容器 ID 来查找片段。

- **FindFragmentByTag** &ndash; 此方法用于查找包含在布局文件中提供的标记或在事务中添加的标记的片段。

由于片段和活动都引用 `FragmentManager`，因此使用相同的技术在它们之间来回通信。 应用程序可以通过使用这两种方法之一来查找引用片段，将此引用强制转换为相应类型，然后直接对片段调用方法。 下面的代码片段举例说明了这一点：

活动还可以使用 `FragmentManager` 来查找片段：

```csharp
var emailList = FragmentManager.FindFragmentById<EmailListFragment>(Resource.Id.email_list_fragment);
emailList.SomeCustomMethod(parameter1, parameter2);
```

### <a name="communicating-with-the-activity"></a>与活动通信

片段可以使用 `Fragment.Activity` 属性来引用它的主机。 通过将活动强制转换为更具体的类型，活动可以对它的主机调用方法和属性，如下面的示例所示：

```csharp
var myActivity = (MyActivity) this.Activity;
myActivity.SomeCustomMethod();
```
