---
title: Xamarin.Forms 文本样式
description: 本文介绍如何在 Xamarin.Forms 应用程序中的文本样式。 样式可以定义一次并使用的许多视图，但一种样式仅用于一种类型的视图。
ms.prod: xamarin
ms.assetid: 57C0CFD6-A568-46B8-ADA1-BF25681893CF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: 73aa3115e92d1e3954f5ae3eb8dcb84abf9d9efb
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998763"
---
# <a name="xamarinforms-text-styles"></a>Xamarin.Forms 文本样式

_在 Xamarin.Forms 中的文本样式_

样式可用于调整标签、 条目和编辑器的外观。 样式可以定义一次并使用的许多视图，但一种样式仅用于一种类型的视图。
可以提供样式`Key`有选择地使用特定的控件和应用`Style`属性。

<a name="Built-In_Styles" />

## <a name="built-in-styles"></a>内置样式

Xamarin.Forms 包含多个[内置](xref:Xamarin.Forms.Device.Styles)样式的常见方案：

- `BodyStyle`
- `CaptionStyle`
- `ListItemDetailTextStyle`
- `ListItemTextStyle`
- `SubtitleStyle`
- `TitleStyle`

若要将其中一个内置样式应用，使用`DynamicResource`标记扩展来指定的样式：

```xaml
<Label Text="I'm a Title" Style="{DynamicResource TitleStyle}"/>
```

在 C# 中，内置样式从选择`Device.Styles`:

```csharp
label.Style = Device.Styles.TitleStyle;
```

![](styles-images/builtinstyles.png "设备样式示例")

<a name="Custom_Styles" />

## <a name="custom-styles"></a>自定义样式

样式包含的资源库和资源库包含的属性和属性的值将设置为。

在 C# 中，将按如下所示定义自定义大小 30 的红色文本的标签的样式：

```csharp
var LabelStyle = new Style (typeof(Label)) {
    Setters = {
        new Setter {Property = Label.TextColorProperty, Value = Color.Red},
        new Setter {Property = Label.FontSizeProperty, Value = 30}
    }
};

var label = new Label { Text = "Check out my style.", Style = LabelStyle };
```

在 XAML:

```xaml
<ContentPage.Resources>
    <ResourceDictionary>
        <Style x:Key="LabelStyle" TargetType="Label">
            <Setter Property="TextColor" Value="Red"/>
            <Setter Property="FontSize" Value="30"/>
        </Style>
    </ResourceDictionary>
</ContentPage.Resources>

<ContentPage.Content>
    <StackLayout>
        <Label Text="Check out my style." Style="{StaticResource LabelStyle}" />
    </StackLayout>
</ContentPage.Content>
```

请注意资源 （包括所有样式） 定义内`ContentPage.Resources`，这是更熟悉的同级`ContentPage.Content`元素。

![](styles-images/customstyle.png "自定义样式示例")

<a name="Applying_Styles" />

## <a name="applying-styles"></a>应用样式

一旦创建一个样式后，它可以应用于任何视图匹配其`TargetType`。

在 XAML 中，自定义样式应用于视图通过提供他们`Style`具有属性`StaticResource`标记扩展引用所需的样式：

```xaml
<Label Text="Check out my style." Style="{StaticResource LabelStyle}" />
```

在 C# 中，样式可以是直接应用于视图或添加到并从页面的检索`ResourceDictionary`。 若要直接添加：

```csharp
var label = new Label { Text = "Check out my style.", Style = LabelStyle };
```

若要添加和检索从页面的`ResourceDictionary`:

```csharp
this.Resources.Add ("LabelStyle", LabelStyle);
label.Style = (Style)Resources["LabelStyle"];
```

内置样式的应用方式不同，，因为它们需要响应的辅助功能设置。 若要将应用中 XAML，内置样式`DynamicResource`使用标记扩展：

```xaml
<Label Text="I'm a Title" Style="{DynamicResource TitleStyle}"/>
```

在 C# 中，内置样式从选择`Device.Styles`:

```csharp
label.Style = Device.Styles.TitleStyle;
```

## <a name="accessibility"></a>可访问性

内置样式存在是为了使其更易于尊重辅助工具首选项。 在使用任何内置样式时，如果用户将相应地设置其可访问性首选项将自动增加字体大小。

请考虑在同一页的视图设置样式与内置样式与辅助功能设置启用和禁用下面的示例：

已禁用：

![](styles-images/pre-access.png "与辅助功能已禁用的设备样式")

已启用:

![](styles-images/post-access.png "具有可访问性启用设备样式")

若要确保可访问性，请确保在应用中，任何与文本相关样式的基础使用内置样式一致地使用样式。 请参阅[样式](~/xamarin-forms/user-interface/styles/index.md)有关扩展和一般情况下使用样式的详细信息。


## <a name="related-links"></a>相关链接

- [借助 Xamarin.Forms，第 12 章创建移动应用](https://developer.xamarin.com/r/xamarin-forms/book/chapter12.pdf)
- [样式](~/xamarin-forms/user-interface/styles/index.md)
- [文本 （示例）](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)
- [样式](xref:Xamarin.Forms.Style)
