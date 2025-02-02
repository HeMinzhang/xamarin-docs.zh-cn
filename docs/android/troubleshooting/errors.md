---
title: Xamarin Android 错误矩阵
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 7EBE4C01-8EFC-4B7E-97BA-D879994F59AB
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/13/2018
ms.openlocfilehash: 037b5a8939ab5866a10e7ecc58a1e2a5bdfb5bfd
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/06/2019
ms.locfileid: "70757399"
---
# <a name="xamarinandroid-errors-matrix"></a>Xamarin Android 错误矩阵

## <a name="errors-reference"></a>错误参考

本文档提供有关 Xamarin 中各种错误代码的信息。

|类别|描述|
|--- |--- |
|XA0xxx|mandroid 错误|
|XA1xxx|文件复制/符号链接（项目相关）错误|
|XA2xxx|链接器错误|
|XA3xxx|AOT 错误|
|XA4xxx|代码生成错误|
|XA5xxx|GCC 和工具链错误|
|XA6xxx|mandroid 内部工具错误|
|XA7xxx|保留|
|XA8xxx|保留|
|XA9xxx|授权错误|

## <a name="error-codes"></a>错误代码

### <a name="xa0xxx-errors"></a>XA0xxx 错误

|错误代码|描述|
|--- |--- |
|XA0000|意外错误-请填写[bug 报告](https://github.com/xamarin/xamarin-android/issues/new)。|
|XA0001|"-提供了无任何特定于设备的操作的 devname。|
|XA0002|未能分析环境变量 "{0}"。|
|XA0003|应用程序名称{0}".exe" 与 SDK 或产品程序集（.dll）名称冲突。|
|XA0004|New refcounting 逻辑还要求 sgen 启用。|
|XA0005|输出目录 "{0}" 不存在。|
|XA0006|"{0}" 上没有 unixodbc-devel 平台，请使用--platform = x-plat 指定 SDK|
|XA0007|根程序集 "{0}" 不存在。|
|XA0008|只应提供一个根程序集。|
|XA0009|加载程序集时出错{0}：。|
|XA0010|无法分析命令行参数： {0}。|
|XA0011|{0}是针对比 monotouch.dialog 支持的更新运行{1}时（）生成的。|
|XA0012|提供了不完整的数据来{0}完成 ""。|
|XA0013|分析支持还需要启用 sgen。|
|XA0014|iOS {0}不支持生成面向 ARMv6 的应用程序。|
|XA0020|无法确定 mandroid 路径。|
|XA0100|EmbeddedNativeLibrary "{0}" 在 Android 应用程序项目中无效。 请改用 AndroidNativeLibrary。|

### <a name="xa1xxx-errors"></a>XA1xxx 错误

|错误代码|描述|
|--- |--- |
|XA1001|在指定的目录中找不到应用程序。|
|XA1002|未能创建符号链接，文件已被复制。|
|XA1003|无法终止应用程序 "{0}"。 可能需要手动终止应用程序。|
|XA1004|无法获取已安装应用程序的列表。|
|XA1005|无法在设备 "{0}{1}" 上终止应用程序 ""： {2}。 可能需要手动终止应用程序。|
|XA1006|无法在设备 "{0}{1}" 上安装应用程序 ""： {2}。|
|XA1007|无法在设备 "{0}{1}" 上启动应用程序 ""： {2}。 你仍可以通过点击来手动启动应用程序。|
|XA1008|未能启动模拟器： {0}。|
|XA1009|未能将程序集 "{0}" 复制到 "{1}"：。 {2}|
|XA1010|未能加载程序集 "{0}"：。 {1}|
|XA1011|未能添加缺少的资源文件： "{0}"。|
|XA1101|无法启动应用程序。|
|XA1102|无法附加到应用程序（以终止它）： {0}。|
|XA1103|无法分离。|
|XA1104|未能发送数据包： {0}。|
|XA1105|意外的响应类型。|
|XA1106|无法获取设备上的应用程序列表：请求超时。|
|XA1107|应用程序未能启动。|
|XA1201|无法加载模拟器： {0}。|
|XA1301|已忽略本机{0}库 "{1}" （），因为它与当前的生成体系结构（{2}）不匹配。|

### <a name="xa2xxx-errors"></a>XA2xxx 错误

|错误代码|描述|
|--- |--- |
|XA2001|无法链接程序集。|
|XA2002|无法解析引用： {0}。|
|XA2003|选项 "{0}" 将被忽略，因为链接已禁用。|
|XA2004|找不到额外的{0}链接器定义文件 ""。|
|XA2005|未能分析 "{0}" 中的定义。|
|XA2006|未能解析对 "{0}{2}" 中的元数据项{1}"" 的引用（在 "" 中定义）。|

### <a name="xa3xxx-errors"></a>XA3xxx 错误

这些是 AOT 错误。

|错误代码|描述|
|--- |--- |
|XA3001|无法对程序集 "{0}" 进行 AOT。|
|XA3002|AOT 限制：方法 "{0}" 必须是静态的，因为它是用 [MonoPInvokeCallback] 修饰的。|
|XA3003|冲突--debug 和--llvm 选项。 已禁用软调试。|

### <a name="xa4xxx-errors"></a>XA4xxx 错误

这些是代码生成错误。

|错误代码|描述|
|--- |--- |
|XA4001|主模板无法 expansed 到 "{0}"。|
|XA4101|注册器无法为类型 "{0}" 生成签名。|
|XA4102|注册器在方法 "{0}{2}" 的签名中发现无效的类型 ""。 请改用{1}""。|
|XA4103|注册器在方法 "{0}{2}" 的签名中发现无效的类型 ""：该类型实现 INativeObject，但没有采用两个（IntPtr，bool）参数的构造函数。|
|XA4104|注册器无法封送方法 "{0}{1}" 的签名中类型 "" 的返回值。|
|XA4105|注册器无法封送方法 "{0}{1}" 的签名中 "" 类型的参数。|
|XA4106|注册器无法封送方法 "{0}{1}" 的签名中结构 "" 的返回值。|
|XA4107|注册器无法封送方法 "{0}{1}" 的签名中 "" 类型的参数。|
|XA4108|注册器无法获取托管类型 "{0}" 的 ObjectiveC 类型。|
|XA4109|未能编译生成的注册器代码。 请提交[bug 报告](http://bugzilla.xamarin.com)。|
|XA4110|注册器无法封送方法 "{0}{1}" 的签名中 "" 类型的 out 参数。|
|XA4111|注册器无法在方法 "{0}{1}" 中为类型 "" 生成签名。|
|XA4200|只能为 "claas" 类型生成 ACW。|
|XA4201|无法确定类型{0}的 JNI 名称。|
|XA4203|指定的类型名称必须是完全限定的。|
|XA4204|无法解析接口类型 "{0}"。 是否缺少程序集引用?|
|XA4205|只能对具有0个参数的方法使用 [ExportField]。|
|XA4206|[Export] 不能用于泛型类型。|
|XA4207|[ExportField] 不能用于泛型类型。|
|XA4208|不能对返回 void 的方法使用 [ExportFieldAttribute]。|
|XA4209|无法为类创建 JavaTypeInfo： {0} ，原因为。 {1}|
|XA4210|当你使用 ExportAttribute 或 ExportFieldAttribute 时，需要添加对的引用。|
|XA4211|//uses-sdk/@android:targetSdkVersion Androidmanifest.xml "{0}" 小于 $ （TargetFrameworkVersion） "{1}"。 使用 API{1}进行 ACW 编译。|

### <a name="xa5xxx-errors"></a>XA5xxx 错误

|错误代码|描述|
|--- |--- |
|XA5101|缺少 "{0}" 编译器。 请安装 Android NDK。|
|XA5102|未能从程序集转换为本机代码。 请提交[bug 报告](http://bugzilla.xamarin.com)。|
|XA5103|未能编译文件 "{0}"。 请提交[bug 报告](http://bugzilla.xamarin.com)。|
|XA5201|本机链接失败。 请查看向 gcc 提供的用户标志：{0}|
|XA5202|本机链接失败。 请查看生成日志。|
|XA5303|本机链接警告： {0}。|
|XA5300|Android SDK 找不到或未完全安装。|
|XA5301|缺少 "条带" 工具。 请安装 Xcode "命令行工具" 组件。|
|XA5302|缺少 "dsymutil" 工具。 请安装 Xcode "命令行工具" 组件。|
|XA5203|未能生成调试符号（dSYM 目录）。 请查看生成日志。|
|XA5204|无法去除最终的二进制文件。 请查看生成日志。|
|XA5205|缺少 "aapt" 工具。 请安装 Android SDK 的生成工具包。|
|XA5206|{0}。 Android 资源目录{1}不存在。|
|XA5207|{0}。 Java 库文件{1}不存在。|
|XA5208|下载失败。 请下载{0}并将其{1}放入目录。|
|XA5209|解压缩失败。 请下载{0}并将其解压缩{1}到目录。|
|XA5210|{0}。 本机库文件{1}不存在。|

### <a name="xa6xxx-errors"></a>XA6xxx 错误

|错误代码|描述|
|--- |--- |
|XA6001|运行版本的 Mono.cecil 不支持程序集剥离。|
|XA6002|无法提取程序集 "{0}"。|
|XA6003|System.unauthorizedaccessexception 消息。|

### <a name="xa9xxx-errors"></a>XA9xxx 错误

这些错误代码为 "授权" 和 "激活错误"。

#### <a name="xa9000"></a>XA9000

 **导致**许可证已过期

 **检查时间：** Build

|发起|独立|业务（试用）|业务|企业|
|--- |--- |--- |--- |--- |
|警告|警告|警告|警告|警告|

#### <a name="xa9001"></a>XA9001

 **导致**试用已过期

 **检查时间：** Build

|发起|独立|业务（试用）|业务|企业|
|--- |--- |--- |--- |--- |
|警告|警告|警告|警告|警告|

#### <a name="xa9002"></a>XA9002

 **导致**AndroidJavaSource

 **检查时间：** Build

|发起|独立|业务（试用）|业务|企业|
|--- |--- |--- |--- |--- |
|错误|确定|确定|确定|确定|

 **导致**AndroidJavaLibrary

 **检查时间：** Build

|发起|独立|业务（试用）|业务|企业|
|--- |--- |--- |--- |--- |
|错误|确定|确定|确定|确定|

 **如果绑定程序集嵌入了 .jar，则会在包时（而不是生成时间）捕获该程序集。**

 **导致**AndroidNativeLibrary

 **检查时间：** Build

|发起|独立|业务（试用）|业务|企业|
|--- |--- |--- |--- |--- |
|错误|确定|确定|确定|确定|

#### <a name="xa9003"></a>XA9003

 **导致**System.Runtime.Serialization

 **检查时间：** package

|发起|独立|业务（试用）|业务|企业|
|--- |--- |--- |--- |--- |
|错误|错误|确定|确定|确定|

 **导致**System.ServiceModel.Web

 **检查时间：** package

|发起|独立|业务（试用）|业务|企业|
|--- |--- |--- |--- |--- |
|错误|错误|确定|确定|确定|

 **导致**Mono.Data.Tds

 **检查时间：** Build

|发起|独立|业务（试用）|业务|企业|
|--- |--- |--- |--- |--- |
|错误|错误|确定|确定|确定|

 **这是被允许的。**

 **导致**Mono.Android.Export

 **检查时间：** Build

|发起|独立|业务（试用）|业务|企业|
|--- |--- |--- |--- |--- |
|错误|确定|确定|确定|确定|

#### <a name="xa9004"></a>XA9004

 **原因：** --分析

 **检查时间：** package

|发起|独立|业务（试用）|业务|企业|
|--- |--- |--- |--- |--- |
|错误|错误|确定|确定|确定|

#### <a name="xa9005"></a>XA9005

 **导致**大小限制（32kb）。

 **检查时间：** package

|发起|独立|业务（试用）|业务|企业|
|--- |--- |--- |--- |--- |
|错误|确定|-|-|-|

#### <a name="xa9006"></a>XA9006

 **导致**SqlClient 命名空间。

 **检查时间：** package

|发起|独立|业务（试用）|业务|企业|
|--- |--- |--- |--- |--- |
|错误|错误|确定|确定|确定|

#### <a name="xa9008"></a>XA9008

 **导致**正在从命令行生成。

 **检查时间：** Build

|发起|独立|业务（试用）|业务|企业|
|--- |--- |--- |--- |--- |
|错误|错误|确定|确定|确定|

#### <a name="xa9009"></a>XA9009

 **导致**缺少序列号。

 **检查时间：** Untestable

|发起|独立|业务（试用）|业务|企业|
|--- |--- |--- |--- |--- |
|错误|错误|错误|错误|错误|

#### <a name="xa9010"></a>XA9010

 **导致**ProductId 无效。

 **检查时间：** Build

|发起|独立|业务（试用）|业务|企业|
|--- |--- |--- |--- |--- |
|错误|错误|错误|错误|错误|

等效于 XA9018。

#### <a name="xa9011"></a>XA9011

 **导致**未能更新许可证文件（到新的文件格式）。

 **检查时间：** 激活

|发起|独立|业务（试用）|业务|企业|
|--- |--- |--- |--- |--- |
|错误|错误|错误|错误|错误|

#### <a name="xa9012"></a>XA9012

 **导致**无 internet

 **检查时间：** 激活

|发起|独立|业务（试用）|业务|企业|
|--- |--- |--- |--- |--- |
|错误|错误|错误|错误|错误|

#### <a name="xa9013"></a>XA9013

 **导致**未知错误

 **检查时间：** 激活

|发起|独立|业务（试用）|业务|企业|
|--- |--- |--- |--- |--- |
|错误|错误|错误|错误|错误|

#### <a name="xa9014"></a>XA9014

 **导致**激活代码无效

 **检查时间：** 激活

|发起|独立|业务（试用）|业务|企业|
|--- |--- |--- |--- |--- |
|错误|错误|错误|错误|错误|

#### <a name="xa9017"></a>XA9017

 **导致**激活服务器不会返回有效的许可证。

 **检查时间：** 激活

|发起|独立|业务（试用）|业务|企业|
|--- |--- |--- |--- |--- |
|错误|错误|错误|错误|错误|

#### <a name="xa9018"></a>XA9018

**导致**许可证无效

**检查时间：** Build

|发起|独立|业务（试用）|业务|企业|
|--- |--- |--- |--- |--- |
|-|-|错误|-|-|
