---
title: Xamarin.Forms ListView
description: 本指南介绍了 Xamarin.Forms ListView，用于在美观的交互式列表中显示数据。
ms.prod: xamarin
ms.assetid: FEFDF7E0-720F-4BD1-863F-4477226AA695
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/14/2015
ms.openlocfilehash: dc039a7a984fae9bd856a9e7147ad899f86f0592
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/18/2018
ms.locfileid: "35244995"
---
# <a name="xamarinforms-listview"></a>Xamarin.Forms ListView

ListView 是用于呈现数据，尤其是需要滚动的长列表的列表视图。 本指南将演示如何使用 ListView:

1. **[数据源](data-and-databinding.md)** &ndash;填充数据，无论数据绑定 ListView。
2. **[单元格外观](customizing-cell-appearance.md)** &ndash;自定义内置的单元格的外观或创建你自己的自定义单元格。
3. **[列表外观](customizing-list-appearance.md)** &ndash;自定义 ListView 的外观。 设置页眉和页脚、 启用组和更改的行的高度。
4. **[交互性](interactivity.md)** &ndash;处理分流点，并选择，实现下拉刷新，并添加的上下文操作。
5. **[性能](performance.md)** &ndash;避免性能问题。

## <a name="use-cases"></a>用例
请确保 ListView 是右侧控件为您的需要。 在任何情况下，其中显示数据的可滚动列表，可以使用 ListView。 Listview 支持上下文操作和数据绑定。

不要将 ListView 与相混淆[TableView](~/xamarin-forms/user-interface/tableview.md)。 TableView 控制会是更好的选择的选项或数据的非绑定到列表。 例如，iOS 设置应用中，有一组主要是预定义的选项，是更好地适合使用 ListView 比 TableView。

请注意，ListView 是最佳也适用于同构数据&ndash;，即所有数据应都为同一类型。 这是因为只有一种类型的单元格可用于在列表中的每一行。 TableViews 可以支持多个单元格类型，因此它们是更好的选择时需要使用多种视图。


## <a name="components"></a>组件数
ListView 有多个组件可用于执行每个平台的本机功能。 下面描述了每个组件：

- **[页眉和页脚](customizing-list-appearance.md#Headers_and_Footers)** &ndash;与列表的数据分开的开头和结尾的列表，在显示的文本或视图。 页眉和页脚可以绑定到数据源独立地从 ListView 的数据源。
- **[组](customizing-list-appearance.md#Grouping)** &ndash; ListView 中的数据可以分组以便更易于导航。 组通常包括数据绑定：

![](images/grouping-depth.png "使用分组数据的 ListView")

- **[单元格](customizing-cell-appearance.md)** &ndash; ListView 中的数据也会出现在单元格。 每个单元格对应于一行数据。 有内置的单元格可供选择，也可以定义自己的自定义单元格。 内置和自定义单元格可以是在 XAML 或代码中使用或定义。
  - **[内置](customizing-cell-appearance.md#Built_in_Cells)** &ndash;内置的单元格，尤其是 TextCell 和 ImageCell，可以有很高的性能，因为它们对应于每个平台上的本机控件。
       - **[TextCell](customizing-cell-appearance.md#TextCell)**  &ndash;显示文本，并且可选择带有详细信息文本的字符串。 详细信息的文本呈现为与强调文字颜色较小的字体中的第二个行。
       - **[ImageCell](customizing-cell-appearance.md#ImageCell)**  &ndash;显示文本与图像。 将显示为与左侧图像 TextCell。
  - **[自定义单元格](customizing-cell-appearance.md#customcells)** &ndash;时需要提供复杂的数据，自定义单元格是很好。 例如，自定义视图可用来提供包括专辑和艺术家的歌曲的列表：

![](images/image-cell-default.png "使用 ImageCells ListView")

若要了解有关自定义 ListView 中的单元格的详细信息，请参阅[自定义 ListView 单元格的外观](customizing-cell-appearance.md)。

## <a name="functionality"></a>功能
ListView 还支持多种交互样式，包括：

- **[下拉刷新](interactivity.md#Pull_to_Refresh)** &ndash; ListView 每个平台上支持下拉刷新。
- **[上下文操作](interactivity.md#Context_Actions)** &ndash; ListView 支持在列表中的单个项上采取措施。 例如，您可以在 iOS 上，实现轻扫操作或长按在 Android 上的操作。
- **[所选内容](interactivity.md#selectiontaps)** &ndash;可以侦听的选择和取消点击某行时采取措施。

![](images/context-default.png "ListView 的上下文操作")

若要了解有关 ListView 的交互功能的详细信息，请参阅[操作和与 ListView 的交互操作](interactivity.md)。


## <a name="related-links"></a>相关链接

- [使用与 ListView （示例）](https://developer.xamarin.com/samples/WorkingWithListview)
- [两个双向绑定 （示例）](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/SwitchEntryTwoBinding)
- [构建在单元格 （示例）](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/BuiltInCells)
- [自定义单元格 （示例）](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/CustomCells)
- [分组 （示例）](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/Grouping)
- [自定义呈现器视图 （示例）](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/WorkingWithListviewNative)
- [ListView 交互性 （示例）](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/interactivity)
