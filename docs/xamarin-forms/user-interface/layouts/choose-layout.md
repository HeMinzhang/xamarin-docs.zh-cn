---
title: 选择 "Xamarin" 布局
description: Xamarin. Forms 布局类允许您在应用程序中排列和分组 UI 控件。
ms.prod: xamarin
ms.assetid: 05A39752-A174-447E-A30D-3CC9EF98CB96
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/21/2018
ms.openlocfilehash: 161da8948f356fef997a411855598bc99d2f49b7
ms.sourcegitcommit: 5f972a757030a1f17f99177127b4b853816a1173
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/21/2019
ms.locfileid: "69893994"
---
# <a name="choose-a-xamarinforms-layout"></a>选择 "Xamarin" 布局

[![下载示例](~/media/shared/download.png) 下载示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)

Xamarin. Forms 布局类允许您在应用程序中排列和分组 UI 控件。 选择布局类需要了解布局是如何定位其子元素的, 以及布局如何调整其子元素的大小。 此外, 可能还需要嵌套布局以创建所需的布局。

下图显示了可通过主 Xamarin. Forms 布局类实现的典型布局:

[ ![Xamarin 中的主要布局类]。窗体(images/layouts.png "xamarin 布局类")](images/layouts-large.png#lightbox "Xamarin. Forms 布局类")

## <a name="stacklayout"></a>StackLayout

一[`StackLayout`](xref:Xamarin.Forms.StackLayout)维堆栈中的元素是水平或垂直的。 属性指定元素的方向, 默认方向为[`Vertical`。](xref:Xamarin.Forms.StackOrientation) [`Orientation`](xref:Xamarin.Forms.StackLayout.Orientation) `StackLayout`通常用于在页面上排列 UI 的子节。

下面的 XAML 演示如何创建包含三个[`StackLayout`](xref:Xamarin.Forms.StackLayout) [`Label`](xref:Xamarin.Forms.Label)对象的垂直:

```xaml
<StackLayout Margin="20,35,20,25">
    <Label Text="The StackLayout has its Margin property set, to control the rendering position of the StackLayout." />
    <Label Text="The Padding property can be set to specify the distance between the StackLayout and its children." />
    <Label Text="The Spacing property can be set to specify the distance between views in the StackLayout." />
</StackLayout>
```

在中[`Orientation`](xref:Xamarin.Forms.StackLayout.Orientation) [`Horizontal`](xref:Xamarin.Forms.StackOrientation), 如果未显式设置元素的大小, 则它将展开以填充可用宽度或高度 (如果属性设置为)。 [`StackLayout`](xref:Xamarin.Forms.StackLayout)

[`StackLayout`](xref:Xamarin.Forms.StackLayout)通常用作父布局, 其中包含其他子布局。 但是, `StackLayout`不应使用`StackLayout`对象组合来[`Grid`](xref:Xamarin.Forms.Grid)重现布局。 下面的代码演示了此错误做法的示例:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Details.HomePage"
             Padding="0,20,0,0">
    <StackLayout>
        <StackLayout Orientation="Horizontal">
            <Label Text="Name:" />
            <Entry Placeholder="Enter your name" />
        </StackLayout>
        <StackLayout Orientation="Horizontal">
            <Label Text="Age:" />
            <Entry Placeholder="Enter your age" />
        </StackLayout>
        <StackLayout Orientation="Horizontal">
            <Label Text="Occupation:" />
            <Entry Placeholder="Enter your occupation" />
        </StackLayout>
        <StackLayout Orientation="Horizontal">
            <Label Text="Address:" />
            <Entry Placeholder="Enter your address" />
        </StackLayout>
    </StackLayout>
</ContentPage>
```

这比较浪费，因为会执行不需要的布局计算。 相反, 使用[`Grid`](xref:Xamarin.Forms.Grid)可以更好地实现所需的布局。

> [!TIP]
> 使用[`StackLayout`](xref:Xamarin.Forms.StackLayout)时, 请确保只将一个子元素设置为[`LayoutOptions.Expands`](xref:Xamarin.Forms.LayoutOptions.Expands)。 此属性可确保指定子级会占用 `StackLayout` 可以向它提供的最大空间，而多次执行这些计算比较浪费。

有关详细信息, 请参阅[Xamarin. Forms StackLayout](stack-layout.md)。

## <a name="grid"></a>Grid

[`Grid`](xref:Xamarin.Forms.Grid)用于显示行和列中的元素, 这些元素的大小可以是比例的也可以是绝对的。 网格的行和列使用[`RowDefinitions`](xref:Xamarin.Forms.Grid.RowDefinitions)和[`ColumnDefinitions`](xref:Xamarin.Forms.Grid.ColumnDefinitions)属性指定。

若要定位特定[`Grid`](xref:Xamarin.Forms.Grid)单元中的[`Grid.Column`](xref:Xamarin.Forms.Grid.ColumnProperty)元素, 请[`Grid.Row`](xref:Xamarin.Forms.Grid.RowProperty)使用和附加属性。 若要使元素跨多个行和列, 请[`Grid.RowSpan`](xref:Xamarin.Forms.Grid.RowSpanProperty)使用[`Grid.ColumnSpan`](xref:Xamarin.Forms.Grid.ColumnSpanProperty)和附加属性。

> [!NOTE]
> [`Grid`](xref:Xamarin.Forms.Grid)布局不应与表混淆, 并且不会显示表格数据。 与 HTML 表不同, `Grid`用于布局内容。 若要显示表格数据, 请考虑使用[ListView](~/xamarin-forms/user-interface/listview/index.md)、 [CollectionView](~/xamarin-forms/user-interface/collectionview/index.md)或[TableView](~/xamarin-forms/user-interface/tableview.md)。

下面的 XAML 演示如何创建包含两[`Grid`](xref:Xamarin.Forms.Grid)个行和两个列的:

```xaml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="50" />
        <RowDefinition Height="50" />
    </Grid.RowDefinitions>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="Auto" />
        <ColumnDefinition />
    </Grid.ColumnDefinitions>    
    <Label Text="Column 0, Row 0"
           Width="200" />
    <Label Grid.Column="1"
           Text="Column 1, Row 0" />
    <Label Grid.Row="1"
           Text="Column 0, Row 1" />
    <Label Grid.Column="1"
           Grid.Row="1"
           Text="Column 1, Row 1" />
</Grid>
```

在此示例中, 调整大小的方式如下:

- 每一行都具有与设备无关的50单元的显式高度。
- 第一列的宽度设置为[`Auto`](xref:Xamarin.Forms.GridLength.Auto), 因此, 其宽度为其子级所需的宽度。 在这种情况下, 它是200与设备无关的单位宽, 以容纳第[`Label`](xref:Xamarin.Forms.Label)一个的宽度。

可以通过使用自动调整大小在列或行内分布空间, 从而使列和行大小适合其内容。 这可通过将的高度[`RowDefinition`](xref:Xamarin.Forms.RowDefinition)(或的宽度[`ColumnDefinition`](xref:Xamarin.Forms.ColumnDefinition)) 设置为来[`Auto`](xref:Xamarin.Forms.GridLength.Auto)实现。 还可以使用比例大小调整, 按加权比例在网格行和列之间分配可用空间。 这可通过将的高度`RowDefinition`(或的宽度`ColumnDefinition`) 设置为使用`*`运算符的值来实现。

> [!CAUTION]
> 尝试确保尽可能少的行和列设置为[`Auto`](xref:Xamarin.Forms.GridLength.Auto)大小。 每个自动调整大小的行或列都会导致布局引擎执行额外布局计算。 而是应在可能时使用固定大小的行和列。 另外, 还可以设置行和列, 使其与[`GridUnitType.Star`](xref:Xamarin.Forms.GridUnitType.Star)枚举值占用比例的空间。

有关详细信息, 请参阅[Xamarin Grid](grid.md)。

## <a name="flexlayout"></a>FlexLayout

[`FlexLayout`](xref:Xamarin.Forms.FlexLayout) [类似`StackLayout`](xref:Xamarin.Forms.StackLayout)于中的, 它在堆栈中水平或垂直显示子元素。 但是, `FlexLayout`如果在单个行或列中容纳太多项, 还可以包装其子级, 还可以更精细地控制其子元素的大小、方向和对齐方式。

下面的 XAML 演示如何创建一个[`FlexLayout`](xref:Xamarin.Forms.FlexLayout) , 它在单个列中显示其视图:

```xaml
<FlexLayout Direction="Column"
            AlignItems="Center"
            JustifyContent="SpaceEvenly">
    <Label Text="FlexLayout in Action" />
    <Button Text="Button" />
    <Label Text="Another Label" />
</FlexLayout>
```

在此示例中, 布局的工作方式如下:

- 属性设置为`Column`, 这将导致在一列项中`FlexLayout`排列的子。 [`Direction`](xref:Xamarin.Forms.FlexLayout.Direction)
- 属性设置为`Center`, 这会使每个项在水平居中。 [`AlignItems`](xref:Xamarin.Forms.FlexLayout.AlignItems)
- 将[`JustifyContent`](xref:Xamarin.Forms.FlexLayout.JustifyContent)属性设置为`SpaceEvenly`, 这将在所有项之间、在第一项的上方和最后一项的下方分配所有剩余的垂直空间。

有关详细信息, 请参阅[Xamarin. Forms FlexLayout](flex-layout.md)。

## <a name="relativelayout"></a>RelativeLayout

[`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout)用于相对于布局或同级元素的属性来定位元素和调整其大小。 默认情况下, 元素位于布局的左上角。 `RelativeLayout`可用于创建跨设备大小按比例缩放的 ui。

在中[`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout), 位置和大小指定为 "约束"。 约束具有[`Factor`](xref:Xamarin.Forms.ConstraintExpression.Factor)和[`Constant`](xref:Xamarin.Forms.ConstraintExpression.Constant)属性, 这些属性可用于将位置和大小定义为其他对象的多个属性 (或小数) 和一个常数。 此外, 常量可以为负数。

> [!NOTE]
> 支持[`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout)将元素放在其自身的边界之外。

以下 XAML 演示了如何在中[`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout)排列元素:

```xaml
<RelativeLayout>
    <BoxView Color="Blue"
             HeightRequest="50"
             WidthRequest="50"
             RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=0}"
             RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0}" />
    <BoxView Color="Red"
             HeightRequest="50"
             WidthRequest="50"
             RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=.85}"
             RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0}" />
    <BoxView x:Name="pole"
             Color="Gray"
             WidthRequest="15"
             RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=.75}"
             RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=.45}"
             RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=.25}" />
    <BoxView Color="Green"
             RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=.10, Constant=10}"
             RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=.2, Constant=20}"
             RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToView, ElementName=pole, Property=X, Constant=15}"
             RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToView, ElementName=pole, Property=Y, Constant=0}" />
</RelativeLayout>
```

在此示例中, 布局的工作方式如下:

- 向蓝色[`BoxView`](xref:Xamarin.Forms.BoxView)提供与设备无关的50x50 单元的显式大小。 它置于布局的左上角, 这是默认位置。
- 红色[`BoxView`](xref:Xamarin.Forms.BoxView)的给定大小为50x50 与设备无关的单位。 它置于布局的右上角。
- 将为[`BoxView`](xref:Xamarin.Forms.BoxView)灰色指定15个与设备无关的单位, 并将其 height 设置为其父级高度的 75%。
- 绿色[`BoxView`](xref:Xamarin.Forms.BoxView)不是以明确大小提供的。 其位置设置为相对于指定`BoxView` `pole`的。

> [!WARNING]
> 尽量避免使用`RelativeLayout` 。 它会导致 CPU 不得不执行显著更多的工作。

有关详细信息, 请参阅[Xamarin. Forms RelativeLayout](relative-layout.md)。

## <a name="absolutelayout"></a>AbsoluteLayout

[`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout)用于使用显式值或相对于布局大小的值对元素进行定位和调整大小。 位置由子级的左上角指定, 相对于的`AbsoluteLayout`左上角。

[`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout)应视为只在您可以对子级施加大小或元素的大小不会影响其他子级的位置时使用的特殊用途的布局。 此布局的标准用途是创建覆盖层, 其中包含具有其他控件的页面, 可能是为了防止用户与页面上的普通控件交互。

> [!IMPORTANT]
> `HorizontalOptions`并`VerticalOptions`属性不起作用的子级上`AbsoluteLayout`。

在中[`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout) [`AbsoluteLayout.LayoutBounds`](xref:Xamarin.Forms.AbsoluteLayout.LayoutBoundsProperty) , 附加属性用于指定元素的水平位置、垂直位置、宽度和高度。 此外, [`AbsoluteLayout.LayoutFlags`](xref:Xamarin.Forms.AbsoluteLayout.LayoutFlagsProperty)附加属性指定将如何解释布局边界。

以下 XAML 演示了如何在中[`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout)排列元素:

```xaml
<AbsoluteLayout Margin="40">
    <BoxView Color="Red"
             AbsoluteLayout.LayoutFlags="PositionProportional"
             AbsoluteLayout.LayoutBounds="0.5, 0, 100, 100"
             Rotation="30" />
    <BoxView Color="Green"
             AbsoluteLayout.LayoutFlags="PositionProportional"
             AbsoluteLayout.LayoutBounds="0.5, 0, 100, 100"
             Rotation="60" />
    <BoxView Color="Blue"
             AbsoluteLayout.LayoutFlags="PositionProportional"
             AbsoluteLayout.LayoutBounds="0.5, 0, 100, 100" />
</AbsoluteLayout>
```

在此示例中, 布局的工作方式如下:

- 每[`BoxView`](xref:Xamarin.Forms.BoxView)个都给定显式大小的 100x100, 并在水平居中的位置显示在同一位置。
- 红色[`BoxView`](xref:Xamarin.Forms.BoxView)旋转30度, 绿色`BoxView`旋转60度。
- 在每[`BoxView`](xref:Xamarin.Forms.BoxView)个上[`AbsoluteLayout.LayoutFlags`](xref:Xamarin.Forms.AbsoluteLayout.LayoutFlagsProperty) , 附加的属性都`PositionProportional`设置为, 指示该位置与宽度和高度之比的剩余空间成正比。

> [!CAUTION]
> 请尽可能避免[`AbsoluteLayout.AutoSize`](xref:Xamarin.Forms.AbsoluteLayout.AutoSize)使用属性, 因为这会导致布局引擎执行其他布局计算。

有关详细信息, 请参阅[Xamarin. Forms AbsoluteLayout](absolute-layout.md)。

## <a name="input-transparency"></a>输入透明度

每个可视元素都[`InputTransparent`](xref:Xamarin.Forms.VisualElement.InputTransparent)有一个属性, 该属性用于定义元素是否接收输入。 其默认值是`false`，确保元素接收输入。

在布局类上设置此属性时, 其值将传输到子元素。 因此, 在布局[`InputTransparent`](xref:Xamarin.Forms.VisualElement.InputTransparent)类上`true`将属性设置为时, 将导致布局中的所有元素都不接收输入。

## <a name="layout-performance"></a>布局性能

若要获得最佳布局性能, 请遵循[优化布局性能](~/xamarin-forms/deploy-test/performance.md#optimize-layout-performance)中的准则。

此外, 还可以通过使用布局压缩 (从可视化树中删除指定布局) 来改善页面呈现性能。 有关详细信息, 请参阅[布局压缩](layout-compression.md)。

## <a name="related-links"></a>相关链接

- [布局 （示例）](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)
- [Xamarin 格式布局 (视频)](https://youtu.be/4HlLjTZQzjM)
- [Xamarin. Forms StackLayout](stack-layout.md)
- [Xamarin 网格](grid.md)
- [Xamarin. Forms FlexLayout](flex-layout.md)
- [Xamarin. Forms AbsoluteLayout](absolute-layout.md)
- [Xamarin. Forms RelativeLayout](relative-layout.md)
- [优化布局性能](~/xamarin-forms/deploy-test/performance.md#optimize-layout-performance)
- [布局压缩](layout-compression.md)
