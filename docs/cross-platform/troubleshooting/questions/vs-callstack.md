---
title: 如何收集 Visual Studio 进程的当前调用堆栈？
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 64c24b09-2c4a-43ad-b94d-6cd05a1aee44
author: davidortinau
ms.author: daortin
ms.date: 03/30/2017
ms.openlocfilehash: aa8ce8791c598fa4891257b3d832478ecc5ee136
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/22/2020
ms.locfileid: "86938796"
---
# <a name="how-do-i-collect-the-current-call-stacks-of-the-visual-studio-process"></a>如何收集 Visual Studio 进程的当前调用堆栈？

当 GUI 在 Visual Studio 中锁定（挂起、冻结）时，要收集的一系列重要诊断信息是 Visual Studio 进程的所有线程中的一组调用堆栈。 若要为 Visual Studio 的挂起实例保存此信息，可以使用 Visual Studio 的第二个实例：

1. 启动 Visual Studio 的第二个实例（新窗口）。

2. 关闭 Visual Studio 的新实例中的任何已打开的解决方案。

3. 选择 "**调试" > "附加到进程**"。

   ![选择 "调试" > 附加到进程 "](vs-callstack-images/image1.png)

4. 从“可用进程”**** 列表中选择 `devenv.exe` 原始挂起实例。

5. 选择“调试”>“全部中断”****。

   ![选择 "调试" > 全部中断](vs-callstack-images/image2.png)

6. 选择 "**调试" > "将转储另存为**"。

   ![选择 "调试" > 将转储另存为](vs-callstack-images/image3.png)

7. 将 "**保存类型**" 更改为 "**小型转储" （ \* dmp）**。 这将产生比**使用堆小型转储**更小的文件，而堆通常与诊断冻结无关。

   ![这将产生比使用堆小型转储更小的文件，而堆通常与诊断冻结无关](vs-callstack-images/image4.png)

8. 保存转储文件。 如果联机提交文件，可以对其进行压缩以减小大小。
