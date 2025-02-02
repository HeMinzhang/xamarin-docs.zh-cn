---
title: Xamarin 中的通知
description: 本部分说明如何在 Xamarin 中实现本地通知。 它将解释 iOS 通知的各种 UI 元素，并讨论创建和显示通知所涉及的 API。
ms.prod: xamarin
ms.assetid: 5BB76915-5DB0-48C7-A267-FA9F7C50793E
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 07/13/2018
ms.openlocfilehash: eb8956e44a3148d79fff59d728cb63818b9bab60
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/04/2019
ms.locfileid: "70292741"
---
# <a name="notifications-in-xamarinios"></a>Xamarin 中的通知

> [!IMPORTANT]
> 本部分中的信息适用于 iOS 9 及更早的。 对于 iOS 10 及更高版本，请参阅[用户通知框架指南](~/ios/platform/user-notifications/index.md)。

iOS 具有三种方式向用户指示已收到通知：

- **声音或振动**-iOS 可以播放声音以通知用户。 如果禁用声音，则可以将设备配置为振动。
- **警报**-可以在屏幕上显示有关通知的信息。
- **徽章**-发布通知后，可以在应用程序图标上显示一个数字（徽章）。

iOS 还提供*通知中心，该通知中心*会向用户显示所有通知，无论是本地的还是远程的。 用户可通过从屏幕顶部向下轻扫来访问此内容：

![通知中心](local-notifications-in-ios-images/image13.png "通知中心")

## <a name="creating-local-notifications-in-ios"></a>在 iOS 中创建本地通知

iOS 使得创建和处理本地通知变得非常简单。
首先，iOS 8 要求应用程序要求用户提供显示通知的权限。 尝试发送本地通知之前，请将以下代码添加到应用-附加的[示例](https://docs.microsoft.com/samples/xamarin/ios-samples/localnotifications)将其放入**AppDelegate**的**FinishedLaunching**方法。

```csharp
var notificationSettings = UIUserNotificationSettings.GetSettingsForTypes(
    UIUserNotificationType.Alert | UIUserNotificationType.Badge | UIUserNotificationType.Sound, null
);
application.RegisterUserNotificationSettings(notificationSettings);
```

[![确认发送本地通知的能力](local-notifications-in-ios-images/image0-sml.png "确认发送本地通知的能力")](local-notifications-in-ios-images/image0.png#lightbox)

若要计划本地通知，请创建`UILocalNotification`对象， `FireDate`设置，并`UIApplication.SharedApplication`通过对象上的`ScheduleLocalNotification`方法对其进行安排。 下面的代码片段演示了如何计划一个通知，该通知将在将来激发一分钟，并显示一条消息，其中包含一条消息：

```csharp
UILocalNotification notification = new UILocalNotification();
notification.FireDate = NSDate.FromTimeIntervalSinceNow(15);
//notification.AlertTitle = "Alert Title"; // required for Apple Watch notifications
notification.AlertAction = "View Alert";
notification.AlertBody = "Your 15 second alert has fired!";
UIApplication.SharedApplication.ScheduleLocalNotification(notification);
```

以下屏幕截图显示了此警报的外观：

[![](local-notifications-in-ios-images/image2-sml.png "示例警报")](local-notifications-in-ios-images/image2.png#lightbox)

请注意，如果用户选择*不允许*通知，则不会显示任何内容。

如果要使用数字将徽章应用于应用程序图标，则可以设置它，如下面的代码行所示：

```csharp
notification.ApplicationIconBadgeNumber = 1;
```

若要播放带有图标的声音，请在通知上设置 SoundName 属性，如以下代码片段所示：

```csharp
notification.SoundName = UILocalNotification.DefaultSoundName;
```

如果通知声音超过30秒，则 iOS 将改为播放默认声音。

> [!IMPORTANT]
> IOS 模拟器中有一个 bug，该 bug 会两次激发委托通知。 在设备上运行应用程序时，不应出现此问题。

## <a name="handling-notifications"></a>处理通知

iOS 应用程序以几乎完全相同的方式处理远程和本地通知。 当应用程序运行时，将`ReceivedLocalNotification`调用`AppDelegate`类中`ReceivedRemoteNotification`的方法或方法，并且通知信息将作为参数传递。

应用程序可以通过不同的方式处理通知。 例如，应用程序可能只显示一个警报，提醒用户关于某个事件。 或者，通知可用于向用户显示进程已完成的警报，如将文件同步到服务器。

下面的代码演示如何处理本地通知并显示警报，并将徽章编号重置为零：

```csharp
public override void ReceivedLocalNotification(UIApplication application, UILocalNotification notification)
{
    // show an alert
    UIAlertController okayAlertController = UIAlertController.Create(notification.AlertAction, notification.AlertBody, UIAlertControllerStyle.Alert);
    okayAlertController.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Default, null));

    Window.RootViewController.PresentViewController(okayAlertController, true, null);

    // reset our badge
    UIApplication.SharedApplication.ApplicationIconBadgeNumber = 0;
}
```

如果应用程序未运行，则 iOS 会播放声音并/或更新图标徽章（如果适用）。 当用户启动与警报相关联的应用程序时，应用程序将启动，并`FinishedLaunching`将调用应用程序委托上的方法，并且将`launchOptions`通过参数传入通知信息。 如果选项字典包含该键`UIApplication.LaunchOptionsLocalNotificationKey`， `AppDelegate`则知道应用程序是从本地通知启动的。 以下代码段演示了此过程：

```csharp
// check for a local notification
if (launchOptions.ContainsKey(UIApplication.LaunchOptionsLocalNotificationKey))
{
    var localNotification = launchOptions[UIApplication.LaunchOptionsLocalNotificationKey] as UILocalNotification;
    if (localNotification != null)
    {
        UIAlertController okayAlertController = UIAlertController.Create(localNotification.AlertAction, localNotification.AlertBody, UIAlertControllerStyle.Alert);
        okayAlertController.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Default, null));

        Window.RootViewController.PresentViewController(okayAlertController, true, null);

        // reset our badge
        UIApplication.SharedApplication.ApplicationIconBadgeNumber = 0;
    }
}
```

对于远程通知， `launchOptions`将拥有一个`LaunchOptionsRemoteNotificationKey` ，其中包含与`NSDictionary`远程通知有效负载相关联的。 可以通过`alert`、 `badge`和`sound`键来提取通知有效负载。 下面的代码段演示如何获取远程通知：

```csharp
NSDictionary remoteNotification = options[UIApplication.LaunchOptionsRemoteNotificationKey];
if(remoteNotification != null)
{
    string alert = remoteNotification["alert"];
}
```

## <a name="summary"></a>总结

本部分说明如何在 Xamarin 中创建和发布通知。 它显示应用程序如何通过重写`ReceivedLocalNotification`中`AppDelegate`的方法`ReceivedRemoteNotification`或方法来对通知做出反应。

## <a name="related-links"></a>相关链接

- [本地通知（示例）](https://docs.microsoft.com/samples/xamarin/ios-samples/localnotifications)
- [面向开发人员的本地和推送通知](https://developer.apple.com/notifications/)
- [本地和推送通知编程指南](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/)
- [UIApplication](http://iosapi.xamarin.com/?link=T%3aMonoTouch.UIKit.UIApplication)
- [UILocalNotification](http://iosapi.xamarin.com/?link=T%3aMonoTouch.UIKit.UILocalNotification)
