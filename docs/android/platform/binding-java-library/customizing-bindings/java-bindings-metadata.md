---
title: Java 绑定元数据
description: Xamarin.Android 中的 C# 代码通过绑定调用 Java 库，这些绑定是一种机制，用于提取 Java 本机接口 (JNI) 中指定的低级别详细信息。 Xamarin.Android 提供生成这些绑定的工具。 使用此工具，开发人员可以控制如何使用元数据创建绑定，进而允许修改命名空间和重命名成员等过程。 本文档讨论元数据的工作原理，概述了元数据支持的属性，并说明如何通过修改此元数据来解决绑定问题。
ms.prod: xamarin
ms.assetid: 27CB3C16-33F3-F580-E2C0-968005A7E02E
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/09/2018
ms.openlocfilehash: 25a5d79084f7caa78eec4011c047bd19a63ef748
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/10/2020
ms.locfileid: "75487784"
---
# <a name="java-bindings-metadata"></a>Java 绑定元数据

_Xamarin.Android 中的 C# 代码通过绑定调用 Java 库，这些绑定是一种机制，用于提取 Java 本机接口 (JNI) 中指定的低级别详细信息。Xamarin.Android 提供生成这些绑定的工具。使用此工具，开发人员可以控制如何使用元数据创建绑定，进而允许修改命名空间和重命名成员等过程。本文档讨论元数据的工作原理，概述了元数据支持的属性，并说明如何通过修改此元数据来解决绑定问题。_

## <a name="overview"></a>概述

Xamarin.Android Java 绑定库  尝试通过一个有时被称为“绑定生成器”  的工具来自动完成绑定现有 Android 库所需的大量工作。 绑定 Java 库时，Xamarin.Android 将检查 Java 类并生成所有要绑定的包、类型和成员的列表。 此 API 列表存储在一个 XML 文件中，在发行  版本中，该文件位于 \{project directory}\obj\Release\api.xml  ，在调试  版本中，该文件位于 \{project directory}\obj\Debug\api.xml  。

![obj/Debug 文件夹中的 api.xml 文件的位置](java-bindings-metadata-images/java-bindings-metadata-01.png)

绑定生成器将使用 api.xml  文件作为指导来生成所需的 C# 包装类。 此 XML 文件的内容是 Google Android 开源项目  格式的变体。
以下代码片段是 api.xml  文件的内容示例：

```xml
<api>
    <package name="android">
        <class abstract="false" deprecated="not deprecated" extends="java.lang.Object"
            extends-generic-aware="java.lang.Object" 
            final="true" 
            name="Manifest" 
            static="false" 
            visibility="public">
            <constructor deprecated="not deprecated" final="false"
                name="Manifest" static="false" type="android.Manifest"
                visibility="public">
            </constructor>
        </class>
...
</api>
```

在此示例中，api.xml  在扩展 `java.lang.Object` 的 `android` 包中声明一个 `Manifest` 类。

在许多情况下，需要人工协助使 Java API 感觉更像 .NET 或修正阻止绑定程序集编译的问题。 例如，可能需要将 Java 包名称更改为 .NET 命名空间、重命名类或更改方法的返回类型。

不能通过直接修改 api.xml  来实现这些更改。
而要在“Java 绑定库”模板提供的特殊 XML 文件中记录更改。 在编译 Xamarin.Android 绑定程序集时，绑定生成器在创建绑定程序集时将受到这些映射文件的影响

可以在项目的“转换”  文件夹中找到这些 XML 映射文件：

- **MetaData.xml** &ndash; 允许对最终 API 进行更改，如更改生成绑定的命名空间。 

- **EnumFields.xml** &ndash; 包含 Java `int` 常量和 C# `enums` 之间的映射。 

- **EnumMethods.xml** &ndash; 允许更改方法参数，并将返回类型从 Java `int` 常数更改为 C# `enums`。 

MetaData.xml  文件是这些文件中的最常见的导入，因为它允许对绑定进行一般用途的更改，例如：

- 重命名命名空间、类、方法或字段，使其遵循 .NET 约定。 

- 删除不需要的命名空间、类、方法或字段。 

- 将类移到不同的命名空间。 

- 添加其他支持类，使绑定的设计遵循 .NET 框架模式。 

让我们更详细地讨论 Metadata.xml  。

## <a name="metadataxml-transform-file"></a>Metadata.xml 转换文件

正如我们了解到的，绑定生成器使用文件 Metadata.xml  来影响绑定程序集的创建。
元数据格式使用 [XPath](https://www.w3.org/TR/xpath/) 语法，与 [GAPI 元数据](https://www.mono-project.com/docs/gui/gtksharp/gapi/#metadata)指南中介绍的 GAPI 元数据  几乎相同。 此实现几乎是 XPath 1.0 的完整实现，因此支持 1.0 标准版中的项。 此文件是一种功能强大的基于 XPath 的机制，可用于更改、添加、隐藏或移动 API 文件中的任何元素或属性。 元数据规范中的所有规则元素都包含一个路径属性，用于标识要向其应用规则的节点。 这些规则按以下顺序应用：

- **add-node** &ndash; 将子节点追加到 path 属性指定的节点。
- **attr** &ndash; 设置 path 属性指定的元素的属性值。
- **remove-node** &ndash; 删除与指定 XPath 匹配的节点。

下面是 Metadata.xml  文件的一个示例：

```xml
<metadata>
    <!-- Normalize the namespace for .NET -->
    <attr path="/api/package[@name='com.evernote.android.job']" 
        name="managedName">Evernote.AndroidJob</attr>

    <!-- Don't  need these packages for the Xamarin binding/public API --> 
    <remove-node path="/api/package[@name='com.evernote.android.job.v14']" />
    <remove-node path="/api/package[@name='com.evernote.android.job.v21']" />

    <!-- Change a parameter name from the generic p0 to a more meaningful one. -->
    <attr path="/api/package[@name='com.evernote.android.job']/class[@name='JobManager']/method[@name='forceApi']/parameter[@name='p0']" 
        name="name">api</attr>
</metadata>
```

下面列出了 Java API 一些比较常用的 XPath 元素：

- `interface` &ndash; 用于查找 Java 接口。 例如，`/interface[@name='AuthListener']`

- `class` &ndash; 用于查找类。 例如，`/class[@name='MapView']`

- `method` &ndash; 用于在 Java 类或接口上查找方法。 例如，`/class[@name='MapView']/method[@name='setTitleSource']`

- `parameter` &ndash; 标识方法的参数。 例如，`/parameter[@name='p0']`

### <a name="adding-types"></a>添加类型

`add-node` 元素将通知 Xamarin.Android 绑定项目将新包装类添加到 api.xml  。 例如，下面的代码片段将指示绑定生成器使用构造函数和单个字段创建类：

```xml
<add-node path="/api/package[@name='org.alljoyn.bus']">
    <class abstract="false" deprecated="not deprecated" final="false" name="AuthListener.AuthRequest" static="true" visibility="public" extends="java.lang.Object">
        <constructor deprecated="not deprecated" final="false" name="AuthListener.AuthRequest" static="false" type="org.alljoyn.bus.AuthListener.AuthRequest" visibility="public" />
        <field name="p0" type="org.alljoyn.bus.AuthListener.Credentials" />
    </class>
</add-node>
```

### <a name="removing-types"></a>删除类型

可能会指示 Xamarin.Android 绑定生成器忽略 Java 类型，而不绑定它。 这是通过将 `remove-node` XML 元素添加到 metadata.xml  文件来完成的：

```xml
<remove-node path="/api/package[@name='{package_name}']/class[@name='{name}']" />
```

### <a name="renaming-members"></a>重命名成员

不能通过直接编辑 api.xml  文件对成员进行重命名，因为 Xamarin.Android 需要原始 Java 本机接口 (JNI) 名称。 因此，不能更改 `//class/@name` 属性；如果更改，绑定将不起作用。

请考虑要重命名 `android.Manifest` 类型的情况。
为实现此目的，我们可能会尝试直接编辑 api.xml  并重命名类，如下所示：

```xml
<attr path="/api/package[@name='android']/class[@name='Manifest']" 
    name="name">NewName</attr>
```

这将导致绑定生成器为包装类创建以下 C# 代码：

```csharp
[Register ("android/NewName")]
public class NewName : Java.Lang.Object { ... }
```

请注意，包装类已重命名为 `NewName`，而原始 Java 类型仍为 `Manifest`。 Xamarin.Android 绑定类无法再访问 `android.Manifest` 上的任何方法；包装类绑定到不存在的 Java 类型。

若要正确更改包装类型（或方法）的托管名称，需要设置 `managedName` 属性，如以下示例所示：

```xml
<attr path="/api/package[@name='android']/class[@name='Manifest']" 
    name="managedName">NewName</attr>
```

<a name="Renaming_EventArg_Wrapper_Classes" />

#### <a name="renaming-eventarg-wrapper-classes"></a>重命名 `EventArg` 包装类

当 Xamarin.Android 绑定生成器侦听器类型  标识 `onXXX` setter 方法时，将生成一个 C# 事件和 `EventArgs` 子类，以支持基于 Java 的侦听器模式的 .NET 风格的 API。 例如，请考虑下面的 Java 类和方法：

```xml
com.someapp.android.mpa.guidance.NavigationManager.on2DSignNextManuever(NextManueverListener listener);
```

Xamarin.Android 会将前缀 `on` 从 setter 方法中删除，将 `2DSignNextManuever` 用作 `EventArgs` 子类的名称的基础。 子类名称如下所示：

```csharp
NavigationManager.2DSignNextManueverEventArgs
```

这不是合法的 C# 类名。 若要更正此问题，绑定作者必须使用 `argsType` 属性，并为 `EventArgs` 子类提供有效的 C# 名称：

```xml
<attr path="/api/package[@name='com.someapp.android.mpa.guidance']/
    interface[@name='NavigationManager.Listener']/
    method[@name='on2DSignNextManeuver']" 
    name="argsType">NavigationManager.TwoDSignNextManueverEventArgs</attr>
```

## <a name="supported-attributes"></a>支持的属性

以下各节介绍用于转换 Java API 的一些属性。

### <a name="argstype"></a>argsType

此属性位于 setter 方法上，用于命名将为支持 Java 侦听器而生成的 `EventArg` 子类。 这将在本指南后面[重命名 EventArg 包装类](#Renaming_EventArg_Wrapper_Classes)部分中详细介绍。

### <a name="eventname"></a>eventName

指定事件名称。 如果为空，它将禁止生成事件。
这将在标题为[重命名 EventArg 包装类](#Renaming_EventArg_Wrapper_Classes)的部分中详细介绍。

### <a name="managedname"></a>managedName

此属性用于更改包、类、方法或参数的名称。 例如，将 Java 类的名称从 `MyClass` 更改为 `NewClassName`：

```xml
<attr path="/api/package[@name='com.my.application']/class[@name='MyClass']" 
    name="managedName">NewClassName</attr>
```

下一个示例演示了如何使用 XPath 表达式将方法 `java.lang.object.toString` 重命名为 `Java.Lang.Object.NewManagedName`：

```xml
<attr path="/api/package[@name='java.lang']/class[@name='Object']/method[@name='toString']" 
    name="managedName">NewMethodName</attr>
```

### <a name="managedtype"></a>managedType

`managedType` 用于更改方法的返回类型。 在某些情况下，绑定生成器将错误地推断 Java 方法的返回类型，这将导致编译时错误。 在此情况下，一种可行的解决方案是更改方法的返回类型。

例如，绑定生成器认为 Java 方法 `de.neom.neoreadersdk.resolution.compareTo()` 应返回 `int` 并采用 `Object` 作为参数，这将导致错误消息“错误 CS0535：  DE.Neom.Neoreadersdk.Resolution’”不实现接口成员“Java.Lang.IComparable.CompareTo(Java.Lang.Object)”。 下面的代码片段演示如何将生成的 C# 方法的第一个参数类型从 `DE.Neom.Neoreadersdk.Resolution` 更改为 `Java.Lang.Object`： 

```xml
<attr path="/api/package[@name='de.neom.neoreadersdk']/
    class[@name='Resolution']/
    method[@name='compareTo' and count(parameter)=1 and
    parameter[1][@type='de.neom.neoreadersdk.Resolution']]/
    parameter[1]" name="managedType">Java.Lang.Object</attr> 
```

### <a name="managedreturn"></a>managedReturn

更改方法的返回类型。 这不会更改返回属性（因为更改返回属性会导致对 JNI 签名的不兼容更改）。 在下面的示例中，`append` 方法的返回类型从 `SpannableStringBuilder` 更改为 `IAppendable`（召回该 C# 不支持协变返回类型）：

```xml
<attr path="/api/package[@name='android.text']/
    class[@name='SpannableStringBuilder']/
    method[@name='append']" 
    name="managedReturn">Java.Lang.IAppendable</attr>
```

### <a name="obfuscated"></a>obfuscated

模糊处理 Java 库的工具可能会影响 Xamarin.Android 绑定生成器及其生成 C# 包装类的功能。 混淆类的特征包括： 

- 类名包括 $  ，即 a$.class 
- 类名完全由小写字母组成，即 a.class 

此代码片段举例说明如何生成“非混淆”C# 类型：

```xml
<attr path="/api/package[@name='{package_name}']/class[@name='{name}']" 
    name="obfuscated">false</attr>
```

### <a name="propertyname"></a>propertyName

此属性可用于更改托管属性的名称。

使用 `propertyName` 的一种特殊情况涉及到 Java 类只有一个 getter 方法用于字段的情况。 在这种情况下，绑定生成器将需要创建只写属性，而在 .NET 中不建议此做法。 以下代码片段演示了如何通过将 `propertyName` 设置为空字符串来“删除”.NET 属性：

```xml
<attr path="/api/package[@name='org.java_websocket.handshake']/class[@name='HandshakeImpl1Client']/method[@name='setResourceDescriptor' 
    and count(parameter)=1 
    and parameter[1][@type='java.lang.String']]" 
    name="propertyName"></attr>
<attr path="/api/package[@name='org.java_websocket.handshake']/class[@name='HandshakeImpl1Client']/method[@name='getResourceDescriptor' 
    and count(parameter)=0]" 
    name="propertyName"></attr>
```

请注意，setter 和 getter 方法仍将由绑定生成器创建。

### <a name="sender"></a>sender

当方法映射到事件时，指定方法的哪个参数应为 `sender` 参数。 值可以是 `true` 或 `false`。 例如：

```xml
<attr path="/api/package[@name='android.app']/
    interface[@name='TimePickerDialog.OnTimeSetListener']/
    method[@name='onTimeSet']/
    parameter[@name='view']" 
    name="sender">true</ attr>
```

### <a name="visibility"></a>可见性

此属性用于更改类、方法或属性的可见性。 例如，可能需要升级 `protected` Java 方法，使其对应的 C# 包装器为 `public`：

```xml
<!-- Change the visibility of a class -->
<attr path="/api/package[@name='namespace']/class[@name='ClassName']" name="visibility">public</attr>

<!-- Change the visibility of a method --> 
<attr path="/api/package[@name='namespace']/class[@name='ClassName']/method[@name='MethodName']" name="visibility">public</attr>
```

## <a name="enumfieldsxml-and-enummethodsxml"></a>EnumFields.xml 和 EnumMethods.xml

在某些情况下，Android 库使用整数常量表示传递到库的属性或方法的状态。 在许多情况下，将这些整数常量绑定到中 C# 中的枚举很有用。 若要简化此映射，请在绑定项目中使用 EnumFields.xml  和 EnumMethods.xml  文件。 

### <a name="defining-an-enum-using-enumfieldsxml"></a>使用 EnumFields.xml 定义枚举

EnumFields.xml  文件包含 Java `int` 常量和 C# `enums` 之间的映射。 下面的示例演示了如何为一组 `int` 常量创建 C# 枚举： 

```xml 
<mapping jni-class="com/skobbler/ngx/map/realreach/SKRealReachSettings" clr-enum-type="Skobbler.Ngx.Map.RealReach.SKMeasurementUnit">
    <field jni-name="UNIT_SECOND" clr-name="Second" value="0" />
    <field jni-name="UNIT_METER" clr-name="Meter" value="1" />
    <field jni-name="UNIT_MILIWATT_HOURS" clr-name="MilliwattHour" value="2" />
</mapping>
```

这里我们使用了 Java 类 `SKRealReachSettings`，并在命名空间 `Skobbler.Ngx.Map.RealReach` 中定义了一个名为 `SKMeasurementUnit` 的 C# 枚举。 `field` 条目定义 Java 常量的名称（示例 `UNIT_SECOND`）、枚举条目的名称（示例 `Second`）以及由两个实体表示的整数值（例如 `0`）。 

### <a name="defining-gettersetter-methods-using-enummethodsxml"></a>使用 EnumMethods.xml 方法定义 Getter/Setter 方法

EnumMethods.xml  文件允许更改方法参数，并将返回类型从 Java `int` 常数更改为 C# `enums`。 换句话说，它将 C# 枚举的读取和写入（在“EnumFields”  文件中定义）映射到 Java `int` 常量 `get` 和 `set` 方法。

根据上面定义的 `SKRealReachSettings` 枚举，以下“EnumMethods.xml”  文件将为此枚举定义 getter/setter：

```xml
<mapping jni-class="com/skobbler/ngx/map/realreach/SKRealReachSettings">
    <method jni-name="getMeasurementUnit" parameter="return" clr-enum-type="Skobbler.Ngx.Map.RealReach.SKMeasurementUnit" />
    <method jni-name="setMeasurementUnit" parameter="measurementUnit" clr-enum-type="Skobbler.Ngx.Map.RealReach.SKMeasurementUnit" />
</mapping>
```

第一个 `method` 行将 Java `getMeasurementUnit` 方法的返回值映射到 `SKMeasurementUnit` 枚举。 第二个 `method` 行将 `setMeasurementUnit` 的第一个参数映射到同一枚举。

完成所有这些更改后，可以使用 Xamarin.Android 中的以下代码设置 `MeasurementUnit`： 

```csharp
realReachSettings.MeasurementUnit = SKMeasurementUnit.Second;
```

## <a name="summary"></a>总结

本文讨论了 Xamarin.Android 如何使用元数据来转换 Google  AOSP 格式  的 API 定义。 在介绍了使用 Metadata.xml  可以进行的更改之后，本文研究了重命名成员时会遇到的限制，并展示了支持的 XML 属性列表，描述应在什么情况下使用相应的属性。

## <a name="related-links"></a>相关链接

- [使用 JNI](~/android/platform/java-integration/working-with-jni.md)
- [绑定 Java 库](~/android/platform/binding-java-library/index.md)
- [GAPI 元数据](https://www.mono-project.com/docs/gui/gtksharp/gapi/#metadata)
