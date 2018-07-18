---
title: 可以更改 IPA 文件的输出路径？
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: F5E5DCC6-F7CC-48E2-89E8-709E9C269502
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 9c80a209279a2f032eb6c9efcba1398ca0e267a5
ms.sourcegitcommit: dc882e9631b4ed52596b944a6fbbdde309346943
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/26/2018
ms.locfileid: "31882797"
---
# <a name="can-i-change-the-output-path-of-the-ipa-file"></a>可以更改 IPA 文件的输出路径？

## <a name="for-cycle-7-and-higher"></a>对于周期 7 和更高版本
是，你可以使用自定义的 MSBuild 目标来实现此目的。 最简单的选项可能是要复制`.ipa`文件后已生成了它。

在 Mac 或 Windows 上使用 MSBuild 生成引擎任何 iOS 项目，这些步骤将起作用。 (注意： 所有统一 API 项目使用 MSBuild 生成引擎。)

1. 打开`.csproj`文件，以在文本编辑器中的 iOS 应用项目，然后再在末尾添加以下行 (在关闭之前立即`</Project>`标记):
    
    ```
    <PropertyGroup>
           <CreateIpaDependsOn>
           $(CreateIpaDependsOn);
            CopyIpa
           </CreateIpaDependsOn>
    </PropertyGroup>
    
    <Target Name="CopyIpa"
        Condition="'$(OutputType)' == 'Exe'
            And '$(ComputedPlatform)' == 'iPhone'
            And '$(BuildIpa)' == 'true'">
        <Copy
            SourceFiles="$(IpaPackagePath)"
            DestinationFolder="$(OutputPath)"
        />
    </Target>
    ```

2. 设置为所需的输出文件夹 DestinationFolder。 像往常一样，你可以使用 MSBuild 属性 （如在此参数，如果你想 $(OutputPath))。

## <a name="notes"></a>说明
- `CreateIpaDependsOn`中定义属性`Xamarin.iOS.Common.targets`Xamarin.iOS 的一部分的文件。 其行为下所述*重写 DependsOn 属性*上[ https://msdn.microsoft.com/library/ms366724.aspx ](https://msdn.microsoft.com/library/ms366724.aspx)。

- 你可以使用**移动**任务而不是**复制**任务如果您的首选。 如果你选择选项，构建在 Windows 上，你将需要使用完全限定的任务名称`<Microsoft.Build.Tasks.Move>`以避免多义性 XamarinVS 与生成任务。

## <a name="for-versions-before-xamarin-studio-6005174--xamarin-for-visual-studio-410530"></a>有关 Xamarin Studio 6.0.0.5174 之前的版本 |Xamarin for Visual Studio 4.1.0.530

是，你可以使用自定义的 MSBuild 目标来实现此目的。 最简单的选项可能是要复制`.ipa`文件后已生成了它。

在 Mac 或 Windows 上使用 MSBuild 生成引擎任何 iOS 项目，这些步骤将起作用。 (注意： 所有统一 API 项目使用 MSBuild 生成引擎。)

1. 打开`.csproj`文件，以在文本编辑器中的 iOS 应用项目，然后再在末尾添加以下行 (在关闭之前立即`</Project>`标记)。

    ```csharp
    <PropertyGroup>
        <CreateIpaDependsOn>
            $(CreateIpaDependsOn);
            CopyIpa
        </CreateIpaDependsOn>
    </PropertyGroup>
    
    <Target Name="CopyIpa"
        Condition="'$(OutputType)' == 'Exe'
            And '$(ComputedPlatform)' == 'iPhone'
            And '$(BuildIpa)' == 'true'">
        <Copy
            SourceFiles="$(OutputPath)$(IpaPackageName)"
            DestinationFolder="/Users/macuser/Desktop/"
        />
    </Target>
    ```

2. 设置`DestinationFolder`到所需的输出文件夹。 像往常一样，你可以使用 MSBuild 属性 (如`$(OutputPath)`) 如果您希望此自变量中。

## <a name="notes"></a>说明
- `CreateIpaDependsOn`中定义属性`Xamarin.iOS.Common.targets`Xamarin.iOS 的一部分的文件。 其行为下所述*重写"DependsOn"属性*上[ https://msdn.microsoft.com/library/ms366724.aspx ](https://msdn.microsoft.com/library/ms366724.aspx)。

- 你可以使用**移动**任务而不是**复制**任务如果您的首选。 如果你选择选项，构建在 Windows 上，你将需要使用完全限定的任务名称`<Microsoft.Build.Tasks.Move>`以避免多义性 XamarinVS 与生成任务。
