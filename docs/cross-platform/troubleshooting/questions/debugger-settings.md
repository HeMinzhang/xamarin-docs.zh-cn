---
title: 调试器需要哪些项目设置？
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 3A024E4E-ACA3-4C7A-ADEF-541665D15779
author: conceptdev
ms.author: crdun
ms.date: 05/08/2018
ms.openlocfilehash: 343f8d37d77726d2cdc06a74c44e476af00dde27
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/06/2019
ms.locfileid: "70765149"
---
# <a name="what-project-settings-are-required-for-the-debugger"></a>调试器需要哪些项目设置？

为了使调试器能够按预期工作（命中断点、显示调试日志等），必须同时启用开发人员检测和调试信息显示。

请按照以下步骤检查您的环境设置：

## <a name="visual-studio"></a>Visual Studio
1. 打开项目选项
2. 中转到 "**生成 > 高级 ...** "将调试信息设置为**完整**
3. 每个平台的设置：
   - **> 调试选项**中转到 "Android 选项"。 勾选 "**启用开发人员检测**" 框。
   - 请参阅**IOS 调试 > 调试 & 检测**。 勾选 "**启用调试**" 框。

## <a name="visual-studio-for-mac"></a>Visual Studio for Mac
1. 打开项目选项
2. **> 编译器 "> 常规" 选项**中转到 "生成"。 将调试信息设置为**完整**
3. 每个平台的设置：
    - 请参阅 **> Android build > 调试选项**中的 "生成"。 勾选 "**启用开发人员检测**" 框。
    - 请参阅**生成 > IOS 调试**。 勾选 "**启用调试**" 框。
