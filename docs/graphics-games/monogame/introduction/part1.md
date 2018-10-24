---
title: 第 1 – 创建跨平台 MonoGame 部分
description: 本演练演示如何创建用于 iOS 和 Android 使用 MonoGame 的新项目。 结果是 Visual Studio for Mac 与一个跨平台共享的代码项目，以及为每个平台的一个项目的解决方案。 此项目将显示一个空的蓝色屏幕时执行。
ms.prod: xamarin
ms.assetid: FC69E69B-04D4-45DF-9BBF-2A6CDEAD9B2F
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: bd7990b94e678c205f9ce636f4eb0d28180fc6ec
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/09/2018
ms.locfileid: "33921984"
---
# <a name="part-1--creating-a-cross-platform-monogame"></a>第 1 – 创建跨平台 MonoGame 部分

_本演练演示了如何使用 MonoGame 为 iOS 和 Android 创建新项目。演练的结果是一个 Visual Studio for Mac 解决方案，其中包含一个跨平台共享代码项目以及适用于每个平台的一个项目。执行时，该项目会显示一个空的蓝屏。_

MonoGame 支持开发大部分代码都可以重用的跨平台游戏。本演练将重点介绍如何设置一个解决方案，其中包含一个适用于 iOS 和 Android 的项目，以及一个适用于跨平台代码的共享代码项目。

完成后，我们将拥有一个具有适当结构的项目，可以按每秒 30 帧的速度执行游戏更新逻辑和游戏绘图逻辑。它可以用作任何 MonoGame 项目的基础项目。执行时，我们的项目将如下所示：

![空白的蓝色屏幕](part1-images/image1.png)

## <a name="adding-monogame-to-visual-studio-for-mac"></a>将 MonoGame 添加到 Visual Studio for Mac 中

可以将 MonoGame 作为 Visual Studio for Mac 的加载项添加。在 Mac 上，选择“Visual Studio for Mac”****>****“加载项管理器...”。在 Windows 上，选择“工具”****>****“加载项管理器...”。选择“库”****选项卡，展开“游戏开发”****类别并选择“MonoGame 加载项”****，然后单击“安装”****：

![选择 MonoGame Mac 扩展库的的 visual Studio](part1-images/image2.png)

> [!IMPORTANT]
> **注意**：如果“游戏开发”****部分未出现在加载项管理器中，可以从此处手动下载并安装最新版本：http：//www.monogame.net/downloads/。可能需要重启 Visual Studio for Mac 才能显示模板。

安装完成后，MonoGame 模板会出现在 Visual Studio for Mac 中，如下一部分所示。

## <a name="creating-a-new-solution"></a>创建新的解决方案

在 Visual Studio for Mac 中，选择“文件”**>**“新建解决方案”。在“新建项目”****对话框中，单击****“杂项”，滚动到“常规”****部分，选择“通用 MonoGame 移动应用程序”****选项，然后单击“下一步”。

![创建 MonoGame 应用程序的新建项目对话框](part1-images/image3.png)

将项目命名为 WalkingGame，然后单击“创建”：

![选取的名称和位置的新建项目对话框](part1-images/image4.png)

现在我们的项目将像任何其他 iOS 或 Android 项目一样执行。项目运行时应该显示青蓝色背景：

![空白的蓝色应用背景](part1-images/image5.png)

## <a name="fixing-android-compile-errors"></a>修复 Android 编译错误

当前版本的 MonoGame 模板在 Android 的 `Activity1.cs` 文件中有一些语法错误。如果要解决这些问题，请使用以下内容替换 OnCreate 函数：

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    var g = new Game1();
    SetContentView((View)g.Services.GetService(typeof(View)));
    g.Run();
}
```

## <a name="summary"></a>总结

本演练介绍了如何使用 Visual Studio for Mac 创建跨平台的 MonoGame 项目。本演练的结果是一个空的蓝色屏幕。任何 iOS 和 Android 游戏都可以从该项目着手。

## <a name="related-links"></a>相关链接

- [MonoGame Android NuGet](https://www.nuget.org/packages/MonoGame.Framework.Android/)
- [MonoGame iOS NuGet](https://www.nuget.org/packages/MonoGame.Framework.iOS/)
- [MonoGame API 文档](http://www.monogame.net/documentation/?page=main)
