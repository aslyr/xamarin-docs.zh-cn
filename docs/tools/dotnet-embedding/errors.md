---
title: .NET 嵌入错误
description: 本文档介绍 .NET 嵌入生成的错误。 错误按代码和提供的说明来帮助进行故障排除。
ms.prod: xamarin
ms.assetid: 932C3F0C-D968-42D1-BB14-D97C73361983
author: davidortinau
ms.author: daortin
ms.date: 04/11/2018
ms.openlocfilehash: 9d3dff0de912455a49e9f7a66aae2640a99db746
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/09/2020
ms.locfileid: "84573906"
---
# <a name="net-embedding-errors"></a>.NET 嵌入错误

## <a name="em0xxx-binding-error-messages"></a>EM0xxx：绑定错误消息

例如 参数，环境

<!-- 0xxx: the generator itself, e.g. parameters, environment -->

<a name="EM0000"></a>

### <a name="em0000-unexpected-error---please-fill-a-bug-report-at-httpsgithubcommonoembeddinator-4000issues"></a>EM0000：意外错误-请在以下位置填写 bug 报告https://github.com/mono/Embeddinator-4000/issues

出现意外错误。 请尽可能多地说明[问题](https://github.com/mono/Embeddinator-4000/issues)，其中包括：

* 最大详细级别的完整生成日志
* 再现错误的最小测试用例
* 所有版本信息

获取准确版本信息的最简单方法是使用 " **Xamarin Studio** " 菜单，**关于 Xamarin Studio**项，"**显示详细信息**" 按钮，然后复制/粘贴版本信息（可以使用 "**复制信息**" 按钮）。

<a name="EM0001"></a>

### <a name="em0001-could-not-create-output-directory-x"></a>EM0001：无法创建输出目录`X`

指定的目录名称 `-o=DIR` 不存在并且无法创建。 它可能是无效的文件系统名称。

<a name="EM0002"></a>

### <a name="em0002-option-x-is-not-supported"></a>EM0002： `X` 不支持选项

该工具不支持选项 `X` 。 可能是该工具的另一个版本支持它，或者该 `X` 选项不会应用于此环境中。

<a name="EM0003"></a>

### <a name="em0003-the-platform-x-is-not-valid"></a>EM0003：平台 `X` 无效。

该工具不支持该平台 `X` 。 可能是该工具的另一个版本支持它，或者平台不适用于 `X` 此环境中。

<a name="EM0004"></a>

### <a name="em0004-the-target-x-is-not-valid"></a>EM0004：目标无效 `X` 。

该工具不支持目标 `X` 。 可能是该工具的另一个版本支持它，或者 `X` 目标不应用于此环境中。

<a name="EM0005"></a>

### <a name="em0005-the-compilation-target-x-is-not-valid"></a>EM0005：编译目标 `X` 无效。

该工具不支持编译目标 `X` 。 可能是该工具的另一个版本支持它，或者 `X` 编译目标不会应用于此环境中。

<a name="EM0006"></a>

### <a name="em0006-could-not-find-the-xcode-location"></a>EM0006：找不到 Xcode 位置。

该工具无法使用命令找到当前选定的 Xcode 位置 `xcode-select -p` 。 请验证是否可以成功运行此命令，并返回正确的 Xcode 位置。

<a name="EM0007"></a>

### <a name="em0007-could-not-get-the-sdk-version-for-sdk"></a>EM0007：无法获取 "{sdk}" 的 sdk 版本。

此工具无法使用命令获取 SDK 版本 `xcrun --show-sdk-version --sdk {sdk}` 。 请验证是否可以成功运行此命令，并返回 SDK 版本。

<a name="EM0008"></a>

### <a name="em0008-the-architecture-arch-is-not-valid-for-platform-valid-architectures-for-platform-are-architectures"></a>EM0008：体系结构 "{拱}" 对 "{platform}" 无效。 "{Platform}" 的有效体系结构为： "{体系结构}"。

错误消息中的体系结构对于目标平台无效。 请验证是否为该选项传递了有效的体系结构 `--abi` 。

<a name="EM0009"></a>

### <a name="em0009-the-feature-x-is-not-currently-implemented-by-the-generator"></a>EM0009：该功能 `X` 当前未由生成器实现

这是一个已知问题，我们打算在将来的版本中修复此问题。 欢迎使用发布内容。

<a name="EM0010"></a>

### <a name="em0010-cant-merge-the-frameworks-simulatorframework-and-deviceframework-because-the-file-file-exists-in-both"></a>EM0010：无法合并框架 "{simulatorFramework}" 和 "{deviceFramework}"，因为这两个文件中都存在文件 "{file}"。

该工具无法合并错误消息中提到的框架，因为它们之间有一个公共文件。

这可能表示 .NET 嵌入中的 bug;请 [https://github.com/mono/Embeddinator-4000/issues](https://github.com/mono/Embeddinator-4000/issues) 使用测试用例提交 bug 报告。

<a name="EM0011"></a>

### <a name="em0011-the-assembly-x-does-not-exist"></a>EM0011：程序集 `X` 不存在。

此工具找不到在 `X` 参数中指定的程序集。

<a name="EM0012"></a>

### <a name="em0012-the-assembly-name-x-is-not-unique"></a>EM0012：程序集名称 `X` 不唯一

提供的多个程序集具有相同的内部名称，因此在运行时无法区分它们。

最可能的原因是程序集在命令行参数中指定了多次。 但是，重命名的程序集保留其原始名称，多个副本不能共存。

<a name="EM0013"></a>

### <a name="em0013-cant-find-the-assembly-x-referenced-by-y"></a>EM0013：找不到 "Y" 引用的程序集 "X"。

该工具找不到程序集 "Y" 引用的程序集 "X"。 请确保所有引用的程序集都与要绑定的程序集位于同一目录中。

<a name="EM0014"></a>

### <a name="em0014-could-not-find-product-product-min_version-is-required"></a>EM0014：找不到 {product} （需要 {product} {min_version}）。

在系统上找不到错误消息中提到的依赖项。

<a name="EM0015"></a>

### <a name="em0015-could-not-find-a-valid-version-of-product-found-version-but-at-least-min_version-is-required"></a>EM0015：找不到有效版本的 {product} （找到 {version}，但至少需要 {min_version}）。

在系统上找到错误消息中提到的依赖项，但该依赖项太旧。 请更新到较新的版本。

<a name="EM0016"></a>

### <a name="em0016-could-not-create-symlink-file---target-error-number"></a>EM0016：无法创建符号 "{file}"-> "{target}"：错误 {number}

无法创建错误消息中提到的符号。

<a name="EM0026"></a>

### <a name="em0026-could-not-parse-the-command-line-argument-a-"></a>EM0026 无法分析命令行参数 "A"： *

工具无法分析为命令行选项提供的语法 `A` 。 请查看文档中是否有正确的语法。

<a name="EM0099"></a>

### <a name="em0099-internal-error--please-file-a-bug-report-with-a-test-case-httpsgithubcommonoembeddinator-4000issues"></a>EM0099：内部错误 *。 请使用测试用例（ https://github.com/mono/Embeddinator-4000/issues) 。

如果 .NET 嵌入中的内部一致性检查失败，则会报告此错误消息。

这表示 .NET 嵌入中的 bug;请 [https://github.com/mono/Embeddinator-4000/issues](https://github.com/mono/Embeddinator-4000/issues) 使用测试用例提交 bug 报告。

<!-- 1xxx: code processing -->

## <a name="em1xxx-code-processing"></a>EM1xxx：代码处理

<a name="EM1010"></a>

### <a name="em1010-type-t-is-not-generated-because-x-are-not-supported"></a>EM1010：不 `T` 生成类型，因为 `X` 不支持此类型。

这是一**条警告**，指出类型 `T` 将被忽略（即不会生成任何内容），因为它使用了 `X` 不受支持的功能。

注意：受支持的功能将随着新版本的工具而发展。

<a name="EM1011"></a>

### <a name="em1011-type-t-is-not-generated-because-it-lacks-marshaling-code-with-a-native-counterpart"></a>EM1011： `T` 不生成类型，因为它缺少包含本机对应项的封送处理代码。

这是一**条警告**，指出类型 `T` 将被忽略（即，不会生成任何内容），因为它公开了需要额外封送的 .net framework 中的某些内容。

注意：此工具的未来版本中可能会有一些限制，但有一些限制。

<a name="EM1020"></a>

### <a name="em1020-constructor-c-is-not-generated-because-of-parameter-type-t-is-not-supported"></a>EM1020： `C` 由于不支持参数类型，因此不生成构造函数 `T` 。

这是一个**警告**，指出 `C` 将忽略构造函数（即不会生成任何内容），因为 `T` 不支持类型为的参数。

应该会出现一个更早的警告，其中提供 `T` 了不受支持的类型的详细信息。

注意：受支持的功能将随着新版本的工具而发展。

<a name="EM1021"></a>

### <a name="em1021-constructor-c-has-default-values-for-which-no-wrapper-is-generated"></a>EM1021：构造函数 `C` 具有默认值，而不会生成任何包装。

这是构造**warning**函数的默认参数 `C` 未生成任何额外代码的警告。 最常见的原因是现有方法已具有相同的签名。 例如，在 .NET 中，可以：

```csharp
public class MyType {
    public MyType () { ... }
    public MyType (int i = 0) { ... }
}
```

在这种情况下，只会创建两个生成 `init` 的选择器，这两个均调入 Mono，但对于后者，则不存在包装。

<a name="EM1030"></a>

### <a name="em1030-method-m-is-not-generated-because-return-type-t-is-not-supported"></a>EM1030： `M` 不会生成方法，因为 `T` 不支持返回类型。

这是一**条警告**，指出 `M` 将忽略方法（即，不会生成任何内容），因为不支持其返回类型 `T` 。

应该会出现一个更早的警告，其中提供 `T` 了不受支持的类型的详细信息。

注意：受支持的功能将随着新版本的工具而发展。

<a name="EM1031"></a>

### <a name="em1031-method-m-is-not-generated-because-of-parameter-type-t-is-not-supported"></a>EM1031： `M` 由于不支持参数类型，因此不会生成方法 `T` 。

这是一**条警告**，指出 `M` 将忽略方法（即，不会生成任何内容），因为 `T` 不支持类型为的参数。

应该会出现一个更早的警告，其中提供 `T` 了不受支持的类型的详细信息。

注意：受支持的功能将随着新版本的工具而发展。

<a name="EM1032"></a>

### <a name="em1032-method-m-has-default-values-for-which-no-wrapper-is-generated"></a>EM1032：方法 `M` 具有未生成任何包装的默认值。

这是一个**警告**，即方法的默认参数 `M` 未生成任何其他代码。 最常见的原因是现有方法已具有相同的签名。 例如，在 .NET 中，可以：

```csharp
public class MyType {
    public int Increment () { ... }
    public int Increment (int i = 0) { ... }
}
```

在这种情况下，只会创建两个生成 `Increment` 的选择器，这两个均调入 Mono，但对于后者，则不存在包装。

<a name="EM1033"></a>

### <a name="em1033-method-m-is-not-generated-because-another-method-exposes-the-operator-with-a-friendly-name"></a>EM1033： `M` 不生成方法，因为另一个方法使用友好名称公开运算符。

这是一**条警告**，指出 `M` 不会生成方法，因为另一个方法使用友好名称公开运算符。 (https://msdn.microsoft.com/library/ms229032(v=vs.110).aspx)

<a name="EM1034"></a>

### <a name="em1034-extension-method-m-is-not-generated-inside-a-category-because-they-cannot-be-created-on-primitive-type-t-a-normal-static-method-was-generated"></a>EM1034： `M` 不会在类别内生成 Extension 方法，因为不能对基元类型创建扩展方法 `T` 。 已生成正常的静态方法。

这是对 primivite 类型（例如）的扩展方法的**警告** `System.Int32` 。 在客观-C 中，不能对基元类型创建类别。 相反，生成器将生成常规静态方法。

<a name="EM1040"></a>

### <a name="em1040-property-p-is-not-generated-because-of-parameter-type-t-is-not-supported"></a>EM1040： `P` 由于不支持参数类型，因此不生成属性 `T` 。

这是一**条警告**，指出 `P` 将忽略此属性（即，不会生成任何内容），因为 `T` 不支持公开的类型。

应该会出现一个更早的警告，其中提供 `T` 了不受支持的类型的详细信息。

注意：受支持的功能将随着新版本的工具而发展。

<a name="EM1041"></a>

### <a name="em1041-indexed-properties-on-t-is-not-generated-because-multiple-indexed-properties-are-not-supported"></a>EM1041：上的索引属性 `T` 不是生成的，因为不支持多个索引属性。

这是一**条警告**，指出的索引属性 `T` 将被忽略（即不会生成任何内容），因为不支持多个索引属性。

<a name="EM1050"></a>

### <a name="em1050-field-f-is-not-generated-because-of-field-type-t-is-not-supported"></a>EM1050： `F` 由于不支持字段类型，因此不生成字段 `T` 。

这是一**条警告**，指出字段 `F` 将被忽略（即，不会生成任何内容），因为 `T` 不支持公开的类型。

应该会出现一个更早的警告，其中提供 `T` 了不受支持的类型的详细信息。

注意：受支持的功能将随着新版本的工具而发展。

<a name="EM1051"></a>

### <a name="em1051-element-e-is-generated-instead-as-f-because-its-name-conflicts-with-an-important-objective-c-selector"></a>EM1051： `E` 改为生成元素， `F` 因为其名称与重要的目标-c 选择器冲突。

这是一**条警告**，指出元素 `E` 会生成为， `F` 因为其名称与重要的目标-c 选择器冲突。

[NSObjectProtocol](https://developer.apple.com/reference/objectivec/1418956-nsobject?language=objc)上的选择器在目标-c 中具有重要意义，必须谨慎地重写。

注意：保留的选择器列表将随着工具的新版本而发展。

<a name="EM1052"></a>

### <a name="em1052-element-e-is-not-generated-its-name-conflicts-with-other-elements-on-the-same-class"></a>EM1052： `E` 未生成元素，其名称与同一类上的其他元素冲突。

这是一个**警告**，指出元素的 `E` 名称与同一类上的其他元素冲突。

<a name="EM1053"></a>

### <a name="em1053-target-e-is-not-supported-for-xamarinios-and-xamarinmac-only-the-framework-option-is-considered-supported-and-should-be-used"></a>EM1053： `E` 不支持将 Target 用于 xamarin 和 xamarin。 仅将 `framework` 选项视为受支持，应使用。

这是针对**warning** `E` Xamarin 和 xamarin 使用事例的目标被视为不受支持的警告。 

静态或动态 .NET 嵌入库的使用可能需要额外的工作步骤或进行调整，在大多数用例中应避免使用。

请考虑删除 `--target` 参数或 `--target=framework` 改用。

<!-- 2xxx: code generation -->

## <a name="em2xxx-code-generation"></a>EM2xxx：代码生成

<!-- 3xxx: reserved -->
<!-- 4xxx: reserved -->
<!-- 5xxx: reserved -->
<!-- 6xxx: reserved -->
<!-- 7xxx: reserved -->
<!-- 8xxx: reserved -->
<!-- 9xxx: reserved -->
