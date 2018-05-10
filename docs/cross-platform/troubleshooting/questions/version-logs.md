---
title: 在哪里可以找到我的版本信息和日志？
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: CF386485-EAB0-4B9E-AA17-CB1B6462E505
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 9234ee0553b2cc9376c0e4e39ffc0700deaacda1
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/09/2018
---
# <a name="where-can-i-find-my-version-information-and-logs"></a>在哪里可以找到我的版本信息和日志？

## <a name="outline"></a>轮廓

- [版本信息](#version-information)
    - Windows 版本信息
    - Mac 版本信息
    - Android SDK 工具，平台工具，生成工具
- [IDE 和安装程序日志](#ide-and-installer-logs)
    - [Windows 日志](#windows-logs)
        - Xamarin Studio
        - 面向 Visual Studio 的 Xamarin
        - Xamarin 世界安装程序
        - 单个`.msi`安装程序、 详细日志
        - Visual Studio 启动时，详细日志
    - [Mac 日志](#mac-logs)
        - 生成主机
    - Visual Studio for Mac
        - Xamarin Studio
        - Xamarin 安装程序
- [详细的生成输出](#verbose-build-output-logs)
- [调试对于 Xamarin.Android 和 Xamarin.iOS 应用程序日志](#debug-logs-for-xamarin-apps)
    - Android `adb` logcat 日志
    - iOS 模拟器登录 （Mac)
    - iOS 设备登录 （Mac)

## <a name="a-idversion-information-nameversion-information-version-information"></a><a id="version-information" name="version-information" />版本信息

它通常是最大努力发送恢复中的信息的全部**复制信息**按钮。 否则，我们通常需要请求的其他信息。 例如，操作系统版本，Xcode 版本安装 Android API 级别和.NET 版本都可以是重要时帮助解决问题。

### <a name="a-idwindows-version-information-namewindows-version-information-windows-version-information"></a><a id="windows-version-information" name="windows-version-information" />Windows 版本信息

#### <a name="xamarin-studio"></a>Xamarin Studio

**帮助 > 有关 > 显示详细信息 > 复制信息 [按钮]**

#### <a name="visual-studio"></a>Visual Studio

**帮助 > 有关 Microsoft Visual Studio > 复制信息 [按钮]**

### <a name="a-idmac-version-information-namemac-version-information-mac-version-information"></a><a id="mac-version-information" name="mac-version-information" />Mac 版本信息

#### <a name="visual-studio-for-mac"></a>Visual Studio for Mac

**Visual Studio > 有关 Visual Studio > 显示详细信息 > 复制信息 [按钮]**

### <a name="a-idandroid-sdk-tools-versions-nameandroid-sdk-tools-versions-android-sdk-tools-platform-tools-build-tools"></a><a id="android-sdk-tools-versions" name="android-sdk-tools-versions" />Android SDK 工具，平台工具，生成工具

打开 Android SDK 管理器中，和顶部的屏幕截图**工具**部分。

![](https://kb.xamarin.com/customer/portal/attachments/337323 "屏幕快照的 Android SDK 管理器 > 工具文件夹")

#### <a name="visual-studio-for-mac"></a>Visual Studio for Mac

“工具”>“打开 Android SDK 管理器”

#### <a name="visual-studio"></a>Visual Studio

选择 SDK 管理器工具栏图标：

![](https://kb.xamarin.com/customer/portal/attachments/420238 "Visual Studio 中启动 Android SDK 管理器工具栏图标")

## <a name="a-idide-and-installer-logs-nameide-and-installer-logs-ide-and-installer-logs"></a><a id="ide-and-installer-logs" name="ide-and-installer-logs" />IDE 和安装程序日志

对于每个日志位置，请确保压缩，然后附加整个日志文件夹。

### <a name="a-idwindows-logs-namewindows-logs-windows-logs"></a><a id="windows-logs" name="windows-logs" />Windows 日志

#### <a name="a-idwindows-logs-xamarin-vs-namewindows-logs-xamarin-vs--visual-studio-tools-for-xamarin"></a><a id="windows-logs-xamarin-vs" name="windows-logs-xamarin-vs" /> Xamarin 的 visual Studio 工具

`%LOCALAPPDATA%\Xamarin\Logs`

#### <a name="a-idvs-2017-namevs-2017--visual-studio-2017"></a><a id="vs-2017" name="vs-2017" /> Visual Studio 2017

[如何获取 Visual Studio 安装日志](https://docs.microsoft.com/visualstudio/install/troubleshooting-installation-issues#how-to-get-the-visual-studio-installation-logs)

#### <a name="a-idvs-2015-namevs-2015--visual-studio-2015"></a><a id="vs-2015" name="vs-2015" /> Visual Studio 2015

#### <a name="a-idwindows-universal-installer-namewindows-universal-installer--xamarin-universal-installer"></a><a id="windows-universal-installer" name="windows-universal-installer" /> Xamarin"通用"安装程序

`%LOCALAPPDATA%\Xamarin\Universal`

这些是从日志`XamarinInstaller.exe`安装程序。

#### <a name="a-idindividual-msi-installers-verbose-logs-nameindividual-msi-installers-verbose-logs-individual-msi-installers-verbose-logs"></a><a id="individual-msi-installers-verbose-logs" name="individual-msi-installers-verbose-logs" />单个`.msi`安装程序、 详细日志

```csharp
msiexec /i Xamarin.msi /l*vx "%USERPROFILE%\Desktop\Xamarin.log"
```

参考：[命令行选项](http://msdn.microsoft.com/library/aa367988.aspx)

#### <a name="a-idvisual-studio-startup-verbose-logs-namevisual-studio-startup-verbose-logs-visual-studio-startup-verbose-logs"></a><a id="visual-studio-startup-verbose-logs" name="visual-studio-startup-verbose-logs" />Visual Studio 启动时，详细日志

```csharp
devenv.exe /log "%USERPROFILE%\Desktop\VisualStudio.log"
```

参考： [/Log (devenv.exe)](http://msdn.microsoft.com/library/ms241272.aspx)

### <a name="a-idmac-logs-namemac-logs-mac-logs"></a><a id="mac-logs" name="mac-logs" />Mac 日志

你可以选择**转 > 转到文件夹**菜单项在查找工具中，然后复制并将这些路径的任何粘贴到对话框。

#### <a name="a-idmac-logs-visual-studio-namemac-logs-visual-studio-visual-studio-for-mac"></a><a id="mac-logs-visual-studio" name="mac-logs-visual-studio" />Visual Studio for Mac

`~/Library/Logs/VisualStudio/7.0` （此数字可能更改具体取决于正在使用的版本）

此文件夹也可打开通过"帮助-> 打开日志目录"。

#### <a name="a-idmac-logs-xamarin-studio-namemac-logs-xamarin-studio-xamarin-studio"></a><a id="mac-logs-xamarin-studio" name="mac-logs-xamarin-studio" />Xamarin Studio

`~/Library/Logs/XamarinStudio-6.0` （此数字可能更改具体取决于正在使用的版本）

此文件夹也可打开通过"帮助-> 打开日志目录"。

#### <a name="a-idmac-universal-installer-namemac-universal-installer-xamarin-universal-installer"></a><a id="mac-universal-installer" name="mac-universal-installer" />Xamarin"通用"安装程序

`~/Library/Logs/XamarinInstaller/Universal`

这些是从日志`XamarinInstaller.dmg`安装程序。

#### <a name="a-idmac-build-host-namemac-build-host-xamarin-build-host"></a><a id="mac-build-host" name="mac-build-host" />Xamarin 生成主机

`~/Library/Logs/Xamarin-[MAJOR.MINOR]`

## <a name="a-idverbose-build-output-logs-nameverbose-build-output-logs-verbose-build-output"></a><a id="verbose-build-output-logs" name="verbose-build-output-logs" />详细的生成输出

1.  启用[诊断 MSBuild 输出](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output)。

2.  对于 iOS 应用，也启用**详细 mtouch 输出**通过添加`-v -v -v -v`下**项目属性 > iOS 生成 > 常规 [选项卡] > 其他选项 > 其他 mtouch 参数**。

    **Visual Studio (Windows)**

    [![](https://kb.xamarin.com/customer/portal/attachments/337319 "其他 mtouch 参数输入字段中输入四个短划线 v")](https://kb.xamarin.com/customer/portal/attachments/337319)

    **Visual Studio for Mac**

    [![](https://kb.xamarin.com/customer/portal/attachments/337316 "其他 mtouch 参数输入字段中输入四个短划线 v")](https://kb.xamarin.com/customer/portal/attachments/337316)

3.  清除并重新生成项目。

4.  复制并粘贴到文本文件从 IDE 生成输出。
     - Visual Studio (Windows):**视图 > 输出 > 显示输出来源： 生成**
     - Visual Studio for Mac:**视图 > 填充 > 错误 > 生成输出 [选项卡]**

## <a name="a-iddebug-logs-for-xamarin-apps-namedebug-logs-for-xamarin-apps-debug-logs-for-xamarinandroid-and-xamarinios-apps"></a><a id="debug-logs-for-xamarin-apps" name="debug-logs-for-xamarin-apps" />调试对于 Xamarin.Android 和 Xamarin.iOS 应用程序日志

### <a name="visual-studio-for-mac"></a>Visual Studio for Mac

**视图 > 填充 > 应用程序输出**

![](https://kb.xamarin.com/customer/portal/attachments/337290 "适用于 Mac 的 Visual Studio 中的应用程序输出板图标")


（请注意后启动应用程序将仅显示此菜单项。）

### <a name="visual-studio"></a>Visual Studio

**视图 > 输出 > 显示以下输出： 调试**

![](https://kb.xamarin.com/customer/portal/attachments/337292 "输出板显示了 Visual Studio 中调试选项")

### <a name="a-idadb-logcat-nameadb-logcat-android-adbhttpdeveloperandroidcomtoolshelpadbhtml-logcat-logs"></a><a id="adb-logcat" name="adb-logcat" />Android [ `adb` ](http://developer.android.com/tools/help/adb.html) logcat 日志

运行之后`adb`命令，将附加后**android_logcat.txt**文件从你的桌面。 这些说明假定你具有只有一个附加的设备。

另请参阅[Android 调试日志](~/android/deploy-test/debugging/android-debug-log.md)页。

#### <a name="visual-studio"></a>Visual Studio

1.  **工具 > Android > 启动 Android Adb 命令提示符**
2.  清除日志： `adb logcat -c`
3.  重现此问题。
4.  输出日志： `adb logcat -vtime -d > "%USERPROFILE%\Desktop\android_logcat.txt"`

#### <a name="visual-studio-for-mac"></a>Visual Studio for Mac

1.  **工具 > 打开 Android SDK 命令提示**
2.  清除日志： `adb logcat -c`
3.  重现此问题。
4.  输出日志： `adb logcat -vtime -d > ~/Desktop/android_logcat.txt`

### <a name="a-idios-simulator-logs-nameios-simulator-logs-ios-simulator-logs-on-mac"></a><a id="ios-simulator-logs" name="ios-simulator-logs" />iOS 模拟器登录 （Mac)

*   若要访问系统日志，选择**调试 > 打开系统日志...** 在 iOS 模拟器应用。

    ![](https://kb.xamarin.com/customer/portal/attachments/382617 "调试菜单显示打开的系统日志选项")

*   若要查看从模拟器的崩溃报告，打开 Console.app 并导航到`~/Library/Logs > DiagnosticReports`。

### <a name="a-idios-device-logs-nameios-device-logs-ios-device-logs-on-mac"></a><a id="ios-device-logs" name="ios-device-logs" />iOS 设备登录 （Mac)

#### <a name="visual-studio-for-mac"></a>Visual Studio for Mac

**视图 > 填充 > iOS 设备日志**

#### <a name="xcode"></a>Xcode 

**窗口 > 设备 > ${DeviceName}**

崩溃报告下有**查看设备日志**按钮。 系统日志中的设备显示在下泄露箭头的窗口的底部 <img alt="Disclosure arrow" src="https://kb.xamarin.com/customer/portal/attachments/382618" style="width: 15px; height: 12px;" />.

#### <a name="xcode-5"></a>Xcode 5

**窗口 > 管理器 > 设备 [选项卡] > ${DeviceName}**
