---
title: 应用程序用户界面本地化
description: 本文档介绍国际化和本地化的跨平台概念，并调查它们对应用程序设计的影响。
ms.prod: xamarin
ms.assetid: CC6847B2-23FB-4EDE-9F7E-EF29DD46A5C5
author: conceptdev
ms.author: crdun
ms.date: 03/22/2017
ms.openlocfilehash: b7dfeee92020be2fb40cfdfc5eb1b97d065b97e9
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/06/2019
ms.locfileid: "70758175"
---
# <a name="localization"></a>本地化

本指南介绍了*国际化*和*本地化*背后的概念，并提供了有关如何使用这些概念生成 Xamarin 移动应用程序的说明的链接。

如果要直接跳到本地化 Xamarin 应用的技术细节，请从以下特定于平台的 "操作方法" 文章中选择一个：

- [**Xamarin. Forms**](~/xamarin-forms/app-fundamentals/localization/index.md)使用 RESX 文件的跨平台本地化。
- [**Xamarin**](~/ios/app-fundamentals/localization/index.md)本地平台本地化。
- [**Xamarin Android**](~/android/app-fundamentals/localization.md)本机平台本地化。

## <a name="i18n-and-l10n"></a>国际化和 L10n

*国际化*是使您的代码能够显示不同的语言并调整其在不同区域设置（如数字和日期格式）的显示的过程。 这也称为*全球化*。

*本地化*是后面的步骤–为每种语言创建资源（如字符串和图像）并将其与国际化应用捆绑。

国际化通常会缩短为 "i" 和 "n" 之间的18个字母的 "i18n"。 本地化类似于 L10n，在 "L" 和 "n" 之间有10个字母。

## <a name="overview"></a>概述

本文档介绍了与国际化和本地化相关的概念，以及如何将它们应用于一般的移动应用程序开发。
设计和构建应用程序时，以前可能已硬编码但必须为本地化参数化的内容包括：

- 屏幕布局和文本，
- 图标、图形和颜色，
- 视频和声音文件，
- 动态文本和文本格式（例如数字、货币和日期）、
- 从右到左（RTL）语言和的布局更改
- 数据排序。

无论你的应用面向哪个移动平台，这些提示都有助于你构建高质量的本地化应用程序。

## <a name="design-considerations"></a>设计注意事项

构建应用程序，以便可以将其内容本地化称为国际化。 正确使用国际化不只是允许在运行时加载不同语言字符串-设计良好的应用程序应允许根据语言和区域设置（包括图像、声音和视频）更改所有资源，并可以调整用于应对不同大小字符串的格式设置和布局。

本部分讨论生成国际化应用程序时要考虑的一些设计注意事项。

### <a name="layouts-and-string-length"></a>布局和字符串长度

中文和日语字符串可以非常简短–有时，一个或两个字符对于输入字段标签有足够的意义。

德语字符串（例如）可能非常长;有时，在其他语言中，相对较短的英语单词就会变得非常长-裁剪或意外 reflowing 您的布局。

将 iOS 主屏幕上几项的字符串长度与英语、德语和日语进行比较：

[![](localization-images/language-compare-sml.png "德语和日语字符串长度")](localization-images/language-compare.png#lightbox)

请注意，英语（8个字符）的**设置**需要德语转换的13个字符，但在日语中只需要2个字符。

当标签长度相差很大时，显示标签和输入字段并行的布局很难使用。 通常，标签在字段上方显示的布局更易于本地化，因为屏幕的整个宽度都可用于标签和输入。

一般规则是，如果要生成固定布局（特别是并排元素），则允许的宽度至少为 50%，大于你的英语字符串需要的标签和文本。 这并不能解决每个问题，但会提供一个在许多情况下都可以使用的缓冲区。

### <a name="input-validation"></a>输入验证

请注意编写验证规则时的假设。 要求文本字段输入 "需要" 至少三个字符的英语可能看似有效，因为单个字母很少有任何意义。 但在中文和日语中，单个字符可能是有效输入，而对于这些语言，则不能使用验证消息 "至少需要3个字符"。

其他看似简单的任务（如验证电子邮件地址或网站 URL）将变得更加复杂，因为字符并不局限于 ASCII 子集。

使用国际化编写验证规则-请选择限制最少的规则，或编写逻辑，使其在每种语言中的工作方式不同。

### <a name="images-and-color"></a>图像和颜色

并非每个映像都需要基于用户的语言选项进行更改。 许多图标或照片适用于所有用户，而不是他们所说的语言。
不过，某些资源对本地化是有意义的，例如：

- 描述人员或特定位置的图像–如果用户显示了本地人员/位置，则应用程序可能会感到更密切的关系。
- 图标–某些插图可以是特定于区域性的，你可以通过将图像本地化以反映本地了解来使应用更易于使用。
- 颜色–某些区域性以不同的方式理解颜色–红色可能表示一个区域中出现警告，但在另一个区域祝您好运。 设计应用程序时，请检查本机扬声器，以确定是否应该构建一种本地化颜色的机制。

### <a name="videos-and-sound"></a>视频和声音

对应用程序进行本地化时，视频和声音提出了特殊的挑战，因为在翻译字符串的过程中相对容易，记录多个 voiceover 音轨或视频剪辑可能既昂贵又困难。

视频和声音文件的多个副本也可能会显著增加应用程序的大小（尤其是在将其本地化为大量语言或包含大量媒体文件时）。 用户安装应用后，你可能会考虑仅下载所需的语言资产，但这也可能导致网络速度较慢的用户体验。

通常有多种方法可以解决本地化问题–最重要的一点是将它们提前考虑起来，并确保您的应用程序能够对其进行处理。

### <a name="dates-times-numbers-and-currency"></a>日期、时间、数字和货币

如果使用的是 .NET 格式设置函数，请记住指定区域性，以便正确分析小数分隔符（避免引发转换异常）。 例如，1.99 和1，99都是有效的十进制表示形式，具体取决于你的区域设置。

当数据来自已知源（即从你自己的代码或你控制的 web 服务）时，你可以对与格式（例如，InvariantCulture）相匹配的区域性标识符进行硬编码，这将适用于标准的英语格式。

```csharp
double.Parse("1,999.99", CultureInfo.InvariantCulture);
```

如果应用用户正在输入数据，请使用反映其区域设置的 CultureInfo 实例分析数据：

```csharp
double.Parse("1 999,99", CultureInfo.CreateSpecificCulture("fr-FR"));
```

有关其他信息，请参阅[分析数字字符串](https://msdn.microsoft.com/library/xbtzcc4w(v=vs.110).aspx)和[分析日期和时间字符串](https://msdn.microsoft.com/library/2h3syy57(v=vs.110).aspx)MSDN 文章。

<a name="rtl" />

### <a name="right-to-left-rtl-languages"></a>从右到左（RTL）语言

某些语言（例如阿拉伯语、希伯来语和乌尔都语）是从右到左阅读的。
支持这些语言的应用程序应使用可适应从右到左读取器的屏幕设计，例如：

- 文本应为右对齐。
- 标签应出现在输入字段的右侧。
- 通常反转默认按钮位置。
- 还应反转使用上下文方向的分层导航轻扫和动画（以及其他导航形式和动画）。

IOS 和 Android 都支持从右到左布局和字体渲染，同时提供了可帮助进行上述调整的内置功能。 Xamarin 目前不会自动支持 RTL 呈现。

### <a name="sorting"></a>排序

不同语言定义字母表的排序顺序，即使它们使用相同的字符集也是如此。

有关在[.NET Framework 中使用字符串的最佳实践](https://msdn.microsoft.com/library/dd465121(v=vs.110).aspx)中的[字符串比较的详细信息](https://msdn.microsoft.com/library/dd465121(v=vs.110).aspx#the_details_of_string_comparison)，请参阅语言（CultureInfo）影响排序顺序的示例。

移动平台上的内置数据库功能不太可能支持特定于语言的排序顺序，因此您可能需要在您的业务逻辑中实现其他代码。

### <a name="text-search"></a>文本搜索

请确保编写并测试包含多种语言的搜索算法。 需要考虑的事项包括：

- 自动完成–如果已生成自动完成功能，请确保它与用户语言相关的建议相关。
- 将查询与数据匹配-将以特定语言对输入的查询进行搜索，而不是使用以该语言编写的内容或对应用中的所有内容执行的搜索？
- 词干–如果你的搜索是为了搜索类似的单词、word 根和其他搜索优化而构建的，则这些优化是为你支持的所有语言生成的吗？
- 排序–确保正确地对结果进行排序（请参阅上文）。

### <a name="data-from-external-sources"></a>外部源中的数据

许多应用程序将来自外部数据源的数据从 Twitter 和 RSS 源下载到天气、新闻或股票价格。 当向用户显示此信息时，您需要考虑您可能会向他们显示不相关或不可读的信息。

你可以使用几个策略来尝试并确保你的应用程序显示与用户相关的数据：

- 不同源-应用程序可能会根据用户的语言或区域设置从不同的源下载数据。 与从北美新闻源下载的内容相比，区域设置新闻、天气和股票价格可能更有意义。
- 已本地化的显示–如果要显示 Twitter 或 photo 源，则应在其自己的语言中显示元数据（例如所用的时间），即使内容本身仍以原始语言显示也是如此。
- 翻译–您可以在您的应用程序中生成翻译选项，以便对传入数据进行计算机转换。 这可以是自动或用户的决定–只需确保在发生这种情况时通知用户，因为计算机翻译从来都不完美！

这也可能会影响音频轨或视频的外部链接–在设计应用程序时，请务必提前计划源翻译的内容，或者确保用户界面在内容不会在其语言.

### <a name="dont-over-translate"></a>不过度翻译

应用程序中的某些字符串可能不需要翻译，或者要求翻译人员需要特别注意。 示例可能包括：

- Url –如果列出 URL，则它可能需要也可能不需要按语言进行调整。 例如，facebook.com 不要求在主站点上翻译它自动检测语言。 其他站点具有特定于区域设置的内容，你可能想要提供不同的 URL，例如 yahoo.com 与 yahoo.fr 或 yahoo.it。
- 电话号码–尤其是具有不同国家/地区代码的电话号码或使用特定语言的调用方。
- 联系人详细信息-地址和其他信息可能因语言或区域设置而异。
- 商标 & 产品名称–某些字符串不需要翻译，因为它们始终采用相同的语言编写。

最后，请确保在某些字符串需要特殊处理时，为转换器提供详细说明。

### <a name="formatted-text"></a>带格式文本

通常情况下，移动应用程序不会出现问题，因为字符串的格式通常不会太丰富。 但是，如果你的应用程序需要格式文本（例如粗体或斜体格式），则确保转换器知道如何输入格式，字符串文件会正确地存储它，并且在显示给用户之前正确设置格式（例如，不要意外允许将为用户提供格式设置代码。

## <a name="translation-tips"></a>翻译提示

转换应用程序使用的字符串被视为本地化过程的一部分。 通常，此任务会外包给翻译服务，并由可能不知道你的应用程序或业务的多语言人员执行。

以下提示可帮助你生成更易于正确转换的字符串，从而提高本地化应用的质量。

### <a name="localize-complete-strings-not-words"></a>本地化完整的字符串，而不是字词

有时，开发人员采用尝试指定单个词或句子 "代码段" 的方法，使他们能够在整个应用程序中重复使用它们。 例如，对于文本 "a 5 条消息"。 它们可能指定以下字符串进行转换

**错误**：

```csharp
"You have"
"no"
"message"
"messages"
```

然后，使用字符串串联尝试在代码中即时创建正确的短语：

**错误**：

```csharp
"You have" + " " + numMsgs + " " + "messages"
"You have" + " no " + "messages"
```

不建议**这样**做，因为它不一定适用于所有语言，因而翻译人员很难理解每个短段的上下文。 它还会导致重复使用已翻译的字符串，如果它们在不同的上下文中使用（然后进行了更新），则可能会导致问题。

### <a name="allow-for-parameter-re-ordering"></a>允许对参数重新排序

某些编程语言需要额外的语法来指定字符串中参数的顺序，但 .NET 已支持编号占位符的概念，因此

**不错**：

```csharp
"a {0} b {1} cde {3}"
```

可转换为以下项（占位符的位置和顺序发生更改）

```csharp
"{2} {3} f g h {0}"
```

标记将按预期方式排序。 将字符串发送到转换器时，请确保包含每个占位符所包含内容的说明。

### <a name="use-multiple-strings-for-cardinality"></a>将多个字符串用于基数

避免`"You have {0} message/s."`使用特定字符串来提供每种状态的字符串，以提供更好的用户体验：

**不错**：

```csharp
"You have no messages."
"You have 1 messages."
"You have 2 messages."
"You have {0} messages."
```

你必须在应用程序中编写代码，以计算正在显示的数字，并选择相应的字符串。 某些平台（包括 iOS 和 Android）具有内置功能，可根据当前语言/区域设置的首选项自动选择最佳复数字符串。

### <a name="allowing-for-gender"></a>允许性别

基于拉丁语的语言有时会根据使用者的性别使用不同的词。 如果你的应用知道性别，你应该允许翻译的字符串反映这一点。

还有一个更明显的例子，甚至是在英语中，其中字符串指的是特定的用户或应用程序的用户。 例如，某些站点显示类似于`"Bob commented on his post"`这样的消息，因此你需要用于二元、女性和非二元或未知性别的字符串：

**不错**：

```csharp
"{0} commented on his post"
"{0} commented on her post"
"{0} commented on their post"
```

### <a name="dont-reuse-strings"></a>不重复使用字符串

更准确地说，不要重复使用字符串，因为字符串本身具有不同的用途或含义。

例如：假设您的应用程序中有一个开关，并且开关控件需要 "打开" 和 "关闭" 的文本进行本地化。 还会在应用程序的文本标签中显示该设置的值。 对于开关显示，应使用不同的字符串，而不是切换的状态（即使它们与默认语言中的字符串相同）-例如：

- "打开" –在交换机本身上显示
- "关" –显示在开关本身上
- "打开" –显示在标签中
- "关" –显示在标签中

这为翻译人员提供了最大的灵活性：

- 出于设计原因，开关本身可能使用小写 "on" 和 "off"，但显示标签使用大写 "On" 和 "Off"。
- 某些语言可能需要对开关值进行缩写以适应用户界面控件，而完整（转换）的单词可以出现在标签中。
- 或者，对于某些语言，你的切换的呈现可能使用 "I" 和 "O" 进行区域性熟悉，但你可能仍希望标签读取 "开" 或 "关"。

### <a name="translation-services"></a>翻译服务

#### <a name="machine-translation"></a>机器翻译

若要在应用中生成翻译功能，请考虑[Azure 文本翻译 API](https://azure.microsoft.com/services/cognitive-services/translator-text-api/)。

出于测试目的，您可以使用多种在线翻译工具之一，在开发过程中将一些本地化文本包含在您的应用程序中：

- [必应翻译](https://www.bing.com/translator/)
- [Google 翻译](http://translate.google.com/)

还有很多其他功能可用。 通常情况下，机器翻译质量不会被视为足以释放应用程序，而无需先通过专业翻译人员或本机发言人进行评审和测试。

#### <a name="professional-translation"></a>专业翻译

还提供了一些专业翻译服务，它们将使用您的字符串并将其分发给自己的翻译员，为您提供了一种费用的翻译。

其中一项已知服务是[LionBridge](http://www.lionbridge.com/)。 大多数专业服务支持所有常见的文件类型，包括字符串、XML、RESX 和 .POT/PO。

## <a name="summary"></a>总结

本文介绍了在对应用进行国际化之前应熟悉的一些概念，并对资源进行本地化，还介绍了如何更改每个平台的语言首选项。

这些概念适用于可通过 Xamarin 实现的各种特定于平台和跨平台的国际化技术。

继续阅读你感兴趣的平台的技术详细信息：

- [Xamarin. Forms](~/xamarin-forms/app-fundamentals/localization/index.md)使用 RESX 文件的跨平台本地化。
- [Xamarin](~/ios/app-fundamentals/localization/index.md)本地平台本地化。
- [Xamarin Android](~/android/app-fundamentals/localization.md)本机平台本地化。

## <a name="related-links"></a>相关链接

- [Apple 的本地化概述](https://developer.apple.com/internationalization/)
- [Android 的本地化清单](https://developer.android.com/distribute/tools/localization-checklist.html)
- [开发全球通用应用程序的最佳实践（MSDN）](https://msdn.microsoft.com/library/w7x1y988%28v=vs.90%29.aspx)
