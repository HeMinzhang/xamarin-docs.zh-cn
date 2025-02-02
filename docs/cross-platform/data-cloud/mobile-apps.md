---
title: Microsoft Azure 移动应用
description: 本文档链接到介绍如何构建连接到 Azure 的 Xamarin 应用的指南。 其中讨论了如何使用 Xamarin Azure 组件、用户和推送通知。
ms.prod: xamarin
ms.assetid: 7B9AA8D9-C181-4C33-8AB0-2F56E4DBFC03
author: conceptdev
ms.author: crdun
ms.date: 04/02/2017
ms.openlocfilehash: b94bae8fb1b7c990c5b2478a0da143960a0bcc55
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/06/2019
ms.locfileid: "70765964"
---
# <a name="microsoft-azure-mobile-apps"></a>Microsoft Azure 移动应用

_Azure 门户文档的示例和代码下载。_

<!--
NOTE TO AUTHORS: this page is referenced from
https://azure.microsoft.com/develop/mobile/xamarin/
as https://developer xamarin com/guides/cross-platform/data-cloud/mobile-services/
A redirect has been put in place to /mobile-apps/ HOWEVER the /Resources/ .ZIP files are still located in /mobile-services/ so that the following permalinks don't break

The ZIPs in /Resources/ are also referenced by inbound links
Getting Started http://go.microsoft.com/fwlink/p/?LinkId=331359
Get started with data http://go.microsoft.com/fwlink/p/?LinkId=331302
Get started with push http://go.microsoft.com/fwlink/p/?LinkId=331303
Get started with authentication http://go.microsoft.com/fwlink/p/?LinkId=331328
Get started with Notification Hubs http://go.microsoft.com/fwlink/p/?LinkId=331329
Validate and modify data  http://go.microsoft.com/fwlink/p/?LinkId=331330
-->

这些链接适用于[Azure 移动应用](https://docs.microsoft.com/azure/app-service-mobile/)网站上提供的 Xamarin 文档。
通过下载[Azure 移动客户端](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)向 Xamarin 应用程序添加 azure 功能。

## <a name="working-with-the-xamarin-azure-component"></a>使用 Xamarin Azure 组件

有关使用[Xamarin 客户端库（组件）](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library)完成各种任务的常规文档，请参阅 Azure 移动应用。 此页面包含许多示例代码片段，不包括下面列出的每个演练文章中提供的详细说明和示例。

## <a name="getting-started"></a>入门

本文提供了有关启动和运行第一个 Xamarin Azure 应用程序的分步说明。
其中介绍了如何在门户中创建新的 Azure 移动应用，并下载并运行预配置的应用。

- [iOS](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-xamarin-ios-get-started/)
- [Android](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-xamarin-android-get-started/)
- [Xamarin.Forms](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started)

<!--
## Validate, Modify and Augment Data in Scripts

Demonstrates how to add server-side scripts to Azure Mobile Services data tables to implement server-side validation and other functionality.

- [iOS](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#errors)
- [Android](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#errors)
-->

<!--
## Add Paging to Data

A quick example of paging large sets of data using Skip() and Take().

- [iOS](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#paging)
- [Android](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#paging)
-->

## <a name="get-started-with-users"></a>用户入门

提供使用 Azure 移动服务配置和编码登录屏幕的完整说明。 支持的身份验证提供程序包括 Microsoft、Google、Facebook 和 Twitter。

- [iOS](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-ios-get-started-users/)
- [Android](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-android-get-started-users/)

## <a name="authorize-users-in-scripts"></a>在脚本中授权用户

Javascript 后端的一些示例代码

- [Todo.js](https://github.com/Azure/azure-mobile-apps-node/blob/master/samples/personal-table/tables/TodoItem.js#L38)

## <a name="get-started-with-push"></a>推送入门

有关在 Apple 和 Google 网站上配置推送通知的完整说明，然后从 Azure 移动服务向设备发送推送通知。

- [iOS](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-xamarin-ios-get-started-push)
- [Android](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-xamarin-android-get-started-push)

## <a name="get-started-with-notification-hubs"></a>通知中心入门

有关在 Apple 和 Google 网站上配置推送通知的完整说明，请配置 Azure 通知中心，然后生成到设备的推送通知。

- [iOS](https://docs.microsoft.com/azure/notification-hubs/xamarin-notification-hubs-ios-push-notification-apns-get-started)
- [Android](https://docs.microsoft.com/azure/notification-hubs/xamarin-notification-hubs-push-notifications-android-gcm)

## <a name="related-links"></a>相关链接

- [GettingStarted （示例）](https://github.com/xamarin/mobile-samples/tree/master/Azure/GettingStarted)
- [GetStartedWithData （示例）](https://github.com/xamarin/mobile-samples/tree/master/Azure/GetStartedWithData)
- [GetStartedWithUsers （示例）](https://github.com/xamarin/mobile-samples/tree/master/Azure/GetStartedWithUsers)
- [GetStartedWithPush （示例）](https://github.com/xamarin/mobile-samples/tree/master/Azure/GetStartedWithPush)
- [NotificationHubs （示例）](https://github.com/xamarin/mobile-samples/tree/master/Azure/NotificationHubs)
- [Azure 移动客户端](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
- [Azure 移动应用学习路径](https://azure.microsoft.com/documentation/learning-paths/appservice-mobileapps/)

<!--
- [ValidateModifyData (sample)](https://github.com/xamarin/mobile-samples/tree/master/Azure/ValidateModifyData)
-->
