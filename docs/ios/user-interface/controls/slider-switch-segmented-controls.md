---
title: Xamarin 中的滑块、开关和分段控件
description: 本文档讨论了 Xamarin 中的幻灯片、开关和分段控件，并介绍了如何在 iOS 设计器中以编程方式使用它们。
ms.prod: xamarin
ms.assetid: 85BF0EC8-E581-49CD-B9E7-98BE4C5A0F6B
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/21/2017
ms.openlocfilehash: e95a514286c68879eac4ee80f2c95882b4e2b5b6
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/06/2019
ms.locfileid: "70768468"
---
# <a name="sliders-switches-and-segmented-controls-in-xamarinios"></a>Xamarin 中的滑块、开关和分段控件

<a name="Sliders" />

## <a name="sliders"></a>滑块

使用滑块控件，可以在范围中简单选择数字值。 控件默认为介于0到1之间的值，但可以自定义这些限制。

 [![](slider-switch-segmented-controls-images/image25a.png "滑动")](slider-switch-segmented-controls-images/image25a.png#lightbox)

以下屏幕截图显示了可在设计器中编辑的属性：

 [![](slider-switch-segmented-controls-images/image26a.png "滑块属性")](slider-switch-segmented-controls-images/image25a.png#lightbox)

您可以在代码中设置这些值，如下所示，包括将处理程序绑定到`UILabel`控件中的当前选定值：

```csharp
slider1.MinValue = -1;
slider1.MaxValue = 2;
slider1.Value = 0.5f; // the current value
slider1.ValueChanged += (sender,e) => label1.Text = ((UISlider)sender).Value.ToString ();
```

还可以通过将设置为

```csharp
slider1.ThumbTintColor = UIColor.Blue;
slider1.MinimumTrackTintColor = UIColor.Gray;
slider1.MaximumTrackTintColor = UIColor.Green;
```

自定义滑块如下所示：

 [![](slider-switch-segmented-controls-images/image27a.png "自定义滑块")](slider-switch-segmented-controls-images/image28a.png#lightbox)

> [!IMPORTANT]
> 当前有一个[bug](https://stackoverflow.com/a/19496179)导致`ThumbTint`在运行时无法按预期方式呈现。 可以在上述代码**前面**添加以下代码行作为解决方法。 [[源](https://stackoverflow.com/a/21396794)]：
>
> `slider1.SetThumbImage(UIImage.FromBundle("thumb.png"),UIControlState.Normal);`
> 
> 可以使用任何映像，因为它将被重写，但请确保将其放_在_Resources 目录中，并在代码中调用。

<a name="Switch" />

## <a name="switch"></a>开关

iOS 使用`UISwitch`作为可通过其他平台上的单选按钮表示的布尔输入。 用户可以通过将*滚动块*移到**开启/关闭**位置之间来操作控件。

 [![](slider-switch-segmented-controls-images/image28a.png "转")](slider-switch-segmented-controls-images/image28a.png#lightbox)

可以在设计器的**Properties Pad**中自定义开关的外观，这将允许您控制默认状态、**打开/关闭淡色**颜色和**打开/关闭图像**。 下图对此进行了说明：

 [![](slider-switch-segmented-controls-images/image29a.png "切换属性")](slider-switch-segmented-controls-images/image29a.png#lightbox)

还可以在代码中设置开关的属性，例如，以下代码将显示一个具有默认值的`On`开关：

```csharp
switch1.On = true;
```

 <a name="Segmented_Controls" />

## <a name="segmented-controls"></a>分段控件

分段控件是一种允许用户与少量选项交互的组织方式。 它是水平布局的，每个段都作为一个单独的按钮工作。 使用设计器时，可以在**工具箱 > 控件**下找到分段控件，如下图所示：

 [![](slider-switch-segmented-controls-images/segmentedcontrol.png "分段控件")](slider-switch-segmented-controls-images/segmentedcontrol.png#lightbox)

设计器的一项独特功能允许在设计图面上单独选择每个段，如下所示：

 [![](slider-switch-segmented-controls-images/segmentedcontrolselection.png "分段控件")](slider-switch-segmented-controls-images/segmentedcontrolselection.png#lightbox)

这允许 Properties Pad 用于更精确地控制每个段的属性。 可以在下面的屏幕截图中查看可编辑的属性：

 [![](slider-switch-segmented-controls-images/segmentedcontrolproperties.png "分段控件")](slider-switch-segmented-controls-images/segmentedcontrolproperties.png#lightbox)

应注意的是，iOS7 中已弃用分段控件样式，因此，在 iOS7 应用程序中调整此项的选项将不起作用。

## <a name="related-links"></a>相关链接

- [控件（示例）](https://docs.microsoft.com/samples/xamarin/ios-samples/controls)
- [警报控制器](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/alertcontroller)
