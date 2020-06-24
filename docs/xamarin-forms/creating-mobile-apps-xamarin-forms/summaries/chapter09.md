---
title: 第 9 章摘要。 特定于平台的 API 调用
description: 使用 Xamarin.Forms 创建移动应用：第 9 章摘要。 特定于平台的 API 调用
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 4FFA1BD4-B3ED-461C-9B00-06ABF70D471D
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: e8feb636057f1e11c7df90236dee44697203d51c
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/18/2020
ms.locfileid: "84136846"
---
# <a name="summary-of-chapter-9-platform-specific-api-calls"></a>第 9 章摘要。 特定于平台的 API 调用

[![下载示例](~/media/shared/download.png) 下载示例](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09)

> [!NOTE] 
> 此页上的“注意”指出了 Xamarin.Forms 与书中所述内容的不同之处。

有时需要运行一些因平台而异的代码。 本章将探讨这些方法。

## <a name="preprocessing-in-the-shared-asset-project"></a>共享资产项目中的预处理

Xamarin.Forms 共享资产项目可使用 C# 预处理器指令（`#if`、`#elif` 和 `endif`）为各个平台执行不同的代码。 [**PlatInfoSap1**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/PlatInfoSap1) 中对此进行了演示：

[![可变格式段落的三重屏幕截图](images/ch09fg01-small.png "设备型号和操作系统")](images/ch09fg01-large.png#lightbox "设备型号和操作系统")

但是，结果代码可能比较乱且难以阅读。

## <a name="parallel-classes-in-the-shared-asset-project"></a>共享资产项目中的并行类

[**PlatInfoSap2**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/PlatInfoSap2) 示例中演示了一种更加结构化的方法，用于在 SAP 中执行特定于平台的代码。 每个平台项目都包含同名的类和方法，但都是针对特定平台实现的。 然后，SAP 只需实例化该类并调用该方法。

## <a name="dependencyservice-and-the-portable-class-library"></a>DependencyService 和可移植类库

> [!NOTE] 
> 可移植类库已替换为 .NET Standard 库。 书中的所有示例代码都已转换为使用 .NET Standard 库。

库通常不能访问应用程序项目中的类。 此限制似乎会阻止在库中使用 PlatInfoSap2 中显示的方法****。 不过，Xamarin.Forms 包含一个名为 [`DependencyService`](xref:Xamarin.Forms.DependencyService) 的类，该类使用 .NET 反射从库中访问应用程序项目中的公共类。

库必须定义一个 `interface`，其中包含需要在每个平台中使用的成员。 然后，每个平台都要包含该接口的实现。 用于实现接口的类必须在程序集级别使用 [DependencyAttribute](xref:Xamarin.Forms.DependencyAttribute) 进行标识。

然后，库使用 `DependencyService` 的泛型 [`Get`](xref:Xamarin.Forms.DependencyService.Get*) 方法获取用户实现该接口的平台类的实例。

[**DisplayPlatformInfo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/DisplayPlatformInfo) 示例对此进行了演示。

## <a name="platform-specific-sound-generation"></a>特定于平台的声音生成

[**MonkeyTapWithSound**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/MonkeyTapWithSound) 示例通过访问每个平台中的声音生成功能，将蜂鸣声添加到 MonkeyTap 程序中****。

## <a name="related-links"></a>相关链接

- [第 9 章全文 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch09-Apr2016.pdf)
- [第 9 章示例](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09)
- [依赖项服务](~/xamarin-forms/app-fundamentals/dependency-service/index.md)
