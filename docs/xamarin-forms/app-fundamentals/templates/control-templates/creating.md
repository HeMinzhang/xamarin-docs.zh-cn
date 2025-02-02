---
title: 创建 ControlTemplate
description: 可在应用程序级别或页面级别定义控件模板。 本文演示如何创建和使用控件模板。
ms.prod: xamarin
ms.assetid: A9AEB052-FBF5-4589-9BD4-6D6F62BED7F1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/14/2019
ms.openlocfilehash: 523113a7b54541733e14f947eefa247e4f774b99
ms.sourcegitcommit: 157da886e1f304c6b482aa3f265ef7d78b696ab7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/14/2019
ms.locfileid: "69024517"
---
# <a name="create-a-controltemplate"></a>创建 ControlTemplate

[![下载示例](~/media/shared/download.png) 下载示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/templates-controltemplates-simpletheme)

“可在应用程序级别或页面级别定义控件模板。本文演示如何创建和使用控件模板。”_

## <a name="create-a-controltemplate-in-xaml"></a>使用 XAML 创建 ControlTemplate

要在应用程序级别定义 [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate)，必须将 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 添加到 `App` 类。 默认情况下，从模板创建的所有 Xamarin.Forms 应用程序都使用“App”类来实现 [`Application`](xref:Xamarin.Forms.Application) 子类  。 要在应用程序的 `ResourceDictionary` 中，在应用程序级别使用 XAML 声明 `ControlTemplate`，则默认 App 类必须替换为 XAML App 类和相关的代码隐藏，如下面的代码示例所示   ：

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="SimpleTheme.App">
    <Application.Resources>
        <ResourceDictionary>
            <ControlTemplate x:Key="TealTemplate">
                <Grid>
                    ...
                    <BoxView ... />
                    <Label Text="Control Template Demo App"
                           TextColor="White"
                           VerticalOptions="Center" ... />
                    <ContentPresenter ... />
                    <BoxView Color="Teal" ... />
                    <Label Text="(c) Xamarin 2016"
                           TextColor="White"
                           VerticalOptions="Center" ... />
                </Grid>
            </ControlTemplate>
            <ControlTemplate x:Key="AquaTemplate">
                ...
            </ControlTemplate>
        </ResourceDictionary>
    </Application.Resources>
</Application>
```

每个 [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) 实例都创建为 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 中的可重用对象。  这是通过赋予每个声明唯一的 `x:Key` 属性来实现的，此属性为声明提供 `ResourceDictionary` 中的描述性的键。

以下代码示例演示相关的 `App` 代码隐藏：

```csharp
public partial class App : Application
{
  public App ()
  {
    InitializeComponent ();
    MainPage = new HomePage ();
  }
}
```

除了设置 [`MainPage`](xref:Xamarin.Forms.Application.MainPage) 属性外，代码隐藏还必须调用 `InitializeComponent` 方法来加载和分析相关的 XAML。

下面的代码示例显示将 `TealTemplate` 应用到 [`ContentView`](xref:Xamarin.Forms.ContentView) 的 [`ContentPage`](xref:Xamarin.Forms.ContentPage)：

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="SimpleTheme.HomePage">
    <ContentView x:Name="contentView" Padding="0,20,0,0"
                 ControlTemplate="{StaticResource TealTemplate}">
        <StackLayout VerticalOptions="CenterAndExpand">
            <Label Text="Welcome to the app!" HorizontalOptions="Center" />
            <Button Text="Change Theme" Clicked="OnButtonClicked" />
        </StackLayout>
    </ContentView>
</ContentPage>
```

使用 `StaticResource` 标记扩展将 `TealTemplate` 分配给 [`ContentView.ControlTemplate`](xref:Xamarin.Forms.TemplatedView.ControlTemplate) 属性。 [`ContentView.Content`](xref:Xamarin.Forms.ContentView.Content) 属性设置为 [`StackLayout`](xref:Xamarin.Forms.StackLayout)，它定义要显示在 [`ContentPage`](xref:Xamarin.Forms.ContentPage) 上的内容。 该内容将由包含在 `TealTemplate` 中的 [`ContentPresenter`](xref:Xamarin.Forms.ContentPresenter) 显示。 这会导致如以下屏幕截图中所示的外观：

![](creating-images/teal-theme.png "Teal 控件模板")

### <a name="re-theming-an-application-at-runtime"></a>运行时重新设置应用程序的主题

点击“更改主题”按钮，执行 `OnButtonClicked` 方法，如下代码示例所示  ：

```csharp
void OnButtonClicked (object sender, EventArgs e)
{
  originalTemplate = !originalTemplate;
  contentView.ControlTemplate = (originalTemplate) ? tealTemplate : aquaTemplate;
}
```

该方法将有效的 [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) 实例替换为可供选择的 `ControlTemplate` 实例，得到如下屏幕截图：

![](creating-images/aqua-theme.png "Aqua 控件模板")

> [!NOTE]
> 在 `ContentPage` 上，可以分配 `Content` 属性，也可以设置 `ControlTemplate` 属性。 发生这种情况时，如果 `ControlTemplate` 包含 `ContentPresenter` 实例，分配给 `Content` 属性的内容将由 `ContentPresenter` 在 `ControlTemplate` 中提供。

### <a name="set-a-controltemplate-with-a-style"></a>使用样式设置 ControlTemplate

也可以通过 [`Style`](xref:Xamarin.Forms.Style) 应用 [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) 来进一步扩展主题功能。 这可以通过在 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 中为目标视图创建隐式或显式样式，以及在 [`Style`](xref:Xamarin.Forms.Style) 实例中设置目标视图的 `ControlTemplate` 属性来实现   。 下面的代码示例展示已添加到应用程序级别 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 的隐式样式  ：

```xaml
<Style TargetType="ContentView">
    <Setter Property="ControlTemplate" Value="{StaticResource TealTemplate}" />
</Style>
```

因为 [`Style`](xref:Xamarin.Forms.Style) 实例为隐式，所以它将应用于应用程序中的所有 `ContentView` 实例  。 因此，不再需要设置 [`ContentView.ControlTemplate`](xref:Xamarin.Forms.TemplatedView.ControlTemplate) 属性，如下面的代码示例所示：

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="SimpleTheme.HomePage">
    <ContentView x:Name="contentView" Padding="0,20,0,0">
      ...
    </ContentView>
</ContentPage>
```

有关样式的详细信息，请参阅[样式](~/xamarin-forms/user-interface/styles/index.md)。

### <a name="create-a-controltemplate-at-page-level"></a>在页面级别创建 ControlTemplate

除了在应用程序级别上创建 [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) 实例外，还可以在页面级别上创建它们，如下面的代码示例所示：

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="SimpleTheme.HomePage">
    <ContentPage.Resources>
        <ResourceDictionary>
            <ControlTemplate x:Key="TealTemplate">
                ...
            </ControlTemplate>
            <ControlTemplate x:Key="AquaTemplate">
                ...
            </ControlTemplate>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentView ... ControlTemplate="{StaticResource TealTemplate}">
        ...
    </ContentView>
</ContentPage>
```

在页面级别添加 [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) 时，将 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 添加到 [`ContentPage`](xref:Xamarin.Forms.ContentPage)，然后在 `ResourceDictionary` 中包含 `ControlTemplate` 实例。

## <a name="create-a-controltemplate-in-c35"></a>使用 C&#35; 创建 ControlTemplate

要在应用程序级别定义[`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate)，必须创建表示 `ControlTemplate` 的 `class`。 该类应该派生自用于模板的[布局](~/xamarin-forms/user-interface/layouts/index.yml)，如下面的代码示例所示：

```csharp
class TealTemplate : Grid
{
  public TealTemplate ()
  {
    ...
    var contentPresenter = new ContentPresenter ();
    Children.Add (contentPresenter, 0, 1);
    Grid.SetColumnSpan (contentPresenter, 2);
    ...
  }
}

class AquaTemplate : Grid
{
  ...
}
```

`AquaTemplate` 类与 `TealTemplate` 类相同，只是 [`BoxView.Color`](xref:Xamarin.Forms.BoxView.Color) 和 [`Label.TextColor`](xref:Xamarin.Forms.Label.TextColor) 属性使用了不同的颜色。

下面的代码示例显示将 `TealTemplate` 应用到 [`ContentView`](xref:Xamarin.Forms.ContentView) 的 [`ContentPage`](xref:Xamarin.Forms.ContentPage)：

```csharp
public class HomePageCS : ContentPage
{
  ...
  ControlTemplate tealTemplate = new ControlTemplate (typeof(TealTemplate));
  ControlTemplate aquaTemplate = new ControlTemplate (typeof(AquaTemplate));

  public HomePageCS ()
  {
    var button = new Button { Text = "Change Theme" };
    var contentView = new ContentView {
      Padding = new Thickness (0, 20, 0, 0),
      Content = new StackLayout {
        VerticalOptions = LayoutOptions.CenterAndExpand,
        Children = {
          new Label { Text = "Welcome to the app!", HorizontalOptions = LayoutOptions.Center },
          button
        }
      },
      ControlTemplate = tealTemplate
    };
    ...
    Content = contentView;
  }
}
```

通过在 `ControlTemplate` 构造函数中指定定义控件模板的类的类型，创建 [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) 实例。

[`ContentView.Content`](xref:Xamarin.Forms.ContentView.Content) 属性设置为 [`StackLayout`](xref:Xamarin.Forms.StackLayout)，它定义要显示在 [`ContentPage`](xref:Xamarin.Forms.ContentPage) 上的内容。 该内容将由包含在 `TealTemplate` 中的 [`ContentPresenter`](xref:Xamarin.Forms.ContentPresenter) 显示。 前面概述的相同机制用于运行时将主题更改为 `AquaTheme`。

## <a name="get-a-named-element-from-a-template"></a>从模板中获取命名元素

在实例化模板之后，可以检索控件模板中的命名元素。 这可以通过 `GetTemplateChild` 方法来实现，该方法在实例化的 [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) 可视化树中返回命名元素。

在实例化控件模板后，调用模板的 `OnApplyTemplate` 方法。 因此，应该从 [`TemplatedPage`](xref:Xamarin.Forms.TemplatedPage) 派生页（如 [`ContentPage`](xref:Xamarin.Forms.ContentPage)）或 [`TemplatedView`](xref:Xamarin.Forms.TemplatedView) 派生视图（如 [`ContentView`](xref:Xamarin.Forms.ContentView)）中的 `OnApplyTemplate` 替代调用 `GetTemplateChild` 方法。

> [!IMPORTANT]
> 只有在调用 `OnApplyTemplate` 方法后，才能调用 `GetTemplateChild` 方法。

以下示例显示自定义控件的控件模板：

```xaml
<controls:MyCustomControl ...>
    <controls:MyCustomControl.ControlTemplate>
         <ControlTemplate>
              <Label x:Name="myLabel" />
         </ControlTemplate>
    <controls:MyCustomControl.ControlTemplate>
</controls:MyCustomControl>
```

[`Label`](xref:Xamarin.Forms.Label) 元素已命名，因此可以在自定义控件的代码隐藏中检索到。 这是通过从自定义控件的 `OnApplyTemplate` 替代调用 `GetTemplateChild` 方法来实现的：

```csharp
class MyCustomControl : ContentView
{
    Label myLabel;

    protected override void OnApplyTemplate()
    {  
        myLabel = GetTemplateChild("myLabel");
    }
    //...
}
```

本示例检索名为 `myLabel` 的 [`Label`](xref:Xamarin.Forms.Label) 对象。 然后，`MyCustomControl` 类可以访问并操控 `myLabel`。

## <a name="related-links"></a>相关链接

- [样式](~/xamarin-forms/user-interface/styles/index.md)
- [简单主题（示例）](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/templates-controltemplates-simpletheme)
- [ControlTemplate](xref:Xamarin.Forms.ControlTemplate)
- [ContentPresenter](xref:Xamarin.Forms.ContentPresenter)
- [ContentView](xref:Xamarin.Forms.ContentView)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
