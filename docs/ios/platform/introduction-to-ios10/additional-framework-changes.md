---
title: 其他 iOS 10 框架更改
description: 本文档描述细微的更改和对现有框架在 iOS 10 中所做的增强功能，并讨论了如何使在 Xamarin.iOS 这些更新的使用。
ms.prod: xamarin
ms.assetid: 0E2217F1-FC96-4D0A-ABAB-D40AD8F96502
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/29/2017
ms.openlocfilehash: 4b9a230157593b66446e2949e57a925d94208752
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787554"
---
# <a name="additional-ios-10-frameworks-changes"></a>其他 iOS 10 框架更改

_本文介绍了其他的次要更改或适用于 iOS 10 现有框架的增强功能。_

## <a name="av-foundation-framework-additions"></a>AV Foundation Framework 添加件

AVFoundation framework 包括以下增强功能：

- 在 iOS 10 中，开发人员不再具有实现不同[AVPlayerItem](https://developer.xamarin.com/api/type/AVFoundation.AVPlayerItem/)行为基于内容的类型。 只需设置`Rate`属性和 AVFoundation 将确定足够的内容何时可用于播放而无需停滞。
- 新[AVCapturePhotoOutput](https://developer.xamarin.com/api/type/AVFoundation.AVCaptureFileOutput/)类替换不推荐使用`AVCaptureStillImageOutput`类，并提供用于通过提供复杂的控制和监视捕获进程处理所有摄影工作流的统一的方法和支持新功能，如 Live 照片和原始捕获格式。
- 新`AVPlayerLooper`类，更便于在播放期间循环一段给定的媒体。
- `AVAssetDownloadURLSession`类允许在下载和更高版本播放 FairPlay 加密 HLS 流。
- 默认情况下， [AVCaptureSession](https://developer.xamarin.com/api/type/AVFoundation.AVCaptureSession/)类自动支持宽颜色、 宽色域捕获时的设备硬件支持它。 请参阅 Apple 的[iOS 设备兼容性参考](https://developer.apple.com/library/prerelease/content/documentation/DeviceInformation/Reference/iOSDeviceCompatibility/Introduction/Introduction.html#//apple_ref/doc/uid/TP40013599)有关详细信息。

## <a name="avkit-additions"></a>AVKit 添加件

AVKit framework 现在包括新`UpdatesNowPlayingInfoCenter`属性以指示何时应更新现在游戏信息中心。

## <a name="core-data-enhancements"></a>核心数据增强功能

iOS 10 包括以下增强功能的核心数据 framework:

- [NSManagedObjectContext](https://developer.xamarin.com/api/type/CoreData.NSManagedObjectContext/) SQLite WAL 日志模式支持新的查询生成的数据存储对象功能可以固定到将来提取特定的数据库版本的管理对象上下文 （模式） 的位置和出错的事务。
- 根[NSManagedObjectContext](https://developer.xamarin.com/api/type/CoreData.NSManagedObjectContext/)对象支持并发出错和提取不带序列化。
- [NSPersistentStoreCoordinator](https://developer.xamarin.com/api/type/CoreData.NSPersistentStoreCoordinator/)类维护 SQLite 数据存储区池。
- 几个新的简便方法已添加到`NSManagedObject`使其更轻松地执行提取和创建子类。
- 使用高级`NSPersistenceContainer`引用`NSPersistentStoreCoordinator`， [NSManagedObjectModel](https://developer.xamarin.com/api/type/CoreData.NSManagedObjectModel/)和其他核心数据配置资源。

有关详细信息，请参阅 Apple 的[核心数据 Framework 参考](https://developer.apple.com/reference/coredata)。

## <a name="core-image-enhancements"></a>核心映像增强功能

iOS 10 使到 Core 映像 framework 以下增强功能：

- 开发人员现在可以通过将转换入和移出的色彩空间之前和之后处理处理 Core 映像上下文工作颜色空间的外部颜色空间中的映像。
- 对于使用 A8 或 A9 Cpu 的 iOS 设备，现在支持原始图像格式。 从原始图像解码内置 iSight 照相机或从第三方照相机，core 映像现在提供支持。 使用`FilterWithImageData`或`FilterWithImageURL`方法[CIFilter](https://developer.xamarin.com/api/type/CoreImage.CIFilter/)类来处理原始图像。
- 对进行了多项的呈现性能改进`UIImage`中呈现 （时由 Core 映像映像存储区支持）`UIImageView`对象。 
- `UIImage` 对象标记的宽色域将呈现为宽色域中的颜色`UIImageView`支持广泛的颜色的 iOS 设备上的对象。
- 核心映像内核代码现在可以请求特定像素输出格式。
- `ImageWithExtent`方法[CIFilter](https://developer.xamarin.com/api/type/CoreImage.CIFilter/)类可以用于将自定义处理插入到该筛选器操作。 Core 映像将调用给定的回调之间筛选器时处理输出的映像，或显示。

此外，已添加以下新 Core 映像筛选器：

- `CINinePartTiled`
- `CINinePartStretched`
- `CIHueSaturationValueGradient`
- `CIEdgePreserveUpsampleFilter`
- `CIClamp`

## <a name="core-motion-additions"></a>核心运动添加件

新的核心运动 framework 到 iOS 10，包括启用应用以接收快速、 暂停和恢复跟踪运行时的用户的实时通知计步器事件。 使用[CMPedometer](https://developer.xamarin.com/api/type/CoreMotion.CMPedometer/)以注册前景色或背景计步器事件。

## <a name="foundation-enhancements"></a>Foundation 增强功能

IOS 10 的 Foundation framework 进行了以下增强功能：

- 使用新[NSMeasurementFormatter](https://developer.apple.com/reference/foundation/nsmeasurementformatter)类来设置用于向最终用户显示本地化的度量值的格式。
- 使用新[NSDateInterval](https://developer.apple.com/reference/foundation/nsdateinterval)类负责使如持续时间，用于比较的时间间隔和测试的间隔交集的日期和时间间隔计算。
- 使用新[NSMeasurement](https://developer.apple.com/reference/foundation/nsmeasurement)类转换之间不同单位的度量值 (UOM) 或在不同 UOMs 值执行计算。

- 使用新[NSUnit](https://developer.apple.com/reference/foundation/nsunit)和[NSDimension](https://developer.apple.com/reference/foundation/nsdimension)类用于表示特定 UOMs。
- 多个新属性已添加到[NSLocal](https://developer.apple.com/reference/foundation/nslocale)类来获取本地信息和可用的显示格式。

## <a name="gamekit-enhancements"></a>GameKit 增强功能

在 iOS 10 中的 GameKit framework 进行了以下增强功能：

- **游戏中心应用**已弃用并从 iOS 中删除。 如果应用使用 GameKit，它_必须_提供其自己的界面，以显示 GameKit 功能，例如排名，等等。 
- 新的仅 iCloud 帐户类型由[GKCloudPlayer](https://developer.apple.com/reference/gamekit/gkcloudplayer)类。
- 新[GKGameSession](https://developer.apple.com/reference/gamekit/gkgamesession)类提供通用的解决方案，用于管理在游戏中心上的永久数据存储区。 `GKGameSession` 维护的玩家列表，并应用程序负责实现如何以及何时参与者日期是存储、 检索或播放器之间交换。 在许多情况下游戏会话可以替换现有的基于轮次的匹配项、 实时匹配或永久性方法保存的游戏。

## <a name="gameplaykit-enhancements"></a>GameplayKit 增强功能

在 iOS 10 中的 GameplayKit framework 进行了以下增强功能：

- 使用新[GKMeshGraph](https://developer.apple.com/reference/gameplaykit/gkmeshgraph)类以提供高性能、 自然查找的路径。
- 过程干扰生成已添加，并且可以用于增强自然查找纹理中的要增强真实性，向相机动作数添加要增强真实性并帮助生成丰富游戏世界。
- 使用空间分区进行分区的游戏世界数据的高效搜索。
- 新的 Monte Carlo 规划师 ([GKMonteCarloStrategist](https://developer.apple.com/reference/gameplaykit/gkmontecarlostrategist)) 已添加了对详尽的可能移动计算。
- 三维支持已添加到现有的代理和使用新的路径查找行为[GKAgent3D](https://developer.apple.com/reference/gameplaykit/gkagent3d)和[GKGraphNode3D](https://developer.apple.com/reference/gameplaykit/gkgraphnode3d)类。
- 新[GKScene](https://developer.apple.com/reference/gameplaykit/gkscene)和[GKSKNodeComponent](https://developer.apple.com/reference/gameplaykit/gksknodecomponent)组合 GameplayKit 和 SpriteKit 比以前更容易的类生成。
- 已添加新的决策树 API ([GKDecisionTree](https://developer.apple.com/reference/gameplaykit/gkdecisiontree)和[GKDecisionNode](https://developer.apple.com/reference/gameplaykit/gkdecisionnode)) 来增强游戏构建 AI。

## <a name="healthkit-enhancements"></a>HealthKit 增强功能

在 iOS 10 中的 HealthKit framework 进行了以下增强功能：

- 新的元数据密钥已添加的天气类型 (如`HKWeatherConditionClear`和`HKWeatherConditionCloudy`) 和锻炼类型 (如`HKWorkoutActivityTypeFlexibility`和`HKWorkoutActivityTypeWheelchairRunPace`) 已添加。
- 新`HKCDADocument`已添加类来表示临床文档体系结构 (CDA) 格式化文档。
- 使用新[HKWorkoutConfiguration](https://developer.apple.com/reference/healthkit/hkworkoutconfiguration)类指定`ActivityType`和`LocationType`的测验。
- 新[HKWheelchairUseObject](https://developer.apple.com/reference/healthkit/hkwheelchairuseobject)和`WheelchairUse`方法[HKHealthStore](https://developer.apple.com/reference/healthkit/hkhealthstore)类中添加了使用轮椅与相关的运行状况数据。

## <a name="homekit-enhancements"></a>HomeKit 增强功能

在 iOS 10 中的 HomeKit framework 进行了以下增强功能：

- 已添加新的服务和特征。
- IPad 可以配置为充当一个 HomeKit 集线器，以提供远程附件的访问权限，运行自动化触发器和启用共享的用户权限。
- 已添加了对摄像头和门铃附件的支持。
- 具有针对附件提供了更多上下文和配置。

请参阅我们[简介 HomeKit](~/ios/platform/homekit.md)文档以了解更多信息。

## <a name="metal-enhancements"></a>裸机增强功能

在 iOS 10 中的裸机 framework 进行了以下增强功能：

- 三维应用程序和游戏现在可以使用_分割_以有效地呈现复杂的情形和通过 GPU 的几何图形。
- 提供的资源分配，以优化性能的裸机细化的控制基于堆使用资源的应用和 Memoryless 呈现目标。
- 使用函数专用化来创建高度优化材料和场景浅色组合函数的集合。

若要了解详细信息，请参阅 Apple 的[金属编程指南](https://developer.apple.com/library/prerelease/content/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014221)。

## <a name="modelio-enhancements"></a>ModelIO 增强功能

在 iOS 10 中的 ModelIO framework 进行了以下增强功能：

 - 现在支持价格 （美元） 文件格式。
 - 签名支持已添加到距离字段[MDLVoxelArray](https://developer.apple.com/reference/modelio/mdlvoxelarray)类。
 - 使用新`MDLLightProbeIrradianceDataSource`类，用于帮助在 Light 探测放置。
 - 使用新`MDLMaterialPropertyGraph`类，以轻松地支持运行时更改为模型。

## <a name="photos-enhancements"></a>照片增强功能

在 iOS 10 中的照片 framework 进行了以下增强功能：

- 使用[CIImageProcessorInput](https://developer.apple.com/reference/coreimage/ciimageprocessorinput)和[CIImageProcessorOutput](https://developer.apple.com/reference/coreimage/ciimageprocessoroutput)类以利用新的核心映像处理器功能，用于执行编辑。
- 实时照片编辑现可用于支持照片框架的应用程序的编辑扩展的照片 (用于内部的照片和相机应用)。
- 使用新[PHLivePhotoEditingContext](https://developer.apple.com/reference/photos/phlivephotoeditingcontext)类来编辑同时适用于视频和仍的 Live 照片内容。

## <a name="replaykit-enhancements"></a>ReplayKit 增强功能

在 iOS 10 中的 ReplayKit framework 进行了以下增强功能：

- 使用[RPScreenRecorder](https://developer.apple.com/reference/replaykit/rpscreenrecorder)， [RPBroadcastActivityViewController](https://developer.apple.com/reference/replaykit/rpbroadcastactivityviewcontroller)和[RPBroadcastController](https://developer.apple.com/reference/replaykit/rpbroadcastcontroller)类以支持的广播记录通过第三方媒体站点。
- 在应用程序支持 ReplayKit 第三方广播的服务所需的广播 UI 和广播上载扩展。

## <a name="scenekit-enhancements"></a>SceneKit 增强功能

在 iOS 10 中的 SceneKit framework 进行了以下增强功能：

- [SCNCamera](https://developer.xamarin.com/api/type/SceneKit.SCNCamera/)类可以通过使用 HDR 功能和影响提供更大要增强真实性。 自适应公开用于创建自动效果或使用晕影、 色边和颜色评分为游戏添加 fillmatic 效果。
- SceneKit 现在包括一个新的以物理方式基于呈现 (PBR) 系统，用于更简单资产创作更加真实地模拟结果。
- 使用新[SCNLightingModelPhysicallyBased](https://developer.apple.com/reference/scenekit/scnlightingmodelphysicallybased)明暗度模型来产品各种现实底纹效果时需要仅三个基本属性 (`Diffuse`，`Metalness`和`Roughness`)。
- 自 PBR 了基于环境的照明最底纹工作原理，使用`LightingEnvironment`要分配给整个场景的基于映像的照明属性。
- 使用`IESProfileURL`要导入定义照明的实际照明装置属性基于强度 （在流明） 和 （以度为单位开氏） 的颜色温度之类的实际值。
- PBR 和 HDR 相机功能提供更好的结果比传统的呈现技术，并因此，SceneKit 现在的执行所有颜色计算线性颜色空间 （在宽颜色设备显示使用 P3 的颜色域）。
- SceneKit 现在颜色匹配的所有颜色通过阅读的颜色配置文件信息。
- SceneKit 解释着色器的所有类型的线性 RGB 颜色空间中的颜色组件值。
- 同时线性颜色空间呈现，可以通过指定禁用宽颜色`SCNDisableLinearSpaceRendering`和`SCNDisableWideGamut`中应用的键`Info.plist`。
- 生成任意多边形 primates （不管是从文件加载或通过编程方式生成） 来指定与新的几何图形[SCNGeometryPrimitiveTypePolygon](https://developer.apple.com/reference/scenekit/1772322-scenekit_enumerations/scngeometryprimitivetype/scngeometryprimitivetypepolygon)类。
- 由于 SceneKit 读取，并调整纹理映像中颜色配置文件信息，请使用为所有图像资产目录以确保提供此信息。

## <a name="spritekit-enhancements"></a>SpriteKit 增强功能

在 iOS 10 中的 SpriteKit framework 进行了以下增强功能：

- 自定义的着色器可以提供属性 (`SKAttribute`)，可单独配置通过使用提供的属性值的着色器的每个节点 (`SKAttributeValue`)。
- Tilemaps 现在支持针对 2D、 2.5 D 和使用端滚动游戏正方形、 六边形和等轴磁贴形状`SKTileMapMode`， `SKTileGroup`，`SKTileGroupRule`和`SKTileSet`类。
- 使用新`SKWarpGeometry`类拉伸或扭曲[SKSpriteNode](https://developer.xamarin.com/api/type/SpriteKit.SKSpriteNode/)或[SKEffectNode](https://developer.xamarin.com/api/type/SpriteKit.SKEffectNode/)呈现。 新[SKAction](https://developer.xamarin.com/api/type/SpriteKit.SKAction/)类可以用于进行动画处理 warp 效果之间的转换。
- [SKView](https://developer.xamarin.com/api/type/SpriteKit.SKView/)类提供了若干新方法，以便对何时以及如何呈现场景细化的控制。

## <a name="scrollview-enhancements"></a>ScrollView 增强功能

对在 iOS 10.3 ScrollView 控件进行以下增强功能：

- `UIScrollView` 现在包括`IndexDisplayMode`属性来控制时用户滚动作为索引的显示方式`UIScrollViewIndexDisplayMode`的：
    - `Automatic` -索引显示由操作系统控制。
    - `AlwaysHidden` 始终隐藏-索引显示。

请参阅[iOSTenThree 示例](https://developer.xamarin.com/samples/monotouch/iOS10/iOSTenThree)的用法。

## <a name="uikit-enhancements"></a>UIKit 增强功能

在 iOS 10 中的 UIKit framework 进行了以下增强功能：

- 新[UIPasteboard](https://developer.xamarin.com/api/type/UIKit.UIPasteboard/) API 提供新的选项 （如生存期限制），将自动声明为公共的类类型兼容的内容类型。
- 新的完整交互式的、 基于对象的、 可中断动画支持已添加，并且可以链接到手势。 请参阅 Apple [UIViewAnimating 协议参考](https://developer.apple.com/reference/uikit/uiviewanimating)， [UIViewPropertyAnimator 类引用](https://developer.apple.com/reference/uikit/uiviewpropertyanimator)， [UITimingCurveProvider 协议参考](https://developer.apple.com/reference/uikit/uitimingcurveprovider)， [UICubicTimingParameters 类引用](https://developer.apple.com/reference/uikit/uicubictimingparameters)和[UISpringTimingParameter 类引用](https://developer.apple.com/reference/uikit/uispringtimingparameters)有关详细信息。
- 新`UIPreviewInteraction`和`UIPreviewInteractionDelegate`允许开发人员应用程序以查看和 pop 操作提供的自定义的接口。
- 新`UIAccessibilityCustomRotor`类允许该应用程序提供与辅助技术，如语音通过自定义的、 特定于上下文的功能。
- 使用`UIAccessibilityIsAssistiveTouchRunning`和`UIAccessibilityAssistiveTouchStatusDidChangeNotification`符号以确定是否启用 AssistiveTouch。
- 使用`UIAccessibilityHearingDevicePairedEar`和`UIAccessibilityHearingDevicePairedEarDidChangeNotification`符号来获取的任何状态配对 MFi 听力帮助。
- 若要在标签中支持的动态类型，文本字段和文本框使用新`PreferredFontForTextStyle`方法`UIFont`类。
- 若要确定元素应更新其字体时设备的`UIContentSizeCategory`更改，将使用`AdjustsFontForContentSizeCategory`属性`UIContentSizeCategoryAdjusting`委托。
- `OpenURL`方法`UIApplication`类以异步方式调用，并且现在支持打开操作完成后调用完成处理程序。
- 启动 CloudKit 共享和修改其属性使用新`UICloudSharingController`和`UICloudSharingControllerDelegate`类。
- 充分利用预提取的单元格，若要改善的滚动体验`UICollectionViews`与新`UICollectionViewDataSourcePrefetching`委托。
- 开发人员现可控制选项卡栏项 （例如文本和背景色） 该标记的外观。
- 刷新控件现在支持所有滚动视图和滚动视图子类中 (如`UICollectionView`)。

## <a name="webkit-enhancements"></a>易于使用的功能增强功能

在 iOS 10 中的 WebKit framework 进行了以下增强功能：

- 查看和 pop 支持添加到`WKWebView`类。 使用`ShouldPreviewElement`方法来确定给定的 web 视图应显示预览。


## <a name="related-links"></a>相关链接

- [iOS 10 示例](https://developer.xamarin.com/samples/ios/iOS10/)
- [什么是 iOS 10 中的新增功能](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewIniOS/Articles/iOS10.html#//apple_ref/doc/uid/TP40017084-SW1)
