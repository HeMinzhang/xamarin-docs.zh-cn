---
title: 绑定 Objective-C
description: 本文档提供介绍如何创建 C# 绑定到 OBJECTIVE-C 代码，使开发人员能够使用现成的库，在 Xamarin 应用程序中的各种指南的链接。
ms.prod: xamarin
ms.assetid: DBBAA086-BB0F-8161-DF44-632F4F5DFE5D
author: asb3993
ms.author: amburns
ms.date: 01/25/2016
ms.openlocfilehash: 3f1e1ce324e849c0c939d936eb6ee1470cf24a3b
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/05/2018
ms.locfileid: "37855151"
---
# <a name="binding-objective-c"></a>绑定 Objective-C

本部分包含多种文档，其中包括创建绑定到 OBJECTIVE-C 的库，因此它们可以从 C# 使用 Xamarin.iOS 或 Xamarin.Mac 创建的应用程序调用。

##  <a name="overviewcross-platformmaciosbindingoverviewmd"></a>[概述](~/cross-platform/macios/binding/overview.md)

本文档包含一些的绑定发生内部结构。 它是一个高级的文档使用一些技术的信息。

##  <a name="binding-objective-c-librariescross-platformmaciosbindingobjective-c-librariesmd"></a>[绑定 Objective-C 库](~/cross-platform/macios/binding/objective-c-libraries.md)

本文档介绍用于创建 C# 绑定的 Objective C Api 和 Objective C 中的惯例如何映射到在.NET 中使用的惯例的过程。
如果要绑定只是 C Api，你应为此，P/Invoke framework 使用标准.NET 机制。

##  <a name="binding-definition-reference-guidecross-platformmaciosbindingbinding-types-referencemd"></a>[绑定定义参考指南](~/cross-platform/macios/binding/binding-types-reference.md)

这是介绍了所有可用来驱动绑定生成进程的绑定作者的属性的参考指南。


## <a name="objective-sharpiecross-platformmaciosbindingobjective-sharpieindexmd"></a>[目标 Sharpie](~/cross-platform/macios/binding/objective-sharpie/index.md)

目标 Sharpie 是一个命令行工具来帮助启动绑定的第一次传递。 其工作原理是分析要映射到的公共 API 的本机库的标头文件[绑定定义](~/cross-platform/macios/binding/objective-c-libraries.md)（也可以手动完成该过程）。

## <a name="ios"></a>iOS

[IOS 绑定页](~/ios/platform/binding-objective-c/index.md)链接回这些常用的绑定资源此外到下面的示例。

### <a name="walkthrough-binding-an-objective-c-libraryiosplatformbinding-objective-cwalkthroughmd"></a>[演练： 绑定 OBJECTIVE-C 库](~/ios/platform/binding-objective-c/walkthrough.md)

本文提供了创建使用开放源代码的绑定项目的分步演练[InfColorPicker](https://github.com/InfinitApps/InfColorPicker) Objective C 项目作为示例。 InfColorPicker 库提供了允许用户选择一种颜色根据其 HSB 表示形式，使用户更加友好的颜色选择的可重用视图控制器。 目标 Sharpie 将用于绑定过程中提供帮助。

### <a name="binding-sampleshttpsgithubcommonomonotouch-bindings"></a>[绑定示例](https://github.com/mono/monotouch-bindings)

创建新绑定项目时，可以是第三方绑定的集合使用引用。

## <a name="mac"></a>Mac

从历史上看[Mac 绑定](~/mac/platform/binding.md)已被大量的手动过程。 目前尚[可下载预览](https://forums.xamarin.com/discussion/59760/xamarin-mac-binding-project-preview)Mac 绑定项目对的支持未来版本的 Visual Studio for mac。



## <a name="related-links"></a>相关链接

- [iOS 绑定](~/ios/platform/binding-objective-c/index.md)
- [Mac 绑定](~/mac/platform/binding.md)
- [Xamarin 大学课程： 构建 OBJECTIVE-C 绑定库](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Xamarin 大学课程： 构建使用目标 Sharpie OBJECTIVE-C 绑定库](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)
