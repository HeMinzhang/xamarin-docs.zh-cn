---
title: 在 Xamarin 中使用 ListView
description: ListView 是 Android 应用程序的重要 UI 组件;它用于从菜单选项的简短列表到长列表的联系人或 internet 收藏夹。 它提供了一种简单的方法来显示可使用内置样式或广泛自定义的行的滚动列表。
ms.prod: xamarin
ms.assetid: C2BA2705-9B20-01C2-468D-860BDFEDC157
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/25/2018
ms.openlocfilehash: 5c9a11073a7e65d90e0776d0b43c9e3bd100a10d
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/06/2019
ms.locfileid: "70758645"
---
# <a name="xamarinandroid-listview"></a>Xamarin Android ListView

_ListView 是 Android 应用程序的重要 UI 组件;它用于从菜单选项的简短列表到长列表的联系人或 internet 收藏夹。它提供了一种简单的方法来显示可使用内置样式或广泛自定义的行的滚动列表。_

## <a name="overview"></a>概述

在 Android 应用程序的最基本构建基块中包含了列表视图和适配器。 `ListView`类提供了一种灵活的方式来显示数据，无论它是一个短菜单还是长滚动列表。 它提供快速滚动、索引以及单个或多个选择的可用性功能，以帮助您为应用程序生成移动友好的用户界面。 `ListView` 实例需要*适配器*，以向它馈送行视图中包含的数据。

本指南说明如何实现`ListView`和中的各种`Adapter`类。 它还演示了如何自定义的外观`ListView`，并讨论了行重复使用以减少内存消耗的重要性。 此外，还讨论了如何影响`ListView`和`Adapter`使用活动生命周期。 如果正在使用 Xamarin 进行跨平台应用`ListView`程序，则该控件的结构类似于 ios `UITableView` （ `UITableViewSource`Android `Adapter`类似于）。

首先，简要教程介绍了基本`ListView`的代码示例。 接下来，还提供了更多高级主题的链接， `ListView`以帮助你在实际应用中使用。

> [!NOTE]
> 小组件是的更高级和灵活的`ListView`版本。 `RecyclerView` 由于`RecyclerView`设计为作为`ListView` （和`GridView`）的后续版本`ListView` ，因此我们建议你使用`RecyclerView`而不是进行新的应用程序开发。 有关详细信息，请参阅[RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)。

## <a name="listview-tutorial"></a>ListView 教程

[`ListView`](xref:Android.Widget.ListView)是[`ViewGroup`](xref:Android.Views.ViewGroup)
这会创建可滚动项的列表。 使用将列表项自动插入列表中[`IListAdapter`](xref:Android.Widget.IListAdapter)。

在本教程中，您将创建从字符串数组中读取的国家/地区名称的可滚动列表。 选择列表项后，toast 消息会显示项在列表中的位置。

启动名为**HelloListView**的新项目。

创建名为**list_item**的 xml 文件，并将其保存在**资源/布局/** 文件夹中。 插入以下内容：

```xml
<?xml version="1.0" encoding="utf-8"?>
<TextView xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:padding="10dp"
    android:textSize="16sp">
</TextView>
```

此文件定义要置于中[`ListView`](xref:Android.Widget.ListView)的每个项的布局。

打开`MainActivity.cs`并修改类以扩展[`ListActivity`](xref:Android.App.ListActivity) （而不是[`Activity`](xref:Android.App.Activity)）：

```csharp
public class MainActivity : ListActivity
{
```

为[`OnCreate()`](xref:Android.App.Activity.OnCreate*)）方法插入以下代码：

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    ListAdapter = new ArrayAdapter<string> (this, Resource.Layout.list_item, countries);

    ListView.TextFilterEnabled = true;

    ListView.ItemClick += delegate (object sender, AdapterView.ItemClickEventArgs args)
    {
        Toast.MakeText(Application, ((TextView)args.View).Text, ToastLength.Short).Show();
    };
}
```

请注意，这不会加载活动的布局文件（通常使用[`SetContentView(int)`](xref:Android.App.Activity.SetContentView*)来执行此操作）。
相反，设置[`ListAdapter`](xref:Android.App.ListActivity.ListAdapter)
属性自动添加[`ListView`](xref:Android.Widget.ListView)
填充整个屏幕[`ListActivity`](xref:Android.App.ListActivity)。
此方法采用一个[`ArrayAdapter<T>`](xref:Android.Widget.ArrayAdapter`1)，它管理将放[`ListView`](xref:Android.Widget.ListView)入中的列表项的数组。
此[`ArrayAdapter<T>`](xref:Android.Widget.ArrayAdapter`1)
构造函数获取应用[`Context`](xref:Android.Content.Context)程序、每个列表项的布局说明（在上一步中创建） `T[]`以及或[`Java.Util.IList<T>`](xref:Java.Util.IList)
要插入到中的对象的数组[`ListView`](xref:Android.Widget.ListView)
（下一次定义）。

此[`TextFilterEnabled`](xref:Android.Widget.AbsListView.TextFilterEnabled)
属性将打开的[`ListView`](xref:Android.Widget.ListView)文本筛选，因此，当用户开始键入时，将筛选该列表。

此[`ItemClick`](xref:Android.Widget.AdapterView.ItemClick)
事件可用于为单击订阅处理程序。 当[`ListView`](xref:Android.Widget.ListView)
单击该处理程序，将调用处理程序[`Toast`](xref:Android.Widget.Toast)
使用单击的项中的文本显示消息。

您可以使用平台提供的列表项设计，而不是为[`ListAdapter`](xref:Android.App.ListActivity.ListAdapter)定义自己的布局文件。
例如，尝试使用`Android.Resource.Layout.SimpleListItem1` `Resource.Layout.list_item`而不是。

添加以下`using`语句：

```csharp
using System;
```

接下来，将以下字符串数组作为的成员`MainActivity`添加：

```csharp
static readonly string[] countries = new String[] {
    "Afghanistan","Albania","Algeria","American Samoa","Andorra",
    "Angola","Anguilla","Antarctica","Antigua and Barbuda","Argentina",
    "Armenia","Aruba","Australia","Austria","Azerbaijan",
    "Bahrain","Bangladesh","Barbados","Belarus","Belgium",
    "Belize","Benin","Bermuda","Bhutan","Bolivia",
    "Bosnia and Herzegovina","Botswana","Bouvet Island","Brazil","British Indian Ocean Territory",
    "British Virgin Islands","Brunei","Bulgaria","Burkina Faso","Burundi",
    "Cote d'Ivoire","Cambodia","Cameroon","Canada","Cape Verde",
    "Cayman Islands","Central African Republic","Chad","Chile","China",
    "Christmas Island","Cocos (Keeling) Islands","Colombia","Comoros","Congo",
    "Cook Islands","Costa Rica","Croatia","Cuba","Cyprus","Czech Republic",
    "Democratic Republic of the Congo","Denmark","Djibouti","Dominica","Dominican Republic",
    "East Timor","Ecuador","Egypt","El Salvador","Equatorial Guinea","Eritrea",
    "Estonia","Ethiopia","Faeroe Islands","Falkland Islands","Fiji","Finland",
    "Former Yugoslav Republic of Macedonia","France","French Guiana","French Polynesia",
    "French Southern Territories","Gabon","Georgia","Germany","Ghana","Gibraltar",
    "Greece","Greenland","Grenada","Guadeloupe","Guam","Guatemala","Guinea","Guinea-Bissau",
    "Guyana","Haiti","Heard Island and McDonald Islands","Honduras","Hong Kong","Hungary",
    "Iceland","India","Indonesia","Iran","Iraq","Ireland","Israel","Italy","Jamaica",
    "Japan","Jordan","Kazakhstan","Kenya","Kiribati","Kuwait","Kyrgyzstan","Laos",
    "Latvia","Lebanon","Lesotho","Liberia","Libya","Liechtenstein","Lithuania","Luxembourg",
    "Macau","Madagascar","Malawi","Malaysia","Maldives","Mali","Malta","Marshall Islands",
    "Martinique","Mauritania","Mauritius","Mayotte","Mexico","Micronesia","Moldova",
    "Monaco","Mongolia","Montserrat","Morocco","Mozambique","Myanmar","Namibia",
    "Nauru","Nepal","Netherlands","Netherlands Antilles","New Caledonia","New Zealand",
    "Nicaragua","Niger","Nigeria","Niue","Norfolk Island","North Korea","Northern Marianas",
    "Norway","Oman","Pakistan","Palau","Panama","Papua New Guinea","Paraguay","Peru",
    "Philippines","Pitcairn Islands","Poland","Portugal","Puerto Rico","Qatar",
    "Reunion","Romania","Russia","Rwanda","Sqo Tome and Principe","Saint Helena",
    "Saint Kitts and Nevis","Saint Lucia","Saint Pierre and Miquelon",
    "Saint Vincent and the Grenadines","Samoa","San Marino","Saudi Arabia","Senegal",
    "Seychelles","Sierra Leone","Singapore","Slovakia","Slovenia","Solomon Islands",
    "Somalia","South Africa","South Georgia and the South Sandwich Islands","South Korea",
    "Spain","Sri Lanka","Sudan","Suriname","Svalbard and Jan Mayen","Swaziland","Sweden",
    "Switzerland","Syria","Taiwan","Tajikistan","Tanzania","Thailand","The Bahamas",
    "The Gambia","Togo","Tokelau","Tonga","Trinidad and Tobago","Tunisia","Turkey",
    "Turkmenistan","Turks and Caicos Islands","Tuvalu","Virgin Islands","Uganda",
    "Ukraine","United Arab Emirates","United Kingdom",
    "United States","United States Minor Outlying Islands","Uruguay","Uzbekistan",
    "Vanuatu","Vatican City","Venezuela","Vietnam","Wallis and Futuna","Western Sahara",
    "Yemen","Yugoslavia","Zambia","Zimbabwe"
  };
```

这是将置于中[`ListView`](xref:Android.Widget.ListView)的字符串数组。

运行该应用程序。 您可以滚动列表或键入来筛选它，然后单击某个项以查看消息。 将显示如下所示的内容：

[![带有国家/地区名称的 ListView 的示例屏幕截图](images/01-listview-example-sml.png)](images/01-listview-example.png#lightbox)

请注意，使用硬编码字符串数组并非最佳设计做法。 为了简单起见，本教程使用了一个[`ListView`](xref:Android.Widget.ListView)
小组件. 更好的做法是引用由外部资源定义的字符串数组，如使用项目`string-array` **资源/值/字符串 .xml**文件中的资源。 例如:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
  <string name="app_name">HelloListView</string>
  <string-array name="countries_array">
    <item>Bahrain</item>
    <item>Bangladesh</item>
    <item>Barbados</item>
    <item>Belarus</item>
    <item>Belgium</item>
    <item>Belize</item>
    <item>Benin</item>
  </string-array>
</resources>
```

若要将这些资源字符串[`ArrayAdapter`](xref:Android.Widget.ArrayAdapter`1)用于，请替换原始[`ListAdapter`](xref:Android.App.ListActivity.ListAdapter)
行，如下所示：

```csharp
string[] countries = Resources.GetStringArray (Resource.Array.countries_array);
ListAdapter = new ArrayAdapter<string> (this, Resource.Layout.list_item, countries);
```

运行该应用程序。 将显示如下所示的内容：

[![具有较小名称列表的 ListView 的示例屏幕截图](images/02-smaller-example-sml.png)](images/02-smaller-example.png#lightbox)

## <a name="going-further-with-listview"></a>继续了解 ListView

其余主题（链接如下）全面介绍`ListView`了如何使用类以及可用于它的不同类型的适配器类型。 该结构如下所示：

- **视觉外观**控件的`ListView`各个部分&ndash;及其工作原理。

- **类**用于显示的类的概述。 `ListView` &ndash;

- **在 ListView 中显示数据**如何显示数据的简单列表; 如何实现`ListView's`可用性功能; 如何使用不同的内置行布局; 以及适配器如何通过重复使用行视图来节省内存。 &ndash;

- **自定义外观**&ndash;将的`ListView`样式更改为自定义布局、字体和颜色。

- **使用 SQLite**如何使用显示 SQLite 数据库中的数据。 `CursorAdapter` &ndash;

- **活动生命周期**&ndash; 实现`ListView`活动时的设计注意事项，包括在生命周期中应填充数据的位置以及何时释放资源。

讨论（分为六部分）首先概述`ListView`类本身，然后再介绍如何使用它的更为复杂的示例。

- [ListView 部件和功能](~/android/user-interface/layouts/list-view/parts-and-functionality.md)
- [使用数据填充 ListView](~/android/user-interface/layouts/list-view/populating.md)
- [自定义 ListView 的外观](~/android/user-interface/layouts/list-view/customizing-appearance.md)
- [使用 CursorAdapters](~/android/user-interface/layouts/list-view/cursor-adapters.md)
- [使用 ContentProvider](~/android/user-interface/layouts/list-view/content-provider.md)
- [ListView 和活动生命周期](~/android/user-interface/layouts/list-view/activity-lifecycle.md)

## <a name="summary"></a>总结

这组主题介绍`ListView`了如何使用的内置功能`ListActivity`，并提供了一些示例。 它讨论`ListView`了用于彩色布局和使用 SQLite 数据库的的自定义实现，并简要介绍了`ListView`实现中活动生命周期的相关性。

## <a name="related-links"></a>相关链接

- [AccessoryViews （示例）](https://docs.microsoft.com/samples/xamarin/monodroid-samples/accessoryviews)
- [BasicTableAndroid （示例）](https://docs.microsoft.com/samples/xamarin/monodroid-samples/basictableandroid)
- [BasicTableAdapter （示例）](https://docs.microsoft.com/samples/xamarin/monodroid-samples/basictableadapter)
- [BuiltInViews （示例）](https://docs.microsoft.com/samples/xamarin/monodroid-samples/builtinviews)
- [CustomRowView （示例）](https://docs.microsoft.com/samples/xamarin/monodroid-samples/customrowview)
- [FastScroll （示例）](https://docs.microsoft.com/samples/xamarin/monodroid-samples/fastscroll)
- [SectionIndex （示例）](https://docs.microsoft.com/samples/xamarin/monodroid-samples/sectionindex)
- [SimpleCursorTableAdapter （示例）](https://docs.microsoft.com/samples/xamarin/monodroid-samples/simplecursortableadapter)
- [CursorTableAdapter （示例）](https://docs.microsoft.com/samples/xamarin/monodroid-samples/cursortableadapter)
- [活动生命周期教程](~/android/app-fundamentals/activity-lifecycle/index.md)
- [使用表和单元格（在 Xamarin 中）](~/ios/user-interface/controls/tables/index.md)
- [ListView 类引用](xref:Android.Widget.ListView)
- [ListActivity 类引用](xref:Android.App.ListActivity)
- [BaseAdapter 类引用](xref:Android.Widget.BaseAdapter)
- [ArrayAdapter 类引用](xref:Android.Widget.ArrayAdapter)
- [CursorAdapter 类引用](xref:Android.Widget.CursorAdapter)
