---
ms.topic: include
ms.openlocfilehash: b82ebeaeb28195e3ec0f5a0200c0448b6b76196a
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/13/2020
ms.locfileid: "61259469"
---
## <a name="sensor-speed"></a>[传感器速度](xref:Xamarin.Essentials.SensorSpeed)

- 最快  – 尽快获取传感器数据（不保证在 UI 线程上返回）。
- 游戏  – 适合游戏的速度（不保证在 UI 线程上返回）。
- 默认  - 适合屏幕方向更改的默认速率。
- UI  – 适合常规用户界面的速率。

如果事件处理程序不能保证在 UI 线程上运行，并且如果事件处理程序需要访问用户界面元素，请使用 [`MainThread.BeginInvokeOnMainThread`](~/essentials/main-thread.md) 方法在 UI 线程上运行该代码。
