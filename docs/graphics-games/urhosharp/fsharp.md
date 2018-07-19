---
title: '使用 F # 编程 UrhoSharp'
description: '本文档介绍如何创建使用 F # Visual Studio for mac 中的简单你好 world UrhoSharp 应用程序'
ms.prod: xamarin
ms.assetid: F976AB09-0697-4408-999A-633977FEFF64
author: charlespetzold
ms.author: chape
ms.date: 03/29/2017
ms.openlocfilehash: a4e1a31a2591c799a153e1333e4a4a4a0719a107
ms.sourcegitcommit: e98a9ce8b716796f15de7cec8c9465c4b6bb2997
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/18/2018
ms.locfileid: "39111194"
---
# <a name="programming-urhosharp-with-f"></a>使用 F # 编程 UrhoSharp

可以使用 F # 使用相同的库和 C# 程序员使用的概念进行编程 UrhoSharp。 [使用 UrhoSharp](~/graphics-games/urhosharp/using.md)文章概述了 UrhoSharp 引擎和在这篇文章之前应阅读。

C + + 领域，源自的很多库，如许多 UrhoSharp 函数会返回布尔值或整数，指示成功或失败。 应使用`|> ignore`忽略这些值。

[示例程序](https://github.com/xamarin/recipes/tree/master/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp)UrhoSharp 从 F # 中为"Hello World"。

## <a name="creating-an-empty-project"></a>创建一个空项目

UrhoSharp 没有 F # 模板还可以使用任一启动的可用，因此，若要创建 UrhoSharp 项目[示例](https://github.com/xamarin/recipes/tree/master/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp)或请执行以下步骤：

1. 从 Visual Studio for Mac 中，创建一个新**解决方案**。 选择**iOS > 应用程序 > 单一视图应用**，然后选择**F #** 作为实现语言。 
1. 删除**Main.storyboard**文件。 打开**Info.plist**文件并在**iPhone / iPod 部署信息**窗格中，删除`Main`中的字符串**主界面**下拉列表。
1. 删除**ViewController.fs**文件。

## <a name="building-hello-world-in-urho"></a>构建中 Urho 的 Hello World

现在您就可以开始定义您的游戏的类。 至少，您将需要定义的子类`Urho.Application`并重写其`Start`方法。 若要创建此文件，右键单击你的 F # 项目中，选择**添加新文件...** 并将一个空的 F # 类添加到你的项目。 新文件将添加到你的项目中的文件列表的末尾，但以使其显示必须将其拖*之前*中使用**AppDelegate.fs**。

1. 添加对 Urho NuGet 包的引用。
1. 从现有 Urho 项目中，复制 （大型） 目录**CoreData /** 并**数据 /** 到你的项目**资源 /** 目录。 在 F # 项目中，右键单击**资源**文件夹，然后使用**添加 / 添加现有文件夹**将所有这些文件添加到你的项目。

项目结构现在应如下所示：

![](fsharp-images/solutionpane.png "项目结构现在应如下所示")

将你新创建的类定义的子类型为`Urho.Application`并重写其`Start`方法：

```fsharp
namespace HelloWorldUrho1

open Urho
open Urho.Gui
open Urho.iOS

type HelloWorld(o : ApplicationOptions) =
    inherit Urho.Application(o) 

override this.Start() = 
        let cache = this.ResourceCache
        let helloText = new Text()

        helloText.Value <- "Hello World from Urho3D, Mono, and F#"
        helloText.HorizontalAlignment <- HorizontalAlignment.Center
        helloText.VerticalAlignment <- VerticalAlignment.Center

        helloText.SetColor (new Color(0.f, 1.f, 0.f))
        let f = cache.GetFont("Fonts/Anonymous Pro.ttf")

        helloText.SetFont(f, 30) |> ignore
                  
        this.UI.Root.AddChild(helloText)
            
```

代码是非常简单。 它使用`Urho.Gui.Text`类，以显示特定字体和颜色大小的中心对齐字符串。 

在运行此代码之前，不过，必须初始化 UrhoSharp。 

打开 AppDelegate.fs 文件并修改`FinishedLaunching`方法，如下所示：

```fsharp
namespace HelloWorldUrho1

open System
open UIKit
open Foundation
open Urho
open Urho.iOS

[<Register ("AppDelegate")>]
type AppDelegate () =
    inherit UIApplicationDelegate ()

    override this.FinishedLaunching (app, options) =
        let o = ApplicationOptions.Default
     
        let g = new HelloWorld(o)
        g.Run() |> ignore
       
        true
```

`ApplicationOptions.Default`提供横向模式下应用程序的默认选项。 将这些信息传递`ApplicationOptions`的默认构造函数为你`Application`子类 (请注意，当您定义`HelloWorld`类，在行`inherit Application(o)`调用基类构造函数)。 

`Run`方法在`Application`启动程序。 它被定义为返回`int`，这可以输送到`ignore`。 

生成的程序应如下所示：

![](fsharp-images/helloworldfsharp.png "生成的程序应如下所示")








## <a name="related-links"></a>相关链接

- [在 GitHub （示例） 上浏览](https://github.com/xamarinhttps://developer.xamarin.com/recipes/tree/master/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp)
