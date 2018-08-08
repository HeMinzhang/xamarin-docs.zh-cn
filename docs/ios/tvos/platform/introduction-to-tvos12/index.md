---
title: TvOS 12 简介
description: 本文档提供了当前提供的 C# 绑定高级别中的哪些 Xamarin 的预览版本的 tvOS 12 的新增和更新功能的概述。
ms.prod: xamarin
ms.assetid: 037F7FFF-2155-4017-B99A-839CE7EC5C9C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/25/2018
ms.openlocfilehash: 5cbec23aa81a4637a18f83d9955a78183dadaa21
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615193"
---
# <a name="introduction-to-tvos-12"></a>TvOS 12 简介

![预览](~/media/shared/preview.png)

> [!WARNING]
> Xamarin 的 tvOS 12 支持目前处于预览状态，它可能包含 bug，这意味着不是功能完备，并可能会更改。 将它用于仅试验。

本文档提供的新的和更新 tvOS 概述 12 的 Xamarin 预览版版本目前提供的 C# 绑定的功能。

若要开始构建使用 Xamarin 的 tvOS 12 应用程序，看一看：

- [入门指南](~/ios/platform/introduction-to-ios12/get-started.md)
- Xamarin 预览版[发布博客文章](https://releases.xamarin.com/preview-release-xcode-10-beta-5/)

## <a name="tvuikit"></a>TVUIKit

tvOS 12 包括 TVUIKit，一组 Api，以便 tvOS 开发人员可以使用常用的 tvOS 控件，如海报视图、 标题按钮、 卡视图和字母组合式视图。 tvOS 12 还引入了一个属性，可以让您滚动太长，会完全可见的文本标签。

## <a name="password-autofill"></a>密码自动填充

使用 tvOS 12，用户可以使用其 iOS 设备中登录到 tvOS 应用一次点击。 启用此选项通过组合使用`UITextContentType`使用情况，以指定用户名和密码字段，关联的域来建立 iOS 应用程序和 tvOS 应用和首选的焦点环境，可以选择要在用户接收焦点的项之间的关系提供用户名和密码。

## <a name="focus-engine-enhancements"></a>焦点引擎增强功能

tvOS 12 允许所有应用程序，无论它们的呈现方式，与焦点引擎进行交互。 通过使用 Siri 远程用户的交互，焦点引擎可与任何应用程序选择一个项、 在可能的焦点发生变动，提示和自然地更新焦点。 这通过 UIKit 的自定义应用程序中启用`IUIFocusItemContainer`接口，`UIFocusMovementHint`类，`IUIFocusItemScrollableContainer`接口，其他相关的类和方法。

## <a name="vision-framework"></a>影像框架

影像框架包括改进的面部检测器可以检测人脸的不同的方向。 此外，请求修订，现在可以使用以选择特定的视觉 framework 算法修订版本。

## <a name="natural-language-framework"></a>自然语言框架

自然语言框架使应用程序可以执行各种类型的语言分析。 例如，它可用来识别词类并确定所表示的文本块的语言。

## <a name="related-links"></a>相关链接

- [tvOS 示例](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS – Apple 开发人员 (Apple)](https://developer.apple.com/tvos/)
- [什么是 tvOS 12 (Apple) （视频） 中的新增功能](https://developer.apple.com/videos/play/wwdc2018/208/)
- [电视 (Apple)](https://www.apple.com/tv/)
- Xamarin 预览版[发布博客文章](https://releases.xamarin.com/preview-release-xcode-10-beta-5/)
