---
title: 在 Xamarin 中使用 tvOS 导航和焦点
description: 本文介绍了焦点的概念以及如何使用它来呈现和处理 tvOS 应用程序中的导航。
ms.prod: xamarin
ms.assetid: DD72E95F-AE9B-47D2-B132-5FA5FBD8026E
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/16/2017
ms.openlocfilehash: b84150c8fdca04a9b21c28d126d221d2a9bc5a3f
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/06/2019
ms.locfileid: "70769253"
---
# <a name="working-with-tvos-navigation-and-focus-in-xamarin"></a>在 Xamarin 中使用 tvOS 导航和焦点

_本文介绍了焦点的概念以及如何使用它来呈现和处理 tvOS 应用程序中的导航。_

本文介绍了[焦点](#Focus-and-Selection)的概念以及如何使用它来处理 Xamarin tvOS 应用的用户界面中的[导航](#Navigation)。 我们将检查内置 tvOS 导航控件如何使用焦点、突出显示和选择以提供 Xamarin. tvOS 应用程序的用户界面导航。

[![](navigation-focus-images/intro01.png "tvOS apps 用户界面导航")](navigation-focus-images/intro01.png#lightbox)

接下来，我们将介绍如何将焦点与[视差](#Focus-and-Parallax)和*分层图像*一起使用，为最终用户提供当前导航状态的视觉线索。

最后, 我们将探讨如何使用[焦点](#Working-with-Focus),[重点更新](#Working-with-Focus-Updates),[重点指南](#Working-with-Focus-Guides), 将[焦点放在](#Working-with-Focus-in-Collections)tvOS 应用程序中的图像视图上。[启用并行](#enabling-parallax)

<a name="Navigation" />

## <a name="navigation"></a>导航

你的 tvOS 应用程序的用户将不会与 iOS 直接交互，因为在 iOS 中点击设备上的图像，而不是使用[Siri 远程](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote)在房间内间接显示。 在设计应用程序的用户界面时，需要记住这一点，使其自然流动，同时还会使用户沉浸在 Apple TV 体验中。

成功的 tvOS 应用以平稳地支持应用的目的和它所提供的数据的结构，而无需注意导航本身的方式实现导航。 设计你的导航，使其感觉自然和熟悉，无需占据用户界面或将焦点从内容和应用程序用户体验中消失。

[![](navigation-focus-images/nav01.png "TvOS 设置应用")](navigation-focus-images/nav01.png#lightbox)

使用 Apple TV 时，用户通常会在一组堆积屏幕上导航，每个屏幕都提供一组给定的内容。 反过来，每个新屏幕都可能使用标准 UI 控件（如[按钮](~/ios/tvos/user-interface/buttons.md)、[选项卡栏](~/ios/tvos/user-interface/tab-bars.md)、表、[集合视图](~/ios/tvos/user-interface/collection-views.md)或[拆分视图](~/ios/tvos/user-interface/split-views.md)）来导致内容的一个或多个子屏幕。

对于每个新的数据屏幕，用户将导航到此屏幕堆栈。 通过使用 Siri 遥控器上的**菜单**按钮，用户可以通过堆栈向后导航以返回到上一个屏幕或主菜单。

Apple 建议在设计 tvOS 应用的导航时请牢记以下事项：

- **布局导航，使用户能够快速轻松地查找内容**，用户希望在点击量最少的情况下访问应用中的内容，点击次数和 swipes。 简化你的导航并将内容组织到最少数量的屏幕上。
- **使用触控创建流体接口**-通过使用尽可能少的手势，确保用户可以通过最少的摩擦在可获得_焦点的项_之间移动。
- **设计具有重点**，因为用户要在房间内与内容交互，所以他们需要将焦点移到用户界面项，然后再使用 Siri 远程进行交互。 如果用户需要太多手势来实现其目标，则用户将会遇到应用程序的不满意情况。
- **通过菜单按钮提供向后导航**-若要创建一个简单熟悉的体验，请允许用户使用 Siri 遥控器的**菜单**按钮向后导航。 按 "**菜单**" 按钮应始终返回到上一个屏幕，或返回到应用程序的主菜单。 在应用程序的顶层，按**菜单**按钮应返回到 Apple TV 主屏幕。
- **通常**不会显示 "后退" 按钮，因为在 Siri 遥控器上按下的**菜单**按钮会在屏幕堆栈中向后定位，因此应避免显示重复此行为的其他控件。 此规则的例外情况是，使用应显示 "**取消**" 按钮的破坏性操作（如删除内容）来购买屏幕或屏幕。
- **在单个屏幕上显示大型集合，而不是多**个 Siri 远程，旨在使用手势快速轻松地浏览大量内容。 如果你的应用程序适用于大量可设定焦点的项，请考虑将其保存在单个屏幕上，而不是将其拆分为多个屏幕，这些屏幕需要用户的更多导航。
- **使用标准控件再次导航**，若要创建一个简单而熟悉的用户体验，请尽可能使用内置`UIKit`控件，如页控件、选项卡栏、分段控件、表视图、集合视图和拆分视图。应用的导航。 由于用户已经熟悉这些元素，因此它们将以直观方式浏览您的应用程序。
- **优选水平内容导航**-由于 Apple TV 的性质，在 Siri 遥控器上向右轻扫会比向上和向右轻扫。 为应用设计内容布局时，请考虑此选项。

<a name="Focus-and-Selection" />

## <a name="focus-and-selection"></a>焦点和选择

在 Apple TV 上，当它是当前导航的目标时，图像、按钮或其他 UI 元素将被视为_处于焦点_。

[![](navigation-focus-images/focus01.png "焦点和选择示例")](navigation-focus-images/focus01.png#lightbox)

不同于 iOS 设备，其中用户直接与设备的触摸屏上的元素交互，用户使用 Siri 远程与室内的 tvOS 元素交互。 为了提供和处理此用户交互，Apple TV 使用基于_焦点_的模型。

使用笔势并按下在[Siri 遥控器](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote)上按按钮，用户可将焦点从一个 UI 元素移到另一个 UI 元素。 焦点移到所需的元素后，用户单击以选择内容或激活该元素表示的操作。

当重点变化时，会使用细微的动画和效果（如图像上的视差效果）来清楚地识别当前有焦点的用户界面项。

Apple 对于使用焦点和选择有以下建议：

- **使用内置的 UI 控件来实现运动效果**-通过使用`UIKit`和用户界面中的焦点 API，焦点模型会自动将默认动作和视觉效果应用于 UI 元素。 这会使你的应用对 Apple TV 平台用户具有本机和熟悉，并允许可设定焦点的项目之间进行流畅且直观的移动。
- **将焦点移动到预期方向**-在 Apple TV 上，几乎每个元素都使用间接操作。 例如，用户使用 Siri 遥控器来移动焦点，系统会自动滚动接口，以使当前聚焦的项可见。 如果你的应用程序实现了这种类型的交互，请确保焦点按用户的手势方向移动。 因此，如果用户在 Siri 遥控器上向右轻扫，则应向右移动（这会导致屏幕向左滚动）。 此规则的例外情况是，使用直接操作（在其中，向上轻扫会使元素向上移动）的全屏项。
- **确保重点项显而易见**，因为用户与阿法尔语中的 UI 元素进行交互，所以，当前的聚焦项确实非常重要。这通常会由内置`UIKit`元素自动处理。 对于自定义控件，请使用项大小或阴影等功能来显示焦点。
- **使用视差使重点项的响应能力更**小，Siri 遥控器上的循环手势会使聚焦项的实时实时移动。 此[视差效果](#Focus-and-Parallax)内置于`UIKit` _分层图像_中，使用户有权连接到聚焦项。
- **创建适当大小的可设定焦点的项**-具有充足间距的大型项比小项更容易选择和导航。
- **设计 UI 元素以使其看起来很有针对性或失去焦点**-通常情况下，Apple TV 通过增加其大小来表示重点项。 确保应用的 UI 元素在任何显示大小上看起来很好，如有必要，为大小较大的元素提供资产。
- **表示焦点更改流畅地**-使用动画以平稳地在**聚焦**和**失去焦点**状态的项之间淡化，以防止转换 jarring。
- **不显示光标**-用户希望使用焦点在应用的 UI 上导航，而不是在屏幕上移动光标。 用户界面应始终使用焦点模型来提供一致的用户体验。

<a name="Working-with-Focus" />

### <a name="working-with-focus"></a>使用焦点

有时可能需要创建可成为可设定焦点的项的自定义控件。 如果为，则`CanBecomeFocused`重写属性`true`并返回， `false`否则返回。 例如：

```csharp
public class myView : UIView
{
    public override bool CanBecomeFocused {
        get {return true;}
    }
}
```

你随时都可以使用`Focused` `UIKit`控件的属性来查看它是否为当前项。 如果`true` UI 项当前具有焦点，则为; 否则为。 例如:

```csharp
// Is my view in focus?
if (myView.Focused) {
    // Do something
    ...
}
```

尽管不能通过代码直接将焦点移到另一个 ui 元素，但你可以指定在通过将屏幕设置`PreferredFocusedView`为`true`时将哪个 ui 元素首次获取焦点。 例如:

```csharp
// Make the play button the starting focus item
playButton.PreferredFocusedView = true;
```

<a name="Working-with-Focus-Updates" />

### <a name="working-with-focus-updates"></a>使用焦点更新 

当用户导致焦点从一个 UI 元素移到另一个 UI 元素时（例如，为了响应 Siri 远程上的笔势），_焦点更新事件_将发送到失去焦点的项，并且该项获得焦点。

对于从`UIView`或`UIViewController`继承的自定义元素，你可以重写若干方法以使用焦点更新事件：

- **DidUpdateFocus** -每当视图获得或失去焦点时，都会调用此方法。
- **ShouldUpdateFocus** -使用此方法定义允许焦点移动的位置。

若要请求焦点引擎将焦点移回`PreferredFocusedView` UI 元素，请`SetNeedsUpdateFocus`调用视图控制器的方法。

> [!IMPORTANT]
> 如果`SetNeedsUpdateFocus`调用的视图控制器包含当前具有焦点的视图，则调用仅会生效。

<a name="Working-with-Focus-Guides" />

### <a name="working-with-focus-guides"></a>使用焦点指南

内置于 tvOS 中的焦点引擎非常适合处理水平和垂直网格中的项之间的移动。 通常，您只需将 UI 元素添加到您的接口设计中，焦点引擎就会自动处理这些元素之间的移动，而无需开发人员干预。

但有时，由于 UI 设计的必要条件，用户界面元素不在水平和垂直网格上并且可能无法访问，因为它们彼此之间相互倾斜。 出现这种情况的原因是，焦点引擎旨在处理 UI 项之间的简单向上、向下、向左和右移动。

对于示例，请使用以下 UI 布局：

 [![](navigation-focus-images/guide01.png "使用焦点指南示例")](navigation-focus-images/guide01.png#lightbox)

由于 "**详细信息**" 按钮并不在具有 "**购买**" 按钮的水平和垂直网格上，因此用户将无法访问它。 但是，可以使用_重点指南_轻松地纠正这种情况，以便向焦点引擎提供移动提示。 

焦点指南（`UIFocusGuide`）公开了视图的不可见区域，以使其可用于焦点引擎，从而允许将焦点重定向到另一个视图。

因此，若要解决上面给出的示例，可将以下代码添加到视图控制器，以在 "**详细信息**" 和 "**购买**" 按钮之间创建焦点指南：

```csharp
public UIFocusGuide FocusGuide = new UIFocusGuide ();
...

public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Add Focus Guide to layout
    View.AddLayoutGuide (FocusGuide);

    // Define Focus Guide that will allow the user to move
    // between the More Info and Buy buttons.
    FocusGuide.LeftAnchor.ConstraintEqualTo (BuyButton.LeftAnchor).Active = true;
    FocusGuide.TopAnchor.ConstraintEqualTo (MoreInfoButton.TopAnchor).Active = true;
    FocusGuide.WidthAnchor.ConstraintEqualTo (BuyButton.WidthAnchor).Active = true;
    FocusGuide.HeightAnchor.ConstraintEqualTo (MoreInfoButton.HeightAnchor).Active = true;
}
```

首先， `UIFocusGuide` `AddLayoutGuide`使用方法创建一个新的，并将其添加到视图的布局指南集合中。

接下来，重点指南的顶部、左侧、宽度和高度锚点相对于**详细信息**进行调整，并**购买**按钮将其置于不同位置。 请参阅：

[![](navigation-focus-images/guide02.png "示例焦点指南")](navigation-focus-images/guide02.png#lightbox)

另外，请务必注意，在创建新约束时通过将它们的`Active`属性设置为： `true`

```csharp
FocusGuide.LeftAnchor.ConstraintEqualTo (...).Active = true;
```

在新的重点指南建立并添加到视图中后，可以重写`DidUpdateFocus`视图控制器的方法，并添加以下代码以在 "**详细信息**" 和 "**购买**" 按钮之间移动：

```csharp
public override void DidUpdateFocus (UIFocusUpdateContext context, UIFocusAnimationCoordinator coordinator)
{
    base.DidUpdateFocus (context, coordinator);

    // Get next focusable item from context
    var nextFocusableItem = context.NextFocusedView;

    // Anything to process?
    if (nextFocusableItem == null) return;

    // Decide the next focusable item based on the current
    // item with focus
    if (nextFocusableItem == MoreInfoButton) {
        // Move from the More Info to Buy button
        FocusGuide.PreferredFocusedView = BuyButton;
    } else if (nextFocusableItem == BuyButton) {
        // Move from the Buy to the More Info button
        FocusGuide.PreferredFocusedView = MoreInfoButton;
    } else {
        // No valid move
        FocusGuide.PreferredFocusedView = null;
    }
}
```

首先，此代码`NextFocusedView` `UIFocusUpdateContext`从传入的（`context`）中获取。 如果此视图为`null`，则不需要任何处理，并且方法已退出。

接下来， `nextFocusableItem`计算。 如果它与 "**详细信息**" 或 "**购买**" 按钮相匹配，则使用焦点指南的`PreferredFocusedView`属性将焦点发送到相反的按钮。 例如:

```csharp
// Move from the More Info to Buy button
FocusGuide.PreferredFocusedView = BuyButton;
```

如果这两个按钮都不是焦点的源， `PreferredFocusedView`则会清除属性：

```csharp
// No valid move
FocusGuide.PreferredFocusedView = null;
```

<a name="Working-with-Focus-in-Collections" />

### <a name="working-with-focus-in-collections"></a>使用集合中的焦点

`UICollectionView`确定单个项在`UITableView`或中是否可设定焦点时，您`UICollectionViewDelegate`将重写或`UITableViewDelegate`的方法。 例如：

```csharp
public class CardHandDelegate : UICollectionViewDelegateFlowLayout
{
    ...
    public override bool CanFocusItem (UICollectionView collectionView, NSIndexPath indexPath)
    {
        if (indexPath == null) {
            return false;
        } else {
            var controller = collectionView as CardHandViewController;
            return !controller.Hand [indexPath.Row].IsFaceDown;
        }
    }
}
```

如果当前项`true`可以处于`false`焦点，则方法返回;否则返回。`CanFocusItem`

如果你想要`UICollectionView`使用`UITableView`或来记住并在其失去和重新获得焦点时将焦点还原到最后一个项`RemembersLastFocusedIndexPath` ，请`true`将属性设置为。

<a name="Focus-and-Parallax" />

## <a name="focus-and-parallax"></a>重心和视差

如上所述，当导航事件期间，用户界面元素成为当前项时，将视为_焦点_。 通常，当某个项目成为焦点时，它的大小会略微增加，并使用阴影进行可视化提升。 

如果用户在 Siri 遥控器上发出缓慢的循环手势，则该聚焦项会实时地响应此移动。 在 sway 发生时，会将发光的 sheen 应用于其图像，使表面看起来像是发光。 在给定的非活动状态之后，任何焦点不变的内容会变暗，并且重点项会增长得更大。 

这些效果协同工作以在电视屏幕上的内容与来自沙发的 Apple TV 的用户之间提供精神连接。

在 Apple TV 上，此视差效果在整个系统中使用，以将三维深度和 dynamics 传达给重点项。 tvOS 使用一系列透明、[分层的图像](~/ios/tvos/app-fundamentals/icons-images.md#Layered-Images)，该图像可动态移动和缩放以创建此效果。

需要对 tvOS 应用程序的图标进行分层映像，并支持动态的顶层内容。 虽然不是必需的，但 Apple 强烈建议在应用 UI 中的任何其他可设定焦点的项目中实现分层映像。

### <a name="enabling-parallax"></a>启用视差

控件（或继承自`UIImageView`的任何控件）会自动支持视差效果。 `UIImageView` 默认情况下，此支持处于禁用状态，若要启用它，请使用以下代码：

```csharp
myImageView.AdjustsImageWhenAncestorFocused = true;
```

如果将此属性设置`true`为，则图像视图将在用户和焦点选择时自动获得视差效果。

<a name="Summary" />

## <a name="summary"></a>总结

本文介绍了焦点的概念以及如何使用它来处理 Xamarin tvOS 应用的用户界面中的导航。 它将检查内置 tvOS 导航控件如何使用焦点、突出显示和选择以提供导航。 接下来，了解如何通过视差和分层图像使用焦点，为最终用户提供当前导航状态的视觉线索。 最后，它研究了如何使用焦点、重点更新、集中精力和启用视差。

## <a name="related-links"></a>相关链接

- [tvOS 示例](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS 人体学接口指南](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS 应用编程指南](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
