---
title: 多个平台上的共享代码
description: 本文档所链接到各种指南描述用于共享代码，包括可移植类库、 共享的项目、.NET Standard 和 NuGet 的技术。
ms.prod: xamarin
ms.assetid: 7D179ACF-09A6-46EE-B49D-E27AB5F09CD4
author: conceptdev
ms.author: crdun
ms.date: 07/18/2018
ms.openlocfilehash: 3a2c3f98e3ba83db0794a68ff1d62a9845a111c0
ms.sourcegitcommit: 46bb04016d3c35d91ff434b38474e0cb8197961b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/26/2018
ms.locfileid: "39270184"
---
# <a name="sharing-code-on-multiple-platforms"></a>共享多个平台上的代码

这些文章介绍了可用于跨平台，包括 Windows、 Android、 iOS 和共享代码的不同选项。

## <a name="code-sharing-overviewcode-sharingmd"></a>[代码共享概述](code-sharing.md)

了解不同的代码共享选项可用于 Xamarin 项目，包括.NET 标准库和共享的项目。 也支持可移植类库，但是它们将被视为支持.NET Standard 弃用。

## <a name="net-standardcross-platformapp-fundamentalsnet-standardmd"></a>[.NET Standard](~/cross-platform/app-fundamentals/net-standard.md)

.NET standard 是跨平台共享代码的首的选项。 代码生成针对特定版本 （2.0 提供了与现有的.NET Framework 代码的最佳 API 兼容性），随后可使用支持该级别的其他项目或更高版本。 .NET standard 项目支持在 Visual Studio 2017 和 Visual Studio for mac。

## <a name="shared-projectscross-platformapp-fundamentalsshared-projectsmd"></a>[共享项目](~/cross-platform/app-fundamentals/shared-projects.md)

共享的项目，可以编写多个不同的应用程序项目引用的通用代码。 该代码编译为每个引用的项目的一部分，并可以包括编译器指令以便包含共享的代码库中的特定于平台的功能。 本文介绍共享的项目的工作原理以及如何创建和使用包含 Xamarin 项目。

## <a name="portable-class-librariescross-platformapp-fundamentalspclmd"></a>[可移植类库](~/cross-platform/app-fundamentals/pcl.md)

可移植类库项目，可以生成和分发包含共享的代码在多个平台上运行的程序集。 若要创建可移植类库 （或"PCL"），首先选择要为目标，然后针对组定义这些平台的配置文件中提供.NET Framework 的子集编写代码的平台。 Pcl 被视为最新版本的 Visual Studio; 中不推荐使用建议改为使用.NET Standard 2.0 的开发人员。

## <a name="nuget-projects-multiplatform-libraries-for-code-sharingcross-platformapp-fundamentalsnuget-multiplatform-librariesindexmd"></a>[NuGet 项目： 用于代码共享的多平台库](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/index.md)

可以从 PCL 或.NET standard 项目中; 自动生成 NuGet 包以及共享的项目打包到"bait 和 switch"NuGet 包以使用单独的 NuGet 项目类型。 本部分介绍如何创建每个代码共享方案的 NuGet 包。

## <a name="manually-creating-nuget-packages-for-xamarincross-platformapp-fundamentalsnuget-manualmd"></a>[手动为 Xamarin 创建 NuGet 包](~/cross-platform/app-fundamentals/nuget-manual.md)

创建使用 Xamarin 平台的 NuGet 包的技巧。
