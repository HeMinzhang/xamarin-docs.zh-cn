---
title: Android 无线发送
ms.prod: xamarin
ms.assetid: 4172A798-89EC-444D-BC0C-0A7DD67EF98C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/06/2017
ms.openlocfilehash: 4d294b7aee9d4a6c2118a5ff12968a7f95fb981c
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/06/2019
ms.locfileid: "70757857"
---
# <a name="android-beam"></a>Android 无线发送

Android 无线横梁是 Android 4.0 中引入的近乎现场通信（NFC）技术，它允许应用程序在接近时通过 NFC 共享信息。

[![说明近距离共享信息中的两个设备的关系图](android-beam-images/androidbeam.png)](android-beam-images/androidbeam.png#lightbox)

当两个设备在范围内时，可以通过 NFC 推送消息来使用 Android 无线功能。 有关4cm 的设备可以使用 Android 横梁共享数据。 一个设备上的活动会创建一条消息，并指定可以处理推送该消息的活动（或活动）。 当指定的活动处于前台并且设备处于范围内时，Android 横梁会将消息推送到第二个设备。 在接收设备上，调用了包含消息数据的意向。

Android 支持通过两种方式设置具有 Android 无线消息的消息：

- `SetNdefPushMessage`-在启动 Android 横梁之前，应用程序可以调用 SetNdefPushMessage 来指定要推送到 NFC 上的 NdefMessage，以及推送该活动的活动。 当应用程序正在使用时，如果消息未发生更改，则最好使用此机制。

- `SetNdefPushMessageCallback`-启动 Android 横梁后，应用程序可以处理回调以创建 NdefMessage。 此机制允许消息创建延迟，直到设备处于范围内。 它支持消息根据应用程序中发生的情况而有所不同。

在任一情况下，若要使用 Android 横梁发送数据，应用程序`NdefMessage`将发送，并将数据`NdefRecords`打包到多个中。 让我们看看必须解决的要点，然后才能触发 Android 横梁。 首先，我们将使用创建`NdefMessage`的回调样式。

## <a name="creating-a-message"></a>创建消息

可以`NfcAdapter`在活动的`OnCreate`方法中向注册回调。 例如，假设`NfcAdapter`在活动中`mNfcAdapter`将名为的类变量声明为，我们可以编写以下代码来创建用于构造消息的回调：

```csharp
mNfcAdapter = NfcAdapter.GetDefaultAdapter (this);
mNfcAdapter.SetNdefPushMessageCallback (this, this);
```

实现`NfcAdapter.ICreateNdefMessageCallback`的活动将传递`SetNdefPushMessageCallback`给上述方法。 启动 Android 无线连接时，系统会调用`CreateNdefMessage`，其中活动可以`NdefMessage`按如下所示构造：

```csharp
public NdefMessage CreateNdefMessage (NfcEvent evt)
{
    DateTime time = DateTime.Now;
    var text = ("Beam me up!\n\n" + "Beam Time: " +
        time.ToString ("HH:mm:ss"));
    NdefMessage msg = new NdefMessage (
        new NdefRecord[]{ CreateMimeRecord (
            "application/com.example.android.beam",
            Encoding.UTF8.GetBytes (text)) });
        } };
    return msg;
}

public NdefRecord CreateMimeRecord (String mimeType, byte [] payload)
{
    byte [] mimeBytes = Encoding.UTF8.GetBytes (mimeType);
    NdefRecord mimeRecord = new NdefRecord (
        NdefRecord.TnfMimeMedia, mimeBytes, new byte [0], payload);
    return mimeRecord;
}
```

## <a name="receiving-a-message"></a>接收消息

在接收端，系统使用`ActionNdefDiscovered`操作调用一个意向，可以从该操作中提取 NdefMessage，如下所示：

```csharp
IParcelable [] rawMsgs = intent.GetParcelableArrayExtra (NfcAdapter.ExtraNdefMessages);
NdefMessage msg = (NdefMessage) rawMsgs [0];
```

有关使用 Android 无线媒体的完整代码示例，显示在下面的屏幕截图中，请参阅示例库中的[Android 横梁演示](https://docs.microsoft.com/samples/xamarin/monodroid-samples/androidbeamdemo)。

[![Android 横梁演示中的示例屏幕截图](android-beam-images/24.png)](android-beam-images/24.png#lightbox)

## <a name="related-links"></a>相关链接

- [Android 横梁演示（示例）](https://docs.microsoft.com/samples/xamarin/monodroid-samples/androidbeamdemo)
- [冰淇淋三明治](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 平台](https://developer.android.com/sdk/android-4.0.html)
