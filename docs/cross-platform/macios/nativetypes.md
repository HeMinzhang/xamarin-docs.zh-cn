---
title: IOS 和 macOS 的本机类型
description: 本文档介绍了 Xamarin 的 Unified API 根据编译目标体系结构的需要，如何将 .NET 类型映射到32位和64位本机类型。
ms.prod: xamarin
ms.assetid: B5237770-0FC3-4B01-9E22-766B35C9A952
author: conceptdev
ms.author: crdun
ms.date: 01/25/2016
ms.openlocfilehash: f370bc816fa72f94af8dae32ac295448f858dbb8
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/04/2019
ms.locfileid: "70290797"
---
# <a name="native-types-for-ios-and-macos"></a>IOS 和 macOS 的本机类型

Mac 和 iOS Api 使用32位平台上始终为32位的特定于体系结构的数据类型，在64位平台上使用64位。

例如，在32位系统上， `NSInteger`目标 C 将`int32_t`数据类型映射到，在 64 `int64_t`位系统上映射到。

若要匹配此行为，请在我们的统一 API 上，将上一个`int` （在 .net 中定义为`System.Int32`always）的使用替换为新的数据类型`System.nint`：。 可以将 "n" 视为 "本机"，因此可以将平台的本机整数类型视为。

对于这两种新数据类型，将根据编译标志为32位和64位体系结构编译相同的源代码。

## <a name="new-data-types"></a>新数据类型

下表显示了我们的数据类型更改，以匹配这一新的 32/64 位环境：

|本机类型|32位后备类型|64位后备类型|
|--- |--- |--- |
|`System.nint`|`System.Int32` (`int`)|`System.Int64` (`long`)|
|`System.nuint`|`System.UInt32` (`uint`)|`System.UInt64` (`ulong`)|
|`System.nfloat`|`System.Single` (`float`)|`System.Double` (`double`)|

我们选择了这些名称，以C#允许你的代码以与今天相同的方式来查找。

### <a name="implicit-and-explicit-conversions"></a>隐式转换和显式转换

新数据类型的设计旨在允许单个C#源文件自然使用32或64位存储，具体取决于主机平台和编译设置。

这就需要我们设计一组对特定于平台的数据类型的隐式和显式转换，并将其转换为 .NET 整型和浮点数据类型。

当不可能丢失数据（32位值存储在64位空间上）时，将提供隐式转换运算符。

当有可能存在数据丢失时，将提供显式转换运算符（64位值存储在32或可能32存储位置）。

`int``nint` `nuint`和均可隐式转换为，并且`nfloat` 32 位将始终适合32或64位。 `float` `uint`

`nint``long` `ulong`和均可隐式转换为，并且`double` 32 或64位值将始终适合64位存储。 `nfloat` `nuint`

`nint`必须使用从、 `nuint`和`nfloat`到`int`和`float`的显式转换，因为本机类型可能包含64位的存储。 `uint`

`long`必须使用从、 `ulong`和`double`到`nint`中`nuint` 的显式转换，因为本机类型可能只能保存32`nfloat`位的存储。

## <a name="coregraphics-types"></a>CoreGraphics 类型

与 CoreGraphics 一起使用的 "点"、"大小" 和 "矩形" 数据类型使用32或64位，具体取决于它们运行所在的设备。  最初绑定 iOS 和 Mac Api 时，我们使用了现有的数据结构，这些结构与主机平台的大小（中`System.Drawing`的数据类型）相匹配。

移动到**统一**时，需要将的`System.Drawing`实例替换为其`CoreGraphics`对应项，如下表所示：

|系统中的旧类型|新数据类型 CoreGraphics|描述|
|--- |--- |--- |
|`RectangleF`|`CGRect`|保留浮点矩形信息。|
|`SizeF`|`CGSize`|保留浮点大小信息（宽度、高度）|
|`PointF`|`CGPoint`|保存浮点，点信息（X，Y）|

旧数据类型使用浮动来存储数据结构的元素，而新的数据类型使用`System.nfloat`。

## <a name="related-links"></a>相关链接

- [使用跨平台应用中的本机类型](~/cross-platform/macios/native-types-cross-platform.md)
- [经典与 Unified API 差异](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/api_changes/classic-vs-unified-8.6.0/index.md)
