---
title: Android.Support.v7.AppCompat - 未找到与给定名称匹配的资源：属性“android:actionModeShareDrawable”
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5814069C-FC43-41DE-B5A5-024D05E59929
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/09/2018
ms.openlocfilehash: b8961c9e58d4336a952649ce8181ca6ebdfe3165
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/06/2019
ms.locfileid: "70757224"
---
# <a name="androidsupportv7appcompat---no-resource-found-that-matches-the-given-name-attr-androidactionmodesharedrawable"></a>Android.Support.v7.AppCompat - 未找到与给定名称匹配的资源：属性“android:actionModeShareDrawable”

1. 请确保通过 Android SDK Manager 下载最新的附加内容以及 Android 5.0 （API 21） SDK。

2. 请确保在编译应用程序时将 compileSdkVersion 设置为21。 也可以选择将 targetSdkVersion 设置为21。

3. 如果需要以前的版本，如 API 19，请下载 Nuget 页面上的相应版本：

[https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)

*注意*：如果通过程序包管理器控制台手动安装此版本，请确保同时安装相同版本的 Xamarin. 支持

[https://www.nuget.org/packages/Xamarin.Android.Support.v4/](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)

Stack Overflow 参考：[https://stackoverflow.com/questions/26431676/appcompat-v721-0-0-no-resource-found-that-matches-the-given-name-attr-andro](https://stackoverflow.com/questions/26431676/appcompat-v721-0-0-no-resource-found-that-matches-the-given-name-attr-andro)

## <a name="see-also"></a>请参阅

- [应安装哪些 Android SDK 包？](~/android/troubleshooting/questions/install-android-sdk-packages.md)
