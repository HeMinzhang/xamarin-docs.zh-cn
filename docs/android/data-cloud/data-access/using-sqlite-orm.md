---
title: 结合使用 SQLite.NET 和 Android
description: SQLite.NET PCL NuGet 库为 Xamarin Android 应用提供了简单的数据访问机制。
ms.prod: xamarin
ms.assetid: 3447B7EE-A320-489E-AF02-E5721097760A
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/18/2018
ms.openlocfilehash: 65c8466e2649c6d48cf5651f25d14c073dbcf5e3
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/06/2019
ms.locfileid: "70754435"
---
# <a name="using-sqlitenet-with-android"></a>结合使用 SQLite.NET 和 Android

Xamarin 推荐的 SQLite.NET 库是一个非常基本的 ORM，可让你轻松地在 Android 设备上的本地 SQLite 数据库中存储和检索对象。 ORM 代表对象关系映射&ndash;一个 API，该 API 允许您在不编写 SQL 语句的情况下保存和检索数据库中的 "对象"。

若要在 Xamarin 应用中包括 SQLite.NET 库，请将以下 NuGet 包添加到项目中：

- **包名称：** sqlite 网络-pcl
- **创建者：** Frank A. Krueger
- **ID：** sqlite net pcl
- **Url:** [nuget.org/packages/sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)

[![SQLite.NET NuGet 包](using-sqlite-orm-images/image1a-sml.png "SQLite.NET NuGet 包")](using-sqlite-orm-images/image1a.png#lightbox)

> [!TIP]
> 有许多不同的 SQLite 包可用，请确保选择正确的 SQLite 包（它可能不是搜索的最大结果）。

获得可用的 SQLite.NET 库后，请执行以下三个步骤以使用它来访问数据库：

1. **添加 using 语句**将以下语句添加到需要C#数据访问的文件中： &ndash;

    ```csharp
    using SQLite;
    ```

2. **创建空白数据库**&ndash;可以通过将文件路径传递给 SQLiteConnection 类构造函数来创建数据库引用。 如果文件已存在，则不需要检查是否已&ndash;存在该文件。如果需要，将自动创建该文件，否则将打开现有的数据库文件。 应按照本文档前面讨论的规则确定变量：`dbPath`

    ```csharp
    var db = new SQLiteConnection (dbPath);
    ```

3. **保存数据**&ndash;创建 SQLiteConnection 对象后，将通过调用其方法来执行数据库命令，如 CreateTable 和 Insert，如下所示：

    ```csharp
    db.CreateTable<Stock> ();
    db.Insert (newStock); // after creating the newStock object
    ```

4. **检索数据**&ndash;若要检索对象（或对象列表），请使用以下语法：

    ```csharp
    var stock = db.Get<Stock>(5); // primary key id of 5
    var stockList = db.Table<Stock>();
    ```

## <a name="basic-data-access-sample"></a>基本数据访问示例

在 Android 上运行时，此文档的*DataAccess_Basic*示例代码如下所示。 此代码演示如何执行简单的 SQLite.NET 操作并在应用程序的主窗口中以文本形式显示结果。

**Android**

![Android SQLite.NET 示例](using-sqlite-orm-images/image3.png "Android SQLite.NET 示例")

下面的代码示例演示使用 SQLite.NET 库来封装基础数据库访问的整个数据库交互。
它显示：

1. 创建数据库文件

2. 通过创建对象并保存来插入一些数据

3. 查询数据

需要包含以下命名空间：

```csharp
using SQLite; // from the github SQLite.cs class
```

最后一个要求您已将 SQLite 添加到您的项目中。 请注意，SQLite 数据库表是通过将属性添加到类（ `Stock`类）而不是 CREATE TABLE 命令来定义的。

```csharp
[Table("Items")]
public class Stock {
    [PrimaryKey, AutoIncrement, Column("_id")]
    public int Id { get; set; }
    [MaxLength(8)]
    public string Symbol { get; set; }
}
public static void DoSomeDataAccess () {
       Console.WriteLine ("Creating database, if it doesn't already exist");
   string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "ormdemo.db3");
   var db = new SQLiteConnection (dbPath);
   db.CreateTable<Stock> ();
   if (db.Table<Stock> ().Count() == 0) {
        // only insert the data if it doesn't already exist
        var newStock = new Stock ();
        newStock.Symbol = "AAPL";
        db.Insert (newStock);
        newStock = new Stock ();
        newStock.Symbol = "GOOG";
        db.Insert (newStock);
        newStock = new Stock ();
        newStock.Symbol = "MSFT";
        db.Insert (newStock);
    }
    Console.WriteLine("Reading data");
    var table = db.Table<Stock> ();
    foreach (var s in table) {
        Console.WriteLine (s.Id + " " + s.Symbol);
    }
}
```

如果在`[Table]`未指定表名称参数的情况下使用属性，则会导致基础数据库表具有与类相同的名称（在本例中为 "Stock"）。 如果直接针对数据库编写 SQL 查询，而不是使用 ORM 数据访问方法，则实际的表名非常重要。 同样， `[Column("_id")]`特性是可选的，如果不存在，则会将列添加到与类中的属性同名的表中。

## <a name="sqlite-attributes"></a>SQLite 特性

可应用于类以控制它们在基础数据库中的存储方式的常用特性包括：

- **[PrimaryKey]** &ndash;此特性可应用于整数属性，以强制其成为基础表的主键。 不支持组合主键。

- **[自动增量]** &ndash;此属性将导致插入到数据库中的每个新对象的整数属性值为自动增量

- **[列（名称）]** &ndash;提供可选`name`参数将重写基础数据库列的名称（与属性相同）的默认值。

- **[表（名称）]** &ndash;标记类，使其能够存储在基础 SQLite 表中。 指定可选的 name 参数将重写基础数据库表名称（与类名相同）的默认值。

- **[MaxLength （值）]** &ndash;尝试在数据库插入时，限制文本属性的长度。 在插入对象之前，使用代码应进行验证，因为在尝试执行数据库插入或更新操作时，仅 "选中" 此属性。

- **[Ignore]** &ndash;导致 SQLite.NET 忽略此属性。
    这对于类型不能存储在数据库中的属性或无法由 SQLite 自动解析的模型集合的属性特别有用。

- **[唯一]** &ndash;确保基础数据库列中的值是唯一的。

其中的大多数属性是可选的，SQLite 将使用表名和列名的默认值。 应始终指定整数主键，以便可以对数据高效地执行选择和删除查询。

## <a name="more-complex-queries"></a>更复杂的查询

上`SQLiteConnection`的以下方法可用于执行其他数据操作：

- **插入**&ndash;将新的对象添加到数据库。

- **Get&lt;T&gt;尝试使用主键来**检索对象&ndash; 。

- **表&lt;T&gt;返回表中的**所有对象&ndash; 。

- **删除**&ndash;使用对象的主键删除该对象。

- **查询&lt;T&gt;执行SQL查询，** 该查询返回的行数（作为对象）。&ndash;

- **执行**如果你不希望从 SQL `Query`返回行（如插入、更新和删除说明），请使用此方法（而不是）。 &ndash;

### <a name="getting-an-object-by-the-primary-key"></a>通过主键获取对象

SQLite.Net 提供 Get 方法，以根据其主键检索单个对象。

```csharp
var existingItem = db.Get<Stock>(3);
```

### <a name="selecting-an-object-using-linq"></a>使用 Linq 选择对象

返回集合支持`IEnumerable<T>`的方法，以便您可以使用 Linq 查询或排序表的内容。 下面的代码演示了一个示例，该示例使用 Linq 筛选出以字母 "A" 开头的所有条目：

```csharp
var apple = from s in db.Table<Stock>()
    where s.Symbol.StartsWith ("A")
    select s;
Console.WriteLine ("-> " + apple.FirstOrDefault ().Symbol);
```

### <a name="selecting-an-object-using-sql"></a>使用 SQL 选择对象

尽管 SQLite.Net 可以提供对你的数据的基于对象的访问权限，但有时你可能需要执行比 Linq 允许更复杂的查询（或者可能需要更快的性能）。 可以将 SQL 命令与 Query 方法一起使用，如下所示：

```csharp
var stocksStartingWithA = db.Query<Stock>("SELECT * FROM Items WHERE Symbol = ?", "A");
foreach (var s in stocksStartingWithA) {
    Console.WriteLine ("a " + s.Symbol);
}
```

> [!NOTE]
> 直接编写 SQL 语句时，会创建数据库中表和列的名称的依赖关系，这些表和列是从你的类及其属性生成的。 如果在代码中更改这些名称，则必须记住更新所有手动写入的 SQL 语句。

### <a name="deleting-an-object"></a>删除对象

主密钥用于删除行，如下所示：

```csharp
var rowcount = db.Delete<Stock>(someStock.Id); // Id is the primary key
```

你可以检查`rowcount`以确认受影响的行数（在此示例中已删除）。

## <a name="using-sqlitenet-with-multiple-threads"></a>将 SQLite.NET 用于多个线程

SQLite 支持三种不同的线程模式：*单线程*、*多线程*和*序列化*。 如果要从多个线程访问数据库，而不受任何限制，则可以将 SQLite 配置为使用**序列化**的线程模式。 在应用程序的早期设置此模式很重要（例如，在`OnCreate`方法的开头）。

若要更改线程模式，请`SqliteConnection.SetConfig`调用。 例如，下面这行代码将为**序列化**模式配置 SQLite：

```csharp
using using Mono.Data.Sqlite;
...
SqliteConnection.SetConfig(SQLiteConfig.Serialized);
```

Android 版本 SQLite 有一个限制，需要执行几个步骤。 如果调用`SqliteConnection.SetConfig`产生了 SQLite 异常`library used incorrectly`（如），则必须使用以下解决方法：

1. 链接到本机**libsqlite.so**库，以便`sqlite3_shutdown`应用程序可使用和`sqlite3_initialize` api：

    ```csharp
    [DllImport("libsqlite.so")]
    internal static extern int sqlite3_shutdown();

    [DllImport("libsqlite.so")]
    internal static extern int sqlite3_initialize();
    ```

2. 在`OnCreate`方法的最开始处，添加以下代码以关闭 SQLite，将其配置为**序列化**模式，然后重新初始化 SQLite：

    ```csharp
    using using Mono.Data.Sqlite;
    ...
    sqlite3_shutdown();
    SqliteConnection.SetConfig(SQLiteConfig.Serialized);
    sqlite3_initialize();
    ```

此解决方法也适用于`Mono.Data.Sqlite`库。 有关 SQLite 和多线程处理的详细信息，请参阅[sqlite 和多线程](https://www.sqlite.org/threadsafe.html)。

## <a name="related-links"></a>相关链接

- [DataAccess Basic （示例）](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess 高级（示例）](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Xamarin. 窗体数据访问](~/xamarin-forms/data-cloud/data/databases.md)
