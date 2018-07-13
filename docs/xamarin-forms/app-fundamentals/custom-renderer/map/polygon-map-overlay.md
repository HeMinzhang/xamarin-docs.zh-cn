---
title: 突出显示地图上的区域
description: 本文介绍如何将多边形覆盖添加到映射中，以突出显示地图上的区域。 多边形是一个闭合的形状和中填充其内部。
ms.prod: xamarin
ms.assetid: E79EB2CF-8DD6-44A8-B47D-5F0A94FB0A63
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 0a11e9c25922531727ad2fee3bbed9c8d4e2b80c
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998129"
---
# <a name="highlighting-a-region-on-a-map"></a>突出显示地图上的区域

_本文介绍了如何将多边形覆盖添加到映射，以突出显示地图上的区域。多边形是一个闭合的形状和中填充其内部。_

## <a name="overview"></a>概述

覆盖是在地图上的分层的图形。 覆盖层支持绘制图形的内容，它被放大随着与该映射。 以下屏幕截图显示向地图添加多边形覆盖的结果：

![](polygon-map-overlay-images/screenshots.png)

当[ `Map` ](xref:Xamarin.Forms.Maps.Map) Xamarin.Forms 应用程序，在 iOS 中呈现控件`MapRenderer`类实例化时，这反过来实例化本机`MKMapView`控件。 在 Android 平台上`MapRenderer`类实例化本机`MapView`控件。 在通用 Windows 平台 (UWP)，`MapRenderer`类实例化本机`MapControl`。 渲染过程时可以执行利用通过创建自定义呈现器为实现特定于平台的映射自定义`Map`每个平台上。 执行此操作的过程如下所示：

1. [创建](#Creating_the_Custom_Map)Xamarin.Forms 自定义地图。
1. [使用](#Consuming_the_Custom_Map)Xamarin.Forms 中的自定义映射。
1. [自定义](#Customizing_the_Map)通过每个平台上创建代码图的自定义呈现器映射。

> [!NOTE]
> [`Xamarin.Forms.Maps`](xref:Xamarin.Forms.Maps) 必须初始化和使用之前配置。 有关详细信息，请参阅 [`Maps Control`](~/xamarin-forms/user-interface/map.md)。

有关自定义使用自定义呈现器的映射的信息，请参阅[自定义图钉](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md)。

<a name="Creating_the_Custom_Map" />

### <a name="creating-the-custom-map"></a>创建自定义地图

创建一个子类[ `Map` ](xref:Xamarin.Forms.Maps.Map)类，它将`ShapeCoordinates`属性：

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

`ShapeCoordinates`属性将存储的定义以突出显示的区域的坐标集合。

<a name="Consuming_the_Custom_Map" />

### <a name="consuming-the-custom-map"></a>使用自定义地图

使用`CustomMap`控件通过声明它的实例中的 XAML 页实例：

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

或者，使用`CustomMap`控件通过声明它的实例中的 C# 页实例：

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

初始化`CustomMap`控件：

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

此初始化指定一系列的纬度和经度坐标定义的映射会突出显示的区域。 然后将定位具有地图的视图[ `MoveToRegion` ](xref:Xamarin.Forms.Maps.Map.MoveToRegion*)方法，以通过创建更改的位置和地图的缩放级别[ `MapSpan` ](xref:Xamarin.Forms.Maps.MapSpan)从[ `Position`](xref:Xamarin.Forms.Maps.Position)和一个[ `Distance` ](xref:Xamarin.Forms.Maps.Distance)。

<a name="Customizing_the_Map" />

### <a name="customizing-the-map"></a>自定义地图

现在必须为要向地图添加多边形覆盖每个应用程序项目添加自定义呈现器。

#### <a name="creating-the-custom-renderer-on-ios"></a>在 ios 设备上创建自定义呈现器

创建一个子类`MapRenderer`类并重写其`OnElementChanged`方法添加多边形覆盖：

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

此方法执行下面的配置，前提是自定义呈现器附加到新 Xamarin.Forms 元素：

- `MKMapView.OverlayRenderer`属性设置为对应的委托。
- 从纬度和经度坐标的集合中检索`CustomMap.ShapeCoordinates`属性和数组的形式存储`CLLocationCoordinate2D`实例。
- 通过调用静态创建多边形`MKPolygon.FromCoordinates`方法，后者指定的纬度和经度的每个点。
- 多边形被添加到映射通过调用`MKMapView.AddOverlay`方法。 此方法会自动关闭多边形通过绘制一条连接的第一个和最后一个点线。

然后，实现`GetOverlayRenderer`方法以自定义在覆盖区上的呈现：

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

创建一个子类`MapRenderer`类并重写其`OnElementChanged`和`OnMapReady`添加多边形覆盖的方法：

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

`OnElementChanged`方法检索集合中的纬度和经度坐标`CustomMap.ShapeCoordinates`属性并将它们存储在成员变量中。 然后，它调用`MapView.GetMapAsync`方法，获取基础`GoogleMap`的，提供自定义呈现器附加到新 Xamarin.Forms 元素绑定到视图。 一次`GoogleMap`实例不可用，`OnMapReady`将调用方法，其中通过实例化创建多边形`PolygonOptions`对象，它指定的纬度和经度的每个点。 多边形然后通过调用添加到映射`NativeMap.AddPolygon`方法。 此方法会自动关闭多边形通过绘制一条连接的第一个和最后一个点线。

#### <a name="creating-the-custom-renderer-on-the-universal-windows-platform"></a>在通用 Windows 平台上创建自定义呈现器

创建一个子类`MapRenderer`类并重写其`OnElementChanged`方法添加多边形覆盖：

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

提供自定义呈现器附加到新 Xamarin.Forms 元素，此方法将执行以下操作：

- 从纬度和经度坐标的集合中检索`CustomMap.ShapeCoordinates`属性和转换为`List`的`BasicGeoposition`坐标。
- 多边形创建的实例化`MapPolygon`对象。 `MapPolygon`类用于在地图上显示多点形状，通过设置其`Path`属性设置为`Geopath`对象，其中包含的形状坐标。
- 将其添加到地图上呈现多边形`MapControl.MapElements`集合。 请注意，多边形通过绘制一条连接的第一个和最后一个点线将为自动关闭。

## <a name="summary"></a>总结

本文介绍了如何将多边形覆盖添加到映射，以突出显示的地图区域。 多边形是一个闭合的形状和中填充其内部。


## <a name="related-links"></a>相关链接

- [多边形映射覆盖 （示例）](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/map/polygon/)
- [自定义图钉](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md)
- [Xamarin.Forms.Maps](xref:Xamarin.Forms.Maps)
