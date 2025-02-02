---
title: Xamarin 中的备用应用程序图标
description: 本文档介绍如何在 Xamarin 中使用替代的应用图标。 本文介绍如何将这些图标添加到 Xamarin iOS 项目，如何修改 info.plist 文件，以及如何以编程方式管理应用程序的图标。
ms.prod: xamarin
ms.assetid: 302fa818-33b9-4ea1-ab63-0b2cb312299a
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/29/2017
ms.openlocfilehash: d391c57c2c63cd4e371bd97ba455962aa053f9ed
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/06/2019
ms.locfileid: "70767347"
---
# <a name="alternate-app-icons-in-xamarinios"></a>Xamarin 中的备用应用程序图标

_本文介绍如何在 Xamarin 中使用替代的应用图标。_

Apple 向 iOS 10.3 添加了几项增强功能，使应用程序可以管理其图标：

- `ApplicationIconBadgeNumber`-获取或设置 Springboard 中应用程序图标的徽章。
- `SupportsAlternateIcons`-如果`true`应用程序具有一组备用图标，则为。
- `AlternateIconName`-返回当前选定`null`的备用图标的名称，如果使用主图标，则返回。
- `SetAlternameIconName`-使用此方法将应用的图标切换到给定的替代图标。

![](alternate-app-icons-images/icons04.png "应用更改图标时的示例警报")

<a name="Adding-Alternate-Icons" />

## <a name="adding-alternate-icons-to-a-xamarinios-project"></a>向 Xamarin iOS 项目添加替代图标

若要允许应用切换到备用图标，需要在 Xamarin iOS 应用项目中包含图标图像的集合。 不能使用典型`Assets.xcassets`方法将这些图像添加到项目中，它们必须直接添加到**Resources**文件夹中。

请执行以下操作：

1. 选择文件夹中所需的图标图像，选择 "全部"，并将其拖动到**解决方案资源管理器**中的 "**资源**" 文件夹中：

    ![](alternate-app-icons-images/icons00.png "从文件夹中选择图标图像")

2. 出现提示时，选择 "**复制**"，**对所有选定的文件使用相同的操作**，并单击 **"确定"** 按钮：

    ![](alternate-app-icons-images/icons02.png "\"将文件添加到文件夹\" 对话框")

3. "**资源**" 文件夹在完成后应如下所示：

    ![](alternate-app-icons-images/icons01.png "Resources 文件夹应如下所示")

<a name="Modifying-the-Info.plist-File" />

## <a name="modifying-the-infoplist-file"></a>修改 info.plist 文件

将所需的映像添加到**Resources**文件夹后，需要将[CFBundleAlternateIcons](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CoreFoundationKeys.html#//apple_ref/doc/uid/TP40009249-SW13)项添加到项目的**info.plist**文件中。 此密钥将定义新图标的名称和组成它的映像。

请执行以下操作：

1. 在“解决方案资源管理器”中，双击“Info.plist”文件，将其打开进行编辑。
2. 切换到 "**源**" 视图。
3. 添加 "**捆绑" 图标**键并将 "**类型**" 设置为 "**字典**"。
4. 添加密钥并将 "**类型**" 设置为 "字典"。 `CFBundleAlternateIcons`
5. 添加密钥并将 "**类型**" 设置为 "字典"。 `AppIcon2` 这将是新的备用应用图标集的名称。
6. 添加密钥并将**类型**设置为**数组** `CFBundleIconFiles`
7. 将一个新字符串添加到`CFBundleIconFiles`每个图标文件的数组中`@2x`，并保留扩展名和`@3x`、等后缀（例如`100_icon`）。 为组成备用图标集的每个文件重复此步骤。
8. `AppIcon2`向字典中添加键，将"类型"设置为"布尔值"，将"值"设置为"否"。`UIPrerenderedIcon`
9. 保存对文件所做的更改。

完成后，生成的**info.plist**文件应如下所示：

![](alternate-app-icons-images/icons03.png "已完成的 info.plist 文件")

例如，如果在文本编辑器中打开，则：

```xml
<key>CFBundleIcons</key>
<dict>
    <key>CFBundleAlternateIcons</key>
    <dict>
        <key>AppIcon2</key>
        <dict>
            <key>CFBundleIconFiles</key>
            <array>
                <string>100_icon</string>
                <string>114_icon</string>
                <string>120_icon</string>
                <string>144_icon</string>
                <string>152_icon</string>
                <string>167_icon</string>
                <string>180_icon</string>
                <string>29_icon</string>
                <string>40_icon</string>
                <string>50_icon</string>
                <string>512_icon</string>
                <string>57_icon</string>
                <string>58_icon</string>
                <string>72_icon</string>
                <string>76_icon</string>
                <string>80_icon</string>
                <string>87_icon</string>
            </array>
            <key>UIPrerenderedIcon</key>
            <false/>
        </dict>
    </dict>
</dict>
```

<a name="Managing-the-Apps-Icon" />

## <a name="managing-the-apps-icon"></a>管理应用的图标 

在 Xamarin 项目中包含图标映像和正确配置**info.plist**文件后，开发人员可以使用添加到 iOS 10.3 的许多新功能之一来控制应用的图标。

`UIApplication`类的属性允许开发人员查看应用是否支持替换图标。 `SupportsAlternateIcons` 例如：

```csharp
// Can the app select a different icon?
PrimaryIconButton.Enabled = UIApplication.SharedApplication.SupportsAlternateIcons;
AlternateIconButton.Enabled = UIApplication.SharedApplication.SupportsAlternateIcons;
```

`UIApplication`类的属性允许开发人员获取或设置 Springboard 中应用程序图标的当前徽章号。 `ApplicationIconBadgeNumber` 默认值为零 (0)。 例如:

```csharp
// Set the badge number to 1
UIApplication.SharedApplication.ApplicationIconBadgeNumber = 1;
```

`UIApplication`类的`null`属性允许开发人员获取当前选定的备用应用程序图标的名称，或者，如果应用程序使用主图标，则该属性返回。 `AlternateIconName` 例如:

```csharp
// Get the name of the currently selected alternate
// icon set
var name = UIApplication.SharedApplication.AlternateIconName;

if (name != null ) {
    // Do something with the name
}
```

`UIApplication`类`SetAlternameIconName`的属性允许开发人员更改应用图标。 传递图标的名称以选择或`null`返回到主图标。 例如：

```csharp
partial void UsePrimaryIcon (Foundation.NSObject sender)
{
    UIApplication.SharedApplication.SetAlternateIconName (null, (err) => {
        Console.WriteLine ("Set Primary Icon: {0}", err);
    });
}

partial void UseAlternateIcon (Foundation.NSObject sender)
{
    UIApplication.SharedApplication.SetAlternateIconName ("AppIcon2", (err) => {
        Console.WriteLine ("Set Alternate Icon: {0}", err);
    });
}
```

当应用程序运行时，如果用户选择备用图标，将显示如下所示的警报：

![](alternate-app-icons-images/icons04.png "应用更改图标时的示例警报")

如果用户切换回主图标，将显示如下所示的警报：

![](alternate-app-icons-images/icons05.png "应用更改为主图标时的示例警报")

<a name="Summary" />

## <a name="summary"></a>总结

本文介绍了如何向 Xamarin iOS 项目添加备用应用程序图标并在应用程序中使用它们。

## <a name="related-links"></a>相关链接

- [iOSTenThree 示例](https://docs.microsoft.com/samples/xamarin/ios-samples/ios10-iostenthree/)
