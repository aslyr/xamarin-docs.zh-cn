---
title: ''Xamarin.Essentials:电话拨号程序'' description:'Xamarin.Essentials 中的 PhoneDialer 类使应用程序可以在拨号程序中拨打电话号码' ms.assetid: author: ms.custom: ms.author: ms.date: no-loc:
- 'Xamarin.Forms'
- 'Xamarin.Essentials'

---

# <a name="xamarinessentials-phone-dialer"></a>Xamarin.Essentials：电话拨号程序

PhoneDialer 类使应用程序能够在拨号程序中打开一个电话号码。

## <a name="get-started"></a>入门

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-phone-dialer"></a>使用电话拨号程序

在类中添加对 Xamarin.Essentials 的引用：

```csharp
using Xamarin.Essentials;
```

通过调用具有要用于打开拨号程序的一个电话号码的 `Open` 方法来使用电话拨号程序功能。 当请求 `Open` 时，API 将自动尝试根据国家/地区代码设置号码的格式（如果已指定）。

```csharp
public class PhoneDialerTest
{
    public void PlacePhoneCall(string number)
    {
        try
        {
            PhoneDialer.Open(number);
        }
        catch (ArgumentNullException anEx)
        {
            // Number was null or white space
        }
        catch (FeatureNotSupportedException ex)
        {
            // Phone Dialer is not supported on this device.
        }
        catch (Exception ex)
        {
            // Other error has occurred.
        }
    }
}
```

## <a name="api"></a>API

- [电话拨号程序源代码](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/PhoneDialer)
- [电话拨号程序 API 文档](xref:Xamarin.Essentials.PhoneDialer)

## <a name="related-video"></a>相关视频

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Phone-Dialer-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
