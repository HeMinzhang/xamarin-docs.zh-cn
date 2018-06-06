---
title: 处理触摸 Xamarin.iOS 应用程序中
description: 此文档链接到指南描述如何使用触摸、 多点触控、 笔势和 3D Touch Xamarin.iOS 应用程序中。
ms.prod: xamarin
ms.assetid: E3904713-6018-4755-A315-EB045DFB3500
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 01/23/2017
ms.openlocfilehash: eb8dce8b13345c13a6f95ae7784bd135e7d1f1f5
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/05/2018
ms.locfileid: "34784152"
---
# <a name="handling-touch-in-xamarinios-apps"></a>处理触摸 Xamarin.iOS 应用程序中

与其他移动平台，iOS 有多种方式来处理触摸屏输入。 它可以支持多点触控 — 在屏幕上的联系人的许多点-和复杂的笔势。 本指南介绍一些概念，以及在 iOS 上实施触控和手势 particularities。

iOS 封装在触摸数据`UITouch`类，该类将提供给应用程序可以通过一系列`UIResponder`方法。 应用程序可以重写这些方法中的子类`UIView`和`UIViewController`，这两种继承`UIResponder`。

除了捕获触摸数据，iOS 提供种解释模式到手势收尾工作。 这些笔势识别器反过来可用来解释特定于应用程序的命令，如图像的旋转或页面的一个轮次。 iOS 提供丰富的类来处理具有最小添加的代码的常见手势集合。

触控和手势识别器之间选择可能很令人困惑的一个。 本指南建议，一般情况下，首选项应授予给笔势识别器。 笔势识别器作为离散的类，该类提供更大程度分离问题和更好地封装的实现。 这可以直观地共享不同的视图，最大限度编写的代码之间的逻辑。

但是，有何时需要使用低级别触摸处理和甚至跟踪多个指，例如，若要创建 finger-paint 程序的时间。

## <a name="sections"></a>部分

-  [iOS 中的触控](touch-in-ios.md)
-  [演练： 在 iOS 中使用触摸](ios-touch-walkthrough.md)
-  [多点触控跟踪](touch-tracking.md)

本指南用作简介触摸在 iOS 中。 有关在 iOS 中使用 3D Touch 和 Haptic 反馈的详细信息，其中中引入了 iOS 9 和 10 分别，请参阅下面的特定指导原则：

* [3D Touch](~/ios/platform/3d-touch.md)
* [提供 Haptic 反馈](~/ios/user-interface/ios-ui/haptic-feedback.md)

## <a name="related-links"></a>相关链接

- [iOS Touch 启动 （示例）](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_start)
- [iOS 最终 Touch （示例）](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_final)
- [FingerPaint （示例）](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/FingerPaint)
