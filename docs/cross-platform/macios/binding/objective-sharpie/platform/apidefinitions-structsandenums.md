---
title: ApiDefinitions 和 StructsAndEnums 文件
description: 本文档介绍了生成目标 Sharpie ApiDefinitions.cs 和 StructsAndEnums.cs 文件。 然后使用这些文件从 C# 访问 Objective C 代码。
ms.prod: xamarin
ms.assetid: AC2087C0-BA54-46D8-B70C-6972941C8F73
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: df8d4508db14116a5b36e893f161ac891d58dc46
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/05/2018
ms.locfileid: "37855177"
---
# <a name="apidefinitions--structsandenums-files"></a>ApiDefinitions 和 StructsAndEnums 文件

当目标 Sharpie 已经成功地运行时，它将生成`Binding/ApiDefinitions.cs`和`Binding/StructsAndEnums.cs`文件。
这两个文件都添加到绑定项目在 Visual Studio for Mac 或直接传递给`btouch`或`bmac`工具以生成最终的绑定。

在中*某些*这些生成的文件可能满足所有需要的是，但更通常开发人员将需要手动修改这些生成的文件来解决可能不会自动处理工具 （如那些已标记的任何问题的情况下与[`Verify`属性](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md))。

接下来的步骤包括：

- **调整名称**： 有时要调整的方法和类以匹配.NET Framework 设计准则的名称。
- **方法或属性**： 有时使用目标 Sharpie 的试探法将选择要转换为一个属性的方法。 此时，无法确定这是否是预期的行为。
- **挂接事件**： 无法链接您的类与委托类和自动生成的事件。
- **通知挂钩**： 不能从纯标头文件中提取 API 约定的通知，这将需要的 API 文档行程。 如果你想强类型化的通知，你需要更新结果。
- **API 策展**： 在此点，您可以选择提供额外的构造函数中，添加 （以便留出的 C# 初始化上构造语法） 的方法、 运算符重载和实现自己的接口上的额外定义文件。

请参阅[绑定 API](~/cross-platform/macios/binding/objective-c-libraries.md)描述以查看这些文件如何适应绑定过程中，如下面的关系图中所示：

![](apidefinitions-structsandenums-images/binding-flowchart.png "在此图中显示绑定过程")

请参阅[绑定类型参考](~/cross-platform/macios/binding/binding-types-reference.md)有关这些文件的内容的详细信息。

## <a name="related-links"></a>相关链接

- [Xamarin 大学课程： 构建 OBJECTIVE-C 绑定库](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Xamarin 大学课程： 构建使用目标 Sharpie OBJECTIVE-C 绑定库](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)
