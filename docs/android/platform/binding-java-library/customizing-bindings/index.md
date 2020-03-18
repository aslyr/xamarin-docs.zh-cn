---
title: 自定义绑定
description: 可以通过编辑控制绑定流程的元数据来自定义 Xamarin Android 绑定。 通常需要进行这些手动修改以解决生成错误，并调整生成的 API，使其与 C#/.NET 更加一致。 这些指南介绍此元数据的结构，如何修改元数据，以及如何使用 JavaDoc 恢复方法参数的名称。
ms.prod: xamarin
ms.assetid: 63C5078D-9E42-4F70-AF8C-8CEEA84FB6AF
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 09/25/2017
ms.openlocfilehash: 04f3720d8684129476c955819390e91330a7800a
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/10/2020
ms.locfileid: "73020644"
---
# <a name="customizing-bindings"></a>自定义绑定

_可以通过编辑控制绑定流程的元数据来自定义 Xamarin Android 绑定。通常需要进行这些手动修改以解决生成错误，并调整生成的 API，使其与 C#/.NET 更加一致。这些指南介绍此元数据的结构，如何修改元数据，以及如何使用 JavaDoc 恢复方法参数的名称。_

## <a name="overview"></a>概述

Xamarin.Android 自动执行大部分绑定流程；但某些情况下，需要手动修改才能解决以下问题：

- 解决因缺少类型、混淆类型、重复名称、类可见性问题，以及 Xamarin Android 工具无法处理的其他情况而导致的生成错误。 

- 更改映射，Xamarin.Android 使用该映射将 Android API 绑定到 C# 中的不同类型（例如，许多开发人员喜欢将 Java `int` 常量映射到 C# `enum` 常量）。

- 删除无需绑定的未使用类型。 

- 添加在基础 Java API 中没有对应项的类型。 

可以修改控制绑定流程的元数据以进行上述部分或全部更改。

## <a name="guides"></a>参考线

以下指南介绍控制绑定流程的元数据，并说明如何修改此元数据以解决这些问题：

- [Java 绑定元数据](~/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata.md)概述用于 Java 绑定的元数据。
    它介绍完成 Java 绑定库有时所需的各种手动步骤，并说明如何对绑定公开的 API 进行调整，以更严格地遵循 .NET 设计指南。

- [使用 Javadoc 命名参数](~/android/platform/binding-java-library/customizing-bindings/naming-parameters-with-javadoc.md)说明如何使用绑定 Java 项目生成的 Javadoc 恢复 Java 绑定项目中的参数名称。
