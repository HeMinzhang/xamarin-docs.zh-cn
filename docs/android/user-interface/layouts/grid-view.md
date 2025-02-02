---
title: GridView
ms.prod: xamarin
ms.assetid: 6992C4FF-ECBB-3493-AEE6-3E063C1A8C54
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: f71c275dd2beee6aedf41ecd19c8a4a39ab5a36f
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/16/2019
ms.locfileid: "69522640"
---
# <a name="xamarinandroid-gridview"></a>Xamarin Android GridView

[`GridView`](xref:Android.Widget.GridView)是[`ViewGroup`](xref:Android.Views.ViewGroup)
显示二维、可滚动网格中的项的。 网格项使用[`ListAdapter`](xref:Android.App.ListActivity.ListAdapter)自动插入到布局中。

在本教程中, 你将创建一个图像缩略图网格。 选择某项后, toast 消息会显示该图像的位置。

启动名为**HelloGridView**的新项目。

查找要使用的一些照片, 或[下载这些示例图像](https://developer.android.com/shareables/sample_images.zip)。 将图像文件添加到项目的**资源/可绘制**目录。 在 "**属性**" 窗口中, 将 "生成操作" 设置为 " **AndroidResource**"。

打开**Resources/Layout/main.axml**文件, 并插入以下内容:

```xml
<?xml version="1.0" encoding="utf-8"?>
<GridView xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/gridview"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:columnWidth="90dp"
    android:numColumns="auto_fit"
    android:verticalSpacing="10dp"
    android:horizontalSpacing="10dp"
    android:stretchMode="columnWidth"
    android:gravity="center"
/>
```

这[`GridView`](xref:Android.Widget.GridView)会填充整个屏幕。 特性是非常容易的。 有关有效属性的详细信息, 请参阅[`GridView`](xref:Android.Widget.GridView)参考。

打开`HelloGridView.cs`并插入以下代码:[`OnCreate()`](xref:Android.App.Activity.OnCreate*)
付款方式

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    SetContentView (Resource.Layout.Main);

    var gridview = FindViewById<GridView> (Resource.Id.gridview);
    gridview.Adapter = new ImageAdapter (this);

    gridview.ItemClick += delegate (object sender, AdapterView.ItemClickEventArgs args) {
        Toast.MakeText (this, args.Position.ToString (), ToastLength.Short).Show ();
    };
}
```

为内容视图设置**main.axml**布局后, [`GridView`](xref:Android.Widget.GridView)将[`FindViewById`](xref:Android.App.Activity.FindViewById*)从布局中捕获。 此[`Adapter`](xref:Android.Widget.AdapterView.RawAdapter)
然后, 使用属性将自定义适配器 (`ImageAdapter`) 设置为要在网格中显示的所有项的源。 `ImageAdapter`在下一步中创建。

若要在单击网格中的某一项时执行操作, 请将该[`ItemClick`](xref:Android.Widget.AdapterView.ItemClick)事件订阅为匿名委托。
它显示了[`Toast`](xref:Android.Widget.Toast)一个, 它显示选定项的索引位置 (从零开始) (在实际情况下, 该位置可用于获取其他某个任务的完整大小的图像)。 请注意, 可以使用 Java 样式侦听器类, 而不是 .NET 事件。

创建名`ImageAdapter`为子类[`BaseAdapter`](xref:Android.Widget.BaseAdapter)的新类:

```csharp
public class ImageAdapter : BaseAdapter
{
    Context context;

    public ImageAdapter (Context c)
    {
        context = c;
    }

    public override int Count {
        get { return thumbIds.Length; }
    }

    public override Java.Lang.Object GetItem (int position)
    {
        return null;
    }

    public override long GetItemId (int position)
    {
        return 0;
    }

    // create a new ImageView for each item referenced by the Adapter
    public override View GetView (int position, View convertView, ViewGroup parent)
    {
        ImageView imageView;

        if (convertView == null) {  // if it's not recycled, initialize some attributes
            imageView = new ImageView (context);
            imageView.LayoutParameters = new GridView.LayoutParams (85, 85);
            imageView.SetScaleType (ImageView.ScaleType.CenterCrop);
            imageView.SetPadding (8, 8, 8, 8);
        } else {
            imageView = (ImageView)convertView;
        }

        imageView.SetImageResource (thumbIds[position]);
        return imageView;
    }

    // references to our images
    int[] thumbIds = {
        Resource.Drawable.sample_2, Resource.Drawable.sample_3,
        Resource.Drawable.sample_4, Resource.Drawable.sample_5,
        Resource.Drawable.sample_6, Resource.Drawable.sample_7,
        Resource.Drawable.sample_0, Resource.Drawable.sample_1,
        Resource.Drawable.sample_2, Resource.Drawable.sample_3,
        Resource.Drawable.sample_4, Resource.Drawable.sample_5,
        Resource.Drawable.sample_6, Resource.Drawable.sample_7,
        Resource.Drawable.sample_0, Resource.Drawable.sample_1,
        Resource.Drawable.sample_2, Resource.Drawable.sample_3,
        Resource.Drawable.sample_4, Resource.Drawable.sample_5,
        Resource.Drawable.sample_6, Resource.Drawable.sample_7
    };
}
```

首先, 此方法实现从继承的[`BaseAdapter`](xref:Android.Widget.BaseAdapter)一些必需方法。 构造函数和[`Count`](xref:Android.Widget.BaseAdapter.Count)属性一目了然。 往常[`GetItem(int)`](xref:Android.Widget.BaseAdapter.GetItem*)
应返回适配器中指定位置的实际对象, 但对于此示例, 它将被忽略。 同样[`GetItemId(int)`](xref:Android.Widget.BaseAdapter.GetItemId*)
应返回项的行 id, 但此处不需要。

需要的第一个方法[`GetView()`](xref:Android.Widget.BaseAdapter.GetView*)是。
此方法创建一个新的[`View`](xref:Android.Views.View)
对于添加到的`ImageAdapter`每个图像。 如果调用此,[`View`](xref:Android.Views.View)
传入, 这通常是回收的对象 (至少在调用了一次之后), 因此可以通过检查来查看对象是否为 null。 如果*为*null, 则为[`ImageView`](xref:Android.Widget.ImageView)
实例化并配置图像演示所需的属性:

- [`LayoutParams`](xref:Android.Views.View.LayoutParameters)设置视图&mdash;的高度和宽度, 这可确保无论可绘制的大小如何, 都会根据需要调整和裁剪每个图像, 使其适合这些尺寸。

- [`SetScaleType()`](xref:Android.Widget.ImageView.SetScaleType*)声明应向中心裁剪图像 (如有必要)。

- [`SetPadding(int, int, int, int)`](xref:Android.Views.View.SetPadding*)定义所有边的边距。 (请注意, 如果图像具有不同的纵横比, 则更少的填充将导致更大的图像裁剪, 因为它与给定给 ImageView 的尺寸不匹配。)

如果传递[`View`](xref:Android.Views.View)给[`GetView()`](xref:Android.Widget.BaseAdapter.GetView*)的*不*是 null, 则本地[`ImageView`](xref:Android.Widget.ImageView)
用回收[`View`](xref:Android.Views.View)的对象进行初始化。

在[`GetView()`](xref:Android.Widget.BaseAdapter.GetView*)
方法时, `position`传递给方法的整数用于`thumbIds`从数组中选择一个图像, 并将其设置为的[`ImageView`](xref:Android.Widget.ImageView)图像资源。

剩下的就是定义`thumbIds`可绘制资源的数组。

运行该应用程序。 网格布局应如下所示:

[![显示15个图像的 GridView 的示例屏幕截图](grid-view-images/helloviews4.png)](grid-view-images/helloviews4.png#lightbox)

尝试体验[`GridView`](xref:Android.Widget.GridView)和的行为[`ImageView`](xref:Android.Widget.ImageView)
元素的属性。 例如, 而不是使用[`LayoutParams`](xref:Android.Views.View.LayoutParameters) [`SetAdjustViewBounds()`](xref:Android.Widget.ImageView.SetAdjustViewBounds*)try。

## <a name="references"></a>参考资料

- [`GridView`](xref:Android.Widget.GridView)
- [`ImageView`](xref:Android.Widget.ImageView)
- [`BaseAdapter`](xref:Android.Widget.BaseAdapter)

_此页面的某些部分是基于 Android 开源项目创建和共享的工作的修改, 并根据[创造性 Commons 2.5 归属许可证](http://creativecommons.org/licenses/by/2.5/)中所述的条款使用。_
