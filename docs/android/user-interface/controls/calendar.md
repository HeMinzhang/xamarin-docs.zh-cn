---
title: Calendar
ms.prod: xamarin
ms.assetid: 78666541-CA14-4CD8-A94A-A9621C57813E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: d48af16f50c5a4482342d323b5c73b9c237cc83a
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/04/2018
ms.locfileid: "30767971"
---
# <a name="calendar"></a>Calendar


## <a name="calendar-api"></a>日历 API

一组新的日历 Android 4 中引入的 Api 支持设计为读取或写入到日历提供程序的数据应用程序。 这些 Api 支持丰富的日历数据，包括能够读取和写入事件、 与会者和提醒交互选项。 通过应用程序中使用的日历提供程序，通过 API 添加的数据将出现在随 Android 4 提供内置的日历应用。


## <a name="adding-permissions"></a>添加权限

在使用你的应用程序中的新日历 Api，你需要执行操作的第一个操作是将适当的权限添加到 Android 清单。 你需要添加的权限`android.permisson.READ_CALENDAR`和`android.permission.WRITE_CALENDAR`，具体取决于是否你读取和/或写入日历数据。


## <a name="using-the-calendar-contract"></a>使用日历协定

一旦你设置权限，你可以通过使用日历数据进行交互`CalendarContract`类。 此类提供的数据模型，在它们与日历提供程序交互时，可以使用应用程序。 `CalendarContract`允许应用程序到日历实体，如日历和事件解决 Uri。 它还提供一种方法与在每个实体，如日历的名称和 ID，或某一事件的开始和结束日期的各个字段进行交互。

让我们看一个示例，它使用日历 API。 在此示例中，我们将查看如何枚举日历和其事件，以及如何将新事件添加到日历。


## <a name="listing-calendars"></a>列出日历

首先，让我们看一下如何枚举已注册的日历应用中的日历。 若要执行此操作，我们可以实例化`CursorLoader`。 在 Android 3.0 (API 11) 中引入`CursorLoader`是首选的方法来使用`ContentProvider`。 在最低限度上，我们将需要指定日历和我们想要返回; 的列的内容 Uri此列规范称为_投影_。

调用`CursorLoader.LoadInBackground`方法，我们可以查询的内容提供程序数据，如日历提供程序。
`LoadInBackground` 执行实际加载操作并返回`Cursor`使用查询的结果。

`CalendarContract`可帮助我们指定这两个内容`Uri`和投影。 若要获取的内容`Uri`用于查询日历，我们只是可以用`CalendarContract.Calendars.ContentUri`如下属性：

```csharp
var calendarsUri = CalendarContract.Calendars.ContentUri;
```

使用`CalendarContract`若要指定的日历我们想要的列也同样简单。 我们只需添加中的字段`CalendarContract.Calendars.InterfaceConsts`到一个数组的类。 例如，下面的代码包括日历的 ID、 显示名称和帐户名称：

```csharp
string[] calendarsProjection = {
    CalendarContract.Calendars.InterfaceConsts.Id,
    CalendarContract.Calendars.InterfaceConsts.CalendarDisplayName,
    CalendarContract.Calendars.InterfaceConsts.AccountName
};
```

`Id`务必包括如果正在使用`SimpleCursorAdapter`将数据绑定到 UI，正如我们很快将看到。 内容 Uri 和就地投影，我们实例化`CursorLoader`并调用`CursorLoader.LoadInBackground`方法以返回日历数据的游标，如下所示：

```csharp
var loader = new CursorLoader(this, calendarsUri, calendarsProjection, null, null, null);
var cursor = (ICursor)loader.LoadInBackground();

```

此示例中的 UI 包含`ListView`，与列表表示单个日历中每个项。 下面的 XML 演示包含的标记`ListView`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
android:orientation="vertical"
android:layout_width="fill_parent"
android:layout_height="fill_parent">
  <ListView
    android:id="@android:id/android:list"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content" />
</LinearLayout>
```

此外，我们需要指定每个列表项，我们将放在单独的 XML 文件，如下所示的 UI:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
android:orientation="vertical"
android:layout_width="fill_parent"
android:layout_height="wrap_content">
  <TextView android:id="@+id/calDisplayName"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:textSize="16dip" />
  <TextView android:id="@+id/calAccountName"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:textSize="12dip" />
</LinearLayout>
```

从现在开始，它是用于将数据从光标绑定到 UI 的只是正常 Android 代码。 我们将使用`SimpleCursorAdapter`，如下所示：

```csharp
string[] sourceColumns = {
    CalendarContract.Calendars.InterfaceConsts.CalendarDisplayName,
    CalendarContract.Calendars.InterfaceConsts.AccountName };

int[] targetResources = {
    Resource.Id.calDisplayName, Resource.Id.calAccountName };      

SimpleCursorAdapter adapter = new SimpleCursorAdapter (this,
    Resource.Layout.CalListItem, cursor, sourceColumns, targetResources);

ListAdapter = adapter;
```

在上面的代码中，适配器采用中指定的列`sourceColumns`数组并将其写入到中的用户界面元素`targetResources`游标中每个日历项的数组。 此处使用的活动是的一个子类`ListActivity`; 它包括`ListAdapter`为我们设置该适配器的属性。

下面是屏幕快照显示中显示的日历信息包含最终结果， `ListView`:

[![CalendarDemo 运行在模拟器中，显示两个日历项](calendar-images/11-calendar.png)](calendar-images/11-calendar.png#lightbox)



## <a name="listing-calendar-events"></a>列表日历事件

下一步让我们看一下如何为给定日历枚举事件。
依赖于上面的示例中，我们将介绍当用户选择的日历之一的事件的列表。 因此，我们将需要处理前面的代码中的项选择：

```csharp
ListView.ItemClick += (sender, e) => {
    int i = (e as ItemEventArgs).Position;

    cursor.MoveToPosition(i);
    int calId =
        cursor.GetInt (cursor.GetColumnIndex (calendarsProjection [0]));

    var showEvents = new Intent(this, typeof(EventListActivity));
    showEvents.PutExtra("calId", calId);
    StartActivity(showEvents);
};
```

在此代码中，我们正在创建试图打开类型活动的`EventListActivity`，传递中意图的日历的 ID。 我们将需要知道哪些日历事件的查询的 ID。 在`EventListActivity`的`OnCreate`方法，我们可以检索 ID`Intent`如下所示：

```csharp
_calId = Intent.GetIntExtra ("calId", -1);
```

现在让我们为此查询事件日历 id。 进程的事件的查询是类似于我们查询的更早版本，日历的列表的方法仅我们将使用此时间`CalendarContract.Events`类。 下面的代码创建查询来检索事件：

```csharp
var eventsUri = CalendarContract.Events.ContentUri;

string[] eventsProjection = {
    CalendarContract.Events.InterfaceConsts.Id,
    CalendarContract.Events.InterfaceConsts.Title,
    CalendarContract.Events.InterfaceConsts.Dtstart
};

var loader = new CursorLoader(this, eventsUri, eventsProjection,
                   String.Format ("calendar_id={0}", _calId), null, "dtstart ASC");
var cursor = (ICursor)loader.LoadInBackground();
```

在此代码中，我们首先获取内容`Uri`来自事件`CalendarContract.Events.ContentUri`属性。 然后我们指定我们想要检索 eventsProjection 数组中的事件列。
最后，我们进行实例化`CursorLoader`与此信息和调用加载程序`LoadInBackground`方法以返回`Cursor`与事件数据。

若要在 UI 中显示的事件数据，我们可以使用标记和代码只需像我们之前以显示日历的列表。 同样，我们使用`SimpleCursorAdapter`要绑定到数据`ListView`中下面的代码所示：

```csharp
string[] sourceColumns = {
    CalendarContract.Events.InterfaceConsts.Title,
    CalendarContract.Events.InterfaceConsts.Dtstart };

int[] targetResources = {
    Resource.Id.eventTitle,
    Resource.Id.eventStartDate };

var adapter = new SimpleCursorAdapter (this, Resource.Layout.EventListItem,
    cursor, sourceColumns, targetResources);

adapter.ViewBinder = new ViewBinder ();       
ListAdapter = adapter;
```

此代码和显示日历列表使用前面的代码之间的主要区别是使用`ViewBinder`，这在行上设置：

```csharp
adapter.ViewBinder = new ViewBinder ();
```

`ViewBinder`类使我们可以更进一步地控制如何我们将值绑定到视图。 在这种情况下，我们使用它来从毫秒的事件开始时间转换为日期字符串，如以下实现中所示：

```csharp
class ViewBinder : Java.Lang.Object, SimpleCursorAdapter.IViewBinder
{    
    public bool SetViewValue (View view, Android.Database.ICursor cursor,
        int columnIndex)
    {
        if (columnIndex == 2) {
            long ms = cursor.GetLong (columnIndex);

            DateTime date = new DateTime (1970, 1, 1, 0, 0, 0,
                DateTimeKind.Utc).AddMilliseconds (ms).ToLocalTime ();

            TextView textView = (TextView)view;
            textView.Text = date.ToLongDateString ();

            return true;
        }
        return false;
    }    
}
```

此时将显示的事件的列表，如下所示：

[![显示三个日历事件的示例应用程序的屏幕截图](calendar-images/12-events.png)](calendar-images/12-events.png#lightbox)



## <a name="adding-a-calendar-event"></a>添加一个日历事件

我们已了解如何读取日历数据。 现在让我们了解如何将事件添加到日历。 为此，请务必包括`android.permission.WRITE_CALENDAR`我们前面所述的权限。 若要添加到日历事件，我们将：

1.  创建`ContentValues`实例。
1.  使用中的密钥`CalendarContract.Events.InterfaceConsts`类来填充`ContentValues`实例。
1.  设置时区事件的开始和结束时间。
1.  使用`ContentResolver`将事件数据插入日历。


下面的代码演示了这些步骤：

```csharp
ContentValues eventValues = new ContentValues ();

eventValues.Put (CalendarContract.Events.InterfaceConsts.CalendarId,
    _calId);
eventValues.Put (CalendarContract.Events.InterfaceConsts.Title,
    "Test Event from M4A");
eventValues.Put (CalendarContract.Events.InterfaceConsts.Description,
    "This is an event created from Xamarin.Android");
eventValues.Put (CalendarContract.Events.InterfaceConsts.Dtstart,
    GetDateTimeMS (2011, 12, 15, 10, 0));
eventValues.Put (CalendarContract.Events.InterfaceConsts.Dtend,
    GetDateTimeMS (2011, 12, 15, 11, 0));

eventValues.Put(CalendarContract.Events.InterfaceConsts.EventTimezone,
    "UTC");
eventValues.Put(CalendarContract.Events.InterfaceConsts.EventEndTimezone,
    "UTC");

var uri = ContentResolver.Insert (CalendarContract.Events.ContentUri,
    eventValues);
```

请注意，如果我们未设置时区，类型的异常`Java.Lang.IllegalArgumentException`将引发。 由于事件时间值必须在距离 epoch 的以毫秒为单位表示，我们创建`GetDateTimeMS`方法 (在`EventListActivity`) 将我们日期规范转换为毫秒格式：

```csharp
long GetDateTimeMS (int yr, int month, int day, int hr, int min)
{
    Calendar c = Calendar.GetInstance (Java.Util.TimeZone.Default);

    c.Set (Java.Util.CalendarField.DayOfMonth, 15);
    c.Set (Java.Util.CalendarField.HourOfDay, hr);
    c.Set (Java.Util.CalendarField.Minute, min);
    c.Set (Java.Util.CalendarField.Month, Calendar.December);
    c.Set (Java.Util.CalendarField.Year, 2011);

    return c.TimeInMillis;
}
```

如果我们 UI 中的事件列表中添加一个按钮并运行上面的代码中的按钮单击事件处理程序，添加到日历事件并将其更新我们的列表中，如下所示：

[![使用日历事件后面跟着添加示例事件按钮的示例应用程序的屏幕截图](calendar-images/13.png)](calendar-images/13.png#lightbox)

如果我们打开日历应用，我们将看到事件将被写入存在以及：

[![显示所选的日历事件的日历应用的屏幕快照](calendar-images/14.png)](calendar-images/14.png#lightbox)

如你所见，Android 将允许功能强大且易于访问，以检索和保存日历数据，允许将日历功能无缝集成的应用程序。


## <a name="related-links"></a>相关链接

- [日历演示 （示例）](https://developer.xamarin.com/samples/CalendarDemo/)
- [引入冰激凌德桑威奇](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 平台](http://developer.android.com/sdk/android-4.0.html)
