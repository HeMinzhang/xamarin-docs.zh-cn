---
title: 使用 ContentProvider
ms.prod: xamarin
ms.assetid: 251F7557-328D-0132-F39D-595920A28B87
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: 5948d4b5db53db97c4e76cb7568c109b5faf90fe
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/06/2019
ms.locfileid: "70762198"
---
# <a name="using-a-contentprovider-with-xamarinandroid"></a>将 ContentProvider 与 Xamarin 配合使用

CursorAdapters 也可用于显示来自 ContentProvider 的数据。
Contentprovider 允许访问其他应用程序公开的数据（包括 Android 系统数据，如联系人、媒体和日历信息）。

访问 ContentProvider 的首选方法是使用 LoaderManager 的 CursorLoader。 Android 3.0 （API Level 11，Honeycomb）中引入了 LoaderManager，用于将阻塞任务移出主线程，使用 CursorLoader 允许将数据加载到线程中，然后将数据绑定到 ListView 以供显示。

有关详细信息，请参阅[Contentprovider 简介](~/android/platform/content-providers/index.md)。
