---
title: 目标 Sharpie 验证属性
description: 本文档介绍了客观 Sharpie 生成的 [Verify] 属性。 [Verify] 特性突出显示给开发人员，他们应该在其中手动验证客观 Sharpie 的输出。
ms.prod: xamarin
ms.assetid: 107FBCEA-266B-4295-B7AA-40A881B82B7B
author: conceptdev
ms.author: crdun
ms.date: 01/15/2016
ms.openlocfilehash: eb4a992255fa26be1e83f27f93755e0fd8bda0a4
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/06/2019
ms.locfileid: "70765677"
---
# <a name="objective-sharpie-verify-attributes"></a>目标 Sharpie 验证属性

你经常会发现，客观 Sharpie 生成的绑定将使用`[Verify]`属性进行批注。 这些属性指示您应该通过将绑定与原始 C/目标-C 声明（将在绑定声明中的注释中提供）进行比较来_验证_目标 Sharpie 是否执行了正确的操作。

建议对_所有_绑定声明进行验证，但对于使用`[Verify]`特性批注的声明，最有可能_需要_进行验证。 这是因为在许多情况下，原始本机源代码中没有足够的元数据来推断如何最好地生成绑定。 你可能需要在标头文件中引用文档或代码注释以做出最佳的绑定决策。

验证绑定正确或已修复后，请从绑定中_删除_该`[Verify]`属性。

> [!IMPORTANT]
> `[Verify]`特性特意导致C#编译错误，因此强制验证绑定。 查看（并可能`[Verify]`更正）代码时，应删除属性。

## <a name="verify-hints-reference"></a>验证提示引用

提供给该属性的提示参数可与下面的文档交叉引用。 绑定完成后， `[Verify]`控制台上还会提供任何生成的属性的文档。

|`[Verify]`提示|描述|
|---|---|
|InferredFromPreceedingTypedef|此声明的名称是从原始本机源代码的前面紧邻`typedef`的常用约定推断出来的。 验证推断的名称是否正确，因为此约定是不明确的。|
|ConstantsInterfaceAssociation|没有一种欺骗方法来确定外部变量声明可以关联的目标 C 接口。 这些实例将作为`[Field]`部分接口中的属性绑定到近乎具体的接口以生成更直观的 API，这可能会完全消除 "常量" 接口。|
|MethodToProperty|由于惯例（如不使用任何参数并C#返回一个值，因此，目标 C 方法被绑定为属性）。 通常，这类方法应作为属性绑定以显示更好 API，但有时会发生误报，并且绑定实际上应为方法。|
|StronglyTypedNSArray|本机`NSArray*`被绑定为`NSObject[]`。 可以根据通过 API 文档（如标头文件中的注释）或通过测试检查数组内容中的预期设置，更强地在绑定中键入数组。 例如，仅包含 NSNumber * instancescan 的 NSArray * 将绑定为`NSNumber[]` `NSObject[]`而不是。|

你还可以使用`sharpie verify-docs`工具快速接收提示的文档，例如：

```csharp
sharpie verify-docs InferredFromPreceedingTypedef
```
