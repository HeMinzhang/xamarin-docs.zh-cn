---
title: 将文本和图形集成
description: 本文介绍了如何确定要将文本与 SkiaSharp 图形集成到 Xamarin.Forms 应用程序的呈现的文本字符串的大小，并且此示例代码进行了演示。
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: A0B5AC82-7736-4AD8-AA16-FE43E18D203C
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: be1524029ada79896f83517c3b439f2ad0e2c6d9
ms.sourcegitcommit: 12d48cdf99f0d916536d562e137d0e840d818fa1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/07/2018
ms.locfileid: "39615156"
---
# <a name="integrating-text-and-graphics"></a>将文本和图形集成

_请参阅如何确定要与 SkiaSharp 图形集成，文本的呈现的文本字符串的大小_

本文演示如何测量文本、 可能缩放到特定大小的文本和与其他图形集成，文本：

![](text-images/textandgraphicsexample.png "由矩形包围的文本")

SkiaSharp`Canvas`类还包含用于绘制矩形的方法 ([`DrawRect`](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawRect/p/SkiaSharp.SKRect/SkiaSharp.SKPaint/)) 以及具有圆角矩形 ([`DrawRoundRect`](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawRoundRect/p/SkiaSharp.SKRect/System.Single/System.Single/SkiaSharp.SKPaint/))。 这些方法需要矩形来定义为`SKRect`值。

**帧文本**页面为中心的围绕它与框架组成的圆角矩形对页上的短文本字符串。 [ `FramedTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/FramedTextPage.cs)类显示了如何执行操作。

SkiaSharp 在您使用`SKPaint`类设置文本和字体属性，但你还可用于其获取文本的呈现的大小。 以下的开头`PaintSurface`事件处理程序调用两个不同`MeasureText`方法。 第一个[ `MeasureText` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.MeasureText/p/System.String/)调用具有一个简单的`string`自变量并返回文本的像素宽度基于当前的字体属性。 程序然后计算一个新`TextSize`的属性`SKPaint`对象，基于该呈现宽度，当前`TextSize`属性，并显示区域的宽度。 这主要是为了设置`TextSize`以便要呈现在屏幕的宽度的 90%的文本字符串：

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    string str = "Hello SkiaSharp!";

    // Create an SKPaint object to display the text
    SKPaint textPaint = new SKPaint
    {
        Color = SKColors.Chocolate
    };

    // Adjust TextSize property so text is 90% of screen width
    float textWidth = textPaint.MeasureText(str);
    textPaint.TextSize = 0.9f * info.Width * textPaint.TextSize / textWidth;

    // Find the text bounds
    SKRect textBounds = new SKRect();
    textPaint.MeasureText(str, ref textBounds);
    ...
}
```

第二个[ `MeasureText` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.MeasureText/p/System.String/SkiaSharp.SKRect@/)调用具有`SKRect`参数，因此它获得的宽度和高度的呈现的文本。 `Height`属性的`SKRect`值取决于是否存在大写字母、 升部，并在文本字符串中的下行字母。 不同`Height`文本字符串"mom"、"cat"和"dog"，例如报告值。

`Left`并`Top`的属性`SKRect`结构指示呈现的文本的左上角的坐标，如果通过显示的文本`DrawText`调用与 0 X 和 Y 位置。 例如，此程序运行时 iPhone 7 模拟器`TextSize`分配作为第一个之后调用的计算结果值 90.6254 `MeasureText`。 `SKRect`从第二次调用获取值`MeasureText`具有以下属性值：

- `Left` = 6
- `Top` = &ndash;68
- `Width` = 664.8214
- `Height` = 88;

请记住 X 和 Y 坐标您传递给`DrawText`方法指定左侧和右侧的基准上的文本。 `Top`值表示文本扩展了更高版本的该基线 （减去 68 it 部门从 88） 68 像素基线下方的 20 像素。 `Left`值为 6 表示文本以 6 个像素中的 X 值右侧`DrawText`调用。 这样可以正常字符间间距。 如果你想要在左上角显示的牢固插入显示文本，通过这些负`Left`并`Top`值的 X 和 Y 坐标`DrawText`，在此示例中， &ndash;6 和 68。

`SKRect`结构定义了一些易于使用的属性和方法，其中一些使用中的其余部分`PaintSurface`处理程序。 `MidX`和`MidY`值表示的矩形的中心坐标。 (在 iPhone 7 示例中，这些值是 338.4107 和&ndash;24。)以下代码使用这些值的坐标最简单的计算 center 上显示的文本：

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    // Calculate offsets to center the text on the screen
    float xText = info.Width / 2 - textBounds.MidX;
    float yText = info.Height / 2 - textBounds.MidY;

    // And draw the text
    canvas.DrawText(str, xText, yText, textPaint);
    ...
}
```

`PaintSurface`两次调用处理程序以结束`DrawRoundRect`，这两个需要的参数`SKRect`。 这`SKRect`值是当然类似于`SKRect`获得的值`MeasureText`方法，但它不能为相同。 首先，它需要以便上边缘的文本，不会绘制圆角的矩形，其次，它需要在空间中向变大一些，以便`Left`和`Top`值对应于该矩形将成为在左上角定位。 这两个作业都通过完成`Offset`并`Inflate`定义的方法`SKRect`:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    // Create a new SKRect object for the frame around the text
    SKRect frameRect = textBounds;
    frameRect.Offset(xText, yText);
    frameRect.Inflate(10, 10);

    // Create an SKPaint object to display the frame
    SKPaint framePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        StrokeWidth = 5,
        Color = SKColors.Blue
    };

    // Draw one frame
    canvas.DrawRoundRect(frameRect, 20, 20, framePaint);

    // Inflate the frameRect and draw another
    frameRect.Inflate(10, 10);
    framePaint.Color = SKColors.DarkBlue;
    canvas.DrawRoundRect(frameRect, 30, 30, framePaint);
}
```

接下来，该方法的其余部分是简单直接。 它将创建另一个`SKPaint`对象的边框和调用`DrawRoundRect`两次。 第二次调用使用使另一个 10 个像素的矩形。 第一次调用指定 20 个像素; 圆角的半径第二个圆角半径 30 个像素，因此它们看起来实现并行：

 [![](text-images/framedtext-small.png "三重设计框架的文本页屏幕截图")](text-images/framedtext-large.png#lightbox "带来三倍的帧的文本页屏幕截图")

可以将手机或模拟器侧向若要查看的文本和帧的大小而增加。

如果您只需要在屏幕上的某些文本居中，不会它大约通过设置测量文本`TextAlign`的属性`SKPaint`到`SKTextAlign.Center`。 在中指定的 X 坐标`DrawText`方法然后指示文本的水平居中的位置。 如果传递到屏幕的中点`DrawText`方法中，文本将在水平居中并*几乎*垂直方向上居中因为基线将垂直方向上居中。

文本本身可以被更当作一个图形选项。 一种简单方法是显示的文本字符而不是普通的填充的显示大纲：

[![](text-images/outlinedtext-small.png "三重轮廓文本页面的屏幕截图")](text-images/outlinedtext-large.png#lightbox "三重轮廓文本页面的屏幕截图")

这是只需通过更改普通`Style`的属性`SKPaint`对象从其默认设置为`SKPaintStyle.Fill`到`SKPaintStyle.Stroke`以及通过指定笔划宽度。 `PaintSurface`处理程序**所述文本**页显示了如何执行操作：

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    string text = "OUTLINE";

    // Create an SKPaint object to display the text
    SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        StrokeWidth = 1,
        FakeBoldText = true,
        Color = SKColors.Blue
    };

    // Adjust TextSize property so text is 95% of screen width
    float textWidth = textPaint.MeasureText(text);
    textPaint.TextSize = 0.95f * info.Width * textPaint.TextSize / textWidth;

    // Find the text bounds
    SKRect textBounds = new SKRect();
    textPaint.MeasureText(text, ref textBounds);

    // Calculate offsets to center the text on the screen
    float xText = info.Width / 2 - textBounds.MidX;
    float yText = info.Height / 2 - textBounds.MidY;

    // And draw the text
    canvas.DrawText(text, xText, yText, textPaint);
}
```

 在过去的几个文章中，你必须现在了解了如何使用 SkiaSharp 绘制文本，圆圈，椭圆和圆角的矩形。 下一步是[SkiaSharp 线和路径](~/xamarin-forms/user-interface/graphics/skiasharp/paths/paths.md)在其中将学习如何相互连接的直线绘制图形路径中。


## <a name="related-links"></a>相关链接

- [SkiaSharp Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos （示例）](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
