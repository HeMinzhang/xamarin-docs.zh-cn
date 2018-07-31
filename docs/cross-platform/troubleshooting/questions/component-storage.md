---
title: 存储我的计算机上的组件？
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5EBB49EE-39E5-428B-866F-9FC1BB215B31
author: asb3993
ms.author: amburns
ms.date: 05/08/2018
ms.openlocfilehash: 4152c8ef7eeba3748d9244e27e48f3f9a2c0019b
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350714"
---
# <a name="where-are-the-components-stored-on-my-machine"></a>存储我的计算机上的组件？

每当你的 Xamarin 组件安装到应用程序项目时，它将被放置两个位置：

1. 在解决方案文件夹的根级别上的组件文件夹中。 如果从解决方案中的所有项目中删除该组件，它将从此中删除此文件夹。

2. 副本也存储在以下位置：
    - Windows：`%LocalAppData%\Xamarin\Cache\Components`
    - Mac: `~/Library/Caches/Xamarin/Components`

若要从系统中完全删除一个组件，请删除它从你的项目/解决方案和更高版本的缓存文件夹。
