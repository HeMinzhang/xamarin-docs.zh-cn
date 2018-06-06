---
title: 实时的 Xamarin Player 的限制
description: 本文档介绍了 Xamarin 实时播放器的限制。 它讨论设备要求，功能适用于项目类型和其他杂项主题。
ms.prod: xamarin
ms.assetid: 36A1531E-630A-4B7C-A333-4E67E5DC023C
author: topgenorth
ms.author: toopge
ms.date: 03/29/2018
ms.openlocfilehash: ea71391382f9e1ecb80cbf5f2d5bf127e0d6d1be
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/05/2018
ms.locfileid: "34793670"
---
# <a name="limitations-of-xamarin-live-player"></a>实时的 Xamarin Player 的限制

![预览功能](~/media/shared/preview.png)

## <a name="device-requirements"></a>设备要求
Xamarin 实时播放器应用程序支持以下设备：

# <a name="androidtabandroid"></a>[Android](#tab/android)

- Android 4.2 或更高版本。
- ARM-v7a，ARM v8a、 ARM64 v8a，x86 或 x86_64 处理器。

# <a name="iostabios"></a>[iOS](#tab/ios)

- iOS 9.0 或更高版本。
- ARM64 处理器。

-----

## <a name="limitations"></a>限制

有一些限制可以运行实时的 Xamarin Player，包括以下各项的事情上：

### <a name="xamarinforms"></a>Xamarin.Forms

- 不支持自定义呈现器。
- 不支持效果。
- 不支持使用自定义可绑定属性的自定义控件。
- 不支持嵌入的资源 (即。 在 PCL 中嵌入图像或其他资源)。
- 第三方 MVVM 框架不支持 (ie。Prism、 Mvvm 跨、 Mvvm Light，等等。）。
- 不支持在 iOS 上的资产目录。

### <a name="other-project-types"></a>其他项目类型

- 实时播放器不用于本机 Android 或 iOS 项目 （Android XML 或情节提要用于用户界面）。

### <a name="misc"></a>杂项

- 反射对有限的支持 （当前影响某些常用的 NuGets，SQLite 和 Json.NET 等）。 其他 NuGets 可能仍受支持。
- 某些系统类不能重写 （例如，不能实现子类）。
- （但是它已配置为常见的操作，如照片库 access） 的实时的 Xamarin Player 应用中不能处理需要设置某些平台功能。
- 忽略自定义的目标和生成步骤。 例如，不能合并 Fody、 Refit、 AutoFac 和 AutoMapper 之类的工具。
- 在 Android 上不支持 F # 项目并在 iOS 上的支持有限
- 可能不支持使用自定义的泛型类和接口的高级的方案。

任何其他问题请报告上[bugzilla](https://aka.ms/live-player-report-issue)。

## <a name="related-links"></a>相关链接

- [疑难解答](~/tools/live-player/troubleshooting.md)
- [安装](~/tools/live-player/install.md)
