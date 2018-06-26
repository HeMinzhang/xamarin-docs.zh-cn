---
title: 在 Xamarin.Android 的广播接收方
description: 本部分讨论如何使用广播的接收方。
ms.prod: xamarin
ms.assetid: B2727160-12F2-43EE-84B5-0B15C8FCF4BD
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 04/20/2018
ms.openlocfilehash: 6b2e316eaf67e51801be4fcd670e80ec81c8ff08
ms.sourcegitcommit: 26033c087f49873243751deded8037d2da701655
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/25/2018
ms.locfileid: "36935394"
---
# <a name="broadcast-receivers-in-xamarinandroid"></a>在 Xamarin.Android 的广播接收方

_本部分讨论如何使用广播的接收方。_

## <a name="broadcast-receiver-overview"></a>广播的接收方概述

A_广播的接收方_是一种 Android 组件，它允许应用程序响应消息 (Android [ `Intent` ](https://developer.xamarin.com/api/type/Android.Content.Intent/)) 的广播 Android 操作系统或应用程序。 广播按照_发布-订阅_模型&ndash;导致广播发布和接收感兴趣事件这些组件的事件。 

Android 标识两种类型的广播：

* **显式广播**&ndash;这些类型的广播目标特定的应用程序。 显式广播的最常见用途是为启动一个活动。 下面举例说明时应用程序需要拨打电话号码; 显式广播它将调度面向 Android 和电话号码沿传递内需上的 Phone 应用程序意向。 Android 然后将路由到 Phone 应用程序的意图。
* **隐式广播**&ndash;这些广播调度到设备上的所有应用。 隐式广播的一个示例是`ACTION_POWER_CONNECTED`意图。 此方法会被发布 Android 检测到在设备上的电池正在充电每次。 Android 将路由到已注册了此事件的所有应用此目的。

广播的接收方的一个子类`BroadcastReceiver`类型和它必须重写[ `OnReceive` ](https://developer.xamarin.com/api/member/Android.Content.BroadcastReceiver.OnReceive/p/Android.Content.Context/Android.Content.Intent/)方法。 将执行 android`OnReceive`上的主线程，因此此方法应设计为可快速执行。 在生成中的线程时应小心`OnReceive`因为 Android 可能会终止进程，在方法结束时。 如果广播的接收方必须执行长时间运行的工作，则建议计划_作业_使用`JobScheduler`或_Firebase 作业调度程序_。 将单独指南中讨论计划与某一工作的工作。

_意向的筛选器_用于注册广播的接收方，以便 Android 可以正确地将路由消息。 可以在运行时指定的意向的筛选器 (这有时称为_上下文注册接收方_或_动态注册_)，或者在 Android 清单 (可以静态定义_清单注册接收方_)。 Xamarin.Android 提供的 C# 特性， `IntentFilterAttribute`，，将静态注册意向的筛选器 （这将在本指南后面的更详细地讨论）。 从 Android 8.0 开始，不能为应用程序以静态方式注册隐式广播。

清单注册接收方和上下文注册接收方之间的主要差别在于，上下文注册接收方将仅响应广播时应用程序正在运行，而清单注册的接收方可以响应广播即使应用程序可能未运行也是如此。  

管理广播的接收方和发送广播有两个集的 Api:

1. **`Context`** &ndash; `Android.Content.Context`类可以用于注册一个广播的接收器，将对系统级事件做出响应。 `Context`还用于发布系统级广播。
2. **[`LocalBroadcastManager`](https://developer.android.com/reference/android/support/v4/content/LocalBroadcastManager.html#sendBroadcast(android.content.Intent))** &ndash; 这是一个 API，可通过[Xamarin 支持库 v4 NuGet 包](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)。 此类用于保留广播和隔离的应用程序正在使用它们的上下文中的广播接收方。 此类可用于阻止响应仅限应用程序的广播或将消息发送到专用的接收方的其他应用程序。

广播的接收方可能不会显示对话框，并且强烈建议不要使用，以启动从内广播的接收方的活动。 如果广播的接收方必须通知用户，它应发布通知。

不能将绑定到或启动从内广播的接收方的服务。 

本指南将介绍如何创建广播的接收器以及如何注册它，以便它可能会收到广播。

## <a name="creating-a-broadcast-receiver"></a>创建广播的接收器

若要在 Xamarin.Android 中创建广播的接收方，应用程序应子类`BroadcastReceiver`类中，它与修饰`BroadcastReceiverAttribute`，并重写`OnReceive`方法：

```csharp
[BroadcastReceiver(Enabled = true, Exported = false)]
public class SampleReceiver : BroadcastReceiver
{
    public override void OnReceive(Context context, Intent intent)
    {
        // Do stuff here.

        String value = intent.GetStringExtra("key");
    }
}
```

当 Xamarin.Android 编译该类时，则它也将使用需要元数据注册接收方更新 AndroidManifest。 为静态注册广播接收方，`Enabled`正确必须设置为`true`，否则 Android 将不能创建接收方的实例。
 
`Exported`属性控制是否广播的接收方可以接收来自外部应用程序的邮件。 如果未显式设置属性，该属性的默认值由基于是否存在任何意向-关联的筛选器与广播的接收者的 android。 如果没有至少一个意向-筛选器为广播的接收方则 Android 将假定`Exported`属性是`true`。 如果没有意向-关联的筛选器与广播的接收者，则 Android 将认为此值越`false`。 

`OnReceive`方法接收到的引用`Intent`，已调度到广播的接收方。 这样就可以将值传递到广播的接收方的意图发件人。

### <a name="statically-registering-a-broadcast-receiver-with-an-intent-filter"></a>广播的接收方以静态方式注册意向筛选器

当`BroadcastReceiver`用修饰[ `IntentFilterAttribute` ](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/)，Xamarin.Android 将添加所需`<intent-filter>`元素添加 Android 清单在编译时。 下面的代码段是广播的接收方将在设备已完成启动 （如果用户授予了适当的 Android 权限） 时运行的示例：

```csharp
[BroadcastReceiver(Enabled = true)]
[IntentFilter(new[] { Android.Content.Intent.ActionBootCompleted })]
public class MyBootReceiver : BroadcastReceiver
{
    public override void OnReceive(Context context, Intent intent)
    {
        // Work that should be done when the device boots.     
    }
}
```

还有可能创建将响应自定义意向意向筛选器。 请看下面的示例： 

```csharp
[BroadcastReceiver(Enabled = true)]
[IntentFilter(new[] { "com.xamarin.example.TEST" })]
public class MySampleBroadcastReceiver : BroadcastReceiver
{
    public override void OnReceive(Context context, Intent intent)
    {
        // Do stuff here
    }
}
```

面向 Android 8.0 （API 级别 26） 应用程序，或更高版本可能不以静态方式进行注册隐式广播。 显式广播可能仍静态注册应用。 没有不受此限制的隐式广播的小的列表。 这些异常中所述[隐式广播异常](https://developer.android.com/guide/components/broadcast-exceptions.html)Android 文档中的指南。 对于有兴趣隐式广播的应用必须执行因此动态使用`RegisterReceiver`方法。 这在下一部分介绍。

### <a name="context-registering-a-broadcast-receiver"></a>上下文注册广播的接收方

上下文注册 （也称为动态注册） 的接收方执行通过调用`RegisterReceiver`方法和广播的接收方必须通过调用注销`UnregisterReceiver`方法。 若要防止泄漏资源，务必取消注册已不再相关的上下文 （活动或服务） 的接收方。 例如，服务可能广播想要通知的活动，即可向用户显示的更新。 当活动启动时，它将注册这些意向。 当活动移到后台并且不再对用户可见，它应注销接收方，因为用于显示更新的 UI 不再可见。 下面的代码段演示了如何注册和注销广播接收方在一个活动的上下文中：

```csharp
[Activity(Label = "MainActivity", MainLauncher = true, Icon = "@mipmap/icon")]
public class MainActivity: Activity 
{
    MySampleBroadcastReceiver receiver;

    protected override void OnCreate(Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState);
        receiver = new MySampleBroadcastReceiver()

        // Code omitted for clarity
    }

    protected override OnResume() 
    {
        base.OnResume();
        RegisterReceiver(receiver, new IntentFilter("com.xamarin.example.TEST"));
        // Code omitted for clarity
    }

    protected override OnPause() 
    {
        UnregisterReceiver(receiver);
        // Code omitted for clarity
        base.OnPause();
    }
}
```

在前面的示例中，当活动进入前台，它将注册一个广播的接收器，通过将侦听的自定义的意图`OnResume`生命周期方法。 当活动移到后台，`OnPause()`方法将注销接收方。

## <a name="publishing-a-broadcast"></a>发布广播

广播可能会发布到创建目的对象和调度它与设备上安装的所有应用`SendBroadcast`或`SendOrderedBroadcast`方法。  

1. **Context.SendBroadcast 方法**&ndash;有多个实现此方法。
   这些方法将传播到整个系统的意图。 广播的接收方将在不确定的顺序接收意图。 这提供了极大的灵活性，但意味着可能对于其他应用程序以注册和接收意图会。 这可能会带来潜在的安全风险。 应用程序可能需要实现添加安全性以防止未经授权的访问。 一个可能的解决方案是使用`LocalBroadcastManager`这仅将分发中的应用程序的专用空间的消息。 此代码片段演示了如何调度使用之一为打算`SendBroadcast`方法：

   ```csharp
   Intent message = new Intent("com.xamarin.example.TEST");
   // If desired, pass some values to the broadcast receiver.
   intent.PutExtra("key", "value");
   SendBroadcast(intent);
   ```

    此代码段是另一个示例通过使用发送广播`Intent.SetAction`的方法来确定操作：

    ```csharp 
    Intent intent = new Intent();
    intent.SetAction("com.xamarin.example.TEST");
    intent.PutExtra("key", "value");
    SendBroadcast(intent);
    ```

2. **Context.SendOrderedBroadcast** &ndash;这是方法非常类似于`Context.SendBroadcast`，二者的区别在于，其目的将已发布的次一个地对接收方，recievers 已注册的顺序。

### <a name="localbroadcastmanager"></a>LocalBroadcastManager

[Xamarin 支持库 v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)提供调用一个帮助器类[ `LocalBroadcastManager` ](https://developer.android.com/reference/android/support/v4/content/LocalBroadcastManager.html)。 `LocalBroadcastManager`适用于不希望发送或从设备上的其他应用接收广播的应用。 `LocalBroadcastManager`仅将发布的应用程序，并仅向注册到这些广播接收方的上下文中的消息`LocalBroadcastManager`。 此代码片段是一种注册广播的收件人`LocalBroadcastManager`:

```csharp
Android.Support.V4.Content.LocalBroadcastManager.GetInstance(this). RegisterReceiver(receiver, new IntentFilter("com.xamarin.example.TEST"));
```

在设备上的其他应用无法接收的消息的发布与`LocalBroadcastManager`。 此代码段演示如何调度目的使用`LocalBroadcastManager`:

```csharp
Intent message = new Intent("com.xamarin.example.TEST");
// If desired, pass some values to the broadcast receiver.
intent.PutExtra("key", "value");
Android.Support.V4.Content.LocalBroadcastManager.GetInstance(this).SendBroadcast(message);
```

## <a name="related-links"></a>相关链接

- [BroadcastReceiver](https://developer.xamarin.com/api/type/Android.Content.BroadcastReceiver/)
- [Context.RegisterReceiver](https://developer.xamarin.com/api/member/Android.Content.Context.RegisterReceiver/p/Android.Content.BroadcastReceiver/Android.Content.IntentFilter/System.String/Android.OS.Handler/)
- [Context.SendBroadcast](https://developer.xamarin.com/api/member/Android.Content.Context.SendBroadcast/p/Android.Content.Intent/)
- [Context.UnregisterReceiver](https://developer.xamarin.com/api/member/Android.Content.Context.UnregisterReceiver/p/Android.Content.BroadcastReceiver/)
- [意图](https://developer.xamarin.com/api/type/Android.Content.Intent/)
- [IntentFilter](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/)
- [LocalBroadcastManager](https://developer.android.com/reference/android/support/v4/content/LocalBroadcastManager.html#sendBroadcast(android.content.Intent))
- [在 Android 中的本地通知](~/android/app-fundamentals/notifications/local-notifications.md)
- [Android 支持库 v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)
