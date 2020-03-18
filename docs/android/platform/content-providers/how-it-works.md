---
title: 内容提供程序的工作原理
ms.prod: xamarin
ms.assetid: B9E2EF89-7EBE-45F5-1ED9-7D2C70BE792C
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: e61be6f0189eb825c15fd75764a16706e588ebc9
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/10/2020
ms.locfileid: "73020520"
---
# <a name="how-content-providers-work"></a>内容提供程序的工作原理

`ContentProvider` 交互涉及两个类：

- **ContentProvider** &ndash; 实现以标准方式公开一组数据的 API。 主要方法包括 Query、Insert、Update 和 Delete。

- **ContentResolver** &ndash; 与 `ContentProvider` 通信的静态代理，以便从同一应用程序或其他应用程序访问它的数据。

内容提供程序通常由 SQLite 数据库提供支持，但 API 意味着使用代码并不需要知道基础 SQL 的任何信息。 查询是通过 URI 使用常量来引用列名完成的（以减少对基础数据结构的依赖），并返回 `ICursor` 以供使用代码进行循环访问。

## <a name="consuming-a-contentprovider"></a>使用 ContentProvider

`ContentProviders` 通过在发布数据的应用程序的 AndroidManifest.xml  中注册的 URI 公开其功能。 有一项约定，即 URI 和公开的数据列应作为常量提供，这样可以轻松绑定到数据。 Android 的内置 `ContentProviders` 都提供了便利类，其中的常量引用 [`Android.Providers`](xref:Android.Provider) 命名空间中的数据结构。

### <a name="built-in-providers"></a>内置提供程序

Android 通过 `ContentProviders` 提供对各种系统和用户数据的访问权限：

- *Browser* &ndash; 书签和浏览器历史记录（需要权限 `READ_HISTORY_BOOKMARKS` 和/或 `WRITE_HISTORY_BOOKMARKS`）。

- *CallLog* &ndash; 设备最近的来电或呼叫。

- *Contacts* &ndash; 用户的联系人列表中的详细信息，包括联系人、电话、照片和组。

- *MediaStore* &ndash; 用户设备中的内容：音频（唱片集、艺术家、流派、播放列表）、图像（包括缩略图）和视频。

- *Settings* &ndash; 系统范围内设备设置和首选项。

- *UserDictionary* &ndash; 用于预测文本输入的用户定义字典的内容。

- *Voicemail* &ndash; 语音邮件历史记录。

## <a name="classes-overview"></a>类概述

使用 `ContentProvider` 时用到的主要类如下：

[![内容提供程序应用程序和使用应用程序交互的类图](how-it-works-images/classdiagram1.png)](how-it-works-images/classdiagram1.png#lightbox)

在此图中，`ContentProvider` 实现了查询，并注册了其他应用程序用来查找数据的 URI。 `ContentResolver` 充当 `ContentProvider` 的“代理”（Query、Insert、Update 和 Delete方法）。 `SQLiteOpenHelper` 包含 `ContentProvider` 使用的数据，但它不直接公开给使用应用。
`CursorAdapter` 传递由 `ContentResolver` 返回的游标，以显示在 `ListView` 中。 `UriMatcher` 是在处理查询时解析 URI 的帮助程序类。

每个类的用途如下所述：

- **ContentProvider** &ndash; 实现此抽象类的方法可以公开数据。 此 API 通过添加到类定义的 URI 特性，提供给其他类和应用程序。

- **SQLiteOpenHelper** &ndash; 帮助实现 `ContentProvider` 公开的 SQLite 数据存储。

- **UriMatcher** &ndash; 使用 `ContentProvider` 实现中的 `UriMatcher` 来帮助管理用于查询内容的 URI。

- **ContentResolver** &ndash; 使用代码使用 `ContentResolver` 来访问 `ContentProvider` 实例。 这两个类一起处理进程内通信问题，以便能够在应用程序之间轻松共享数据。 使用代码绝不会显式创建 `ContentProvider` 类；相反，可以根据 `ContentProvider` 应用程序公开的 URI 创建游标，从而访问数据。

- **CursorAdapter** &ndash; 使用 `CursorAdapter` 或 `SimpleCursorAdapter` 来显示通过 `ContentProvider` 访问的数据。

使用 `ContentProvider` API，使用者可以对数据执行各种操作，例如：

- 查询数据，以返回列表或各个记录。
- 修改各个记录。
- 添加新记录。
- 删除记录。

本文档中的一个示例使用系统提供的 `ContentProvider`，另一个简单的只读示例实现自定义 `ContentProvider`。
