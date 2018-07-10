---
title: Xamarin.Forms 轮播页面
description: Xamarin.Forms CarouselPage 是内容的一个页面，用户可以向从左到右轻扫来导航页面，例如库。 本文演示如何使用 CarouselPage 导航页的集合。
ms.prod: xamarin
ms.assetid: 2D14FC9D-DF5F-427E-9006-2AAE61ECF8DC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: b190498911867d29b63d839f56613fb1b80fe56f
ms.sourcegitcommit: 3e980fbf92c69c3dd737554e8c6d5b94cf69ee3a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2018
ms.locfileid: "37935142"
---
# <a name="xamarinforms-carousel-page"></a>Xamarin.Forms 轮播页面

_Xamarin.Forms CarouselPage 是内容的一个页面，用户可以向从左到右轻扫来导航页面，例如库。本文演示如何使用 CarouselPage 导航页的集合。_

## <a name="overview"></a>概述

下面的屏幕截图演示[ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/)每个平台上：

![](carousel-page-images/thirdpage.png "CarouselPage Thid 项")

布局[ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/)是在每个平台上完全相同。 导航页，可以通过轻扫右到左遍历该集合，向前导航和轻扫从左到右向后导航整个集合。 以下屏幕截图显示在首页[ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/)实例：

![](carousel-page-images/firstpage.png "CarouselPage 第一个项")

轻扫从右到左移到第二个页面，如以下屏幕截图中所示：

![](carousel-page-images/secondpage.png "CarouselPage 第二个项")

再次从右到左轻扫会移到第三个页上，而从左到右轻扫返回到以前的页面。

<!--
> [!NOTE]
> The [`CarouselPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) has been deprecated, and will be removed from Xamarin.Forms in a future release. Instead, the [`CarouselView`](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselView/) should be used to provide a gallery-like view, where users can swipe from side to side to move through a collection of items.
-->

## <a name="creating-a-carouselpage"></a>创建 CarouselPage

两种方法可用于创建[ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/):

- [填充](#Populating_a_CarouselPage_with_a_Page_Collection)`CarouselPage`了一系列子[ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)实例。
- [将分配](#Populating_a_CarouselPage_with_a_Template)收藏[ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%601.ItemsSource/)属性和分配[ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/)到[ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%601.ItemTemplate/)属性以返回[`ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)集合中对象的实例。

这两种方法，`CarouselPage`将然后显示每个页面又，移动到下一步页要显示的重击交互。

> [!NOTE]
> 一个[ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/)仅可以使用填充[ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)实例，或`ContentPage`派生类。

<a name="Populating_a_CarouselPage_with_a_Page_Collection" />

### <a name="populating-a-carouselpage-with-a-page-collection"></a>填充页集合与 CarouselPage

以下 XAML 代码示例所示[ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) ，它显示三个[ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)实例：

```xaml
<CarouselPage xmlns="http://xamarin.com/schemas/2014/forms"
              xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
              x:Class="CarouselPageNavigation.MainPage">
    <ContentPage>
        <ContentPage.Padding>
          <OnPlatform x:TypeArguments="Thickness">
              <On Platform="iOS, Android" Value="0,40,0,0" />
          </OnPlatform>
        </ContentPage.Padding>
        <StackLayout>
            <Label Text="Red" FontSize="Medium" HorizontalOptions="Center" />
            <BoxView Color="Red" WidthRequest="200" HeightRequest="200" HorizontalOptions="Center" VerticalOptions="CenterAndExpand" />
        </StackLayout>
    </ContentPage>
    <ContentPage>
        ...
    </ContentPage>
    <ContentPage>
        ...
    </ContentPage>
</CarouselPage>
```

下面的代码示例显示了 C# 中的等效 UI:

```csharp
public class MainPageCS : CarouselPage
{
    public MainPageCS ()
    {
        Thickness padding;
        switch (Device.RuntimePlatform)
        {
            case Device.iOS:
            case Device.Android:
                padding = new Thickness(0, 40, 0, 0);
                break;
            default:
                padding = new Thickness();
                break;
        }

        var redContentPage = new ContentPage {
            Padding = padding,
            Content = new StackLayout {
                Children = {
                    new Label {
                        Text = "Red",
                        FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
                        HorizontalOptions = LayoutOptions.Center
                    },
                    new BoxView {
                        Color = Color.Red,
                        WidthRequest = 200,
                        HeightRequest = 200,
                        HorizontalOptions = LayoutOptions.Center,
                        VerticalOptions = LayoutOptions.CenterAndExpand
                    }
                }
            }
        };
        var greenContentPage = new ContentPage {
            Padding = padding,
            Content = new StackLayout {
                ...
            }
        };
        var blueContentPage = new ContentPage {
            Padding = padding,
            Content = new StackLayout {
                ...
            }
        };

        Children.Add (redContentPage);
        Children.Add (greenContentPage);
        Children.Add (blueContentPage);
    }
}
```

每个[ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)只是显示[ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)特定颜色和一个[ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/)该颜色。

> [!NOTE]
> [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/)不支持 UI 虚拟化。 因此，性能可能会影响如果`CarouselPage`包含太多的子元素。

如果[ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/)嵌入到[ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/)页[ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/)，则[ `MasterDetailPage.IsGestureEnabled` ](xref:Xamarin.Forms.MasterDetailPage.IsGestureEnabledProperty)属性应设置为`false`以防止手势之间的冲突`CarouselPage`和`MasterDetailPage`。

有关详细信息[ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/)，请参阅[第 25 章](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf)的 Charles Petzold 的 Xamarin.Forms 书籍。

<a name="Populating_a_CarouselPage_with_a_Template" />

### <a name="populating-a-carouselpage-with-a-template"></a>填充 CarouselPage 使用模板

以下 XAML 代码示例所示[ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/)构建的分配[ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/)到[ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%601.ItemTemplate/)属性返回的页集合中的对象：

```xaml
<CarouselPage xmlns="http://xamarin.com/schemas/2014/forms"
              xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
              x:Class="CarouselPageNavigation.MainPage">
    <CarouselPage.ItemTemplate>
        <DataTemplate>
            <ContentPage>
                <ContentPage.Padding>
                  <OnPlatform x:TypeArguments="Thickness">
                    <On Platform="iOS, Android" Value="0,40,0,0" />
                  </OnPlatform>
                </ContentPage.Padding>
                <StackLayout>
                    <Label Text="{Binding Name}" FontSize="Medium" HorizontalOptions="Center" />
                    <BoxView Color="{Binding Color}" WidthRequest="200" HeightRequest="200" HorizontalOptions="Center" VerticalOptions="CenterAndExpand" />
                </StackLayout>
            </ContentPage>
        </DataTemplate>
    </CarouselPage.ItemTemplate>
</CarouselPage>
```

[ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/)通过设置使用数据填充[ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%601.ItemsSource/)代码隐藏文件的构造函数中的属性：

```csharp
public MainPage ()
{
    ...
    ItemsSource = ColorsDataModel.All;
}
```

下面的代码示例显示等效[ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) C# 创建的：

```csharp
public class MainPageCS : CarouselPage
{
    public MainPageCS ()
    {
        Thickness padding;
        switch (Device.RuntimePlatform)
        {
            case Device.iOS:
            case Device.Android:
                padding = new Thickness(0, 40, 0, 0);
                break;
            default:
                padding = new Thickness();
                break;
        }

        ItemTemplate = new DataTemplate (() => {
            var nameLabel = new Label {
                FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
                HorizontalOptions = LayoutOptions.Center
            };
            nameLabel.SetBinding (Label.TextProperty, "Name");

            var colorBoxView = new BoxView {
                WidthRequest = 200,
                HeightRequest = 200,
                HorizontalOptions = LayoutOptions.Center,
                VerticalOptions = LayoutOptions.CenterAndExpand
            };
            colorBoxView.SetBinding (BoxView.ColorProperty, "Color");

            return new ContentPage {
                Padding = padding,
                Content = new StackLayout {
                    Children = {
                        nameLabel,
                        colorBoxView
                    }
                }
            };
        });

        ItemsSource = ColorsDataModel.All;
    }
}
```

每个[ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)只是显示[ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)特定颜色和一个[ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/)该颜色。

> [!NOTE]
> [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/)不支持 UI 虚拟化。 因此，性能可能会影响如果`CarouselPage`包含太多的子元素。

如果[ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/)嵌入到[ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/)页[ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/)，则[ `MasterDetailPage.IsGestureEnabled` ](xref:Xamarin.Forms.MasterDetailPage.IsGestureEnabledProperty)属性应设置为`false`以防止手势之间的冲突`CarouselPage`和`MasterDetailPage`。

有关详细信息[ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/)，请参阅[第 25 章](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf)的 Charles Petzold 的 Xamarin.Forms 书籍。

## <a name="summary"></a>总结

本文演示了如何使用[ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/)来导航页面的集合。 `CarouselPage`是一个页面，用户可以向从左到右轻扫来导航页面的内容，类似于库。


## <a name="related-links"></a>相关链接

- [页类型](~/xamarin-forms/user-interface/controls/pages.md)
- [CarouselPage （示例）](https://developer.xamarin.com/samples/xamarin-forms/Navigation/CarouselPage/)
- [CarouselPageTemplate （示例）](https://developer.xamarin.com/samples/xamarin-forms/Navigation/CarouselPageTemplate/)
- [CarouselPage](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/)
