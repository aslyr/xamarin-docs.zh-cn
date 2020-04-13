---
title: Xamarin.Forms 本地数据库
description: Xamarin.Forms 支持使用 SQLite 数据库引擎的数据库驱动型应用程序，如此即可在共享代码中加载和保存对象。 本文介绍 Xamarin.Forms 应用程序如何使用 SQLite.Net 读取和写入数据到本地 SQLite 数据库。
ms.prod: xamarin
ms.assetid: F687B24B-7DF0-4F8E-A21A-A9BB507480EB
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 12/05/2019
ms.openlocfilehash: 2369afc693940d83971a43877da363e2c66b31b2
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/09/2020
ms.locfileid: "80992399"
---
# <a name="xamarinforms-local-databases"></a>Xamarin.Forms 本地数据库

[![下载示例](~/media/shared/download.png)下载示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/todo)

SQLite 数据库引擎允许 Xamarin.Forms 应用程序在共享代码中加载和保存数据对象。 示例应用程序使用 SQLite 数据库表来存储 todo 项。 本文介绍如何在共享代码中使用SQLite.Net在本地数据库中存储和检索信息。

[![iOS 和 Android 上的 Todolist 应用的屏幕截图](databases-images/todo-list-sml.png)](databases-images/todo-list.png#lightbox "在 iOS 和安卓上的待办事项应用")

通过以下步骤将SQLite.NET集成到移动应用中：

1. [安装 NuGet 包](#install-the-sqlite-nuget-package)。
1. [配置常量](#configure-app-constants)。
1. [创建数据库访问类](#create-a-database-access-class)。
1. [访问 Xamarin.forms 中的数据](#access-data-in-xamarinforms)。
1. [高级配置](#advanced-configuration)。

## <a name="install-the-sqlite-nuget-package"></a>安装 SQLite NuGet 软件包

使用 NuGet 包管理器搜索**sqlite-net-pcl**并将最新版本添加到共享代码项目。

许多 NuGet 包都有着类似的名称。 正确的包具有以下属性：

- **创建者：** Frank A. Krueger
- **ID：** sqlite-net-pcl
- **NuGet 链接：** [sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)

> [!NOTE]
> 不管包名称，即便在 .NET Standard 项目中也使用 sqlite-net-pcl NuGet 包****。

## <a name="configure-app-constants"></a>配置应用常量

示例项目包括一个**提供**常见配置数据的Constants.cs文件：

```csharp
public static class Constants
{
    public const string DatabaseFilename = "TodoSQLite.db3";

    public const SQLite.SQLiteOpenFlags Flags =
        // open the database in read/write mode
        SQLite.SQLiteOpenFlags.ReadWrite |
        // create the database if it doesn't exist
        SQLite.SQLiteOpenFlags.Create |
        // enable multi-threaded database access
        SQLite.SQLiteOpenFlags.SharedCache;

    public static string DatabasePath
    {
        get
        {
            var basePath = Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData);
            return Path.Combine(basePath, DatabaseFilename);
        }
    }
}
```

常量文件指定用于初始`SQLiteOpenFlag`化数据库连接的默认枚举值。 `SQLiteOpenFlag`枚举支持以下值：

- `Create`：如果数据库文件不存在，连接将自动创建它。
- `FullMutex`：连接在序列化线程模式下打开。
- `NoMutex`：连接在多线程模式下打开。
- `PrivateCache`：连接不会参与共享缓存，即使它已启用。
- `ReadWrite`：连接可以读取和写入数据。
- `SharedCache`：如果连接已启用，则参与共享缓存。
- `ProtectionComplete`：文件已加密，在设备锁定时无法访问。
- `ProtectionCompleteUnlessOpen`：文件被加密，直到它被打开，但随后可以访问，即使用户锁定设备。
- `ProtectionCompleteUntilFirstUserAuthentication`：文件加密，直到用户启动并解锁设备。
- `ProtectionNone`：数据库文件未加密。

您可能需要根据数据库的使用方式指定不同的标志。 有关 的详细信息`SQLiteOpenFlags`，请参阅在sqlite.org[打开新的数据库连接](https://www.sqlite.org/c3ref/open.html)。

## <a name="create-a-database-access-class"></a>创建数据库访问类

数据库包装类从应用的其余部分抽象数据访问层。 此类集中查询逻辑并简化数据库初始化管理，从而更轻松地随着应用的增长重构或扩展数据操作。 "Todo"应用为此定义了`TodoItemDatabase`类。

### <a name="lazy-initialization"></a>延迟初始化

使用`TodoItemDatabase`.NET`Lazy`类延迟数据库的初始化，直到第一次访问数据库。 使用延迟初始化可防止数据库加载过程延迟应用启动。 有关详细信息，请参阅[延迟&lt;T&gt;类](xref:System.Lazy`1)。

```csharp
public class TodoItemDatabase
{
    static readonly Lazy<SQLiteAsyncConnection> lazyInitializer = new Lazy<SQLiteAsyncConnection>(() =>
    {
        return new SQLiteAsyncConnection(Constants.DatabasePath, Constants.Flags);
    });

    static SQLiteAsyncConnection Database => lazyInitializer.Value;
    static bool initialized = false;

    public TodoItemDatabase()
    {
        InitializeAsync().SafeFireAndForget(false);
    }

    async Task InitializeAsync()
    {
        if (!initialized)
        {
            if (!Database.TableMappings.Any(m => m.MappedType.Name == typeof(TodoItem).Name))
            {
                await Database.CreateTablesAsync(CreateFlags.None, typeof(TodoItem)).ConfigureAwait(false);
                initialized = true;
            }
        }
    }

    //...
}
```

数据库连接是一个静态字段，可确保单个数据库连接用于应用的生命周期。 与在单个应用会话期间多次打开和关闭连接相比，使用持久静态连接可提供更好的性能。

该方法`InitializeAsync`负责检查是否已存在用于存储`TodoItem`对象的表。 如果表不存在，则此方法会自动创建它。

### <a name="the-safefireandforget-extension-method"></a>安全消防和忘记扩展方法

实例化`TodoItemDatabase`类时，它必须初始化数据库连接，这是一个异步过程。 但是：

- 类构造函数不能异步。
- 不等待的异步方法不会引发异常。
- 使用`Wait`方法阻止线程_并_吞下异常。

为了启动异步初始化，避免阻止执行，并有机会捕获异常，示例应用程序使用名为`SafeFireAndForget`的扩展方法。 扩展`SafeFireAndForget`方法为`Task`类提供了其他功能：

```csharp
public static class TaskExtensions
{
    // NOTE: Async void is intentional here. This provides a way
    // to call an async method from the constructor while
    // communicating intent to fire and forget, and allow
    // handling of exceptions
    public static async void SafeFireAndForget(this Task task,
        bool returnToCallingContext,
        Action<Exception> onException = null)
    {
        try
        {
            await task.ConfigureAwait(returnToCallingContext);
        }

        // if the provided action is not null, catch and
        // pass the thrown exception
        catch (Exception ex) when (onException != null)
        {
            onException(ex);
        }
    }
}
```

该方法`SafeFireAndForget`等待提供`Task`对象的异步执行，并允许您附加`Action`引发异常时调用的 。

有关详细信息，请参阅[基于任务的异步模式 （TAP）。](https://docs.microsoft.com/dotnet/standard/asynchronous-programming-patterns/task-based-asynchronous-pattern-tap)

### <a name="data-manipulation-methods"></a>数据操作方法

该`TodoItemDatabase`类包括四种类型的数据操作的方法：创建、读取、编辑和删除。 SQLite.NET库提供了一个简单的对象关系图 （ORM），允许您存储和检索对象而无需编写 SQL 语句。

```csharp
public class TodoItemDatabase {

    // ...

    public Task<List<TodoItem>> GetItemsAsync()
    {
        return Database.Table<TodoItem>().ToListAsync();
    }

    public Task<List<TodoItem>> GetItemsNotDoneAsync()
    {
        // SQL queries are also possible
        return Database.QueryAsync<TodoItem>("SELECT * FROM [TodoItem] WHERE [Done] = 0");
    }

    public Task<TodoItem> GetItemAsync(int id)
    {
        return Database.Table<TodoItem>().Where(i => i.ID == id).FirstOrDefaultAsync();
    }

    public Task<int> SaveItemAsync(TodoItem item)
    {
        if (item.ID != 0)
        {
            return Database.UpdateAsync(item);
        }
        else
        {
            return Database.InsertAsync(item);
        }
    }

    public Task<int> DeleteItemAsync(TodoItem item)
    {
        return Database.DeleteAsync(item);
    }
}
```

## <a name="access-data-in-xamarinforms"></a>访问 Xamarin.窗体中的数据

Xamarin.Forms`App`类公开类的`TodoItemDatabase`实例：

```csharp
static TodoItemDatabase database;
public static TodoItemDatabase Database
{
    get
    {
        if (database == null)
        {
            database = new TodoItemDatabase();
        }
        return database;
    }
}
```

此属性允许 Xamarin.Forms 组件调用`Database`实例上的数据检索和操作方法以响应用户交互。 例如：

```csharp
var saveButton = new Button { Text = "Save" };
saveButton.Clicked += async (sender, e) =>
{
    var todoItem = (TodoItem)BindingContext;
    await App.Database.SaveItemAsync(todoItem);
    await Navigation.PopAsync();
};
```

## <a name="advanced-configuration"></a>高级配置

SQLite 提供了一个强大的 API，其功能比本文和示例应用中介绍的功能更多。 以下各节介绍对可伸缩性很重要的功能。

有关详细信息，请参阅有关sqlite.org[的 SQLite 文档](https://www.sqlite.org/docs.html)。

### <a name="write-ahead-logging"></a>提前写入日志记录

默认情况下，SQLite 使用传统的回滚日志。 未更改的数据库内容的副本将写入单独的回滚文件，然后更改直接写入数据库文件。 删除回滚日志时，将发生 COMMIT。

先写入前记录 （WAL） 先将更改写入单独的 WAL 文件。 在 WAL 模式下，COMMIT 是一种特殊记录，追加到 WAL 文件，允许在单个 WAL 文件中发生多个事务。 WAL 文件在称为_检查点_的特殊操作中合并回数据库文件。

对于本地数据库，WAL 可以更快，因为读取器和编写器不会相互阻止，从而允许读取和写入操作是并发的。 但是，WAL 模式不允许更改_页面大小_，向数据库添加其他文件关联，并添加额外的_检查点_操作。

要在SQLite.NET中启用 WAL，`EnableWriteAheadLoggingAsync`请调用`SQLiteAsyncConnection`实例上的方法：

```csharp
await Database.EnableWriteAheadLoggingAsync();
```

有关详细信息，请参阅 sqlite.org 上的[SQLite 提前写入日志记录](https://www.sqlite.org/wal.html)。

### <a name="copying-a-database"></a>复制数据库

在以下几种情况下，可能需要复制 SQLite 数据库：

- 数据库已随应用程序一起提供，但必须复制或移动到移动设备上的可写入存储。
- 您需要进行数据库的备份或复制。
- 您需要对数据库文件进行版本控制、移动或重命名。

通常，移动、重命名或复制数据库文件与任何其他文件类型的过程相同，并有几个其他注意事项：

- 在尝试移动数据库文件之前，应关闭所有数据库连接。
- 如果使用[提前写入日志记录](#write-ahead-logging)，SQLite 将创建共享内存访问 （.shm） 文件和（提前写入日志） （.wal） 文件。 请确保对这些文件应用任何更改。

有关详细信息，请参阅[Xamarin.Forms 中的文件处理](~/xamarin-forms/data-cloud/data/files.md)。

## <a name="related-links"></a>相关链接

- [Todo 示例应用程序](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/todo)
- [SQLite.NET NuGet 包](https://www.nuget.org/packages/sqlite-net-pcl/)
- [SQLite 文档](https://www.sqlite.org/docs.html)
- [将 SQLite 与安卓一起使用](~/android/data-cloud/data-access/using-sqlite-orm.md)
- [将 SQLite 与 iOS 一起使用](~/ios/data-cloud/data/using-sqlite-orm.md)
- [基于任务的异步模式 （TAP）](https://docs.microsoft.com/dotnet/standard/asynchronous-programming-patterns/task-based-asynchronous-pattern-tap)
- [惰&lt;性&gt;T 类](xref:System.Lazy`1)
