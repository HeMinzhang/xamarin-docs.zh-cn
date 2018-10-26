---
title: 为何无法我的 Android 发布版本连接到 Internet？
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: A6FE770B-A19A-4BF8-95E9-2CF880D4AFC5
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/09/2018
ms.openlocfilehash: cd27d5c884086cd0fade4364851039fd0cd915a0
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/25/2018
ms.locfileid: "50117898"
---
# <a name="why-cant-my-android-release-build-connect-to-the-internet"></a>为何无法我的 Android 发布版本连接到 Internet？

## <a name="cause"></a>原因

此问题的最常见原因是**INTERNET**权限将自动包括在调试版本，但必须手动设置为发布版本。 这是因为 Internet 权限用于允许将调试程序附加到进程，针对"DebugSymbols"所述[此处](~/android/deploy-test/building-apps/build-process.md)。


## <a name="fix"></a>修复

若要解决此问题，可能需要在 Android 清单中的 Internet 权限。 这可以通过在清单编辑器或该清单的 sourcecode 完成：

-   在编辑器中修复： 在 Android 项目，请转到**属性-> AndroidManifest.xml-> 所需的权限**，并检查**Internet**

-   在 Sourcecode 中修复： 在源编辑器中打开 AndroidManifest 并将权限标记内的添加`<Manifest>`标记：

    ```xml
    <Manifest>
    ...
    <uses-permission android:name="android.permission.INTERNET" />
    </Manifest>
    ```
