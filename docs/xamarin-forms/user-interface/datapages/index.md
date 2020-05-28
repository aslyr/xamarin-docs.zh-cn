---
title: Xamarin.FormsDataPages
description: 本文介绍 Xamarin.Forms DataPages，它提供了一个 API，用于快速轻松地将数据源绑定到预先生成的视图。
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 7d99870dd975d0996ffcd05d4aef153f3515ec9e
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/28/2020
ms.locfileid: "84134311"
---
# <a name="xamarinforms-datapages"></a>Xamarin.FormsDataPages

![](~/media/shared/preview.png "This API is currently in preview")

> [!IMPORTANT]
> DataPages 需要 Xamarin.Forms 主题引用以呈现。 这涉及到安装[ Xamarin.Forms 。主题：基本](https://www.nuget.org/packages/Xamarin.Forms.Theme.Base/)NuGet 包到你的项目中，然后是[ Xamarin.Forms 。主题浅](https://www.nuget.org/packages/Xamarin.Forms.Theme.Light/)或[ Xamarin.Forms 。主题深色](https://www.nuget.org/packages/Xamarin.Forms.Theme.Dark/)NuGet 包。

Xamarin.FormsDataPages 于2016年推出，并以预览版的形式提供，供客户试用并提供反馈。

DataPages 提供了一个 API，用于快速轻松地将数据源绑定到预先生成的视图。 列表项和详细信息页将自动呈现数据，并可使用主题进行自定义。

若要查看演化的主题演示如何工作，请查看[入门指南](get-started.md)。

[![](images/demo-sml.png "DataPages Sample Application")](images/demo.png#lightbox "DataPages Sample Application")

## <a name="introduction"></a>简介

数据源和关联的数据页使开发人员能够快速轻松地使用受支持的数据源，并使用可使用主题自定义的内置 UI 基架对其进行呈现。

通过包括将 DataPages 添加到 Xamarin.Forms 应用程序** Xamarin.Forms 。页面**NuGet 包。

### <a name="data-sources"></a>数据源

预览版有一些可供使用的预生成数据源：

* **JsonDataSource**
* **AzureDataSource** （单独 NuGet）
* **AzureEasyTableDataSource** （单独 NuGet）

有关使用的示例，请参阅[入门指南](get-started.md) `JsonDataSource` 。

### <a name="pages--controls"></a>页面 & 控件

包括以下页面和控件，以便轻松绑定到提供的数据源：

* **ListDataPage** –请参阅[入门示例](get-started.md)。
* **DirectoryPage** –启用了分组的列表。
* **PersonDetailPage** –针对特定对象类型（联系人项）自定义的单个数据项视图。
* **DataView** –用于以一般方式从源公开数据的视图。
* **CardView** –包含图像、标题文本和说明文本的样式视图。
* **HeroImage** -图像呈现视图。
* 列表 **-预**建的视图，其布局类似于本机 IOS 和 Android 列表项。

有关示例，请参阅[DataPages 控件参考](controls.md)。

### <a name="under-the-hood"></a>在后台

Xamarin.Forms数据源遵循 `IDataSource` 接口。

Xamarin.Forms基础结构通过以下属性与数据源进行交互：

* `Data`–可显示的数据项的只读列表。
* `IsLoading`–一个布尔值，该值指示数据是否已加载并可用于呈现。
* `[key]`–用于检索元素的索引器。

有两种方法 `MaskKey` `UnmaskKey` 可用于隐藏（或显示）数据项属性（即 阻止其呈现）。
键对应于数据项对象的命名属性。
