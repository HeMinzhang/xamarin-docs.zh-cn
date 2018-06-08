---
title: 可重用 EventToCommandBehavior
description: 行为可以用于将命令与不设计与命令进行交互的控件相关联。 本文演示如何使用 Xamarin.Forms 行为来调用命令事件激发时。
ms.prod: xamarin
ms.assetid: EC7F6556-9776-40B8-9424-A8094482A2F3
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: e89400c74c3d1afbf8954d0f88387c5967ebd534
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/07/2018
ms.locfileid: "34848211"
---
# <a name="reusable-eventtocommandbehavior"></a>可重用 EventToCommandBehavior

_行为可以用于将命令与不设计与命令进行交互的控件相关联。本文演示如何使用 Xamarin.Forms 行为来调用命令事件激发时。_

## <a name="overview"></a>概述

`EventToCommandBehavior`类是在响应中执行命令的可重用 Xamarin.Forms 自定义行为*任何*事件激发。 默认情况下，事件自变量为此事件将传递到命令，并可以根据需要转换通过[ `IValueConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.IValueConverter/)实现。

以下行为属性必须设置为使用行为：

- **EventName** – 行为侦听事件的名称。
- **命令**– **ICommand**要执行。 行为期望找到`ICommand`实例上[ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/)附加可能从父元素继承的控件。

此外可以设置以下可选行为属性：

- **CommandParameter** – `object` ，将传递给命令。
- **转换器**– [ `IValueConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.IValueConverter/)将更改的事件自变量数据格式，如之间传递的实现*源*和*目标*由绑定引擎。

## <a name="creating-the-behavior"></a>创建行为

`EventToCommandBehavior`类派生自`BehaviorBase<T>`类，该类又派生自[ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/)类。 用途`BehaviorBase<T>`类旨在为需要的任何 Xamarin.Forms 行为的基类[ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/)设置为附加的控件的行为。 这将确保行为可以绑定到并执行`ICommand`指定的`Command`属性时使用行为。

`BehaviorBase<T>`类提供可重写[ `OnAttachedTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/Xamarin.Forms.BindableObject/)设置的方法[ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/)的该行为，并可重写[ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/)方法清除的`BindingContext`。 此外，类存储到中的附加控件的引用`AssociatedObject`属性。

### <a name="implementing-bindable-properties"></a>实现可绑定属性

`EventToCommandBehavior`类定义了四个[ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/)事件激发时，情况下，执行用户定义命令。 这些属性下面的代码示例所示：

```csharp
public class EventToCommandBehavior : BehaviorBase<View>
{
  public static readonly BindableProperty EventNameProperty =
    BindableProperty.Create ("EventName", typeof(string), typeof(EventToCommandBehavior), null, propertyChanged: OnEventNameChanged);
  public static readonly BindableProperty CommandProperty =
    BindableProperty.Create ("Command", typeof(ICommand), typeof(EventToCommandBehavior), null);
  public static readonly BindableProperty CommandParameterProperty =
    BindableProperty.Create ("CommandParameter", typeof(object), typeof(EventToCommandBehavior), null);
  public static readonly BindableProperty InputConverterProperty =
    BindableProperty.Create ("Converter", typeof(IValueConverter), typeof(EventToCommandBehavior), null);

  public string EventName { ... }
  public ICommand Command { ... }
  public object CommandParameter { ... }
  public IValueConverter Converter { ...  }
  ...
}
```

当`EventToCommandBehavior`使用类时，`Command`属性应为数据绑定到`ICommand`以响应事件激发中定义要执行`EventName`属性。 该行为将希望查找`ICommand`上[ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/)附加的控件。

默认情况下，事件的事件自变量将传递给命令。 此数据可以根据需要转换之间传递*源*和*目标*由绑定引擎，通过指定[ `IValueConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.IValueConverter/)实现作为`Converter`属性值。 或者，可以为传递参数命令通过指定`CommandParameter`属性值。

### <a name="implementing-the-overrides"></a>实现重写

`EventToCommandBehavior`类重写[ `OnAttachedTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/Xamarin.Forms.BindableObject/)和[ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/)方法`BehaviorBase<T>`类，如下面的代码示例中所示：

```csharp
public class EventToCommandBehavior : BehaviorBase<View>
{
  ...
  protected override void OnAttachedTo (View bindable)
  {
    base.OnAttachedTo (bindable);
    RegisterEvent (EventName);
  }

  protected override void OnDetachingFrom (View bindable)
  {
    DeregisterEvent (EventName);
    base.OnDetachingFrom (bindable);
  }
  ...
}
```

[ `OnAttachedTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/Xamarin.Forms.BindableObject/)方法通过调用执行安装程序`RegisterEvent`方法，并传递的值中`EventName`属性作为参数。 [ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/)方法通过调用执行清理`DeregisterEvent`方法，并传递的值中`EventName`属性作为参数。

### <a name="implementing-the-behavior-functionality"></a>实现行为功能

行为的用途是执行定义的命令`Command`以响应事件激发定义的属性`EventName`属性。 核心行为功能下面的代码示例所示：

```csharp
public class EventToCommandBehavior : BehaviorBase<View>
{
  ...
  void RegisterEvent (string name)
  {
    if (string.IsNullOrWhiteSpace (name)) {
      return;
    }

    EventInfo eventInfo = AssociatedObject.GetType ().GetRuntimeEvent (name);
    if (eventInfo == null) {
      throw new ArgumentException (string.Format ("EventToCommandBehavior: Can't register the '{0}' event.", EventName));
    }
    MethodInfo methodInfo = typeof(EventToCommandBehavior).GetTypeInfo ().GetDeclaredMethod ("OnEvent");
    eventHandler = methodInfo.CreateDelegate (eventInfo.EventHandlerType, this);
    eventInfo.AddEventHandler (AssociatedObject, eventHandler);
  }

  void OnEvent (object sender, object eventArgs)
  {
    if (Command == null) {
      return;
    }

    object resolvedParameter;
    if (CommandParameter != null) {
      resolvedParameter = CommandParameter;
    } else if (Converter != null) {
      resolvedParameter = Converter.Convert (eventArgs, typeof(object), null, null);
    } else {
      resolvedParameter = eventArgs;
    }        

    if (Command.CanExecute (resolvedParameter)) {
      Command.Execute (resolvedParameter);
    }
  }
  ...
}
```

`RegisterEvent`方法执行以响应`EventToCommandBehavior`要附加到控件，并且它接收的值`EventName`属性作为参数。 然后，该方法尝试找到中定义的事件`EventName`属性，可在附加的控件。 假设事件可以位于，`OnEvent`方法注册为事件的处理程序方法。

`OnEvent`以响应事件激发中定义执行方法`EventName`属性。 假设`Command`属性引用是有效`ICommand`，此方法尝试检索要传递给参数`ICommand`，如下所示：

- 如果`CommandParameter`属性定义了一个参数，它就会检索。
- 否则为如果`Converter`属性定义[ `IValueConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.IValueConverter/)实现，该转换器执行，并根据之间传递转换事件自变量数据*源*和*目标*由绑定引擎。
- 否则，事件自变量被假定为参数。

数据绑定`ICommand`然后执行，则将在参数中传递给该命令，条件是[ `CanExecute` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Command.CanExecute/p/System.Object/)方法返回`true`。

尽管不显示在这里，`EventToCommandBehavior`还包括`DeregisterEvent`方法执行的[ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/)方法。 `DeregisterEvent`方法用于查找并取消注册在定义事件`EventName`属性，清理任何潜在的内存泄漏。

## <a name="consuming-the-behavior"></a>使用行为

`EventToCommandBehavior`类可以附加到[ `Behaviors` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Behaviors/)集合的控件，如下面的 XAML 代码示例中所示：

```xaml
<ListView ItemsSource="{Binding People}">
  <ListView.Behaviors>
    <local:EventToCommandBehavior EventName="ItemSelected" Command="{Binding OutputAgeCommand}"
                                  Converter="{StaticResource SelectedItemConverter}" />
  </ListView.Behaviors>
</ListView>
<Label Text="{Binding SelectedItemText}" />
```

以下代码示例显示相应的 C# 代码：

```csharp
var listView = new ListView ();
listView.SetBinding (ItemsView<Cell>.ItemsSourceProperty, "People");
listView.Behaviors.Add (new EventToCommandBehavior {
  EventName = "ItemSelected",
  Command = ((HomePageViewModel)BindingContext).OutputAgeCommand,
  Converter = new SelectedItemEventArgsToSelectedItemConverter ()
});

var selectedItemLabel = new Label ();
selectedItemLabel.SetBinding (Label.TextProperty, "SelectedItemText");
```

`Command`的行为的属性被数据绑定到`OutputAgeCommand`属性关联的 ViewModel，虽然`Converter`属性设置为`SelectedItemConverter`实例，它将返回[ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.SelectedItem/)的[ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)从[ `SelectedItemChangedEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SelectedItemChangedEventArgs/)。

在运行时，行为将响应与控件交互。 当中选择一项[ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)、 [ `ItemSelected` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemSelected/)事件将激发，其将执行`OutputAgeCommand`视图模型中。 这反过来更新 ViewModel`SelectedItemText`属性， [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)将绑定到，如以下屏幕截图中所示：

[![](event-to-command-behavior-images/screenshots-sml.png "示例应用程序与 EventToCommandBehavior")](event-to-command-behavior-images/screenshots.png#lightbox "示例应用程序与 EventToCommandBehavior")

使用此行为时触发事件时，执行命令的优点是命令可以是与未设计为与命令进行交互的控件相关联。 此外，这将移除的样板事件处理代码从代码隐藏文件。

## <a name="summary"></a>总结

这篇文章演示了使用 Xamarin.Forms 行为来调用命令事件激发时。 行为可以用于将命令与不设计与命令进行交互的控件相关联。


## <a name="related-links"></a>相关链接

- [EventToCommand 行为 （示例）](https://developer.xamarin.com/samples/xamarin-forms/behaviors/eventtocommandbehavior/)
- [行为](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior/)
- [行为<T>](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/)
