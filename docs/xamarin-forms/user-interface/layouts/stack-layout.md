---
title: Xamarin.Forms StackLayout
description: 此文章介绍了如何使用 Xamarin.Forms StackLayout 类通过一个维度中呈现的视图集合。
ms.prod: xamarin
ms.assetid: 6A91EA70-268C-462C-AAAF-F8DA011403F8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/25/2015
ms.openlocfilehash: 6e278c466c352ad19575cd3a84d6e38e14ec2587
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/08/2018
ms.locfileid: "35244592"
---
# <a name="xamarinforms-stacklayout"></a>Xamarin.Forms StackLayout

`StackLayout` 将组织的一维的行 （"堆栈"） 中的视图水平或垂直。 视图中`StackLayout`可以基于使用布局选项的布局中的空间大小。 定位由视图添加到的布局和视图的布局选项的顺序确定。

[![](stack-layout-images/layouts-sml.png "Xamarin.Forms 布局")](stack-layout-images/layouts.png#lightbox "Xamarin.Forms 布局")

## <a name="purpose"></a>目标

`StackLayout` 并不太复杂比其他视图。 可以通过只需添加到视图中创建简单的线性接口`StackLayout`，和由嵌套它们创建更复杂的接口。

## <a name="usage--behavior"></a>使用情况和行为

### <a name="spacing"></a>间距

默认情况下，`StackLayout`将添加视图之间的 6px 边距。 这可以控制或设置通过设置有无边距`Spacing`StackLayout 上的属性。 下面演示了如何设置间距和不同间距选项的效果：

在 XAML 中：

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="LayoutSamples.StackLayoutDemo"
Title="StackLayout Demo">
  <ContentPage.Content>
    <StackLayout Spacing="10" x:Name="layout">
      <Button Text="StackLayout" VerticalOptions="Start"
        HorizontalOptions="FillAndExpand" />
      <BoxView Color="Yellow" VerticalOptions="FillAndExpand"
        HorizontalOptions="FillAndExpand" />
            <BoxView Color="Green" VerticalOptions="FillAndExpand"
        HorizontalOptions="FillAndExpand" />
            <BoxView HeightRequest="75" Color="Blue" VerticalOptions="End"
        HorizontalOptions="FillAndExpand" />
    </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

在 C# 中：

```csharp
public class StackLayoutCode : ContentPage
{
  public StackLayoutCode ()
  {
    var layout = new StackLayout ();
    var button = new Button { Text = "StackLayout", VerticalOptions = LayoutOptions.Start,
     HorizontalOptions = LayoutOptions.FillAndExpand };
    var yellowBox = new BoxView { Color = Color.Yellow, VerticalOptions = LayoutOptions.FillAndExpand, HorizontalOptions = LayoutOptions.FillAndExpand };
    var greenBox = new BoxView { Color = Color.Green, VerticalOptions = LayoutOptions.FillAndExpand, HorizontalOptions = LayoutOptions.FillAndExpand };
    var blueBox = new BoxView { Color = Color.Blue, VerticalOptions = LayoutOptions.FillAndExpand,
      HorizontalOptions = LayoutOptions.FillAndExpand, HeightRequest = 75 };

    layout.Children.Add(button);
    layout.Children.Add(yellowBox);
    layout.Children.Add(greenBox);
    layout.Children.Add(blueBox);
    layout.Spacing = 10;
    Content = layout;
  }
}
```

间距 = 0:

![](stack-layout-images/spacing-zero.png "与间距 StackLayout = 0")

间距的 10 个：

![](stack-layout-images/spacing-ten.png "与间距 StackLayout = 10")

### <a name="sizing"></a>大小调整

StackLayout 中的视图的大小取决于高度和宽度请求和布局选项。 `StackLayout` 将强制执行填充。 以下`LayoutOption`s 将导致视图以占用所从布局可用空间：

- **CenterAndExpand** &ndash;布局中的将视图中心和扩展以占用所布局将为其提供空间。
- **EndAndExpand** &ndash;布局 （底部或最右侧的边界） 的末尾放置视图，并且扩展以占用所布局将为其提供空间。
- **FillAndExpand** &ndash;放置视图，以便其不填充和占用所布局将为其提供空间。
- **StartAndExpand** &ndash;布局的开头将视图并占用所父级将提供空间。

有关详细信息，请参阅[扩展](~/xamarin-forms/user-interface/layouts/layout-options.md#expansion)。

### <a name="positioning"></a>定位

中 StackLayout 视图可以定位并调整其大小使用`LayoutOptions`。 可以指定每个视图`VerticalOptions`和`HorizontalOptions`，定义如何在视图将确定自己的位置相对于布局。 以下预定义`LayoutOptions`可用：

- **Center** &ndash;布局中的将视图中心。
- **结束**&ndash;布局 （底部或最右侧的边界） 的末尾放置视图。
- **填充**&ndash;放置视图，以便它具有不填充。
- **启动**&ndash;放置布局开头的视图。

下面的代码演示设置布局选项：

在 XAML 中：

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="LayoutSamples.StackLayoutDemo"
Title="StackLayout Demo">
  <ContentPage.Content>
    <StackLayout x:Name="layout">
      <Button VerticalOptions="Start"
        HorizontalOptions="FillAndExpand" />
      <BoxView VerticalOptions="FillAndExpand"
        HorizontalOptions="FillAndExpand" />
            <BoxView VerticalOptions="FillAndExpand"
        HorizontalOptions="FillAndExpand" />
            <BoxView HeightRequest="75" VerticalOptions="End"
        HorizontalOptions="FillAndExpand" />
    </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

在 C# 中：

```csharp
public class StackLayoutCode : ContentPage
{
  public StackLayoutCode ()
  {
    var layout = new StackLayout ();
    var button = new Button { VerticalOptions = LayoutOptions.Start,
     HorizontalOptions = LayoutOptions.FillAndExpand };
    var oneBox = new BoxView { VerticalOptions = LayoutOptions.FillAndExpand, HorizontalOptions = LayoutOptions.FillAndExpand };
    var twoBox = new BoxView {  VerticalOptions = LayoutOptions.FillAndExpand, HorizontalOptions = LayoutOptions.FillAndExpand };
    var threeBox = new BoxView {  VerticalOptions = LayoutOptions.FillAndExpand, HorizontalOptions = LayoutOptions.FillAndExpand };

    layout.Children.Add(button);
    layout.Children.Add(oneBox);
    layout.Children.Add(twoBox);
    layout.Children.Add(threeBox);
    Content = layout;
  }
}
```

有关详细信息，请参阅[对齐](~/xamarin-forms/user-interface/layouts/layout-options.md#alignment)。

## <a name="exploring-a-complex-layout"></a>浏览复杂布局

每个布局有优点和缺点为特定布局的创建。 在这一系列的布局文章，整个示例应用程序已创建具有相同的页面布局实现使用三种不同的布局。

请考虑下面的 XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="TheBusinessTumble.StackLayoutPage"
BackgroundColor="Maroon"
Title="StackLayouts">
    <ContentPage.Content>
    <ScrollView>
      <StackLayout Spacing="0" Padding="0" BackgroundColor="Maroon">
        <BoxView HorizontalOptions="FillAndExpand" HeightRequest="100"
          VerticalOptions="Start" Color="Gray" />
        <Button BorderRadius="30" HeightRequest="60" WidthRequest="60"
          BackgroundColor="Red" HorizontalOptions="Center" VerticalOptions="Start" />
        <StackLayout HeightRequest="100" VerticalOptions="Start" HorizontalOptions="FillAndExpand" Spacing="20" BackgroundColor="Maroon">
          <Label Text="User Name" FontSize="28" HorizontalOptions="Center"
            VerticalOptions="Center" FontAttributes="Bold" />
          <Entry Text="Bio + Hashtags" TextColor="White"
            BackgroundColor="Maroon" HorizontalOptions="FillAndExpand" VerticalOptions="CenterAndExpand" />
        </StackLayout>
        <StackLayout Orientation="Horizontal" HeightRequest="50" BackgroundColor="White" Padding="5">
          <StackLayout Spacing="0" BackgroundColor="White" Orientation="Horizontal" HorizontalOptions="Start">
            <BoxView BackgroundColor="Black" WidthRequest="40" HeightRequest="40"  HorizontalOptions="StartAndExpand" VerticalOptions="Center" />
            <Label FontSize="14" TextColor="Black" Text="Accent Color" HorizontalOptions="StartAndExpand" VerticalOptions="Center" />
          </StackLayout>
          <StackLayout Spacing="0" BackgroundColor="White" Orientation="Horizontal" HorizontalOptions="EndAndExpand">
            <BoxView BackgroundColor="Maroon" WidthRequest="40" HeightRequest="40" HorizontalOptions="Start" VerticalOptions="Center" />
            <Label FontSize="14" TextColor="Black" Text="Primary Color" HorizontalOptions="StartAndExpand" VerticalOptions="Center" />
          </StackLayout>
        </StackLayout>
        <StackLayout Orientation="Horizontal">
          <Label FontSize="14" Text="Age:" TextColor="White" HorizontalOptions="Start" VerticalOptions="Center" WidthRequest="100" />
          <Entry HorizontalOptions="FillAndExpand" Text="35" TextColor="White" BackgroundColor="Maroon" />
        </StackLayout>
        <StackLayout Orientation="Horizontal">
          <Label FontSize="14" Text="Interests:" TextColor="White"
          HorizontalOptions="Start" VerticalOptions="Center" WidthRequest="100" />
          <Entry HorizontalOptions="FillAndExpand" Text="Xamarin.Forms" TextColor="White" BackgroundColor="Maroon" />
        </StackLayout>
        <StackLayout Orientation="Horizontal">
          <Label FontSize="14" Text="Ask me about:" TextColor="White"
          HorizontalOptions="Start" VerticalOptions="Center" WidthRequest="100"/>
          <Entry HorizontalOptions="FillAndExpand" Text="Xamarin, C#, .NET, Mono..." TextColor="White" BackgroundColor="Maroon" />
        </StackLayout>
      </StackLayout>
    </ScrollView>
    </ContentPage.Content>
</ContentPage>

```

上面的代码将导致以下布局：

![](stack-layout-images/stack.png "复杂 StackLayout")

请注意， `StackLayouts`s 嵌套的因为在某些情况下嵌套布局可以更方便地比提供相同的布局中的所有元素。 另请注意，因为`StackLayout`不支持重叠项，找到的页面不具有布局激动人心的一些其他布局的页中。



## <a name="related-links"></a>相关链接

- [LayoutOptions](~/xamarin-forms/user-interface/layouts/layout-options.md)
- [布局 （示例）](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [BusinessTumble 示例 （示例）](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
