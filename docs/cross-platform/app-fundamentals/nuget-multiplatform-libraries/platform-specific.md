---
title: NuGet 为创建新的特定于平台的类库项目
description: 本文档介绍如何创建一个的 NuGet 软件包，它包含多个平台的特定于平台的代码。
ms.prod: xamarin
ms.assetid: D8BC4906-805F-4AFB-8D1A-88B7BF87E17F
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 00a02973d6016ad63e4317279515acc2b4e2e81b
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/05/2018
ms.locfileid: "34780638"
---
# <a name="creating-new-platform-specific-library-projects-for-nuget"></a>NuGet 为创建新的特定于平台的类库项目

面向特定平台，如 iOS 和 Android 的多平台库项目最适用于共享项目中。

NuGet 可以包含特定于 iOS 和 Android 的代码，以及公用的.NET 代码。

创建多个程序集和内置于单个的 NuGet 包。 NuGet 标准确保包可添加到所有支持的项目类型，如 Xamarin.iOS 和 Android 项目。

## <a name="steps-to-create-a-cross-platform-library-nuget"></a>若要创建跨平台库 NuGet 的步骤

1. 选择**文件 > 新解决方案**(或右键单击现有的解决方案，然后选择**添加 > 新建项目**)。

2. 选择**多平台库**从**多平台 > 库**部分：

  [![](platform-specific-images/mulitplatform-library-sml.png "配置的单个基本代码的多平台库")](platform-specific-images/multiplatform-library.png#lightbox)

3. 输入**名称**和**说明**，然后选择**特定于平台**:

  [![](platform-specific-images/specific-configure-sml.png "配置适用于 iOS 和 Android 的特定于平台的库")](platform-specific-images/specific-configure.png#lightbox)

4. 完成向导。 以下项目添加到解决方案中：

  - **Android 项目**– （可选） 可以将特定于 Android 的代码添加到此项目。
  - **iOS 项目**– （可选） 可以将特定于 iOS 的代码添加到此项目。
  - **NuGet 项目**– 对此项目中添加任何代码。 它引用其他项目，并包含 NuGet 程序包输出的元数据配置。
  - **共享项目**– 应将常用代码添加到此项目，包括内的特定于平台的代码`#if`编译器指令。

5. 右键单击 NuGet 项目并选择**选项**，然后打开**NuGet 包 > 元数据**部分，然后输入[必需的元数据](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)（为以及任何可选元数据）：

  [![](platform-specific-images/specific-metadata-sml.png "输入必需的元数据")](platform-specific-images/specific-metadata.png#lightbox)

6. 另外，请在**项目选项**窗口中，打开**引用程序集**部分并选择共享的库将支持通过"诱饵 and 交换机"的 PCL 配置文件：

  ![](platform-specific-images/specific-reference-assemblies.png "此外在项目选项窗口中，打开引用程序集部分并选择共享的库将支持通过诱饵哪些 PCL 配置文件")

  > [!NOTE]
> "诱饵 and 交换机"意味着 PCL 程序集将仅包含由 （它不能包含特定于平台的代码） 的库公开的 API。 NuGet 添加到 Xamarin 项目，将根据 PCL 中，编译共享的库，但特定于平台的程序集包含实际使用的 iOS 或 Android 项目的代码。

7. 右键单击项目并选择**创建 NuGet 包**（或生成或部署解决方案） 和 **.nupkg** NuGet 包文件将保存在 **/bin/** 文件夹 (调试或发布，具体取决于配置） 时。

  ![](platform-specific-images/create-nuget-package.png "NuGet 包文件将保存在 bin 文件夹中调试或发布，具体取决于配置")


## <a name="verifying-the-output"></a>验证输出

NuGet 包还为 ZIP 文件，因此可以检查生成的包的内部结构。

选择以下屏幕截图显示特定于平台的 NuGet 支持 iOS 和 Android，并且具有两个引用程序集的内容：

![](platform-specific-images/nuget-output.png "NuGet 包中包含文件")


## <a name="related-links"></a>相关链接

- [元数据指南](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)
