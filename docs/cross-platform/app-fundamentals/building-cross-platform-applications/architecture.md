---
title: 第 2 部分 - 体系结构
description: 本文档介绍了有助于构建跨平台应用程序的体系结构模式。 它讨论了典型的应用程序层（数据层、数据访问层等）以及常见的移动软件模式（MVVM、MVC 等）
ms.prod: xamarin
ms.assetid: 2176DB2D-E84A-3757-CFAB-04A586068D50
author: conceptdev
ms.author: crdun
ms.date: 03/27/2017
ms.openlocfilehash: e1b1a98bf06bbd03b382f0b7263e6965d4efad15
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/06/2019
ms.locfileid: "70762106"
---
# <a name="part-2---architecture"></a>第 2 部分 - 体系结构

构建跨平台应用程序的一个关键原则是创建一个结构，该体系结构适用于跨平台的代码共享最大化。 遵循面向对象的以下编程原则，有助于构建结构良好的应用程序：

- **封装**–确保类甚至是体系结构层只公开执行其所需功能的最小 API，并隐藏实现细节。 在类级别，这意味着对象表现为 "黑框"，使用代码时，无需知道它们如何完成任务。 在体系结构级别，这意味着实现外观等模式，这些模式鼓励简化的 API，以便在更多抽象层中代表代码协调更复杂的交互。 这意味着，UI 代码（例如）应该只负责显示屏幕和接受用户输入;和决不会直接与数据库交互。 同样，数据访问代码应该只读取和写入数据库，而永远不会与按钮或标签直接交互。
- **责任分离**–确保每个组件（在体系结构和类级别上）都具有清晰且明确定义的目的。 每个组件只应执行其定义的任务，并通过可供需要使用它的其他类访问的 API 公开此功能。
- **多态性**–对支持多个实现的接口（或抽象类）的编程意味着可以在平台之间编写和共享核心代码，同时仍与特定于平台的功能进行交互。

自然结果是在真实世界上建模的应用程序，或具有单独逻辑层的抽象实体。 将代码分为多个层，使应用程序更易于理解、测试和维护。 建议每个层中的代码在物理上是独立的（在目录中，甚至是针对非常大的应用程序的单独项目中），并以逻辑方式分离（使用命名空间）。

 <a name="Typical_Application_Layers" />

## <a name="typical-application-layers"></a>典型的应用程序层

在本文档和案例研究中，我们指的是以下六个应用程序层：

- **数据层**–非易失性数据持久性，可能是 SQLite 数据库，但可以使用 XML 文件或任何其他合适的机制来实现。
- **数据访问层**-数据层的包装，提供对数据的创建、读取、更新、删除（CRUD）访问权限，而无需向调用方公开实现细节。 例如，DAL 可能包含用于查询或更新数据的 SQL 语句，但引用代码不需要了解这一点。
- **业务层**–（有时称为业务逻辑层或 BLL）包含业务实体定义（模型）和业务逻辑。 适用于业务外观模式的候选项。
- **服务访问层**–用于访问云中的服务：从复杂的 web 服务（REST、JSON、WCF）到简单地从远程服务器检索数据和图像。 封装网络行为，并提供要由应用程序和 UI 层使用的简单 API。
- **应用程序层**–通常特定于平台的代码（通常在平台之间不共享）或特定于应用程序的代码（通常不是可重用的代码）。 很好地测试是否将代码放置在应用程序层与 UI 层之间，以确定类是否有任何实际的显示控件，或（b）是否可以在多个屏幕或设备之间共享（例如 iPhone 和 iPad）。
- **用户界面（UI）层**–面向用户的层，其中包含屏幕、小组件和管理它们的控制器。

应用程序不一定包含所有层–例如，服务访问层不会存在于不访问网络资源的应用程序中。 一个非常简单的应用程序可能会合并数据层和数据访问层，因为操作非常基本。

 <a name="Common_Mobile_Software_Patterns" />

## <a name="common-mobile-software-patterns"></a>常见的移动软件模式

模式是一种用于捕获常见问题的周期性解决方案的方式。 可以通过几种关键模式来了解如何构建可维护/可理解的移动应用程序。

- **Model、View、ViewModel （MVVM）** –模型-视图-ViewModel 模式适用于支持数据绑定的框架，如 Xamarin。 它已由启用了 XAML 的 Sdk 那时推广，如 Windows Presentation Foundation （WPF）和 Silverlight;其中，ViewModel 是通过数据绑定和命令在数据（模型）和用户界面（视图）之间进行的。
- "**模型"、"视图"、"控制器" （mvc）** -一种常见且经常被误解的模式，这是在生成用户界面时最常使用 MVC，并为 UI 屏幕（视图）的实际定义和处理交互的引擎提供分离（控制器）和填充它的数据（模型）。 模型实际上是一个完全可选的部分，因此理解此模式的核心位于视图和控制器中。 MVC 是适用于 iOS 应用程序的常用方法。
- **业务外观**-也称为经理模式，为复杂工作提供了一个简化的入门点。 例如，在任务跟踪应用程序中，您可能有一个`TaskManager`具有`GetAllTasks()` 、 `GetTask(taskID)` `SaveTask (task)` 、等方法的类。`TaskManager`类提供对实际保存/检索任务对象的内部工作原理的外观。
- **Singleton** –单一实例模式提供的一种方法是只能存在特定对象的单个实例。 例如，在移动应用程序中使用 SQLite 时，只需要数据库的一个实例。 使用单一实例模式是确保这一点的一种简单方法。
- **Provider** –梦寐以求的一种模式（类似于策略或基本依赖项注入），用于鼓励跨 SILVERLIGHT、WPF 和 WinForms 应用程序重复使用代码。 共享代码可针对接口或抽象类编写，在使用代码时，将编写和传入特定于平台的具体实现。
- **Async** –不要与 async 关键字混淆，当需要执行长时间运行的工作时，无需持有 UI 或当前正在处理的工作时，将使用异步模式。 在最简单的形式中，异步模式只描述了在当前线程继续处理并侦听后台进程的响应时，应在另一个线程（或类似的线程抽象，如任务）中启动长时间运行的任务。，然后在返回数据和或状态时更新 UI。

将更详细地检查每个模式，因为案例研究中阐释了这些模式的实际使用情况。 维基百科提供对[MVVM](https://en.wikipedia.org/wiki/Model–view–viewmodel)、 [MVC](https://en.wikipedia.org/wiki/Model–view–controller)、[外观](https://en.wikipedia.org/wiki/Facade_pattern)、[单一实例](https://en.wikipedia.org/wiki/Singleton_pattern)、[策略](https://en.wikipedia.org/wiki/Strategy_pattern)和[提供程序](https://en.wikipedia.org/wiki/Provider_model)模式（通常为[设计模式](https://en.wikipedia.org/wiki/Design_Patterns)）的更详细说明。
