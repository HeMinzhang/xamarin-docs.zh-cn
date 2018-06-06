---
title: iOS Backgrounding 技术
description: 本文档链接到指南描述在 iOS 中的各种 backgrounding 技术： 后台任务、 后台传输服务、 背景提取和远程通知。
ms.prod: xamarin
ms.assetid: 011A8D48-1CDC-486A-A2B0-C4946118E7A9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: ebf3c07a319a79994093f89f8e54f4cba7402533
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783750"
---
# <a name="ios-backgrounding-techniques"></a>iOS Backgrounding 技术

在以下部分中，我们将探讨现有 backgrounding 选项旁边的以下 iOS 功能：

-  [机会后台任务](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/ios-backgrounding-with-tasks.md#background_tasks_in_iOS_7)-通过机会小区块中运行后台任务，设备处于唤醒状态以进行其他处理时保留的电池使用时间。
-  [后台传输服务](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/ios-backgrounding-with-tasks.md#background-transfers)-可靠地上载和下载文件而不考虑网络状态或文件大小。
-  [后台提取](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/updating-an-application-in-the-background.md#background_fetch)-刷新应用程序从后台系统确定时间间隔。
-  [远程通知](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/updating-an-application-in-the-background.md#remote_notifications)-使用推送通知，以触发后台中的内容更新之前在用户打开该应用程序，用于通知用户，或以无提示方式更新的选项。
-  **后台 UI 更新**-为用户、 准备应用程序 UI 和更新应用程序的快照，一切都是通过背景。
