---
title: 突出显示地图上的区域
description: 本文介绍如何向地图添加多边形叠加层，以突出显示地图上的某一区域。 多边形是内部空间已填充的封闭形状。
ms.prod: xamarin
ms.assetid: E79EB2CF-8DD6-44A8-B47D-5F0A94FB0A63
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 45a85c8ac2f40e92078d46f5765aaf2af1518b77
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/30/2019
ms.locfileid: "68650742"
---
# <a name="highlighting-a-region-on-a-map"></a>突出显示地图上的区域

[![下载示例](~/media/shared/download.png) 下载示例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-map-polygon)

本文介绍如何向地图添加多边形叠加层，以突出显示地图上的某一区域。多边形是内部空间已填充的封闭形状。_

## <a name="overview"></a>概述

叠加层是地图上的分层图形。 叠加层支持绘制在地图缩放时随之缩放的图形内容。 以下屏幕截图显示了向地图添加多边形叠加层的结果：

![](polygon-map-overlay-images/screenshots.png)

当 Xamarin.Forms 应用程序呈现 [`Map`](xref:Xamarin.Forms.Maps.Map) 控件时，将在 iOS 中实例化 `MapRenderer` 类，而该操作又会实例化本机 `MKMapView` 控件。 在 Android 平台上，`MapRenderer` 类实例化本机 `MapView` 控件。 在通用 Windows 平台 (UWP) 上，`MapRenderer` 类实例化本机 `MapControl`。 通过在每个平台上为 `Map` 创建自定义呈现器，可以利用呈现过程来实现特定于平台的地图自定义。 执行此操作的过程如下：

1. [创建](#Creating_the_Custom_Map) Xamarin.Forms 自定义地图。
1. [使用](#Consuming_the_Custom_Map) Xamarin.Forms 中的自定义地图。
1. 通过在每个平台上为地图创建自定义呈现器来[自定义](#Customizing_the_Map)地图。

> [!NOTE]
> 必须在使用前对 [`Xamarin.Forms.Maps`](xref:Xamarin.Forms.Maps) 进行初始化和配置。 有关详细信息，请参阅 [`Maps Control`](~/xamarin-forms/user-interface/map.md)。

有关使用自定义呈现器自定义地图的信息，请参阅[自定义图钉](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md)。

<a name="Creating_the_Custom_Map" />

### <a name="creating-the-custom-map"></a>创建自定义地图

创建 [`Map`](xref:Xamarin.Forms.Maps.Map) 类的子类，该子类添加 `ShapeCoordinates` 属性：

```csharp
public class CustomMap : Map
{
  public List<Position> ShapeCoordinates { get; set; }

  public CustomMap ()
  {
    ShapeCoordinates = new List<Position> ();
  }
}
```

`ShapeCoordinates` 属性将存储定义要突出显示的区域的坐标集合。

<a name="Consuming_the_Custom_Map" />

### <a name="consuming-the-custom-map"></a>使用自定义地图

通过在 XAML 页面实例中声明 `CustomMap` 控件的实例以使用该控件：

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:MapOverlay;assembly=MapOverlay"
             x:Class="MapOverlay.MapPage">
    <ContentPage.Content>
        <local:CustomMap x:Name="customMap" MapType="Street" WidthRequest="{x:Static local:App.ScreenWidth}" HeightRequest="{x:Static local:App.ScreenHeight}" />
    </ContentPage.Content>
</ContentPage>
```

或者，通过在 C# 页面实例中声明 `CustomMap` 控件的实例以使用该控件：

```csharp
public class MapPageCS : ContentPage
{
    public MapPageCS ()
    {
        var customMap = new CustomMap {
            MapType = MapType.Street,
            WidthRequest = App.ScreenWidth,
            HeightRequest = App.ScreenHeight
        };
        ...
        Content = customMap;
    }
}
```

根据需要初始化 `CustomMap` 控件：

```csharp
public partial class MapPage : ContentPage
{
  public MapPage ()
  {
    ...
    customMap.ShapeCoordinates.Add (new Position (37.797513, -122.402058));
    customMap.ShapeCoordinates.Add (new Position (37.798433, -122.402256));
    customMap.ShapeCoordinates.Add (new Position (37.798582, -122.401071));
    customMap.ShapeCoordinates.Add (new Position (37.797658, -122.400888));

    customMap.MoveToRegion (MapSpan.FromCenterAndRadius (new Position (37.79752, -122.40183), Distance.FromMiles (0.1)));
  }
}
```

此初始化指定一系列纬度和经度坐标，以定义要突出显示的地图区域。 然后使用 [`MoveToRegion`](xref:Xamarin.Forms.Maps.Map.MoveToRegion*) 方法（该方法通过根据 [`Position`](xref:Xamarin.Forms.Maps.Position) 和 [`Distance`](xref:Xamarin.Forms.Maps.Distance) 创建 [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan) 来更改地图的位置和缩放级别）定位地图的视图。

<a name="Customizing_the_Map" />

### <a name="customizing-the-map"></a>自定义地图

现在必须将自定义呈现器添加到每个应用程序项目中，以将多边形叠加层添加到地图中。

#### <a name="creating-the-custom-renderer-on-ios"></a>在 iOS 上创建自定义呈现器

创建 `MapRenderer` 类的一个子类并替代其 `OnElementChanged` 方法以添加多边形叠加层：

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace MapOverlay.iOS
{
    public class CustomMapRenderer : MapRenderer
    {
        MKPolygonRenderer polygonRenderer;

        protected override void OnElementChanged(ElementChangedEventArgs<View> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null) {
                var nativeMap = Control as MKMapView;
                if (nativeMap != null) {
                    nativeMap.RemoveOverlays(nativeMap.Overlays);
                    nativeMap.OverlayRenderer = null;
                    polygonRenderer = null;
                }
            }

            if (e.NewElement != null) {
                var formsMap = (CustomMap)e.NewElement;
                var nativeMap = Control as MKMapView;

                nativeMap.OverlayRenderer = GetOverlayRenderer;

                CLLocationCoordinate2D[] coords = new CLLocationCoordinate2D[formsMap.ShapeCoordinates.Count];

                int index = 0;
                foreach (var position in formsMap.ShapeCoordinates)
                {
                    coords[index] = new CLLocationCoordinate2D(position.Latitude, position.Longitude);
                    index++;
                }

                var blockOverlay = MKPolygon.FromCoordinates(coords);
                nativeMap.AddOverlay(blockOverlay);
            }
        }
        ...
    }
}

```

如果自定义呈现器附加到新的 Xamarin.Forms 元素，则此方法执行以下配置：

- 将 `MKMapView.OverlayRenderer` 属性设置为相应的委托。
- 从 `CustomMap.ShapeCoordinates` 属性检索经纬度坐标的集合并存储为 `CLLocationCoordinate2D` 实例数组。
- 多边形是通过调用静态 `MKPolygon.FromCoordinates` 方法创建的，该方法指定每个点的纬度和经度。
- 通过调用 `MKMapView.AddOverlay` 方法将多边形添加到地图中。 此方法画一条连接第一个点和最后一个点的线，通过这种方式使多边形自动封闭。

然后，实现 `GetOverlayRenderer` 方法以自定义叠加层的呈现：

```csharp
public class CustomMapRenderer : MapRenderer
{
  MKPolygonRenderer polygonRenderer;
  ...

  MKOverlayRenderer GetOverlayRenderer(MKMapView mapView, IMKOverlay overlayWrapper)
  {
      if (polygonRenderer == null && !Equals(overlayWrapper, null)) {
          var overlay = Runtime.GetNSObject(overlayWrapper.Handle) as IMKOverlay;
          polygonRenderer = new MKPolygonRenderer(overlay as MKPolygon) {
              FillColor = UIColor.Red,
              StrokeColor = UIColor.Blue,
              Alpha = 0.4f,
              LineWidth = 9
          };
      }
      return polygonRenderer;
  }
}
```

#### <a name="creating-the-custom-renderer-on-android"></a>在 Android 上创建自定义呈现器

创建 `MapRenderer` 类的一个子类并替代其 `OnElementChanged` 和 `OnMapReady` 方法以添加多边形叠加层：

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace MapOverlay.Droid
{
    public class CustomMapRenderer : MapRenderer
    {
        List<Position> shapeCoordinates;

        public CustomMapRenderer(Context context) : base(context)
        {
        }

        protected override void OnElementChanged(Xamarin.Forms.Platform.Android.ElementChangedEventArgs<Map> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null)
            {
                // Unsubscribe
            }

            if (e.NewElement != null)
            {
                var formsMap = (CustomMap)e.NewElement;
                shapeCoordinates = formsMap.ShapeCoordinates;
                Control.GetMapAsync(this);
            }
        }

        protected override void OnMapReady(Android.Gms.Maps.GoogleMap map)
        {
            base.OnMapReady(map);

            var polygonOptions = new PolygonOptions();
            polygonOptions.InvokeFillColor(0x66FF0000);
            polygonOptions.InvokeStrokeColor(0x660000FF);
            polygonOptions.InvokeStrokeWidth(30.0f);

            foreach (var position in shapeCoordinates)
            {
                polygonOptions.Add(new LatLng(position.Latitude, position.Longitude));
            }
            NativeMap.AddPolygon(polygonOptions);
        }
    }
}
```

`OnElementChanged` 方法从 `CustomMap.ShapeCoordinates` 属性检索经纬度坐标集合，并将它们存储在一个成员变量中。 如果自定义呈现器附加到新的 Xamarin.Forms 元素，则调用 `MapView.GetMapAsync` 方法，该方法获取与视图关联的基础 `GoogleMap`。 一旦 `GoogleMap` 实例可用，将调用 `OnMapReady` 方法，其中通过实例化指定各点经纬度的 `PolygonOptions` 对象创建多边形。 然后，通过调用 `NativeMap.AddPolygon` 方法将多边形添加到地图中。 此方法画一条连接第一个点和最后一个点的线，通过这种方式使多边形自动封闭。

#### <a name="creating-the-custom-renderer-on-the-universal-windows-platform"></a>在通用 Windows 平台上创建自定义呈现器

创建 `MapRenderer` 类的一个子类并替代其 `OnElementChanged` 方法以添加多边形叠加层：

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace MapOverlay.UWP
{
    public class CustomMapRenderer : MapRenderer
    {
        protected override void OnElementChanged(ElementChangedEventArgs<Map> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null)
            {
               // Unsubscribe
            }

            if (e.NewElement != null)
            {
                var formsMap = (CustomMap)e.NewElement;
                var nativeMap = Control as MapControl;

                var coordinates = new List<BasicGeoposition>();
                foreach (var position in formsMap.ShapeCoordinates)
                {
                    coordinates.Add(new BasicGeoposition() { Latitude = position.Latitude, Longitude = position.Longitude });
                }

                var polygon = new MapPolygon();
                polygon.FillColor = Windows.UI.Color.FromArgb(128, 255, 0, 0);
                polygon.StrokeColor = Windows.UI.Color.FromArgb(128, 0, 0, 255);
                polygon.StrokeThickness = 5;
                polygon.Path = new Geopath(coordinates);
                nativeMap.MapElements.Add(polygon);
            }
        }
    }
}
```

如果自定义呈现器附加到新的 Xamarin.Forms 元素，则此方法执行以下操作：

- 从 `CustomMap.ShapeCoordinates` 属性检索经纬度坐标集合，并将其转换为 `BasicGeoposition` 坐标的 `List`。
- 通过实例化 `MapPolygon` 对象来创建多边形。 将 `Path` 属性设置为包含形状坐标的 `Geopath` 对象，使 `MapPolygon` 类用于在地图上显示多点形状。
- 通过将多边形添加到 `MapControl.MapElements` 集合中，可在地图上呈现多边形。 注意，绘制一条连接第一个点和最后一个点的线即可使多边形自动封闭。

## <a name="summary"></a>总结

本文介绍如何向地图添加多边形叠加层，以突出显示地图的某一区域。 多边形是内部空间已填充的封闭形状。


## <a name="related-links"></a>相关链接

- [多边形地图叠加层（示例）](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-map-polygon)
- [自定义图钉](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md)
- [Xamarin.Forms.Maps](xref:Xamarin.Forms.Maps)
