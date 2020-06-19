---
title: 企业应用程序中的验证
description: 本章介绍 eShopOnContainers mobile 应用如何执行用户输入验证。 这包括指定验证规则、触发验证和显示验证错误。
ms.prod: xamarin
ms.assetid: 56e4f0fc-48d9-4033-91ec-173bb46a5e4d
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: a4b82552956ab0e75d0a76a14ce7c919c744e09a
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/18/2020
ms.locfileid: "84565325"
---
# <a name="validation-in-enterprise-apps"></a>企业应用程序中的验证

任何接受用户输入的应用都应确保输入有效。 例如，应用程序可以检查输入中是否仅包含特定范围内的字符、是否为特定长度，或是否匹配特定格式。 如果没有验证，用户可以提供导致应用失败的数据。 验证将强制实施业务规则，并阻止攻击者注入恶意数据。

在 ViewModel （MVVM）模式的上下文中，视图模型或模型通常需要执行数据验证并向视图发出任何验证错误信号，以便用户可以更正这些错误。 EShopOnContainers 移动应用执行视图模型属性的同步客户端验证，通过突出显示包含无效数据的控件，并通过显示错误消息通知用户数据无效的原因，向用户通知任何验证错误。 图6-1 显示了在 eShopOnContainers 移动应用程序中执行验证所涉及的类。

[![](validation-images/validation.png "Validation classes in the eShopOnContainers mobile app")](validation-images/validation-large.png#lightbox "Validation classes in the eShopOnContainers mobile app")

**图 6-1**： eShopOnContainers 移动应用中的验证类

需要验证的视图模型属性的类型为 `ValidatableObject<T>` ，并且每个 `ValidatableObject<T>` 实例都已将验证规则添加到其 `Validations` 属性中。 验证是通过调用实例的方法从视图模型调用的 `Validate` `ValidatableObject<T>` ，该方法检索验证规则并对属性执行这些验证规则 `ValidatableObject<T>` `Value` 。 任何验证错误都将放入 `Errors` 实例的属性 `ValidatableObject<T>` 中，并 `IsValid` 更新实例的属性 `ValidatableObject<T>` 以指示验证是成功还是失败。

属性更改通知由 `ExtendedBindableObject` 类提供，因此 [`Entry`](xref:Xamarin.Forms.Entry) 控件可以绑定到 `IsValid` `ValidatableObject<T>` 视图模型类中的实例的属性，以通知所输入的数据是否有效。

## <a name="specifying-validation-rules"></a>指定验证规则

验证规则是通过创建从接口派生的类来指定的 `IValidationRule<T>` ，如下面的代码示例所示：

```csharp
public interface IValidationRule<T>  
{  
    string ValidationMessage { get; set; }  
    bool Check(T value);  
}
```

此接口指定验证规则类必须提供 `boolean` `Check` 用于执行所需验证的方法，并指定一个 `ValidationMessage` 属性，其值为验证错误消息，如果验证失败，将显示该消息。

下面的代码示例演示 `IsNotNullOrEmptyRule<T>` 验证规则，该规则用于在 `LoginView` eShopOnContainers 移动应用中使用模拟服务时对用户在上输入的用户名和密码进行验证：

```csharp
public class IsNotNullOrEmptyRule<T> : IValidationRule<T>  
{  
    public string ValidationMessage { get; set; }  

    public bool Check(T value)  
    {  
        if (value == null)  
        {  
            return false;  
        }  

        var str = value as string;  
        return !string.IsNullOrWhiteSpace(str);  
    }  
}
```

`Check`方法返回一个 `boolean` ，它指示值参数是 `null` 、空还是仅由空白字符组成。

下面的代码示例演示了用于验证电子邮件地址的验证规则：

```csharp
public class EmailRule<T> : IValidationRule<T>  
{  
    public string ValidationMessage { get; set; }  

    public bool Check(T value)  
    {  
        if (value == null)  
        {  
            return false;  
        }  

        var str = value as string;  
        Regex regex = new Regex(@"^([\w\.\-]+)@([\w\-]+)((\.(\w){2,3})+)$");  
        Match match = regex.Match(str);  

        return match.Success;  
    }  
}
```

`Check`方法返回一个 `boolean` ，它指示值参数是否为有效的电子邮件地址。 为此，可在构造函数中搜索指定的正则表达式模式的第一个匹配项的值参数 `Regex` 。 是否可以通过检查对象的属性的值来确定是否在输入字符串中找到了正则表达式模式 `Match` `Success` 。

> [!NOTE]
> 属性验证有时可以涉及依赖属性。 依赖属性的一个示例是，属性 A 的有效值集取决于在属性 B 中设置的特定值。若要检查属性 A 的值是否为允许的值之一，将需要检索属性 B 的值。此外，当属性 B 的值更改时，需要重新验证属性 A。

## <a name="adding-validation-rules-to-a-property"></a>将验证规则添加到属性

在 eShopOnContainers 移动应用中，查看需要验证的模型属性的类型为 `ValidatableObject<T>` ，其中 `T` 是要验证的数据的类型。 下面的代码示例显示了两个这样的属性的示例：

```csharp
public ValidatableObject<string> UserName  
{  
    get  
    {  
        return _userName;  
    }  
    set  
    {  
        _userName = value;  
        RaisePropertyChanged(() => UserName);  
    }  
}  

public ValidatableObject<string> Password  
{  
    get  
    {  
        return _password;  
    }  
    set  
    {  
        _password = value;  
        RaisePropertyChanged(() => Password);  
    }  
}
```

若要进行验证，必须将验证规则添加到 `Validations` 每个实例的 `ValidatableObject<T>` 集合中，如下面的代码示例所示：

```csharp
private void AddValidations()  
{  
    _userName.Validations.Add(new IsNotNullOrEmptyRule<string>   
    {   
        ValidationMessage = "A username is required."   
    });  
    _password.Validations.Add(new IsNotNullOrEmptyRule<string>   
    {   
        ValidationMessage = "A password is required."   
    });  
}
```

此方法将 `IsNotNullOrEmptyRule<T>` 验证规则添加到 `Validations` 每个实例的集合 `ValidatableObject<T>` ，并指定验证规则属性的值 `ValidationMessage` ，该属性指定验证失败时将显示的验证错误消息。

## <a name="triggering-validation"></a>触发验证

EShopOnContainers 移动应用中使用的验证方法可以手动触发属性验证，并在属性发生更改时自动触发验证。

### <a name="triggering-validation-manually"></a>手动触发验证

可以为视图模型属性手动触发验证。 例如，当用户点击上的 "**登录**" 按钮时，将在 eShopOnContainers 移动应用中出现这种情况 `LoginView` 。 命令委托调用中的 `MockSignInAsync` 方法，该方法 `LoginViewModel` 通过执行方法来调用验证 `Validate` ，如以下代码示例所示：

```csharp
private bool Validate()  
{  
    bool isValidUser = ValidateUserName();  
    bool isValidPassword = ValidatePassword();  
    return isValidUser && isValidPassword;  
}  

private bool ValidateUserName()  
{  
    return _userName.Validate();  
}  

private bool ValidatePassword()  
{  
    return _password.Validate();  
}
```

`Validate`方法 `LoginView` 通过对每个实例调用 Validate 方法，对上用户输入的用户名和密码进行验证 `ValidatableObject<T>` 。 下面的代码示例演示类中的 Validate 方法 `ValidatableObject<T>` ：

```csharp
public bool Validate()  
{  
    Errors.Clear();  

    IEnumerable<string> errors = _validations  
        .Where(v => !v.Check(Value))  
        .Select(v => v.ValidationMessage);  

    Errors = errors.ToList();  
    IsValid = !Errors.Any();  

    return this.IsValid;  
}
```

此方法清除 `Errors` 集合，然后检索添加到对象的集合中的任何验证规则 `Validations` 。 将 `Check` 执行每个检索到的验证规则的方法，并且将 `ValidationMessage` 无法验证数据的任何验证规则的属性值添加到实例的 `Errors` 集合中 `ValidatableObject<T>` 。 最后， `IsValid` 设置该属性，并将其值返回给调用方法，指示验证是成功还是失败。

### <a name="triggering-validation-when-properties-change"></a>属性更改时触发验证

只要绑定属性发生更改，也可以触发验证。 例如，当中的双向绑定 `LoginView` 设置 `UserName` 或 `Password` 属性时，将触发验证。 下面的代码示例演示了这种情况的发生方式：

```xaml
<Entry Text="{Binding UserName.Value, Mode=TwoWay}">  
    <Entry.Behaviors>  
        <behaviors:EventToCommandBehavior  
            EventName="TextChanged"  
            Command="{Binding ValidateUserNameCommand}" />  
    </Entry.Behaviors>  
    ...  
</Entry>
```

[`Entry`](xref:Xamarin.Forms.Entry)控件将绑定到该 `UserName.Value` 实例的属性 `ValidatableObject<T>` ，并向该控件的 `Behaviors` 集合添加一个 `EventToCommandBehavior` 实例。 此行为 `ValidateUserNameCommand` 在响应中的 [ `TextChanged` ] 事件触发时执行，在中的 `Entry` 文本 `Entry` 发生更改时引发。 反过来， `ValidateUserNameCommand` 委托执行 `ValidateUserName` 方法，该方法对 `Validate` 实例执行方法 `ValidatableObject<T>` 。 因此，每次用户在控件中为用户名输入一个字符时 `Entry` ，都会对输入的数据进行验证。

有关行为的详细信息，请参阅[实现行为](~/xamarin-forms/enterprise-application-patterns/mvvm.md#implementing-behaviors)。

## <a name="displaying-validation-errors"></a>显示验证错误

EShopOnContainers 移动应用通过以下方式向用户通知任何验证错误：突出显示包含无效数据的控件，并显示一条错误消息，告知用户数据在包含无效数据的控件下方无效的原因。 当更正无效数据时，该行将变为黑色，并删除错误消息。 图6-2 显示验证错误时 eShopOnContainers 移动应用中的登录视图。

![](validation-images/validation-login.png "Displaying validation errors during login")

**图6-2：** 在登录过程中显示验证错误

### <a name="highlighting-a-control-that-contains-invalid-data"></a>突出显示包含无效数据的控件

`LineColorBehavior`附加的行为用于突出显示 [`Entry`](xref:Xamarin.Forms.Entry) 发生验证错误的控件。 下面的代码示例演示如何将 `LineColorBehavior` 附加行为附加到 `Entry` 控件：

```xaml
<Entry Text="{Binding UserName.Value, Mode=TwoWay}">
    <Entry.Style>
        <OnPlatform x:TypeArguments="Style">
            <On Platform="iOS, Android" Value="{StaticResource EntryStyle}" />
            <On Platform="UWP" Value="{StaticResource UwpEntryStyle}" />
        </OnPlatform>
    </Entry.Style>
    ...
</Entry>
```

[`Entry`](xref:Xamarin.Forms.Entry)控件使用显式样式，如以下代码示例所示：

```xaml
<Style x:Key="EntryStyle"  
       TargetType="{x:Type Entry}">  
    ...  
    <Setter Property="behaviors:LineColorBehavior.ApplyLineColor"  
            Value="True" />  
    <Setter Property="behaviors:LineColorBehavior.LineColor"  
            Value="{StaticResource BlackColor}" />  
    ...  
</Style>
```

此样式设置 `ApplyLineColor` `LineColor` `LineColorBehavior` 控件上附加行为的和附加属性 [`Entry`](xref:Xamarin.Forms.Entry) 。 有关样式的详细信息，请参阅[样式](~/xamarin-forms/user-interface/styles/index.md)。

当 `ApplyLineColor` 设置附加属性的值或更改时， `LineColorBehavior` 附加行为将执行 `OnApplyLineColorChanged` 方法，如以下代码示例所示：

```csharp
public static class LineColorBehavior  
{  
    ...  
    private static void OnApplyLineColorChanged(  
                BindableObject bindable, object oldValue, object newValue)  
    {  
        var view = bindable as View;  
        if (view == null)  
        {  
            return;  
        }  

        bool hasLine = (bool)newValue;  
        if (hasLine)  
        {  
            view.Effects.Add(new EntryLineColorEffect());  
        }  
        else  
        {  
            var entryLineColorEffectToRemove =   
                    view.Effects.FirstOrDefault(e => e is EntryLineColorEffect);  
            if (entryLineColorEffectToRemove != null)  
            {  
                view.Effects.Remove(entryLineColorEffectToRemove);  
            }  
        }  
    }  
}
```

此方法的参数提供行为所附加到的控件的实例，以及附加属性的新旧值 `ApplyLineColor` 。 `EntryLineColorEffect`如果附加属性为，则将类添加到控件的 [`Effects`](xref:Xamarin.Forms.Element.Effects) 集合 `ApplyLineColor` `true` ，否则将从控件的集合中删除该类 `Effects` 。 有关行为的详细信息，请参阅[实现行为](~/xamarin-forms/enterprise-application-patterns/mvvm.md#implementing-behaviors)。

`EntryLineColorEffect`子类为 [`RoutingEffect`](xref:Xamarin.Forms.RoutingEffect) 类，如下面的代码示例所示：

```csharp
public class EntryLineColorEffect : RoutingEffect  
{  
    public EntryLineColorEffect() : base("eShopOnContainers.EntryLineColorEffect")  
    {  
    }  
}
```

[`RoutingEffect`](xref:Xamarin.Forms.RoutingEffect)类表示一个独立于平台的效果，该效果包装了平台特定的内部效果。 这简化了效果删除过程，因为对于特定于平台的效果，没有对类型信息的编译时访问。 `EntryLineColorEffect`调用基类构造函数，传入一个参数，该参数由解析组名称的连接和在每个特定于平台的效果类上指定的唯一 ID 组成。

下面的代码示例演示 `eShopOnContainers.EntryLineColorEffect` iOS 实现：

```csharp
[assembly: ResolutionGroupName("eShopOnContainers")]  
[assembly: ExportEffect(typeof(EntryLineColorEffect), "EntryLineColorEffect")]  
namespace eShopOnContainers.iOS.Effects  
{  
    public class EntryLineColorEffect : PlatformEffect  
    {  
        UITextField control;  

        protected override void OnAttached()  
        {  
            try  
            {  
                control = Control as UITextField;  
                UpdateLineColor();  
            }  
            catch (Exception ex)  
            {  
                Console.WriteLine("Can't set property on attached control. Error: ", ex.Message);  
            }  
        }  

        protected override void OnDetached()  
        {  
            control = null;  
        }  

        protected override void OnElementPropertyChanged(PropertyChangedEventArgs args)  
        {  
            base.OnElementPropertyChanged(args);  

            if (args.PropertyName == LineColorBehavior.LineColorProperty.PropertyName ||  
                args.PropertyName == "Height")  
            {  
                Initialize();  
                UpdateLineColor();  
            }  
        }  

        private void Initialize()  
        {  
            var entry = Element as Entry;  
            if (entry != null)  
            {  
                Control.Bounds = new CGRect(0, 0, entry.Width, entry.Height);  
            }  
        }  

        private void UpdateLineColor()  
        {  
            BorderLineLayer lineLayer = control.Layer.Sublayers.OfType<BorderLineLayer>()  
                                                             .FirstOrDefault();  

            if (lineLayer == null)  
            {  
                lineLayer = new BorderLineLayer();  
                lineLayer.MasksToBounds = true;  
                lineLayer.BorderWidth = 1.0f;  
                control.Layer.AddSublayer(lineLayer);  
                control.BorderStyle = UITextBorderStyle.None;  
            }  

            lineLayer.Frame = new CGRect(0f, Control.Frame.Height-1f, Control.Bounds.Width, 1f);  
            lineLayer.BorderColor = LineColorBehavior.GetLineColor(Element).ToCGColor();  
            control.TintColor = control.TextColor;  
        }  

        private class BorderLineLayer : CALayer  
        {  
        }  
    }  
}
```

`OnAttached`方法检索控件的本机控件 Xamarin.Forms [`Entry`](xref:Xamarin.Forms.Entry) ，并通过调用方法更新线条颜色 `UpdateLineColor` 。 `OnElementPropertyChanged`重写 `Entry` 通过在附加 `LineColor` 属性更改或更改的属性时更新线条颜色来响应控件上的可绑定属性更改 [`Height`](xref:Xamarin.Forms.VisualElement.Height) `Entry` 。 有关效果的更多信息，请参阅[效果](~/xamarin-forms/app-fundamentals/effects/index.md)。

当在控件中输入有效数据时 [`Entry`](xref:Xamarin.Forms.Entry) ，它将在控件的底部应用一条黑线，以指示不存在验证错误。 图6-3 显示了一个示例。

![](validation-images/validation-blackline.png "Black line indicating no validation error")

**图 6-3**：指示无验证错误的黑色线条

[`Entry`](xref:Xamarin.Forms.Entry)控件还将 [`DataTrigger`](xref:Xamarin.Forms.DataTrigger) 添加到其 [`Triggers`](xref:Xamarin.Forms.VisualElement.Triggers) 集合中。 下面的代码示例演示 `DataTrigger` ：

```xaml
<Entry Text="{Binding UserName.Value, Mode=TwoWay}">  
    ...  
    <Entry.Triggers>  
        <DataTrigger   
            TargetType="Entry"  
            Binding="{Binding UserName.IsValid}"  
            Value="False">  
            <Setter Property="behaviors:LineColorBehavior.LineColor"   
                    Value="{StaticResource ErrorColor}" />  
        </DataTrigger>  
    </Entry.Triggers>  
</Entry>
```

这会 [`DataTrigger`](xref:Xamarin.Forms.DataTrigger) 监视 `UserName.IsValid` 属性，如果其值为 `false` ，它将执行 [`Setter`](xref:Xamarin.Forms.Setter) ，这会将 `LineColor` 附加行为的附加属性更改 `LineColorBehavior` 为红色。 图6-4 显示了一个示例。

![](validation-images/validation-redline.png "Red line indicating validation error")

**图 6-4**：指示验证错误的红色行

[`Entry`](xref:Xamarin.Forms.Entry)当输入的数据无效时，控件中的行会保持为红色; 否则，它将更改为黑色以指示输入的数据有效。

有关触发器的详细信息，请参阅[触发器](~/xamarin-forms/app-fundamentals/triggers.md)。

### <a name="displaying-error-messages"></a>显示错误消息

UI 在其数据验证失败的每个控件下的标签控件中显示验证错误消息。 下面的代码示例演示了在 [`Label`](xref:Xamarin.Forms.Label) 用户未输入有效用户名的情况下显示验证错误消息的：

```xaml
<Label Text="{Binding UserName.Errors, Converter={StaticResource FirstValidationErrorConverter}}"  
       Style="{StaticResource ValidationErrorLabelStyle}" />
```

每个 [`Label`](xref:Xamarin.Forms.Label) 绑定到要 `Errors` 验证的视图模型对象的属性。 `Errors`属性由 `ValidatableObject<T>` 类提供，且的类型为 `List<string>` 。 由于 `Errors` 属性可能包含多个验证错误，因此将 `FirstValidationErrorConverter` 使用实例来检索要显示的集合中的第一个错误。

## <a name="summary"></a>摘要

EShopOnContainers 移动应用执行视图模型属性的同步客户端验证，通过突出显示包含无效数据的控件，并通过显示错误消息通知用户数据无效的原因，向用户通知任何验证错误。

需要验证的视图模型属性的类型为 `ValidatableObject<T>` ，并且每个 `ValidatableObject<T>` 实例都已将验证规则添加到其 `Validations` 属性中。 验证是通过调用实例的方法从视图模型调用的 `Validate` `ValidatableObject<T>` ，该方法检索验证规则并对属性执行这些验证规则 `ValidatableObject<T>` `Value` 。 任何验证错误都将放入 `Errors` 实例的属性 `ValidatableObject<T>` 中，并 `IsValid` 更新实例的属性 `ValidatableObject<T>` 以指示验证是成功还是失败。

## <a name="related-links"></a>相关链接

- [下载电子书（2Mb）](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers （GitHub）（示例）](https://github.com/dotnet-architecture/eShopOnContainers)
