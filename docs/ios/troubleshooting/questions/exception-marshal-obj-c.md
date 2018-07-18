---
title: 为什么不会通过与我 iOS 9 的应用程序： System.Exception： 无法封送 OBJECTIVE-C 的对象？
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 8805ABEC-48D4-4CCB-A226-3A5B2ECE4BF0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: f7382ac963249a3f3646a917d8700e3a12873ec9
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/04/2018
ms.locfileid: "30775970"
---
# <a name="why-does-my-ios-9-app-fail-with-systemexception-failed-to-marshal-the-objective-c-object"></a>为什么不会通过与我 iOS 9 的应用程序： System.Exception： 无法封送 OBJECTIVE-C 的对象？

你可能会看到此窗体的错误：

> System.Exception： 未能封送 Objective C 对象...找不到此对象的现有的托管的实例...

IOS 9 中的 API 更改需要当作为基础 API 调用非托管的代码，需要它时，使用回调构造函数。 使用以下行添加到类回调构造函数： 

`public foo (IntPtr handle) : base (handle) ` 

### <a name="next-steps"></a>后续步骤

有关进一步的帮助，请与我们联系，或如果此问题仍然存在即使利用的上述信息，请参阅[哪些支持选项均可用于 Xamarin？](~/cross-platform/troubleshooting/support-options.md)有关联系人选项，建议的信息以及如何如果需要的文件的新 bug。 
