---
title: Xamarin.Forms 数据模板
description: DataTemplate 可用于指定数据的外观上受支持的控件，并通常绑定到数据，以显示。
ms.prod: xamarin
ms.assetid: 838F4BDB-B719-457F-8633-27E9B267A2A0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/11/2017
ms.openlocfilehash: b461cb8be9dcf618b7a497b001186fde5f2b2aa1
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/08/2018
ms.locfileid: "35239851"
---
# <a name="xamarinforms-data-templates"></a>Xamarin.Forms 数据模板

_DataTemplate 可用于指定数据的外观上受支持的控件，并通常绑定到数据，以显示。_

## <a name="introductionintroductionmd"></a>[介绍](introduction.md)

Xamarin.Forms 数据模板提供受支持的控件上定义数据的表示形式的功能。 本文介绍数据模板，检查它们是所必需的原因。

## <a name="creating-a-datatemplatecreatingmd"></a>[创建 DataTemplate](creating.md)

数据模板可以在中创建内联， [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)，或从自定义的类型或相应 Xamarin.Forms 单元格类型。 如果没有无需重复使用在其他位置的数据模板，则应使用内联模板。 或者，可以通过定义它作为自定义类型，或控制级别页级或应用程序级别资源重复使用数据模板。

## <a name="creating-a-datatemplateselectorselectormd"></a>[创建 DataTemplateSelector](selector.md)

A [ `DataTemplateSelector` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/)可用来选择[ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/)在运行时基于的数据绑定属性的值。 这使多个`DataTemplate`实例应用于相同类型的对象，自定义的特定对象的外观。 本文演示如何创建和使用`DataTemplateSelector`。


## <a name="related-links"></a>相关链接

- [数据模板 （示例）](https://developer.xamarin.com/samples/xamarin-forms/templates/datatemplates/)
