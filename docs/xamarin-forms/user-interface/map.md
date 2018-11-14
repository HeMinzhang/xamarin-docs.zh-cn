---
title: Xamarin.Forms 映射
description: 本文介绍如何使用 Xamarin.Forms Map 类如何使用每个平台上本机地图 Api 提供熟悉的映射的用户体验。
ms.prod: xamarin
ms.assetid: 59CD1344-8248-406C-9144-0C8A67141E5B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/27/2016
ms.openlocfilehash: cfa450c977ac9f1d6370f40d27f5d704cc774767
ms.sourcegitcommit: 7eed80186e23e6aff3ddbbf7ce5cd1fa20af1365
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/11/2018
ms.locfileid: "51526671"
---
# <a name="xamarinforms-map"></a>Xamarin.Forms 映射

_Xamarin.Forms 每个平台上使用本机映射 Api。_

Xamarin.Forms.Maps 每个平台上使用本机映射 Api。 这对于用户，提供快速、 熟悉地图体验，但意味着一些配置步骤所需遵守每个平台特定 API 要求。
配置完成后，`Map`控制的工作方式就像在常见的代码中的任何其他 Xamarin.Forms 元素。

* [映射初始化](#Maps_Initialization)-使用`Map`需要在启动时附加的初始化代码。
* [平台配置](#Platform_Configuration)-每个平台所需映射来处理一些配置。
* [在 C# 中使用映射](#Using_Maps)-显示映射和固定使用 C#。
* [在 XAML 中使用映射](#Using_Xaml)-显示具有 XAML 的映射。

已在使用地图控件[MapsSample](https://developer.xamarin.com/samples/WorkingWithMaps/)示例，如下所示。

 [![MobileCRM 示例中的地图](map-images/maps-zoom-sml.png "地图控件示例")](map-images/maps-zoom.png#lightbox "地图控件示例")

映射功能可以通过创建进一步增强[映射自定义呈现器](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md)。

<a name="Maps_Initialization" />

## <a name="maps-initialization"></a>映射初始化

将地图添加到 Xamarin.Forms 应用程序时**Xamarin.Forms.Maps**是一个单独的 NuGet 包，应添加到解决方案中的每个项目。
在 Android 上，这也存在依赖关系添加 Xamarin.Forms.Maps 时，会自动下载的 GooglePlayServices (另一个 NuGet)。

安装 NuGet 包后, 一些初始化代码需要在每个应用程序项目中，*后*`Xamarin.Forms.Forms.Init`方法调用。 对于 iOS 使用以下代码：

```csharp
Xamarin.FormsMaps.Init();
```

必须将相同的参数传递在 Android 上`Forms.Init`:

```csharp
Xamarin.FormsMaps.Init(this, bundle);
```

有关通用 Windows 平台 (UWP) 中使用以下代码：

```csharp
Xamarin.FormsMaps.Init("INSERT_AUTHENTICATION_TOKEN_HERE");
```

为每个平台在以下文件中添加此调用：

-  **iOS** -AppDelegate.cs 文件中，在`FinishedLaunching`方法。
-  **Android** -MainActivity.cs 文件`OnCreate`方法。
-  **UWP** -MainPage.xaml.cs 文件中，在`MainPage`构造函数。

已添加 NuGet 包并在每个应用程序内调用初始化方法后`Xamarin.Forms.Maps`Api 可以使用共享项目代码的公共.NET Standard 库项目中。

<a name="Platform_Configuration" />

## <a name="platform-configuration"></a>平台配置

之前该映射将显示在某些平台上需要其他配置步骤。

### <a name="ios"></a>iOS

若要访问在 iOS 上的位置服务，必须设置以下项**Info.plist**:

- iOS 11
    - [`NSLocationWhenInUseUsageDescription`](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26) – 使用位置服务，当应用程序正在使用中
    - [`NSLocationAlwaysAndWhenInUseUsageDescription`](https://developer.apple.com/documentation/corelocation/choosing_the_authorization_level_for_location_services/requesting_always_authorization?language=objc) – 在任何时候使用位置服务
- iOS 10 及更早版本
    - [`NSLocationWhenInUseUsageDescription`](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26) – 使用位置服务，当应用程序正在使用中
    - [`NSLocationAlwaysUsageDescription`](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW18) – 在任何时候使用位置服务    

若要支持 iOS 11 和更早版本，可以包括所有三个密钥： `NSLocationWhenInUseUsageDescription`， `NSLocationAlwaysAndWhenInUseUsageDescription`，和`NSLocationAlwaysUsageDescription`。

中的这些项的 XML 表示形式**Info.plist**如下所示。 应更新`string`值以反映你的应用程序如何使用位置信息：

```xml
<key>NSLocationAlwaysUsageDescription</key>
<string>Can we use your location at all times?</string>
<key>NSLocationWhenInUseUsageDescription</key>
<string>Can we use your location when your app is being used?</string>
<key>NSLocationAlwaysAndWhenInUseUsageDescription</key>
<string>Can we use your location at all times?</string>
```

**Info.plist**还可以在中添加条目**源**在其他视图**Info.plist**文件：

![适用于 iOS 8 Info.plist](map-images/ios8-map-permissions.png "iOS 8 需要 Info.plist 条目")


### <a name="android"></a>Android

若要使用[Google 地图 API v2](https://developers.google.com/maps/documentation/android/)必须在 Android 上生成 API 密钥并将其添加到你的 Android 项目。
Xamarin 文档中的说明进行操作并遵照[获取 Google Maps API v2 密钥](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md)。
按照这些说明进行操作之后, 将粘贴中的 API 密钥**properties/Androidmanifest.xml**文件 （查看源和查找/更新的以下元素）：

```xml
<application ...>
    <meta-data android:name="com.google.android.maps.v2.API_KEY" android:value="YOUR_API_KEY" />
</application>
```

如果没有有效的 API 密钥地图控件将显示为灰色框在 Android 上。

> [!NOTE]
> 请注意，为了使 APK 来访问 Google 地图，您必须包括 sha-1 指纹，包使用对 APK 进行签名每个密钥存储 （调试和发布） 的名称。 例如，如果一台计算机用于调试和生成发布 APK 的另一台计算机，您应包括 sha-1 证书指纹从第一台计算机的调试密钥存储和从的发布密钥存储的 sha-1 证书指纹第二台计算机。 此外，切记要编辑的密钥凭据，如果应用程序的**包名称**更改。 请参阅[获取 Google Maps API v2 密钥](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md)。

您还需要通过右键单击 Android 项目，然后选择启用适当的权限**选项 > 生成 > Android 应用程序**和勾选以下：

* `AccessCoarseLocation`
* `AccessFineLocation`
* `AccessLocationExtraCommands`
* `AccessMockLocation`
* `AccessNetworkState`
* `AccessWifiState`
* `Internet`

下面的屏幕截图显示了其中一些：

![针对 Android 所需权限](map-images/android-map-permissions.png "适用于 Android 的所需的权限")

需要最后两个，因为应用程序需要网络连接才能下载地图数据。 了解 Android[权限](http://developer.android.com/reference/android/Manifest.permission.html)若要了解详细信息。

### <a name="universal-windows-platform"></a>通用 Windows 平台

若要在通用 Windows 平台上使用映射必须生成授权令牌。 有关详细信息，请参阅[请求一个地图身份验证密钥](https://msdn.microsoft.com/library/windows/apps/mt219694.aspx)MSDN 上。

然后应在中指定的身份验证令牌`FormsMaps.Init("AUTHORIZATION_TOKEN")`方法调用中，与必应地图应用进行身份验证。

<a name="Using_Maps" />

## <a name="using-maps"></a>使用 Maps

请参阅[MapPage.cs](https://github.com/xamarin/xamarin-forms-samples/blob/master/MobileCRM/MobileCRM.Shared/Pages/MapPage.cs) MobileCRM 示例以举例说明如何在代码中使用地图控件中。 一个简单`MapPage`类看起来像此-请注意，新`MapSpan`创建来定位地图的视图：

```csharp
public class MapPage : ContentPage {
    public MapPage() {
        var map = new Map(
            MapSpan.FromCenterAndRadius(
                    new Position(37,-122), Distance.FromMiles(0.3))) {
                IsShowingUser = true,
                HeightRequest = 100,
                WidthRequest = 960,
                VerticalOptions = LayoutOptions.FillAndExpand
            };
        var stack = new StackLayout { Spacing = 0 };
        stack.Children.Add(map);
        Content = stack;
    }
}
```

### <a name="map-type"></a>映射类型

此外可以通过设置更改地图内容`MapType`属性，以显示正则街道地图 （默认值）、 卫星照片或这两者的组合。

```csharp
map.MapType == MapType.Street;
```

有效`MapType`的值为：

-  混合
-  附属
-  街道 （默认值）


### <a name="map-region-and-mapspan"></a>地图区域和 MapSpan

上面的代码段中所示，提供`MapSpan`map 构造函数的实例设置的初始视图 （中心点和缩放级别） 的映射在加载时。 `MoveToRegion`然后使用 map 类上的方法来更改映射的位置或缩放级别。 有两种方法来创建一个新`MapSpan`实例：

-  **MapSpan.FromCenterAndRadius()** 的静态方法，用于创建从 span`Position`并指定`Distance`。
-  **新 MapSpan （)** 的构造函数使用`Position`和程度的纬度和经度来显示。


若要更改地图的缩放级别而无需更改位置，请创建一个新`MapSpan`使用从当前位置`VisibleRegion.Center`地图控件的属性。 一个`Slider`无法用于控制此类地图缩放 （但是，缩放直接在地图控件中当前不能更新滑块的值）：

```csharp
var slider = new Slider (1, 18, 1);
slider.ValueChanged += (sender, e) => {
    var zoomLevel = e.NewValue; // between 1 and 18
    var latlongdegrees = 360 / (Math.Pow(2, zoomLevel));
    map.MoveToRegion(new MapSpan (map.VisibleRegion.Center, latlongdegrees, latlongdegrees));
};
```

 [![与 zoom 的地图](map-images/maps-zoom-sml.png "地图控件缩放")](map-images/maps-zoom.png#lightbox "地图控件缩放")

### <a name="map-pins"></a>映射的 Pin

可以在代码图上标记位置`Pin`对象。

```csharp
var position = new Position(37,-122); // Latitude, Longitude
var pin = new Pin {
            Type = PinType.Place,
            Position = position,
            Label = "custom pin",
            Address = "custom detail info"
        };
map.Pins.Add(pin);
```

 `PinType` 可以设置为以下值，可能会影响在 pin 呈现 （具体取决于平台） 的方法之一：

-  泛型
-  位置
-  SavedPin
-  SearchResult


<a name="Using_Xaml" />

## <a name="using-xaml"></a>使用 Xaml

此外可进行地图定位 Xaml 布局中，此代码片段中所示。

```xaml
<?xml version="1.0" encoding="UTF-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps"
    x:Class="MapDemo.MapPage">
    <StackLayout VerticalOptions="StartAndExpand" Padding="30">
        <maps:Map WidthRequest="320" HeightRequest="200"
            x:Name="MyMap"
            IsShowingUser="true"
            MapType="Hybrid"
        />
    </StackLayout>
</ContentPage>
```

`MapRegion`并`Pins`可以在代码中使用设置`MyMap`引用 （或任何命名映射）。 请注意，额外`xmlns`命名空间定义用于 Xamarin.Forms.Maps 控件的引用。

```csharp
MyMap.MoveToRegion(
    MapSpan.FromCenterAndRadius(
        new Position(37,-122), Distance.FromMiles(1)));
```

<a name="Summary" />

## <a name="summary"></a>总结

Xamarin.Forms.Maps 是必须添加到 Xamarin.Forms 解决方案中的每个项目的单独 NuGet。 附加的初始化代码是必需的为 iOS、 Android 和 UWP 和某些配置步骤。

一次配置映射 API 可用于呈现使用 pin 标记，只需几行代码中的映射。 映射可以使用进一步增强[自定义呈现器](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md)。


## <a name="related-links"></a>相关链接

- [MapsSample](https://developer.xamarin.com/samples/WorkingWithMaps/)
- [映射自定义呈现器](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md)
- [Xamarin.Forms 示例](https://developer.xamarin.com/samples/xamarin-forms/all/)
