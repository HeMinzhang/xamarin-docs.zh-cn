---
title: Xamarin 中的 TextKit
description: 本文档介绍如何在 Xamarin 中使用 TextKit。 TextKit 提供强大的文本布局和呈现功能。
ms.prod: xamarin
ms.assetid: 1D0477E8-CD1E-48A9-B7C8-7CA892069EFF
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/19/2017
ms.openlocfilehash: dba65eb11b6ae22a51610804e3a6a73549f4565b
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/06/2019
ms.locfileid: "70769764"
---
# <a name="textkit-in-xamarinios"></a>Xamarin 中的 TextKit

TextKit 是一个新的 API，它提供强大的文本布局和呈现功能。 它构建在低级别核心文本框架之上，但比核心文本更易于使用。

为了使 TextKit 的功能可用于标准控件，已重新实现多个 iOS 文本控件以使用 TextKit，其中包括：

- UITextView
- UITextField
- UILabel

## <a name="architecture"></a>体系结构

TextKit 提供了用于将文本存储与布局和显示分离的分层体系结构，包括以下类：

- `NSTextContainer`–提供坐标系统和用于布局文本的几何图形。
- `NSLayoutManager`–通过将文本转换为字型来布局文本。
- `NSTextStorage`–保存文本数据，并处理批文本属性更新。 任何批更新都将传递到布局管理器，以实际处理更改，如重新计算布局和重绘文本。

这三个类应用于呈现文本的视图。 内置文本`UITextView`处理视图（如`UITextField`、和`UILabel` ）已设置，但你也可以创建它们并将其应用于任何`UIView`实例。

下图演示了此体系结构：

 ![](textkit-images/textkitarch.png "此图说明了 TextKit 的体系结构")

## <a name="text-storage-and-attributes"></a>文本存储和属性

`NSTextStorage`类包含视图所显示的文本。 它还将对文本所做的任何更改（例如对字符或其属性的更改）传递到布局管理器以供显示。 `NSTextStorage`继承自`MSMutableAttributed` string，允许对要在和`EndEditing`调用之间`BeginEditing`分批指定的文本属性进行更改。

例如，下面的代码段分别指定对前景色和背景色的更改，并以特定范围为目标：

```csharp
textView.TextStorage.BeginEditing ();
textView.TextStorage.AddAttribute(UIStringAttributeKey.ForegroundColor, UIColor.Green, new NSRange(200, 400));
textView.TextStorage.AddAttribute(UIStringAttributeKey.BackgroundColor, UIColor.Black, new NSRange(210, 300));
textView.TextStorage.EndEditing ();
```

调用`EndEditing`后，将更改发送到布局管理器，而布局管理器又为要显示在视图中的文本执行任何所需的布局和呈现计算。

## <a name="layout-with-exclusion-path"></a>具有排除路径的布局

TextKit 还支持布局，并允许对复杂的方案（如多列文本）和围绕指定的路径（称为*排除路径*）进行文本传输。 排除路径适用于文本容器，该容器修改文本布局的几何，导致文本绕指定路径流动。

添加排除路径需要在布局管理`ExclusionPaths`器上设置属性。 设置此属性将导致布局管理器使文本布局失效，并围绕排除路径流式传输文本。

### <a name="exclusion-based-on-a-cgpath"></a>基于 CGPath 的排除

请考虑以下`UITextView`子类实现：

```csharp
public class ExclusionPathView : UITextView
{
    CGPath exclusionPath;
    CGPoint initialPoint;
    CGPoint latestPoint;
    UIBezierPath bezierPath;

    public ExclusionPathView (string text)
    {
        Text = text;
        ContentInset = new UIEdgeInsets (20, 0, 0, 0);
        BackgroundColor = UIColor.White;
        exclusionPath = new CGPath ();
        bezierPath = UIBezierPath.Create ();

        LayoutManager.AllowsNonContiguousLayout = false;
    }

    public override void TouchesBegan (NSSet touches, UIEvent evt)
    {
        base.TouchesBegan (touches, evt);

        var touch = touches.AnyObject as UITouch;

        if (touch != null) {
            initialPoint = touch.LocationInView (this);
        }
    }

    public override void TouchesMoved (NSSet touches, UIEvent evt)
    {
        base.TouchesMoved (touches, evt);

        UITouch touch = touches.AnyObject as UITouch;

        if (touch != null) {
            latestPoint = touch.LocationInView (this);
            SetNeedsDisplay ();
        }
    }

    public override void TouchesEnded (NSSet touches, UIEvent evt)
    {
        base.TouchesEnded (touches, evt);

        bezierPath.CGPath = exclusionPath;
        TextContainer.ExclusionPaths = new UIBezierPath[] { bezierPath };
    }

    public override void Draw (CGRect rect)
    {
        base.Draw (rect);

        if (!initialPoint.IsEmpty) {

            using (var g = UIGraphics.GetCurrentContext ()) {

                g.SetLineWidth (4);
                UIColor.Blue.SetStroke ();

                if (exclusionPath.IsEmpty) {
                    exclusionPath.AddLines (new CGPoint[] { initialPoint, latestPoint });
                } else {
                    exclusionPath.AddLineToPoint (latestPoint);
                }

                g.AddPath (exclusionPath);
                g.DrawPath (CGPathDrawingMode.Stroke);
            }
        }
    }
}
```

此代码添加了对使用核心图形在文本视图上绘图的支持。 `UITextView`由于该类现在已生成以使用 TextKit 来呈现文本和布局，因此它可以使用 TextKit 的所有功能，例如设置排除路径。

> [!IMPORTANT]
> 此示例子类`UITextView`用于添加触控绘图支持。 若`UITextView`要获取 TextKit 的功能，无需提供子类。

用户在文本视图上进行绘制后，绘制`CGPath`的将通过设置`UIBezierPath.CGPath`属性`UIBezierPath`应用到实例：

```csharp
bezierPath.CGPath = exclusionPath;
```

更新以下代码行会使文本布局围绕路径进行更新：

```csharp
TextContainer.ExclusionPaths = new UIBezierPath[] { bezierPath };
```

下面的屏幕截图说明了文本布局是如何更改以在绘制路径周围流动的：

<!-- ![](textkit-images/exclusionpath1.png "This screenshot illustrates how the text layout changes to flow around the drawn path")-->
![](textkit-images/exclusionpath2.png "此屏幕截图说明文本布局如何更改以在绘制路径周围流动")

请注意，在这种`AllowsNonContiguousLayout`情况下，布局管理器的属性设置为 false。 这将导致重新计算文本更改的所有情况的布局。 如果将此值设置为 true，则可以通过避免完全布局刷新（尤其是在大文档时）来提高性能。 但是，在`AllowsNonContiguousLayout`某些情况下，将设置为 true 会阻止排除路径更新布局，例如，如果在运行时输入文本，则在设置路径之前没有尾随回车符。

## <a name="related-links"></a>相关链接

- [IOS 7 简介（示例）](https://docs.microsoft.com/samples/xamarin/ios-samples/introtoios7)
- [iOS 7 用户界面概述](~/ios/platform/introduction-to-ios7/ios7-ui.md)
- [后台处理](~/ios/app-fundamentals/backgrounding/index.md)
