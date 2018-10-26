---
title: Windows 上调试 Android 需要哪些 USB 驱动程序？
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 36EC7341-A2A4-409C-BD4F-330BAC505123
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/22/2018
ms.openlocfilehash: 8b996a1cc89acedc47c7169ec579dfb99dae788f
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/25/2018
ms.locfileid: "50114530"
---
# <a name="what-usb-drivers-do-i-need-to-debug-android-on-windows"></a>Windows 上调试 Android 需要哪些 USB 驱动程序？

## <a name="finding-usb-drivers"></a>找到 USB 驱动程序

若要在 Android 设备上进行调试时开发 Windows;需要安装兼容的 USB 驱动程序。 Android SDK 管理器默认情况下添加了对如下所述的用于 Nexus 设备支持包括"Google USB 驱动程序": [http://developer.android.com/sdk/win-usb.html](http://developer.android.com/sdk/win-usb.html)

其他设备需要专门发布的设备制造商的 USB 驱动程序。 本指南中包含的最常见的制造商一些链接： [http://developer.android.com/tools/extras/oem-usb.html](http://developer.android.com/tools/extras/oem-usb.html)

## <a name="alternatives"></a>替代项

具体取决于 manfacturer，很难跟踪所需的确切 USB 驱动程序。 在包括使用 Android 仿真程序或使用外部测试服务的 Windows 开发一些测试 Android 应用的替代方案。 其中包括：

- [App Center 测试](https://docs.microsoft.com/appcenter/test-cloud/)-云服务在数百个实际 Android 设备上运行测试。

- [适用于 Android 的 Visual Studio 模拟器](https://visualstudio.microsoft.com/vs/msft-android-emulator/)

- [在 Android Emulator 上调试](~/android/deploy-test/debugging/debug-on-emulator.md)

