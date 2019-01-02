---
title: Xamarin.Forms 简介
description: 本文介绍了 Xamarin.Forms 以及如何开始使用它编写应用程序。
ms.prod: xamarin
ms.assetid: f619595f-3ee7-439b-a1bc-d13e5106e6e9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/25/2018
ms.openlocfilehash: c716f39faad0b58159df5631bf415239a2c658b1
ms.sourcegitcommit: 06f88979db160fb8dd1c9ee0d5000d8749107489
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/28/2018
ms.locfileid: "53806946"
---
# <a name="an-introduction-to-xamarinforms"></a>Xamarin.Forms 简介

[![下载示例](~/media/shared/download.png) 下载示例](https://developer.xamarin.com/samples/xamarin-forms/GettingStarted/)

_Xamarin.Forms 是一种框架，开发人员可以使用它生成适用于 Android、iOS 和 Windows 的跨平台应用程序。平台之间将共享代码和用户界面定义，但使用本机控件呈现它们。本文介绍了 Xamarin.Forms 以及如何开始在 Visual Studio 中使用 C# 和 XAML 编写应用程序。_

Xamarin.Forms 应用程序使用 [.NET Standard](~/cross-platform/app-fundamentals/net-standard.md) 项目包含共享代码，然后将应用程序项目分离，以便使用共享代码并生成各个平台所需的输出。 创建新的 Xamarin.Forms 应用时，解决方案将包含共享代码项目（包括 C# 和 XAML 文件）及特定于平台的项目，如此屏幕截图所示：

![Visual Studio 中的 Xamarin.Forms 模板解决方案](introduction-to-xamarin-forms-images/solution-both.png)

编写 Xamarin.Forms 应用时，代码和用户界面将添加到顶部的 .NET Standard 项目中，Android、iOS 和 UWP 项目均引用此项目。 生成并运行 Android、iOS 和 UWP 项目以测试和部署应用。

## <a name="examining-a-xamarinforms-application"></a>检查 Xamarin.Forms 应用程序

Visual Studio 中的默认 Xamarin.Forms 应用模板显示单个文本标签。 如果运行该应用程序，它应类似于以下屏幕截图：

[![](introduction-to-xamarin-forms-images/image05-sml.png "默认 Xamarin.Forms 应用程序")](introduction-to-xamarin-forms-images/image05.png#lightbox)

屏幕截图中的每个屏幕对应于 Xamarin.Forms 中的一个页面。 [`Page`](xref:Xamarin.Forms.Page) 在 Android 表示为一个活动，在 iOS 中表示为一个视图控制器，在 Windows 通用平台 (UWP) 中则表示为一个页面。 以上屏幕截图中的示例实例化 [`ContentPage`](xref:Xamarin.Forms.ContentPage) 对象，并使用该对象显示 [`Label`](xref:Xamarin.Forms.Label)。

为了最大限度重用启动代码，Xamarin.Forms 应用程序有一个名为 `App` 的单个类，该类负责实例化第一个将要显示的 [`Page`](xref:Xamarin.Forms.Page)。 `App` 类的示例可以在以下代码中看到（在 App.xaml.cs 中）：

```csharp
public partial class App : Application
{
  public App ()
  {
    InitializeComponent();
    MainPage = new MainPage(); // sets the App.MainPage property to an instance of the MainPage class
  }
}
```

此代码实例化名称为 `MainPage` 的新 [`ContentPage`](xref:Xamarin.Forms.ContentPage) 对象，该对象将在页面上垂直和水平居中显示单个 [`Label`](xref:Xamarin.Forms.Label)。 MainPage.xaml 文件中的 XAML 如下所示：

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" xmlns:local="clr-namespace:AwesomeApp" x:Class="AwesomeApp.MainPage">
    <StackLayout>
        <Label Text="Hello Xamarin.Forms"
           HorizontalOptions="Center"
           VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

### <a name="launching-the-initial-xamarinforms-page-on-each-platform"></a>在每个平台上启动 Xamarin.Forms 初始页面

> [!TIP]
> 在此部分中提供了特定于平台的信息，以便于了解 Xamarin.Forms 的工作方式。
> 项目模板已包含这些类；无需自己为它们编码。
>
> 可以跳至[用户界面](#user-interface)部分，稍后再阅读此部分。

若要在应用程序内部使用某个页面（如上面示例中的 MainPage），每个平台应用程序启动时必须初始化 Xamarin.Forms 框架并提供该页面的一个实例。 初始化步骤因平台而异，此内容将在以下部分讨论。

#### <a name="ios"></a>iOS

若要在 iOS 中启动 Xamarin.Forms 初始页面，平台项目应包括 `AppDelegate` 类（该类从 `Xamarin.Forms.Platform.iOS.FormsApplicationDelegate` 类继承），如以下代码示例所示：

```csharp
[Register("AppDelegate")]
public partial class AppDelegate : global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate
{
    public override bool FinishedLaunching(UIApplication app, NSDictionary options)
    {
      global::Xamarin.Forms.Forms.Init ();
      LoadApplication (new App ());
      return base.FinishedLaunching (app, options);
    }
}
```

通过调用 `Init` 方法，`FinishedLaunching` 替代初始化 Xamarin.Forms 框架。 这会导致在调用将根视图控制器设置为 `LoadApplication` 方法之前，将特定于 iOS 的 Xamarin.Forms 实现加载到应用程序。

#### <a name="android"></a>Android

若要在 Android 中启动 Xamarin.Forms 初始页面，平台项目应包括可使用 `MainLauncher` 属性创建 `Activity` 的代码，且具有从 `FormsAppCompatActivity` 类继承的活动，如以下代码示例所示：

```csharp
namespace HelloXamarinFormsWorld.Android
{
    [Activity(Label = "HelloXamarinFormsWorld", Theme = "@style/MainTheme", MainLauncher = true,
        ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
    public class MainActivity : FormsAppCompatActivity
    {
        protected override void OnCreate(Bundle bundle)
        {
            base.OnCreate(bundle);
            Xamarin.Forms.Forms.Init(this, bundle);
            LoadApplication (new App ());
        }
    }
}
```

通过调用 `Init` 方法，`OnCreate` 替代初始化 Xamarin.Forms 框架。 这会导致在加载 Xamarin.Forms 应用程序之前，将特定于 Android 的 Xamarin.Forms 实现加载到应用程序。

#### <a name="universal-windows-platform-uwp"></a>通用 Windows 平台 (UWP)

在通用 Windows 平台 (UWP) 应用程序中，可从 `App` 类调用初始化 Xamarin.Forms 框架的 `Init` 方法：

```csharp
Xamarin.Forms.Forms.Init (e);

if (e.PreviousExecutionState == ApplicationExecutionState.Terminated)
{
  ...
}
```

这会将特定于 UWP 的 Xamarin.Forms 实现加载到应用程序。 可通过 `MainPage` 类启动 Xamarin.Forms 初始页面，如以下代码示例所示：

```csharp
public partial class MainPage
{
    public MainPage()
    {
      this.InitializeComponent();
      this.LoadApplication(new HelloXamarinFormsWorld.App());
    }
}
```

可通过 `LoadApplication` 方法加载 Xamarin.Forms 应用程序。 创建新的 Xamarin.Forms 项目时，Visual Studio 会添加以上所有代码。

## <a name="user-interface"></a>用户界面

在 Xamarin.Forms 中创建用户界面可以采用两种方法：

- 完全使用 C# 源代码创建用户界面。
- Extensible Application Markup Language (XAML)，一种用于描述用户界面的声明性标记语言。

无论使用哪种方法，均可获得相同的结果（并且下文对两种方法均提供了说明）。 有关 Xamarin.Forms XAML 的详细信息，请参阅 [XAML 基础](~/xamarin-forms/xaml/xaml-basics/index.md)。

### <a name="views-and-layouts"></a>视图和布局

可使用 4 个主要控件组创建 Xamarin.Forms 应用程序的用户界面。

- **页面** - Xamarin.Forms 页呈现跨平台移动应用程序屏幕。 有关页面的详细信息，请参阅 [Xamarin.Forms 页面](~/xamarin-forms/user-interface/controls/pages.md)。
- **布局** - Xamarin.Forms 布局是用于将视图组合到逻辑结构的容器。 有关布局的详细信息，请参阅 [Xamarin.Forms 布局](~/xamarin-forms/user-interface/controls/layouts.md)。
- **视图** - Xamarin.Forms 视图是显示在用户界面上的控件，如标签、按钮和文本输入框。 有关视图的详细信息，请参阅 [Xamarin.Forms 视图](~/xamarin-forms/user-interface/controls/views.md)。
- **单元格** - Xamarin.Forms 单元格是专门用于列表中的项的元素，描述列表中每个项的绘制方式。 有关单元格的详细信息，请参阅 [Xamarin.Forms 单元格](~/xamarin-forms/user-interface/controls/cells.md)。

在运行时，每个控件都会映射到其本身的本机等效项（即屏幕呈现的内容）。

控件在布局内部进行托管。 下文介绍了一种常用的布局 &ndash; [`StackLayout`](xref:Xamarin.Forms.StackLayout) 类。

### <a name="stacklayout"></a>StackLayout

[`StackLayout`](xref:Xamarin.Forms.StackLayout) 在屏幕上自动排列控件而不考虑屏幕大小，从而简化了跨平台应用程序开发。 根据添加顺序，以垂直方式或水平方式逐个放置每个子元素。 `StackLayout` 使用的空间大小取决于 [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) 和 [`VerticalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) 属性的设置方式，但默认情况下，`StackLayout` 尝试使用整个屏幕。

以下 XAML 代码举例说明了如何使用 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 排列三个 [`Label`](xref:Xamarin.Forms.Label) 控件：

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml" x:Class="HelloXamarinFormsWorldXaml.StackLayoutExample1" Padding="20">
  <StackLayout Spacing="10">
    <Label Text="Stop" BackgroundColor="Red" Font="20" />
    <Label Text="Slow down" BackgroundColor="Yellow" Font="20" />
    <Label Text="Go" BackgroundColor="Green" Font="20" />
  </StackLayout>
</ContentPage>
```

以下代码示例显示相应的 C# 代码：

```csharp
public class StackLayoutExample : ContentPage
{
    public StackLayoutExample()
    {
        Padding = new Thickness(20);
        var red = new Label
        {
            Text = "Stop", BackgroundColor = Color.Red, FontSize = 20
        };
        var yellow = new Label
        {
            Text = "Slow down", BackgroundColor = Color.Yellow, FontSize = 20
        };
        var green = new Label
        {
            Text = "Go", BackgroundColor = Color.Green, FontSize = 20
        };

        Content = new StackLayout
        {
            Spacing = 10,
            Children = { red, yellow, green }
        };
    }
}
```

默认情况下，[`StackLayout`](xref:Xamarin.Forms.StackLayout) 采用垂直方向，如以下屏幕截图所示：

[![](introduction-to-xamarin-forms-images/image09-sml.png "垂直 StackLayout")](introduction-to-xamarin-forms-images/image09.png#lightbox "Vertical StackLayout")

可以将 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 更改为水平方向，如以下 XAML 代码示例所示：

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml" x:Class="HelloXamarinFormsWorldXaml.StackLayoutExample2" Padding="20">
  <StackLayout Spacing="10" VerticalOptions="End" Orientation="Horizontal" HorizontalOptions="Start">
    <Label Text="Stop" BackgroundColor="Red" Font="20" />
    <Label Text="Slow down" BackgroundColor="Yellow" Font="20" />
    <Label Text="Go" BackgroundColor="Green" Font="20" />
  </StackLayout>
</ContentPage>
```

以下代码示例显示相应的 C# 代码：

```csharp
public class StackLayoutExample: ContentPage
{
    public StackLayoutExample()
    {
        // Code that creates red, yellow, green labels removed for clarity (see above)
        Content = new StackLayout
        {
            Spacing = 10,
            VerticalOptions = LayoutOptions.End,
            Orientation = StackOrientation.Horizontal,
            HorizontalOptions = LayoutOptions.Start,
            Children = { red, yellow, green }
        };
    }
}
```

以下屏幕截图显示布局结果：

[![](introduction-to-xamarin-forms-images/image10-sml.png "水平 StackLayout")](introduction-to-xamarin-forms-images/image10.png#lightbox "Horizontal StackLayout")

可通过 `HeightRequest` 和 `WidthRequest` 属性设置控件大小，如以下 XAML 代码示例所示：

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml" x:Class="HelloXamarinFormsWorldXaml.StackLayoutExample3" Padding="20">
  <StackLayout Spacing="10" VerticalOptions="End" Orientation="Horizontal" HorizontalOptions="Start">
    <Label Text="Stop" BackgroundColor="Red" Font="20" WidthRequest="100" />
    <Label Text="Slow down" BackgroundColor="Yellow" Font="20" WidthRequest="100" />
    <Label Text="Go" BackgroundColor="Green" Font="20" WidthRequest="200" />
  </StackLayout>
</ContentPage>
```

以下代码示例显示相应的 C# 代码：

```csharp
var red = new Label
{
    Text = "Stop", BackgroundColor = Color.Red, FontSize = 20, WidthRequest = 100
};
var yellow = new Label
{
    Text = "Slow down", BackgroundColor = Color.Yellow, FontSize = 20, WidthRequest = 100
};
var green = new Label
{
    Text = "Go", BackgroundColor = Color.Green, FontSize = 20, WidthRequest = 200
};

Content = new StackLayout
{
    Spacing = 10,
    VerticalOptions = LayoutOptions.End,
    Orientation = StackOrientation.Horizontal,
    HorizontalOptions = LayoutOptions.Start,
    Children = { red, yellow, green }
};
```

以下屏幕截图显示布局结果：

[![](introduction-to-xamarin-forms-images/image11-sml.png "带 LayoutOptions 的水平 StackLayout")](introduction-to-xamarin-forms-images/image11.png#lightbox)

有关 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 类的详细信息，请参阅 [StackLayout](~/xamarin-forms/user-interface/layouts/stack-layout.md)。

## <a name="lists-in-xamarinforms"></a>Xamarin.Forms 中的列表

[`ListView`](xref:Xamarin.Forms.ListView) 控件负责在屏幕上显示项集合 - `ListView` 中的每一个项都包含在单个单元格中。 默认情况下，`ListView` 使用内置 [`TextCell`](xref:Xamarin.Forms.TextCell) 模板并呈现单行文本。

以下代码示例演示一个简单的 [`ListView`](xref:Xamarin.Forms.ListView) 示例：

```csharp
var listView = new ListView
{
    RowHeight = 40
};
listView.ItemsSource = new string []
{
    "Buy pears", "Buy oranges", "Buy mangos", "Buy apples", "Buy bananas"
};
Content = new StackLayout
{
    VerticalOptions = LayoutOptions.FillAndExpand,
    Children = { listView }
};
```

以下屏幕截图显示生成的 [`ListView`](xref:Xamarin.Forms.ListView)：

 ![](introduction-to-xamarin-forms-images/image13.png "ListView")

有关 [`ListView`](xref:Xamarin.Forms.ListView) 控件的详细信息，请参阅 [ListView](~/xamarin-forms/user-interface/listview/index.md)。

### <a name="binding-to-a-custom-class"></a>绑定到自定义类

[`ListView`](xref:Xamarin.Forms.ListView) 控件还可以通过默认的 [`TextCell`](xref:Xamarin.Forms.TextCell) 模板显示自定义对象。

以下代码示例演示 `TodoItem` 类：

```csharp
public class TodoItem
{
    public string Name { get; set; }
    public bool Done { get; set; }
}
```

可按如下代码示例所示填充 [`ListView`](xref:Xamarin.Forms.ListView) 控件：

```csharp
listView.ItemsSource = new TodoItem [] {
    new TodoItem { Name = "Buy pears" },
    new TodoItem { Name = "Buy oranges", Done=true} ,
    new TodoItem { Name = "Buy mangos" },
    new TodoItem { Name = "Buy apples", Done=true },
    new TodoItem { Name = "Buy bananas", Done=true }
};
```

可以创建一个绑定以设置 [`ListView`](xref:Xamarin.Forms.ListView) 要显示的 `TodoItem` 属性类型，如以下代码示例所示：

```csharp
listView.ItemTemplate = new DataTemplate(typeof(TextCell));
listView.ItemTemplate.SetBinding(TextCell.TextProperty, "Name");
```

这会创建一个绑定，指定 `TodoItem.Name` 属性的路径，并生成前面显示的屏幕截图。

有关绑定到自定义类的详细信息，请参阅 [ListView 数据源](~/xamarin-forms/user-interface/listview/data-and-databinding.md)。

### <a name="selecting-an-item-in-a-listview"></a>选择 ListView 中的项

为响应用户触摸 [`ListView`](xref:Xamarin.Forms.ListView) 中的单元格，应处理 [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) 事件，如以下代码示例所示：

```csharp
listView.ItemSelected += async (sender, e) => {
    await DisplayAlert("Tapped!", e.SelectedItem + " was tapped.", "OK");
};
```

当包含在 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) 中时，[`PushAsync`](xref:Xamarin.Forms.NavigationPage.PushAsync(Xamarin.Forms.Page)) 方法可用来打开具有内置后退导航的新页面。 [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) 事件可以访问通过 [`e.SelectedItem`](xref:Xamarin.Forms.SelectedItemChangedEventArgs.SelectedItem) 属性与单元格关联的对象，将其绑定到新页面并使用 `PushAsync` 显示新页面，如以下代码示例所示：

```csharp
listView.ItemSelected += async (sender, e) => {
    var todoItem = (TodoItem)e.SelectedItem;
    var todoPage = new TodoItemPage(todoItem); // so the new page shows correct data
    await Navigation.PushAsync(todoPage);
};
```

每个平台实现内置后退导航的方法都各不相同。 有关详细信息，请参阅[导航](#Navigation)。

有关 [`ListView`](xref:Xamarin.Forms.ListView) 选择的详细信息，请参阅 [ListView 交互性](~/xamarin-forms/user-interface/listview/interactivity.md)。

### <a name="customizing-the-appearance-of-a-cell"></a>自定义单元格的外观

通过子类化 [`ViewCell`](xref:Xamarin.Forms.ViewCell) 类，并将该类的类型设置为 [`ListView`](xref:Xamarin.Forms.ListView) 的 [`ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) 属性，可以自定义单元格的外观。

以下屏幕截图所示的单元格由一个 [`Image`](xref:Xamarin.Forms.Image) 和两个 [`Label`](xref:Xamarin.Forms.Label) 控件组成：

 ![](introduction-to-xamarin-forms-images/image14.png "ListView 自定义单元格外观")

若要创建此自定义布局，应子类化 [`ViewCell`](xref:Xamarin.Forms.ViewCell) 类，如以下代码示例所示：

```csharp
class EmployeeCell : ViewCell
{
    public EmployeeCell()
    {
        var image = new Image
        {
            HorizontalOptions = LayoutOptions.Start
        };
        image.SetBinding(Image.SourceProperty, new Binding("ImageUri"));
        image.WidthRequest = image.HeightRequest = 40;

        var nameLayout = CreateNameLayout();
        var viewLayout = new StackLayout()
        {
           Orientation = StackOrientation.Horizontal,
           Children = { image, nameLayout }
        };
        View = viewLayout;
    }

    static StackLayout CreateNameLayout()
    {
        var nameLabel = new Label
        {
            HorizontalOptions= LayoutOptions.FillAndExpand
        };
        nameLabel.SetBinding(Label.TextProperty, "DisplayName");

        var twitterLabel = new Label
        {
           HorizontalOptions = LayoutOptions.FillAndExpand,
           FontSize = 24
        };
        twitterLabel.SetBinding(Label.TextProperty, "Twitter");

        var nameLayout = new StackLayout()
        {
           HorizontalOptions = LayoutOptions.StartAndExpand,
           Orientation = StackOrientation.Vertical,
           Children = { nameLabel, twitterLabel }
        };
        return nameLayout;
    }
}
```

此段代码执行下列任务：

-  添加 [`Image`](xref:Xamarin.Forms.Image) 控件并将其绑定到 `Employee` 对象的 `ImageUri` 属性。 若要深入了解数据绑定，请参阅[数据绑定](#Data_Binding)。
-  创建垂直方向的 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 来存放两个 [`Label`](xref:Xamarin.Forms.Label) 控件。 `Label` 控件被绑定到 `DisplayName` 和 `Employee` 对象的 `Twitter` 属性。
-  创建 [`StackLayout`](xref:Xamarin.Forms.StackLayout)用于托管现有的 [`Image`](xref:Xamarin.Forms.Image) 和 `StackLayout`。 使用水平方向排列其子级。

创建好自定义单元格后，可以通过将其包装在 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 中由 [`ListView`](xref:Xamarin.Forms.ListView) 控件使用，如以下代码示例所示：

```csharp
List<Employee> myListOfEmployeeObjects = GetAListOfAllEmployees();
var listView = new ListView
{
    RowHeight = 40
};
listView.ItemsSource = myListOfEmployeeObjects;
listView.ItemTemplate = new DataTemplate(typeof(EmployeeCell));
```

此代码为 [`ListView`](xref:Xamarin.Forms.ListView) 提供 `Employee` 的 `List`。 可使用 `EmployeeCell` 类呈现每个单元格。 `ListView` 将 `Employee` 对象作为其 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) 传递给 `EmployeeCell`。

有关自定义单元格的外观的详细信息，请参阅[单元格的外观](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md)。

### <a name="using-xaml-to-create-and-customize-a-list"></a>使用 XAML 创建和自定义列表

以下代码示例演示上一部分中 [`ListView`](xref:Xamarin.Forms.ListView) 的 XAML 等效项：

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:local="clr-namespace:XamarinFormsXamlSample;assembly=XamarinFormsXamlSample"
             xmlns:constants="clr-namespace:XamarinFormsSample;assembly=XamarinFormsXamlSample"
             x:Class="XamarinFormsXamlSample.Views.EmployeeListPage"
             Title="Employee List">
  <ListView x:Name="listView" IsVisible="false" ItemsSource="{x:Static local:App.Employees}" ItemSelected="EmployeeListOnItemSelected">
    <ListView.ItemTemplate>
      <DataTemplate>
        <ViewCell>
          <ViewCell.View>
            <StackLayout Orientation="Horizontal">
              <Image Source="{Binding ImageUri}" WidthRequest="40" HeightRequest="40" />
              <StackLayout Orientation="Vertical" HorizontalOptions="StartAndExpand">
                <Label Text="{Binding DisplayName}" HorizontalOptions="FillAndExpand" />
                <Label Text="{Binding Twitter}" Font="{x:Static constants:Fonts.Twitter}"/>
              </StackLayout>
            </StackLayout>
          </ViewCell.View>
        </ViewCell>
      </DataTemplate>
    </ListView.ItemTemplate>
  </ListView>
</ContentPage>
```

此 XAML 定义包含 [`ListView`](xref:Xamarin.Forms.ListView) 的 [`ContentPage`](xref:Xamarin.Forms.ContentPage)。 可通过 [`ItemsSource`](xref:Xamarin.Forms.ItemsView`1.ItemsSource) 属性设置 `ListView` 的数据源。 在 [`ListView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) 元素内定义 `ItemsSource` 中每一行的布局。

## <a name="data-binding"></a>数据绑定

数据绑定连接两个对象，即源和目标。 源对象提供数据。 目标对象使用（并经常显示）来自源对象的数据。 例如，[`Label`](xref:Xamarin.Forms.Label)（目标对象）通常会将其 [`Text`](xref:Xamarin.Forms.Label.Text) 属性绑定到源对象中的公共 `string` 属性。 下图说明了这种绑定关系：

![](introduction-to-xamarin-forms-images/data-binding.png "数据绑定")

数据绑定的主要优点是让你无需再担心视图和数据源之间的数据同步。 幕后的绑定框架源会将源对象中的更改自动推送到目标对象，且目标对象中的更改可选择性地推送回源对象。

创建数据绑定只需两个步骤：

- 目标对象的 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) 属性必须设置为源。
- 必须在目标和源之间建立绑定。 在 XAML 中，此过程可通过使用 [`Binding`](xref:Xamarin.Forms.Xaml.BindingExtension) 标记扩展实现。 在 C# 中，此过程可通过 [`SetBinding`](xref:Xamarin.Forms.BindableObject.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase)) 方法实现。

若要深入了解数据绑定，请参阅[数据绑定基本知识](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)。

### <a name="xaml"></a>XAML

以下代码示例说明如何在 XAML 中执行数据绑定：

```xaml
<Entry Text="{Binding FirstName}" ... />
```

在 [`Entry.Text`](xref:Xamarin.Forms.Entry.Text) 属性和源 对象的 `FirstName` 属性之间建立绑定。 `Entry` 控件中所做的更改将自动传播到 `employeeToDisplay` 对象。 同样，如果更改了 `employeeToDisplay.FirstName` 属性，Xamarin.Forms 绑定引擎也会更新 `Entry` 控件的内容。 这称为双向绑定。 为了使双向绑定发挥作用，模型类必须实现 `INotifyPropertyChanged` 接口。

尽管可以在 XAML 中设置 `EmployeeDetailPage` 类的 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) 属性，但此处在代码隐藏中设置 `Employee` 对象的实例：

```csharp
public EmployeeDetailPage(Employee employee)
{
    InitializeComponent();
    this.BindingContext = employee;
}
```

虽然可以分别设置每个目标对象的 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) 属性，但没有必要。 `BindingContext` 是特殊属性，其所有子级都会继承该属性。 因此，当 [`ContentPage`](xref:Xamarin.Forms.ContentPage) 上的 `BindingContext` 设置为 `Employee` 实例时，`ContentPage` 的所有子级都具有相同的 `BindingContext`，并且都可绑定到 `Employee` 对象的公共属性。

### <a name="c35"></a>C&#35;

以下代码示例说明如何在 C# 中执行数据绑定：

```csharp
public EmployeeDetailPage(Employee employeeToDisplay)
{
    this.BindingContext = employeeToDisplay;
    var firstName = new Entry()
    {
        HorizontalOptions = LayoutOptions.FillAndExpand
    };
    firstName.SetBinding(Entry.TextProperty, "FirstName");
    ...
}
```

向 [`ContentPage`](xref:Xamarin.Forms.ContentPage) 构造函数传递 `Employee` 对象的一个实例，并将 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) 设置为要绑定到的对象。 实例化 [`Entry`](xref:Xamarin.Forms.Entry) 控件，并在 [`Entry.Text`](xref:Xamarin.Forms.Entry.Text) 属性和源对象的 `FirstName` 属性之间设置绑定。 `Entry` 控件中所做的更改将自动传播到 `employeeToDisplay` 对象。 同样，如果更改了 `employeeToDisplay.FirstName` 属性，Xamarin.Forms 绑定引擎也会更新 `Entry` 控件的内容。 这称为双向绑定。 为了使双向绑定发挥作用，模型类必须实现 `INotifyPropertyChanged` 接口。

`SetBinding` 方法采用两个参数。 第一个参数指定绑定类型的信息。 第二个参数提供绑定内容或绑定方式的信息。 大多数情况下，第二个参数只是一个字符串，持有 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) 上的属性名称。 使用下列语法可直接绑定到 `BindingContext`：

```csharp
someLabel.SetBinding(Label.TextProperty, new Binding("."));
```

使用点语法指示 Xamarin.Forms 使用 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)（而非 `BindingContext` 上的属性）作为数据源。 如果 `BindingContext` 是简单类型（例如 `string` 或 `int`），此语法非常有用。

### <a name="property-change-notification"></a>属性更改通知

创建绑定后，默认情况下目标对象只接收源对象的值。 若要使 UI 与数据源同步，当源对象发生更改时，必须通过一种方法来通知目标对象。 `INotifyPropertyChanged` 接口就提供了这种机制。 当基础属性值发生更改时，实现此接口可以通知任何数据绑定控件。

当它的某个属性更新为新值时，实现 `INotifyPropertyChanged` 的对象必须引发 `PropertyChanged` 事件，如以下代码示例所示：

```csharp
public class MyObject : INotifyPropertyChanged
{
    public event PropertyChangedEventHandler PropertyChanged;

    string _firstName;
    public string FirstName
    {
        get { return _firstName; }
        set
        {
            if (value.Equals(_firstName, StringComparison.Ordinal))
            {
                // Nothing to do - the value hasn't changed;
                return;
            }
            _firstName = value;
            OnPropertyChanged();

        }
    }

    void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        var handler = PropertyChanged;
        if (handler != null)
        {
            handler(this, new PropertyChangedEventArgs(propertyName));
        }
    }
}
```

当 `MyObject.FirstName` 属性发生更改时，会调用 `OnPropertyChanged` 法，从而引发 `PropertyChanged` 事件。 为了避免不必要的事件触发，如果属性值没有发生更改，则不引发 `PropertyChanged` 事件。

请注意，在 `OnPropertyChanged` 方法中，`propertyName` 参数标有 `CallerMemberName` 属性。 这可确保当使用 `null` 值调用 `OnPropertyChanged` 方法时，`CallerMemberName` 属性提供调用 `OnPropertyChanged` 的方法的名称。

## <a name="navigation"></a>导航

Xamarin.Forms 提供多种不同的页导航体验，具体取决于使用的 [`Page`](xref:Xamarin.Forms.Page) 类型。 对于 [`ContentPage`](xref:Xamarin.Forms.ContentPage) 实例，提供两种导航体验：

- [分层导航](#Hierarchical_Navigation)
- [模式导航](#Modal_Navigation)

[`CarouselPage`](xref:Xamarin.Forms.CarouselPage)[`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) 和 [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) 类提供替代导航体验。 有关详细信息，请参阅[导航](~/xamarin-forms/app-fundamentals/navigation/index.md)。

### <a name="hierarchical-navigation"></a>分层导航

[`NavigationPage`](xref:Xamarin.Forms.NavigationPage) 类提供分层导航体验，用户可以随心所欲地向前或向后导航页面。 此类将导航实现为 [`Page`](xref:Xamarin.Forms.Page) 对象的后进先出 (LIFO) 堆栈。

在分层导航中，使用 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) 类在 [`ContentPage`](xref:Xamarin.Forms.ContentPage) 对象的堆栈内进行导航。 若要从一页移动到另一页，应用程序会将新页推送到导航堆栈中，在堆栈中，该页会变为活动页。 若要返回到前一页，应用程序会从导航堆栈弹出当前页，而使最顶层的页成为活动页。

添加到导航堆栈中的第一页称为应用程序的根页，以下代码示例显示了实现此过程的方法：

```csharp
public App ()
{
    MainPage = new NavigationPage(new EmployeeListPage());
}
```

若要导航到 `LoginPage`，需要调用当前页的 [`Navigation`](xref:Xamarin.Forms.VisualElement.Navigation) 属性上的 [`PushAsync`](xref:Xamarin.Forms.NavigationPage.PushAsync(Xamarin.Forms.Page)) 方法，如以下代码示例所示：

```csharp
await Navigation.PushAsync(new LoginPage());
```

这会将新的 `LoginPage` 对象推送到导航堆栈中，在堆栈中，它成为活动页。

通过设备上的返回按钮（无论是设备上的物理按钮还是屏幕按钮），可以从导航堆栈中弹出活动页。 若要以编程方式返回到前一页，`LoginPage` 实例必须调用 [`PopAsync`](xref:Xamarin.Forms.NavigationPage.PopAsync) 方法，如以下代码示例所示：

```csharp
await Navigation.PopAsync();
```

有关分层导航的详细信息，请参阅[分层导航](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md)。

### <a name="modal-navigation"></a>模式导航

Xamarin.Forms 支持模式页面。 模式页面鼓励用户完成独立任务，在完成或取消该任务之前，不允许导航离开该任务。

模式页面可以是 Xamarin.Forms 支持的任何 [`Page`](xref:Xamarin.Forms.Page) 类型。 若要显示模式页面，应用程序会将页面推送到导航堆栈中，在堆栈中，该页会变为活动页。 若要返到回前一页，应用程序会从导航堆栈弹出当前页面，而使最顶层的页成为活动页。

可以由任何 [`Page`](xref:Xamarin.Forms.Page) 派生类型上的 [`Navigation`](xref:Xamarin.Forms.VisualElement.Navigation) 属性公开模式导航方法。 也可使用 [`Navigation`](xref:Xamarin.Forms.VisualElement.Navigation) 属性公开 [`ModalStack`](xref:Xamarin.Forms.INavigation.ModalStack) 属性，并从中获得导航堆栈中的模式页面。 但是，在模式导航中没有执行模式堆栈操作或弹出到根页的概念。 这是因为基础平台普遍都不支持这些操作。

> [!NOTE]
> 执行模式页面导航无需具有 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) 实例。

若要以模式方式导航到 `LoginPage`，需要调用当前页的 [`Navigation`](xref:Xamarin.Forms.VisualElement.Navigation) 属性上的 [`PushModalAsync`](xref:Xamarin.Forms.INavigation.PushModalAsync*) 方法，如以下代码示例所示：

```csharp
await Navigation.PushModalAsync(new LoginPage());
```

这会将 `LoginPage` 实例推送到导航堆栈中，在堆栈中，它成为活动页。

通过设备上的返回按钮（无论是设备上的物理按钮还是屏幕按钮），可以从导航堆栈中弹出活动页。 若要以编程方式返回原始页，`LoginPage` 实例必须调用 [`PopModalAsync`](xref:Xamarin.Forms.INavigation.PopModalAsync) 方法，如以下代码示例所示：

```csharp
await Navigation.PopModalAsync();
```

这会从导航堆栈中删除 `LoginPage` 实例，而使最顶层的页成为活动页。

有关模式导航的详细信息，请参阅[模式页面](~/xamarin-forms/app-fundamentals/navigation/modal.md)。

## <a name="next-steps"></a>后续步骤

本文介绍了如何开始编写 Xamarin.Forms 应用程序。 建议的后续步骤包括了解以下功能：

- 控件模板让你在运行时能够轻松设计或重新设计应用程序页面的主题。 有关详细信息，请参阅[控件模板](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md)。
- 数据模板让你可以在支持的控件上定义数据表示形式。 有关详细信息，请参阅[数据模板](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)。
- 共享代码可通过 [`DependencyService`](xref:Xamarin.Forms.DependencyService) 类访问本机功能。 有关详细信息，请参阅[通过 DependencyService 访问本机功能](~/xamarin-forms/app-fundamentals/dependency-service/index.md)。
- Xamarin.Forms 具有简单的消息传送服务，用于发送和接收消息，减少两个类之间的耦合。 有关详细信息，请参阅[通过 MessagingCenter 发布和订阅](~/xamarin-forms/app-fundamentals/messaging-center.md)。
- 通过 `Renderer` 类可以在每个平台上以不同方式呈现每个页面、布局和控件，反过来又可以创建本机控件，在屏幕上排列该控件，并添加共享代码中指定的行为。 开发人员可以实现自定义 `Renderer` 类，以自定义控件的外观和/或行为。 有关详细信息，请参阅[自定义呈现器](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)。
- 还可以自定义每个平台上的本机控件的效果。 通过子类化 [`PlatformEffect`](xref:Xamarin.Forms.PlatformEffect`2) 控件在特定于平台的项目中创建效果，并将其附加到相应的 Xamarin.Forms 控件中使用。 有关详细信息，请参阅[效果](~/xamarin-forms/app-fundamentals/effects/index.md)。

此外，也可以阅读 Charles Petzold 撰写的[使用 Xamarin.Forms 创建移动应用](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md)一书，了解有关 Xamarin.Forms 的详细信息。 可获取此书的 PDF 版本或多种电子书格式的版本。

## <a name="related-links"></a>相关链接

- [XAML 基础知识](~/xamarin-forms/xaml/xaml-basics/index.md)
- [控件引用](~/xamarin-forms/user-interface/controls/index.md)
- [用户界面](~/xamarin-forms/user-interface/index.md)
- [Xamarin.Forms 示例](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [入门示例](https://developer.xamarin.com/samples/xamarin-forms/GettingStarted/)
- [Xamarin.Forms API 参考](xref:Xamarin.Forms)
- [免费自学教程（视频）](https://university.xamarin.com/self-guided)
