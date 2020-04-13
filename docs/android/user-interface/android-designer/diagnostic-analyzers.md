---
title: 安卓布局诊断分析仪
description: 本指南列出了 Xamarin.Android 上当前支持的所有 Android 布局诊断分析仪。
ms.prod: xamarin
ms.assetid: BD252EA7-7E69-4DB4-96AB-D52CC0510C8F
ms.technology: xamarin-android
author: decriptor
ms.author: stepsha
ms.date: 04/07/2020
ms.openlocfilehash: 6203ce444bb97fa453a912a724f7f5724558b32a
ms.sourcegitcommit: 765b69ed451a0f48625ea597c3f39de95f3ae693
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/09/2020
ms.locfileid: "80987616"
---
# <a name="android-designer-diagnostic-analyzers"></a>Android 设计器诊断分析仪

本指南列出了当前支持的所有 Android 布局诊断分析器。

## <a name="accessibility"></a>可访问性

以下分析器有助于改进辅助功能支持：

| ID | Title | Severity | 说明 |
|----|-------|----------|-------------|
| ContentDescription | 没有图像`contentDescription` | 警告 | 图像`contentDescription`上缺少属性 |

## <a name="correctness"></a>正确性

以下分析器有助于修复布局中的正确性问题：

| ID | Title | Severity | 说明 | 帮助 |
|----|-------|----------|-------------|------|
| 适配器查看子级 | 适配器视图与子视图 | 警告 | 适配器视图不能在 XML 中具有子级 | [链接](xref:Android.Widget.AdapterView)。 |
| 缺少 Id | 片段应指定 或`id``tag` | 警告 |此`<fragment>`标记应指定`id`或`tag`，以在活动重新启动时保留状态 | [链接](xref:Android.App.Fragment)。 |
| 嵌套滚动垂直 | 嵌套垂直滚动元素 | 警告 | 嵌套滚动小部件 |
| 嵌套滚动水平 | 嵌套水平滚动元素 | 警告 | 嵌套滚动小部件 |
| 滚动查看大小 | 滚动查看fill_parent/match_parent大小错误的子级 | 警告 | 滚动查看fill_parent/match_parent大小错误的子级 |
| 滚动查看计数 | 滚动视图只能有一个子级 | 警告 | 滚动视图只能有一个子级 |
| 缺少 Android 命名空间 | 缺少属性上的 Android 命名空间 | 错误 | 缺少 Android XML 命名空间;您的属性将被解释为自定义属性 |
| 重复的ID | 重复的代用品 | 错误 | 单个布局中的重复 ID |
| 包括布局参数缺失宽度和高度 | 缺少宽度和高度 | 错误 | 在包括时忽略的布局参数 | [链接](https://stackoverflow.com/questions/2631614/does-android-xml-layouts-include-tag-really-work)。 |
| 包括布局参数缺失宽度 | 缺少宽度 | 错误 | 在包括时忽略的布局参数 | [链接](https://stackoverflow.com/questions/2631614/does-android-xml-layouts-include-tag-really-work)。 |
| 包括布局参数缺失高度 | 缺少高度 | 错误 | 在包括时忽略的布局参数 | [链接](https://stackoverflow.com/questions/2631614/does-android-xml-layouts-include-tag-really-work)。 |
| 方向 | 缺少显式方向 | 错误 | 缺少显式方向 |
| 可疑0dp | 可疑 0dp 尺寸 | 错误 | 可疑 0dp 尺寸 |
| 所需的 Size 宽度 | 缺少宽度属性 | 错误 | 缺少属性：layout_width |
| 所需的 Size Height | 缺少高度属性 | 错误 | 缺少属性：layout_height |
| 网页视图布局 | 家长wrap_content Web 视图 | 错误 |
| 错误案例 | 视图标记的案例错误 | 错误 | 视图标记的案例错误 | [链接](xref:Android.App.Fragment)。 |

## <a name="design"></a>设计

以下分析器有助于改进加入布局文件的方式：

| ID | Title | Severity | 说明 |
|----|-------|----------|-------------|
| 硬编码颜色 | 硬编码颜色 | 信息 | 硬编码颜色通常会导致不一致 |
| 硬编码尺寸  | 硬编码大小  | 信息 | 硬编码大小通常会导致不一致  |
| 硬编码文本  | 硬编码文本  | 警告 | 硬编码文本 |
| 未解决的资源 | 未解析的资源 URL | 警告 | 无法解析此资源 URL |
| Xml 错误 | XML 语法错误 | 错误 | XML 语法错误 |

## <a name="performance"></a>性能

以下分析器有助于提高布局的性能：

| ID | Title | Severity | 说明 |
|----|-------|----------|-------------|
| 嵌套权重 | 嵌套布局权重 | 警告 | 嵌套权重对性能不利 |
| 太多视图 | 布局的视图太多 | 警告 | 布局的视图太多 |
| 太深的布局 | 布局层次结构太深 | 警告 | 布局层次结构太深 |
| 无用的父母 | 无用的父布局 | 警告 | 无用的父布局 |
| 无用的叶子 | 无用的叶子布局 | 警告 | 这种`%1$s`观点是无用的（没有孩子，不`background`，不`id`，不`style`） |

## <a name="usability"></a>可用性

以下分析仪有助于提高客户的布局可用性：

| ID | Title | Severity | 说明 |
|----|-------|----------|-------------|
| 负利润 | 负边距 | 警告 | 负边距 |
| 缺少输入类型 | 编辑文本，没有输入类型 | 警告 | 未指定输入类型 |
| 输入类型电话 | 编辑文本似乎是一个电话号码 | 警告 | 视图名称表明这是一个电话号码，但它不包括`phone`在`inputType` |
| 输入类型编号 | 编辑文本似乎是一个数字 | 警告 | 视图名称表示这是一个数字，但它不包括数字`inputType`（如`numberDecimal`） |
| 输入类型密码 | 编辑文本似乎是密码 | 警告 | 视图名称表示这是密码，但它不包括`password`在`inputType`（如`textVisiblePassword`） |
| 输入类型 | 编辑文本似乎是 PIN | 警告 | 视图名称表明这是一个密码 （PIN），但它不包括`numberPassword`在`inputType` |
| 输入类型电子邮件 | 编辑文本似乎是一封电子邮件 | 警告 | 视图名称表示这是一个电子邮件地址，但它不包括`email`在`inputType`（如`textEmailAddress`） |
| 输入类型 | 编辑文本似乎是一个 URI | 警告 | 视图名称表明这是一个 URI，但它不包括`textUri`在`inputType` |
| 输入类型日期 | 编辑文本似乎是日期 | 警告 | 视图名称表示这是一个日期，但它不包括`date`在`inputType`（如`datetime`） |
