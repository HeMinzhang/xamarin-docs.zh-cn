---
title: Web 视图
ms.prod: xamarin
ms.assetid: 807F214A-166D-B342-0BBA-525517577F6B
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: 2718953c9e5628374c45fa3741d1ad3be3125dd9
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/26/2019
ms.locfileid: "68510153"
---
# <a name="xamarinandroid-web-view"></a>Xamarin Android Web 视图

[`WebView`](xref:Android.Webkit.WebView)允许您创建自己的窗口, 用于查看网页 (甚至开发完整的浏览器)。 在本教程中, 你将创建一个简单的[`Activity`](xref:Android.App.Activity)
可以查看和导航网页。

创建名为**HelloWebView**的新项目。

打开 **Resources/Layout/Main.axml** 并插入以下代码：

```xml
<?xml version="1.0" encoding="utf-8"?>
<WebView  xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/webview"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent" />
```

由于此应用程序将访问 Internet, 因此你必须将相应的权限添加到 Android 清单文件中。 打开项目的 "属性", 指定应用程序运行所需的权限。 `INTERNET`启用权限, 如下所示:

![在 Android 清单中设置 INTERNET 权限](web-view-images/01-set-internet-permissions.png)

现在, 打开**MainActivity.cs**并为 Webkit 添加 using 指令:

```csharp
using Android.Webkit;
```

在`MainActivity`类的顶部, 声明一个[`WebView`](xref:Android.Webkit.WebView)对象:

```csharp
WebView web_view;
```

当**Web 视图**请求加载 URL 时, 默认情况下会将请求委托给默认浏览器。 若要使**web 视图**加载 URL (而不是默认浏览器), 您必须`Android.Webkit.WebViewClient`为`ShouldOverriderUrlLoading`方法创建子类并重写方法。 `WebViewClient` 提供`WebView`此自定义的实例。 为此, 请在中`HelloWebViewClient` `MainActivity`添加以下嵌套类:

```csharp
public class HelloWebViewClient : WebViewClient
{
    public override bool ShouldOverrideUrlLoading (WebView view, string url)
    {
        view.LoadUrl(url);
        return false;
    }
}
```

当`ShouldOverrideUrlLoading`返回`false`时, 它会向 Android 发出信号`WebView` , 指示当前实例已处理请求, 无需执行其他操作。 

如果面向 API 级别24或更高版本, 请使用的重载`ShouldOverrideUrlLoading` , 该重载`IWebResourceRequest`采用`string`第二个参数的, 而不是:

```csharp
public class HelloWebViewClient : WebViewClient
{
    // For API level 24 and later
    public override bool ShouldOverrideUrlLoading (WebView view, IWebResourceRequest request)
    {
        view.LoadUrl(request.Url.ToString());
        return false;
    }
}
```

接下来, 对的[`OnCreate()`](xref:Android.App.Activity.OnCreate*)方法使用以下代码:

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    // Set our view from the "main" layout resource
    SetContentView (Resource.Layout.Main);

    web_view = FindViewById<WebView> (Resource.Id.webview);
    web_view.Settings.JavaScriptEnabled = true;
    web_view.SetWebViewClient(new HelloWebViewClient());
    web_view.LoadUrl ("https://www.xamarin.com/university");
}
```

这[`WebView`](xref:Android.Webkit.WebView)会`= true` 
 [`WebView`](xref:Android.Webkit.WebView) [`JavaScriptEnabled`](xref:Android.Webkit.WebSettings.JavaScriptEnabled) [\# ](https://github.com/xamarin/recipes/tree/master/Recipes/android/controls/webview/call_csharp_from_javascript)从布局中初始化成员, 并为 with 启用 JavaScript (请参阅 javascript 中的调用 C) [`Activity`](xref:Android.App.Activity)用于了解如何从 JavaScript 调用 C\#函数的信息。 最后, 使用[`LoadUrl(String)`](xref:Android.Webkit.WebView)加载初始网页。

生成并运行应用。 应会看到一个简单的网页查看器应用, 如以下屏幕截图所示:

[![显示 Web 视图的应用示例](web-view-images/02-simple-webview-app-sml.png)](web-view-images/02-simple-webview-app.png#lightbox)

若要处理 "**后退**" 按钮按键, 请添加以下 using 语句:

```csharp
using Android.Views;
```

接下来, 将以下方法添加到`HelloWebView`活动中:

```csharp
public override bool OnKeyDown (Android.Views.Keycode keyCode, Android.Views.KeyEvent e)
{
    if (keyCode == Keycode.Back && web_view.CanGoBack ())
    {
        web_view.GoBack ();
        return true;
    }
    return base.OnKeyDown (keyCode, e);
}
```

此[`OnKeyDown(int, KeyEvent)`](xref:Android.App.Activity.OnKeyDown*)
当活动运行时按下按钮时, 将调用回调方法。 内的条件使用[`KeyEvent`](xref:Android.Views.KeyEvent)来检查按下的键是否是**后退** [`WebView`](xref:Android.Webkit.WebView)按钮, 以及是否确实能够向后导航 (如果有历史记录)。 如果两者都为 true, [`GoBack()`](xref:Android.Webkit.WebView.GoBack)则调用方法, 该方法将[`WebView`](xref:Android.Webkit.WebView)在历史记录中导航回来一步。 返回`true`指示事件已处理。 如果不满足此条件, 则会将事件发送回系统。

再次运行该应用程序。 现在应可以跟踪链接, 并在页面历史记录中导航回来:

[![操作中 "后退" 按钮的示例屏幕截图](web-view-images/03-back-button-sml.png)](web-view-images/03-back-button.png#lightbox)

*此页面的某些部分是基于 Android 开源项目创建和共享的工作的修改, 并根据*
[*创造性 Commons 2.5 归属许可证*](http://creativecommons.org/licenses/by/2.5/)中所述的条款使用。

## <a name="related-links"></a>相关链接

- [从 JavaScript 调用 C#](https://github.com/xamarin/recipes/tree/master/Recipes/android/controls/webview/call_csharp_from_javascript)
- [Android.Webkit.WebView](xref:Android.Webkit.WebView)
- [KeyEvent](xref:Android.Webkit.WebView)
