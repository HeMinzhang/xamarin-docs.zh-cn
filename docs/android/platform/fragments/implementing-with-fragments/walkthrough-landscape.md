---
title: 片段演练 - 第 2 部分
ms.prod: xamarin
ms.topic: tutorial
ms.assetid: 444A894D-5197-4726-934F-79BA80A71CB0
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/26/2018
ms.openlocfilehash: 0363213d76d9a67b559614741edf37d296848075
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/06/2019
ms.locfileid: "70761656"
---
# <a name="fragments-walkthrough-ndash-landscape"></a>分段演练&ndash;横向

[片段演练&ndash;第1部分](./walkthrough.md)演示了如何在 Android 应用程序中创建和使用以较小屏幕为目标的片段。 本演练的下一步是修改应用程序，以利用 tablet &ndash;上的额外水平空间，该活动将始终为播放列表`TitlesFragment`（），并`PlayQuoteFragment`将动态添加响应用户所做的选择时的活动：

[![在平板电脑上运行的应用](./walkthrough-landscape-images/01-tablet-screenshot-sml.png)](./walkthrough-landscape-images/01-tablet-screenshot.png#lightbox)

在横向模式下运行的手机还将从此增强中获益：

[![在处于横向模式的 Android 手机上运行的应用](./images/intro-screenshot-phone-land-sml.png)](./images/intro-screenshot-phone-land.png#lightbox)

## <a name="updating-the-app-to-handle-landscape-orientation"></a>更新应用程序以处理横向方向

以下修改将基于在 "片段" 中完成的工作完成[-Phone](./walkthrough.md)

1. 创建一个替代布局以同时`TitlesFragment`显示和。 `PlayQuoteFragment`
1. 更新`TitlesFragment`以检测设备是否同时显示两个片段并相应地更改行为。
1. 当`PlayQuoteActivity`设备处于横向模式时，更新关闭。

## <a name="1-create-an-alternate-layout"></a>1.创建替代布局

当在 Android 设备上创建主活动时，Android 会根据设备的方向确定要加载的布局。 默认情况下，Android 将提供**Resources/layout/activity_main**文件。 对于在横向模式下加载的设备，Android 将提供**Resources/layout-land/activity_main**布局文件。 [Android 资源](/xamarin/android/app-fundamentals/resources-in-android)指南包含有关 android 如何确定要为应用程序加载的资源文件的更多详细信息。

按照[备用布局](/xamarin/android/user-interface/android-designer/alternative-layout-views)指南中所述的步骤，创建面向**横向**的备用布局。 这应将新的布局资源文件添加到项目、**资源/布局/activity_main. main.axml**：

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![解决方案资源管理器中的替代布局](./walkthrough-landscape-images/02-alternate-layout.w157-sml.png)](./walkthrough-landscape-images/02-alternate-layout.w157.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![Solution Pad 中的替代布局](./walkthrough-landscape-images/02-alternate-layout.m743-sml.png)](./walkthrough-landscape-images/02-alternate-layout.m743.png#lightbox)

-----

创建备用布局后，编辑文件**Resources/layout-land/activity_main**的源，使其与此 XML 匹配：

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/two_fragments_layout"
    android:orientation="horizontal"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <fragment android:name="FragmentSample.TitlesFragment"
        android:id="@+id/titles"
        android:layout_weight="1"
        android:layout_width="0px"
        android:layout_height="match_parent" />

    <FrameLayout android:id="@+id/playquote_container"
            android:layout_weight="1"
            android:layout_width="0px"
            android:layout_height="match_parent"
             />
</LinearLayout>
```

为活动提供了资源 ID `two_fragments_layout`并具有两个子视图`fragment` ：和`FrameLayout`。 静态加载时`FrameLayout` ，将充当一个 "占位符"，它将在`PlayQuoteFragment`运行时由替换。 `fragment` 每次在中`TitlesFragment`选择新播放时`playquote_container` ，将使用的`PlayQuoteFragment`新实例更新。

每个子视图都将占据其父视图的全部高度。 每个子视图的宽度由`android:layout_weight`和`android:layout_width`属性控制。 在此示例中，每个子视图将占有 50% 的宽度由父级提供。 有关_布局权重_的详细信息，请参阅[LinearLayout 上的 Google 文档](https://developer.android.com/guide/topics/ui/layout/linear.html)。

## <a name="2-changes-to-titlesfragment"></a>2.对 TitlesFragment 的更改

创建备用布局后，需要更新`TitlesFragment`。 当应用程序在一个活动上显示两个片段时， `TitlesFragment`应`PlayQuoteFragment`在父活动中加载。 否则， `TitlesFragment`应`PlayQuoteActivity`启动承载`PlayQuoteFragment`的。 布尔型标志可帮助`TitlesFragment`确定应使用的行为。 此标志将在`OnActivityCreated`方法中进行初始化。

首先，将一个实例变量添加到`TitlesFragment`类的顶部：

```csharp
bool showingTwoFragments;
```

然后，将以下代码片段添加到`OnActivityCreated`以初始化变量： 

```csharp
var quoteContainer = Activity.FindViewById(Resource.Id.playquote_container);
showingTwoFragments = quoteContainer != null &&
                      quoteContainer.Visibility == ViewStates.Visible;
if (showingTwoFragments)
{
    ListView.ChoiceMode = ChoiceMode.Single;
    ShowPlayQuote(selectedPlayId);
}
```

如果设备在横向模式下运行，则具有资源`FrameLayout` ID `playquote_container`的将显示在屏幕上，因此`showingTwoFragments`将初始化为`true`。 如果设备在纵向模式下运行，则`playquote_container`不会显示在屏幕上，因此`showingTwoFragments`将是`false`。

方法将需要更改其在片段中&ndash;的显示方式或启动新活动的方式。 `ShowPlayQuote`  `ShowPlayQuote`更新方法以便在显示两个片段时加载片段，否则应启动一个活动：

```csharp
void ShowPlayQuote(int playId)
{
    selectedPlayId = playId;
    if (showingTwoFragments)
    {
        ListView.SetItemChecked(selectedPlayId, true);

        var playQuoteFragment = FragmentManager.FindFragmentById(Resource.Id.playquote_container) as PlayQuoteFragment;

        if (playQuoteFragment == null || playQuoteFragment.PlayId != playId)
        {
            var container = Activity.FindViewById(Resource.Id.playquote_container);
            var quoteFrag = PlayQuoteFragment.NewInstance(selectedPlayId);

            FragmentTransaction ft = FragmentManager.BeginTransaction();
            ft.Replace(Resource.Id.playquote_container, quoteFrag);
            ft.Commit();
        }
    }
    else
    {
        var intent = new Intent(Activity, typeof(PlayQuoteActivity));
        intent.PutExtra("current_play_id", playId);
        StartActivity(intent);
    }
}
```

如果用户选择的播放不同于`PlayQuoteFragment`当前正在显示的播放，则将创建一个新`PlayQuoteFragment`的，并在的上下文`FragmentTransaction`中替换的`playquote_container`内容。

### <a name="complete-code-for-titlesfragment"></a>TitlesFragment 的完整代码

完成对的所有更改`TitlesFragment`后，完整类应与以下代码相匹配：

```csharp
public class TitlesFragment : ListFragment
{
    int selectedPlayId;
    bool showingTwoFragments;

    public override void OnActivityCreated(Bundle savedInstanceState)
    {
        base.OnActivityCreated(savedInstanceState);
        ListAdapter = new ArrayAdapter<string>(Activity, Android.Resource.Layout.SimpleListItemActivated1, Shakespeare.Titles);

        if (savedInstanceState != null)
        {
            selectedPlayId = savedInstanceState.GetInt("current_play_id", 0);
        }

        var quoteContainer = Activity.FindViewById(Resource.Id.playquote_container);
        showingTwoFragments = quoteContainer != null &&
                                quoteContainer.Visibility == ViewStates.Visible;
        if (showingTwoFragments)
        {
            ListView.ChoiceMode = ChoiceMode.Single;
            ShowPlayQuote(selectedPlayId);
        }
    }

    public override void OnSaveInstanceState(Bundle outState)
    {
        base.OnSaveInstanceState(outState);
        outState.PutInt("current_play_id", selectedPlayId);
    }

    public override void OnListItemClick(ListView l, View v, int position, long id)
    {
        ShowPlayQuote(position);
    }

    void ShowPlayQuote(int playId)
    {
        selectedPlayId = playId;
        if (showingTwoFragments)
        {
            ListView.SetItemChecked(selectedPlayId, true);

            var playQuoteFragment = FragmentManager.FindFragmentById(Resource.Id.playquote_container) as PlayQuoteFragment;

            if (playQuoteFragment == null || playQuoteFragment.PlayId != playId)
            {
                var container = Activity.FindViewById(Resource.Id.playquote_container);
                var quoteFrag = PlayQuoteFragment.NewInstance(selectedPlayId);

                FragmentTransaction ft = FragmentManager.BeginTransaction();
                ft.Replace(Resource.Id.playquote_container, quoteFrag);
                ft.AddToBackStack(null);
                ft.SetTransition(FragmentTransit.FragmentFade);
                ft.Commit();
            }
        }
        else
        {
            var intent = new Intent(Activity, typeof(PlayQuoteActivity));
            intent.PutExtra("current_play_id", playId);
            StartActivity(intent);
        }
    }
}
```

## <a name="3-changes-to-playquoteactivity"></a>3.对 PlayQuoteActivity 的更改

需要注意的最后一个细节是：当设备`PlayQuoteActivity`处于横向模式时，无需执行此操作。 如果设备处于横向模式，则`PlayQuoteActivity`不可见。 `OnCreate`更新的`PlayQuoteActivity`方法，以便它将自行关闭。 此代码是的`PlayQuoteActivity.OnCreate`最终版本：

```csharp
protected override void OnCreate(Bundle savedInstanceState)
{
    base.OnCreate(savedInstanceState);

    if (Resources.Configuration.Orientation == Android.Content.Res.Orientation.Landscape)
    {
        Finish();
    }

    var playId = Intent.Extras.GetInt("current_play_id", 0);
    var playQuoteFrag = PlayQuoteFragment.NewInstance(playId);
    FragmentManager.BeginTransaction()
                    .Add(Android.Resource.Id.Content, playQuoteFrag)
                    .Commit();
}
```

此修改会为设备方向添加检查。 如果它处于横向模式，则`PlayQuoteActivity`会自行关闭。

## <a name="4-run-the-application"></a>4.运行此应用程序

完成这些更改后，运行应用，将设备旋转到横向模式（如有必要），然后选择 "播放"。 引号应该与播放列表显示在同一屏幕上：

[![在处于横向模式的 Android 手机上运行的应用](./images/intro-screenshot-phone-land-sml.png)](./images/intro-screenshot-phone-land.png#lightbox)

[![在 Android 平板电脑上运行的应用](./images/intro-screenshot-tablet-sml.png)](./images/intro-screenshot-tablet.png#lightbox)
