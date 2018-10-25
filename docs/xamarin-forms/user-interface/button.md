---
title: Xamarin.Forms 按钮
description: 按钮响应点击或单击，将定向的应用程序来执行特定任务。
ms.prod: xamarin
ms.assetid: 62CAEB63-0800-44F4-9B8C-EE632138C2F5
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 07/26/2018
ms.openlocfilehash: ed711efe7f4b138b3ba61437dc96f8aa07d17aeb
ms.sourcegitcommit: 7f6127c2f425fadc675b77d14de7a36103cff675
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2018
ms.locfileid: "35244911"
---
# <a name="xamarinforms-button"></a>Xamarin.Forms 按钮

_按钮响应点击或单击，将定向的应用程序来执行特定任务。_

[ `Button` ](xref:Xamarin.Forms.Button)是所有 Xamarin.Forms 中最基本的交互控件。 `Button`通常会显示一个短文本字符串，指示某个命令，但它还可以显示图像和位图图像，或文本的组合。 用户按`Button`用手指或用来启动该命令的鼠标单击。

大部分下面讨论的主题中的页对应[ **ButtonDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ButtonDemos)示例。

## <a name="handling-button-clicks"></a>处理按钮单击

`Button` 定义[ `Clicked` ](xref:Xamarin.Forms.Button.Clicked)在用户点击时激发的事件`Button`用手指或鼠标指针。 图面中释放手指或鼠标按钮时触发该事件`Button`。 `Button`必须具有其[ `IsEnabled` ](xref:Xamarin.Forms.VisualElement.IsEnabled)属性设置为`true`，以便响应分流点。

**基本按钮单击**页面[ **ButtonDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ButtonDemos)示例演示如何实例化`Button`XAML 和句柄中其`Clicked`事件。 **BasicButtonClickPage.xaml**文件包含`StackLayout`两个`Label`和`Button`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ButtonDemos.BasicButtonClickPage"
             Title="Basic Button Click">
    <StackLayout>

        <Label x:Name="label"
               Text="Click the Button below"
               FontSize="Large"
               VerticalOptions="CenterAndExpand"
               HorizontalOptions="Center" />

        <Button Text="Click to Rotate Text!"
                VerticalOptions="CenterAndExpand"
                HorizontalOptions="Center"
                Clicked="OnButtonClicked" />

    </StackLayout>
</ContentPage>
```

`Button`往往会占用为其允许的所有空间。 例如，如果未设置`HorizontalOptions`的属性`Button`到以外的其他`Fill`，则`Button`将占用其父项的整个宽度。

默认情况下`Button`都是矩形状，但可以通过使用进行舍入的 it 角[ `CornerRadius` ](xref:Xamarin.Forms.Button.CornerRadius)属性，如下所述的部分中[**按钮外观**](#button-appearance).

[ `Text` ](xref:Xamarin.Forms.Button.Text)属性指定在显示的文本`Button`。 [ `Clicked` ](xref:Xamarin.Forms.Button.Clicked)事件设置为事件处理程序名为`OnButtonClicked`。 在代码隐藏文件中，找到此处理程序**BasicButtonClickPage.xaml.cs**:

```csharp
public partial class BasicButtonClickPage : ContentPage
{
    public BasicButtonClickPage ()
    {
        InitializeComponent ();
    }

    async void OnButtonClicked(object sender, EventArgs args)
    {
        await label.RelRotateTo(360, 1000);
    }
}
```

当`Button`点击，`OnButtonClicked`方法执行。 `sender`自变量是`Button`负责此事件的对象。 可以使用此访问`Button`对象，或以区分多个`Button`对象共享相同`Clicked`事件。

此特定`Clicked`处理程序会调用旋转动画函数`Label`360 度，在 1000年毫秒。 下面是在 Windows 10 桌面版上运行 iOS 和 Android 设备，并作为通用 Windows 平台 (UWP) 应用程序的程序：

[![基本按钮 Click](button-images/BasicButtonClick.png "基本按钮 Click")](button-images/BasicButtonClick-Large.png#lightbox "基本按钮单击")

请注意，`OnButtonClicked`方法包括`async`修饰符因为`await`内的事件处理程序使用。 一个`Clicked`事件处理程序需要`async`修饰符仅当处理程序的主体使用`await`。

每个平台呈现`Button`以其自己特定的方式。 在中[**按钮外观**](#button-appearance)部分中，您将了解如何设置颜色并使`Button`边框可见的更多自定义外观。 `Button` 实现[ `IFontElement` ](xref:Xamarin.Forms.Internals.IFontElement)接口，因此，它包含[ `FontFamily` ](xref:Xamarin.Forms.Button.FontFamily)， [ `FontSize` ](xref:Xamarin.Forms.Button.FontSize)，并[ `FontAttributes` ](xref:Xamarin.Forms.Button.FontAttributes)属性。

## <a name="creating-a-button-in-code"></a>在代码中创建一个按钮

往往会实例化`Button`在 XAML 中，但也可以创建`Button`在代码中。 这可能会非常方便您的应用程序需要创建基于数据是使用可枚举的多个按钮时`foreach`循环。

**代码按钮单击**页将演示如何创建的页面功能上等效于**基本按钮单击**但完全在页C#:

```csharp
public class CodeButtonClickPage : ContentPage
{
    public CodeButtonClickPage ()
    {
        Title = "Code Button Click";

        Label label = new Label
        {
            Text = "Click the Button below",
            FontSize = Device.GetNamedSize(NamedSize.Large, typeof(Label)),
            VerticalOptions = LayoutOptions.CenterAndExpand,
            HorizontalOptions = LayoutOptions.Center
        };

        Button button = new Button
        {
            Text = "Click to Rotate Text!",
            VerticalOptions = LayoutOptions.CenterAndExpand,
            HorizontalOptions = LayoutOptions.Center
        };
        button.Clicked += async (sender, args) => await label.RelRotateTo(360, 1000);

        Content = new StackLayout
        {
            Children =
            {
                label,
                button
            }
        };
    }
}
```

类的构造函数中完成的所有内容。 因为`Clicked`处理程序只有一个语句很长，则可以将它附加到事件非常简单：

```csharp
button.Clicked += async (sender, args) => await label.RelRotateTo(360, 1000);
```

当然，您还可以作为一个单独的方法定义的事件处理程序 (就像`OnButtonClick`中的方法**基本按钮单击**) 并将该方法附加到该事件：

```csharp
button.Clicked += OnButtonClicked;
```

## <a name="disabling-the-button"></a>禁用按钮

有时应用程序处于特定状态的特定`Button`单击不是有效的操作。 在这些情况下，`Button`应禁用通过设置其`IsEnabled`属性设置为`false`。 典型的示例是`Entry`伴随文件打开的文件名的控件`Button`:`Button`键入一些文本，才应启用`Entry`。
可以使用`DataTrigger`对于此任务，如中所示[**数据触发器**](~/xamarin-forms/app-fundamentals/triggers.md#data-triggers)一文。

## <a name="using-the-command-interface"></a>使用命令界面

它是应用程序以响应`Button`分流点无需处理`Clicked`事件。 `Button`实现调用了替代通知机制_命令_或_命令_接口。 这包括两个属性：

- [`Command`](xref:Xamarin.Forms.Button.Command) 类型的[ `ICommand` ](xref:System.Windows.Input.ICommand)，在中定义的接口[ `System.Windows.Input` ](xref:System.Windows.Input)命名空间。
- [`CommandParameter`](xref:Xamarin.Forms.Button.CommandParameter) 类型的属性[ `Object` ](xref:System.Object)。

尤其是在实现模型-视图-视图模型 (MVVM) 体系结构时，这种方法是特别适合与数据绑定和。 在文章中讨论了这些主题[数据绑定](~/xamarin-forms/app-fundamentals/data-binding/index.md)，[从数据绑定到 MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)，并[MVVM](~/xamarin-forms/enterprise-application-patterns/mvvm.md)。

在 MVVM 应用程序，ViewModel 定义类型的属性`ICommand`，然后连接到 XAML`Button`具有数据绑定的元素。 此外定义了 Xamarin.Forms [ `Command` ]((xref:Xamarin.Forms.Command`1))并[ `Command<T>` ](xref:Xamarin.Forms.Command`1)类实现`ICommand`接口，并帮助 ViewModel 中定义的类型属性`ICommand`.

命令一文中的更详细地介绍[**命令界面**](~/xamarin-forms/app-fundamentals/data-binding/commanding.md)但**基本按钮命令**页中[ **ButtonDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ButtonDemos)示例显示了基本的方法。

`CommandDemoViewModel`类是非常简单的 ViewModel，用于定义类型的属性`double`名为`Number`，和类型的两个属性`ICommand`名为`MultiplyBy2Command`和`DivideBy2Command`:

```csharp
class CommandDemoViewModel : INotifyPropertyChanged
{
    double number = 1;

    public event PropertyChangedEventHandler PropertyChanged;

    public CommandDemoViewModel()
    {
        MultiplyBy2Command = new Command(() => Number *= 2);

        DivideBy2Command = new Command(() => Number /= 2);
    }

    public double Number
    {
        set
        {
            if (number != value)
            {
                number = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Number"));
            }
        }
        get
        {
            return number;
        }
    }

    public ICommand MultiplyBy2Command { private set; get; }

    public ICommand DivideBy2Command { private set; get; }
}
```

这两个`ICommand`属性的使用类型的两个对象的类的构造函数初始化`Command`。 `Command`构造函数包括一些函数 (称为`execute`构造函数参数) 的两倍，或减半`Number`属性。

**BasicButtonCommand.xaml**文件中设置其`BindingContext`的实例`CommandDemoViewModel`。 `Label`元素和第二个`Button`元素包含到中的三个属性的绑定`CommandDemoViewModel`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:ButtonDemos"
             x:Class="ButtonDemos.BasicButtonCommandPage"
             Title="Basic Button Command">

    <ContentPage.BindingContext>
        <local:CommandDemoViewModel />
    </ContentPage.BindingContext>

    <StackLayout>
        <Label Text="{Binding Number, StringFormat='Value is now {0}'}"
               FontSize="Large"
               VerticalOptions="CenterAndExpand"
               HorizontalOptions="Center" />

        <Button Text="Multiply by 2"
                VerticalOptions="CenterAndExpand"
                HorizontalOptions="Center"
                Command="{Binding MultiplyBy2Command}" />

        <Button Text="Divide by 2"
                VerticalOptions="CenterAndExpand"
                HorizontalOptions="Center"
                Command="{Binding DivideBy2Command}" />
    </StackLayout>
</ContentPage>
```

与两个`Button`点击元素、 执行命令，并更改值的数量：

[![基本按钮命令](button-images/BasicButtonCommand.png "基本按钮命令")](button-images/BasicButtonCommand-Large.png#lightbox)

通过此方法的优点`Clicked`处理程序，该文件，所有逻辑相关的此页的功能都位于在 ViewModel，而不是代码隐藏文件中，实现更好的隔离的用户界面与业务逻辑。

也可能是`Command`对象来控制启用和禁用`Button`元素。 例如，假设你想要限制 2 之间的数字值的范围<sup>10</sup>和 2<sup>&ndash;10</sup>。 可以将另一个函数添加到构造函数 (称为`canExecute`自变量)，它返回`true`如果`Button`应启用。 下面是对修改`CommandDemoViewModel`构造函数：

```csharp
class CommandDemoViewModel : INotifyPropertyChanged
{
    ···
    public CommandDemoViewModel()
    {
        MultiplyBy2Command = new Command(
            execute: () =>
            {
                Number *= 2;
                ((Command)MultiplyBy2Command).ChangeCanExecute();
                ((Command)DivideBy2Command).ChangeCanExecute();
            },
            canExecute: () => Number < Math.Pow(2, 10));

        DivideBy2Command = new Command(
            execute: () =>
            {
                Number /= 2;
                ((Command)MultiplyBy2Command).ChangeCanExecute();
                ((Command)DivideBy2Command).ChangeCanExecute();
            },
            canExecute: () => Number > Math.Pow(2, -10));
    }
    ···
}
```

对调用`ChangeCanExecute`方法`Command`所必需，以便`Command`方法可以调用`canExecute`方法，并确定是否`Button`或不应禁用。 此代码更改，随着数量达到限制，`Button`已禁用：

[![基本按钮命令-修改](button-images/BasicButtonCommandModified.png "基本按钮命令-修改")](button-images/BasicButtonCommandModified-Large.png#lightbox)

可以为两个或多个`Button`元素以绑定到同一`ICommand`属性。 `Button`可以使用可分辨元素[ `CommandParameter` ](xref:Xamarin.Forms.Button.CommandParameter)属性`Button`。 在这种情况下，你将想要使用泛型[ `Command<T>` ](xref:Xamarin.Forms.Command`1)类。 `CommandParameter`对象然后作为参数传递`execute`和`canExecute`方法。 中详细地演示了此技术[**基本命令**](~/xamarin-forms/app-fundamentals/data-binding/commanding.md#basic-commanding)一部分[**命令接口**](~/xamarin-forms/app-fundamentals/data-binding/commanding.md#basic-commanding)一文。

[ **ButtonDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ButtonDemos)示例还使用此方法在其`MainPage`类。 **MainPage.xaml**文件包含`Button`示例的每个页面：

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:ButtonDemos"
             x:Class="ButtonDemos.MainPage"
             Title="Button Demos">
    <ScrollView>
        <FlexLayout Direction="Column"
                    JustifyContent="SpaceEvenly"
                    AlignItems="Center">

            <Button Text="Basic Button Click"
                    Command="{Binding NavigateCommand}"
                    CommandParameter="{x:Type local:BasicButtonClickPage}" />

            <Button Text="Code Button Click"
                    Command="{Binding NavigateCommand}"
                    CommandParameter="{x:Type local:CodeButtonClickPage}" />

            <Button Text="Basic Button Command"
                    Command="{Binding NavigateCommand}"
                    CommandParameter="{x:Type local:BasicButtonCommandPage}" />

            <Button Text="Press and Release Button"
                    Command="{Binding NavigateCommand}"
                    CommandParameter="{x:Type local:PressAndReleaseButtonPage}" />

            <Button Text="Button Appearance"
                    Command="{Binding NavigateCommand}"
                    CommandParameter="{x:Type local:ButtonAppearancePage}" />

            <Button Text="Toggle Button Demo"
                    Command="{Binding NavigateCommand}"
                    CommandParameter="{x:Type local:ToggleButtonDemoPage}" />

            <Button Text="Image Button Demo"
                    Command="{Binding NavigateCommand}"
                    CommandParameter="{x:Type local:ImageButtonDemoPage}" />

        </FlexLayout>
    </ScrollView>
</ContentPage>
```

每个`Button`具有其`Command`属性绑定到一个名为属性`NavigateCommand`，和`CommandParameter`设置为[ `Type` ](xref:System.Type)对应于一个项目中的页类的对象。

是否`NavigateCommand`属性属于类型`ICommand`和代码隐藏文件中定义：

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();

        NavigateCommand = new Command<Type>(async (Type pageType) =>
        {
            Page page = (Page)Activator.CreateInstance(pageType);
            await Navigation.PushAsync(page);
        });

        BindingContext = this;
    }

    public ICommand NavigateCommand { private set; get; }
}
```

构造函数初始化`NavigateCommand`属性设置为`Command<Type>`对象，因为`Type`是一种`CommandParameter`XAML 文件中设置的对象。 这意味着`execute`方法具有一个类型的参数`Type`的对应于此`CommandParameter`对象。 此函数实例化页，然后转到它。

请注意，构造函数以结束通过设置其`BindingContext`到其自身。 这对于要绑定到 XAML 文件中的属性是必需`NavigateCommand`属性。

## <a name="pressing-and-releasing-the-button"></a>按下并松开按钮

除了`Clicked`事件，`Button`还定义了[ `Pressed` ](xref:Xamarin.Forms.Button.Pressed)并[ `Released` ](xref:Xamarin.Forms.Button.Released)事件。 `Pressed`手指按上时发生事件`Button`，或使用指针置于其上按下鼠标按钮`Button`。 `Released`松开手指或鼠标按钮时发生事件。 通常情况下，`Clicked`还在相同的时间触发事件`Released`事件，但如果手指或鼠标指针滑离开的面`Button`之前被释放，`Clicked`事件可能会发生。

`Pressed`并`Released`事件不常使用，但它们可用于特殊用途，如中所示**按下并松开按钮**页。 XAML 文件包含`Label`和一个`Button`使用处理程序将其附加`Pressed`和`Released`事件：

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ButtonDemos.PressAndReleaseButtonPage"
             Title="Press and Release Button">
    <StackLayout>

        <Label x:Name="label"
               Text="Press and hold the Button below"
               FontSize="Large"
               VerticalOptions="CenterAndExpand"
               HorizontalOptions="Center" />

        <Button Text="Press to Rotate Text!"
                VerticalOptions="CenterAndExpand"
                HorizontalOptions="Center"
                Pressed="OnButtonPressed"
                Released="OnButtonReleased" />

    </StackLayout>
</ContentPage>
```

代码隐藏文件之间进行动画处理`Label`时`Pressed`事件发生，但挂起旋转时`Released`发生事件：

```csharp
public partial class PressAndReleaseButtonPage : ContentPage
{
    bool animationInProgress = false;
    Stopwatch stopwatch = new Stopwatch();

    public PressAndReleaseButtonPage ()
    {
        InitializeComponent ();
    }

    void OnButtonPressed(object sender, EventArgs args)
    {
        stopwatch.Start();
        animationInProgress = true;

        Device.StartTimer(TimeSpan.FromMilliseconds(16), () =>
        {
            label.Rotation = 360 * (stopwatch.Elapsed.TotalSeconds % 1);

            return animationInProgress;
        });
    }

    void OnButtonReleased(object sender, EventArgs args)
    {
        animationInProgress = false;
        stopwatch.Stop();
    }
}
```

结果是，`Label`手指与联系时仅会旋转`Button`，并在手指松开时停止：

[![按下并释放按钮](button-images/PressAndReleaseButton.png "按下并释放按钮")](button-images/PressAndReleaseButton-Large.png)

游戏的应用有这种行为： 手指上持有`Button`可能会使特定方向移动一个在屏幕对象。

<a name="button-appearance" />

## <a name="button-appearance"></a>按钮外观

`Button`继承或定义会影响其外观的多个属性：

- [`TextColor`](xref:Xamarin.Forms.Button.TextColor) 是的颜色`Button`文本
- [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) 是对该文本背景的颜色
- [`BorderColor`](xref:Xamarin.Forms.Button.BorderColor) 是的周围区域的颜色 `Button`
- [`FontFamily`](xref:Xamarin.Forms.Button.FontFamily) 使用文本的字体系列
- [`FontSize`](xref:Xamarin.Forms.Button.FontSize) 是文本的大小
- [`FontAttributes`](xref:Xamarin.Forms.Button.FontAttributes) 指示文本是斜体或粗体
- [`BorderWidth`](xref:Xamarin.Forms.Button.BorderWidth) 为边框的宽度
- [`CornerRadius`](xref:Xamarin.Forms.Button.CornerRadius) 为圆角

> [!NOTE]
> `Button`类还具有[ `Margin` ](xref:Xamarin.Forms.View.Margin)并[ `Padding` ](xref:Xamarin.Forms.Button.Padding)控制的布局行为的属性`Button`。 有关详细信息，请参阅[边距和填充](~/xamarin-forms/user-interface/layouts/margin-and-padding.md)。

六篇文章构成这些属性的效果 (不包括`FontFamily`并`FontAttributes`) 中演示**按钮外观**页。 另一个属性， [ `Image` ](xref:Xamarin.Forms.Button.Image)，节中讨论[**位图使用按钮**](#image-button)。

中的视图和数据绑定的所有**按钮外观**XAML 文件中定义页面：

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:ButtonDemos"
             x:Class="ButtonDemos.ButtonAppearancePage"
             Title="Button Appearance">
    <StackLayout>
        <Button x:Name="button"
                Text="Button"
                VerticalOptions="CenterAndExpand"
                HorizontalOptions="Center"
                TextColor="{Binding Source={x:Reference textColorPicker},
                                    Path=SelectedItem.Color}"
                BackgroundColor="{Binding Source={x:Reference backgroundColorPicker},
                                          Path=SelectedItem.Color}"
                BorderColor="{Binding Source={x:Reference borderColorPicker},
                                      Path=SelectedItem.Color}" />

        <StackLayout BindingContext="{x:Reference button}"
                     Padding="10">

            <Slider x:Name="fontSizeSlider"
                    Maximum="48"
                    Minimum="1"
                    Value="{Binding FontSize}" />

            <Label Text="{Binding Source={x:Reference fontSizeSlider},
                                  Path=Value,
                                  StringFormat='FontSize = {0:F0}'}"
                   HorizontalTextAlignment="Center" />

            <Slider x:Name="borderWidthSlider"
                    Minimum="-1"
                    Maximum="12"
                    Value="{Binding BorderWidth}" />

            <Label Text="{Binding Source={x:Reference borderWidthSlider},
                                  Path=Value,
                                  StringFormat='BorderWidth = {0:F0}'}"
                   HorizontalTextAlignment="Center" />

            <Slider x:Name="cornerRadiusSlider"
                    Minimum="-1"
                    Maximum="24"
                    Value="{Binding CornerRadius}" />

            <Label Text="{Binding Source={x:Reference cornerRadiusSlider},
                                  Path=Value,
                                  StringFormat='CornerRadius = {0:F0}'}"
                   HorizontalTextAlignment="Center" />

            <Grid>
                <Grid.RowDefinitions>
                    <RowDefinition Height="Auto" />
                    <RowDefinition Height="Auto" />
                    <RowDefinition Height="Auto" />
                </Grid.RowDefinitions>

                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="*" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>

                <Grid.Resources>
                    <Style TargetType="Label">
                        <Setter Property="VerticalOptions" Value="Center" />
                    </Style>
                </Grid.Resources>

                <Label Text="Text Color:"
                       Grid.Row="0" Grid.Column="0" />

                <Picker x:Name="textColorPicker"
                        ItemsSource="{Binding Source={x:Static local:NamedColor.All}}"
                        ItemDisplayBinding="{Binding FriendlyName}"
                        SelectedIndex="0"
                        Grid.Row="0" Grid.Column="1" />

                <Label Text="Background Color:"
                       Grid.Row="1" Grid.Column="0" />

                <Picker x:Name="backgroundColorPicker"
                        ItemsSource="{Binding Source={x:Static local:NamedColor.All}}"
                        ItemDisplayBinding="{Binding FriendlyName}"
                        SelectedIndex="0"
                        Grid.Row="1" Grid.Column="1" />

                <Label Text="Border Color:"
                       Grid.Row="2" Grid.Column="0" />

                <Picker x:Name="borderColorPicker"
                        ItemsSource="{Binding Source={x:Static local:NamedColor.All}}"
                        ItemDisplayBinding="{Binding FriendlyName}"
                        SelectedIndex="0"
                        Grid.Row="2" Grid.Column="1" />
            </Grid>
        </StackLayout>
    </StackLayout>
</ContentPage>
```

`Button`在页面顶部有其三个`Color`属性绑定到`Picker`在页面底部的元素。 中的项`Picker`元素是从颜色`NamedColor`项目中包含的类。 三个`Slider`元素包含为双向绑定`FontSize`， `BorderWidth`，和`CornerRadius`的属性`Button`。

此程序可以尝试使用所有这些属性的组合：

[![按钮外观](button-images/ButtonAppearance.png "按钮外观")](button-images/ButtonAppearance-Large.png)

若要查看`Button`边框，您将需要设置`BorderColor`到以外的其他`Default`，和`BorderWidth`为正值。

在 iOS 上，您会注意到，大边框宽度起、 强行进入到的内部`Button`和干扰中文本的显示。 如果您选择要用于 iOS 的边框`Button`，可能需要开始和结束`Text`空格保留其可见性属性。

在 UWP 中，选择`CornerRadius`超过高度的一半`Button`引发异常。

## <a name="creating-a-toggle-button"></a>创建一个切换按钮

子类可以`Button`，使其工作原理类似打开-关闭开关： 点击按钮一次以上切换按钮，然后点击它再次切换它禁用。

以下`ToggleButton`类派生自`Button`，并定义名为的新事件`Toggled`和名为的布尔属性`IsToggled`。 这些是相同的两个属性定义的 Xamarin.Forms [ `Switch` ](xref:Xamarin.Forms.Switch):

```csharp
class ToggleButton : Button
{
    public event EventHandler<ToggledEventArgs> Toggled;

    public static BindableProperty IsToggledProperty =
        BindableProperty.Create("IsToggled", typeof(bool), typeof(ToggleButton), false,
                                propertyChanged: OnIsToggledChanged);

    public ToggleButton()
    {
        Clicked += (sender, args) => IsToggled ^= true;
    }

    public bool IsToggled
    {
        set { SetValue(IsToggledProperty, value); }
        get { return (bool)GetValue(IsToggledProperty); }
    }

    protected override void OnParentSet()
    {
        base.OnParentSet();
        VisualStateManager.GoToState(this, "ToggledOff");
    }

    static void OnIsToggledChanged(BindableObject bindable, object oldValue, object newValue)
    {
        ToggleButton toggleButton = (ToggleButton)bindable;
        bool isToggled = (bool)newValue;

        // Fire event
        toggleButton.Toggled?.Invoke(toggleButton, new ToggledEventArgs(isToggled));

        // Set the visual state
        VisualStateManager.GoToState(toggleButton, isToggled ? "ToggledOn" : "ToggledOff");
    }
}
```

`ToggleButton`构造函数附加到一个处理程序`Clicked`事件，以便它可以更改的值`IsToggled`属性。 `OnIsToggledChanged`方法将触发`Toggled`事件。

最后一行`OnIsToggledChanged`方法调用静态`VisualStateManager.GoToState`方法具有两个文本字符串"ToggledOn"和"ToggledOff"。 你可以阅读有关此方法以及如何在应用程序可以响应到文章中的可视状态[ **Xamarin.Forms 视觉状态管理器**](~/xamarin-forms/user-interface/visual-state-manager.md)。

因为`ToggleButton`调用`VisualStateManager.GoToState`，此类本身不需要包括任何其他工具来更改按钮的外观基于其`IsToggled`状态。 负责承载 XAML `ToggleButton`。

**切换按钮演示**页包含两个实例`ToggleButton`，其中包括设置的视觉状态管理器标记`Text`， `BackgroundColor`，和`TextColor`基于可视状态的按钮：

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:ButtonDemos"
             x:Class="ButtonDemos.ToggleButtonDemoPage"
             Title="Toggle Button Demo">

    <ContentPage.Resources>
        <Style TargetType="local:ToggleButton">
            <Setter Property="VerticalOptions" Value="CenterAndExpand" />
            <Setter Property="HorizontalOptions" Value="Center" />
        </Style>
    </ContentPage.Resources>

    <StackLayout Padding="10, 0">
        <local:ToggleButton Toggled="OnItalicButtonToggled">
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup Name="ToggleStates">
                    <VisualState Name="ToggledOff">
                        <VisualState.Setters>
                            <Setter Property="Text" Value="Italic Off" />
                            <Setter Property="BackgroundColor" Value="#C0C0C0" />
                            <Setter Property="TextColor" Value="Black" />
                        </VisualState.Setters>
                    </VisualState>

                    <VisualState Name="ToggledOn">
                        <VisualState.Setters>
                            <Setter Property="Text" Value=" Italic On " />
                            <Setter Property="BackgroundColor" Value="#404040" />
                            <Setter Property="TextColor" Value="White" />
                        </VisualState.Setters>
                    </VisualState>
                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>
        </local:ToggleButton>

        <local:ToggleButton Toggled="OnBoldButtonToggled">
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup Name="ToggleStates">
                    <VisualState Name="ToggledOff">
                        <VisualState.Setters>
                            <Setter Property="Text" Value="Bold Off" />
                            <Setter Property="BackgroundColor" Value="#C0C0C0" />
                            <Setter Property="TextColor" Value="Black" />
                        </VisualState.Setters>
                    </VisualState>

                    <VisualState Name="ToggledOn">
                        <VisualState.Setters>
                            <Setter Property="Text" Value=" Bold On " />
                            <Setter Property="BackgroundColor" Value="#404040" />
                            <Setter Property="TextColor" Value="White" />
                        </VisualState.Setters>
                    </VisualState>
                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>
        </local:ToggleButton>

        <Label x:Name="label"
               Text="Just a little passage of some sample text that can be formatted in italic or boldface by toggling the two buttons."
               FontSize="Large"
               HorizontalTextAlignment="Center"
               VerticalOptions="CenterAndExpand" />

    </StackLayout>
</ContentPage>
```

`Toggled`事件处理程序是在代码隐藏文件中。 他们负责设置`FontAttributes`属性的`Label`根据按钮的状态：

```csharp
public partial class ToggleButtonDemoPage : ContentPage
{
    public ToggleButtonDemoPage ()
    {
        InitializeComponent ();
    }

    void OnItalicButtonToggled(object sender, ToggledEventArgs args)
    {
        if (args.Value)
        {
            label.FontAttributes |= FontAttributes.Italic;
        }
        else
        {
            label.FontAttributes &= ~FontAttributes.Italic;
        }
    }

    void OnBoldButtonToggled(object sender, ToggledEventArgs args)
    {
        if (args.Value)
        {
            label.FontAttributes |= FontAttributes.Bold;
        }
        else
        {
            label.FontAttributes &= ~FontAttributes.Bold;
        }
    }
}
```

下面是在 iOS、 Android 和 UWP 上运行的程序：

[![切换按钮演示](button-images/ToggleButtonDemo.png "切换按钮演示")](button-images/ToggleButtonDemo-Large.png#lightbox)

<a name="image-button" />

## <a name="using-bitmaps-with-buttons"></a>使用位图按钮

`Button`类定义[ `Image` ](xref:Xamarin.Forms.Button.Image)属性，允许用户上显示的位图图像`Button`，单独使用或与文本结合使用。 此外可以指定的文本和图像的排列方式。

`Image`属性属于类型[ `FileImageSource` ](xref:Xamarin.Forms.FileImageSource)，这意味着位图，必须存储为资源中单个平台项目和.NET Standard 库项目中不存在。

Xamarin.Forms 支持每个平台允许图像存储在不同的像素的各种设备上运行应用程序可能的解决方法的多个大小。 这些是名为多个位图或将其存储在为设备的视频，操作系统可以选取最佳匹配项的方式显示分辨率。

对于在位图`Button`，最佳大小通常是 32 位和 64 个与设备无关单位之间，具体取决于如何大型希望其成为。 在此示例中使用的映像为基础的大小为 48 个与设备无关单位。

在 iOS 项目中，**资源**文件夹包含三种大小的此映像：

- 存储为一个 48 像素的方形位图 **/Resources/MonkeyFace.png**
- 存储为 96 像素正方形位图 **/Resource/MonkeyFace@2x.png**
- 存储为一个 144 像素的方形位图 **/Resource/MonkeyFace@3x.png**

提供给所有三个位图**生成操作**的**BundleResource**。

对于 Android 项目中，所有的位图具有相同的名称，但是它们的不同子文件夹中存储**资源**文件夹：

- 存储为 72 像素正方形位图 **/Resources/drawable-hdpi/MonkeyFace.png**
- 存储为 96 像素正方形位图 **/Resources/drawable-xhdpi/MonkeyFace.png**
- 存储为一个 144 像素的方形位图 **/Resources/drawable-xxhdpi/MonkeyFace.png**
- 存储为 192 像素正方形位图 **/Resources/drawable-xxxhdpi/MonkeyFace.png**

这些已为其授予**生成操作**的**AndroidResource**。

在 UWP 项目中，位图可以存储任意位置在项目中，但它们通常存储在自定义文件夹中或**资产**现有文件夹。 UWP 项目包含这些位图：

- 存储为一个 48 像素的方形位图 **/Assets/MonkeyFace.scale-100.png**
- 存储为 96 像素正方形位图 **/Assets/MonkeyFace.scale-200.png**
- 存储为 192 像素正方形位图 **/Assets/MonkeyFace.scale-400.png**

他们所有给定**生成操作**的**内容**。

您可以指定如何`Text`并`Image`属性上排列`Button`使用[ `ContentLayout` ](xref:Xamarin.Forms.Button.ContentLayout)属性`Button`。 此属性属于类型[ `ButtonContentLayout` ](xref:Xamarin.Forms.Button.ButtonContentLayout)，这是在嵌入的类`Button`。 [构造函数](xref:Xamarin.Forms.Button.ButtonContentLayout.%23ctor(Xamarin.Forms.Button.ButtonContentLayout.ImagePosition,System.Double))有两个参数：

- 成员[ `ImagePosition` ](xref:Xamarin.Forms.Button.ButtonContentLayout.ImagePosition)枚举： `Left`， `Top`， `Right`，或`Bottom`，该值指示位图相对于文本的显示方式。
- 一个`double`位图和文本之间的间距的值。

默认值为`Left`和 10 个单位。 两个只读属性`ButtonContentLayout`名为[ `Position` ](xref:Xamarin.Forms.Button.ButtonContentLayout.Position)并[ `Spacing` ](xref:Xamarin.Forms.Button.ButtonContentLayout.Spacing)提供这些属性的值。

在代码中，可以创建`Button`并设置`ContentLayout`如下属性：

```csharp
Button button = new Button
{
    Text = "button text",
    Image = new FileImageSource
    {
        File = "image filename"
    },
    ContentLayout = new Button.ButtonContentLayout(Button.ButtonContentLayout.ImagePosition.Right, 20)
};
```

在 XAML，您需要指定仅枚举成员或间距，或同时按任意顺序以逗号分隔：

```xaml
<Button Text="button text"
        Image="image filename"
        ContentLayout="Right, 20" />
```

**图像按钮演示**页上使用`OnPlatform`若要指定不同的文件名在 iOS、 Android 和 UWP 位图文件。 如果你想要用于所有三个平台使用相同的文件名和避免使用`OnPlatform`，都需要在项目的根目录中存储的 UWP 位图。

第一个`Button`上**图像按钮演示**页上设置`Image`属性但不是`Text`属性：

```xaml
<Button>
    <Button.Image>
        <OnPlatform x:TypeArguments="FileImageSource">
            <On Platform="iOS, Android" Value="MonkeyFace.png" />
            <On Platform="UWP" Value="Assets/MonkeyFace.png" />
        </OnPlatform>
    </Button.Image>
</Button>
```

如果该项目的根目录中存储的 UWP 位图，此标记可以大大简化了：

```xaml
<Button Image="MonkeyFace.png" />
```

若要避免重复标记中的大量**ImageButtonDemo.xaml**文件，隐式`Style`还定义以设置`Image`属性。 这`Style`自动应用于其他五个`Button`元素。 下面是完整的 XAML 文件：

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ButtonDemos.ImageButtonDemoPage">

    <FlexLayout Direction="Column"
                JustifyContent="SpaceEvenly"
                AlignItems="Center">

        <FlexLayout.Resources>
            <Style TargetType="Button">
                <Setter Property="Image">
                    <OnPlatform x:TypeArguments="FileImageSource">
                        <On Platform="iOS, Android" Value="MonkeyFace.png" />
                        <On Platform="UWP" Value="Assets/MonkeyFace.png" />
                    </OnPlatform>
                </Setter>
            </Style>
        </FlexLayout.Resources>

        <Button>
            <Button.Image>
                <OnPlatform x:TypeArguments="FileImageSource">
                    <On Platform="iOS, Android" Value="MonkeyFace.png" />
                    <On Platform="UWP" Value="Assets/MonkeyFace.png" />
                </OnPlatform>
            </Button.Image>
        </Button>

        <Button Text="Default" />

        <Button Text="Left - 10"
                ContentLayout="Left, 10" />

        <Button Text="Top - 10"
                ContentLayout="Top, 10" />

        <Button Text="Right - 20"
                ContentLayout="Right, 20" />

        <Button Text="Bottom - 20"
                ContentLayout="Bottom, 20" />
    </FlexLayout>
</ContentPage>
```

最后一个四`Button`元素使使用`ContentLayout`属性指定的位置和间距的文本和位图：

[![图像按钮演示](button-images/ImageButtonDemo.png "图像按钮演示")](button-images/ImageButtonDemo-Large.png#lightbox)

现在，已了解你可以处理的各种方法`Button`事件和更改`Button`外观。

## <a name="related-links"></a>相关链接

- [ButtonDemos 示例](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ButtonDemos)
- [按钮 API](xref:Xamarin.Forms.Button)
