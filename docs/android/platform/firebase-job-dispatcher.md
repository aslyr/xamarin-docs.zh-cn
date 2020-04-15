---
title: Firebase 作业调度程序
description: 本指南介绍如何使用 Google 提供的 Firebase 作业调度程序库计划后台工作。
ms.prod: xamarin
ms.assetid: 3DB9C7A3-D351-481D-90C5-BEC25D1B9910
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/05/2018
ms.openlocfilehash: 280fe11f935db0a364f3342b22bb9544cdda1e6d
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/13/2020
ms.locfileid: "73020236"
---
# <a name="firebase-job-dispatcher"></a>Firebase 作业调度程序

本指南介绍如何使用 Google 提供的 Firebase 作业调度程序库计划后台工作。 

## <a name="overview"></a>概述

让 Android 应用程序持续响应用户的最佳方法之一是，确保在后台执行复杂或长期工作。 然而，重要的是，后台工作不会对用户使用设备的体验产生负面影响。 

例如，后台作业可能每三到四分钟轮询一个网站，以查询对特定数据集的更改。 这似乎是良性的，但它会对电池使用时间产生灾难性影响。 应用程序将反复唤醒设备，将 CPU 提升到更高的功率状态，启动无线电，发出网络请求，然后处理结果。 由于设备不会立即断电，也不会回到低功耗空闲状态，因此情况变得更糟。 安排不当的后台工作可能会无意中让设备处于一种不必要的过度电源需求状态。 这种看似无害的活动（轮询网站）会导致设备在相对较短的时间内无法使用。

Android 提供了以下 API 来帮助在后台执行工作，但它们本身并不足以进行智能地计划作业。 

- **[意向服务](~/android/app-fundamentals/services/creating-a-service/intent-services.md)** &ndash; 意向服务非常适合执行工作，但无法计划工作。
- **[AlarmManager](https://developer.android.com/reference/android/app/AlarmManager.html)** &ndash; 这些 API 只能计划工作，而无法实际执行工作。 此外，AlarmManager 只允许基于时间的约束，这说明在特定时间或经过一段时间后发出警报。 
- **[JobScheduler](https://developer.android.com/reference/android/app/job/JobScheduler.html)** &ndash; JobSchedule 是一个很棒的 API，可与操作系统配合使用以计划作业。 不过，它只适用于目标 API 级别为 21 或更高级别的 Android 应用。 
- **[广播接收器](~/android/app-fundamentals/broadcast-receivers.md)** &ndash; Android 应用可以设置广播接收方，以便在响应系统范围内的事件或意向时执行工作。 但是，广播接收方不对作业运行时间进行任何控制。 同时，Android 操作系统中的更改将限制广播接收方的工作时间或它们可以响应的工作类型。 

高效执行后台工作（有时亦称为“后台作业”  或“作业”  ）的关键功能有两个：

1. **智能计划工作** &ndash; 重要的是，应用程序在后台以良好的状态执行工作。 理想情况下，应用程序不能要求运行某个作业。 而是应指定作业可以运行之前必须满足的条件，然后计划在满足条件时要运行的工作。 这样的话，Android 就能智能地执行工作。 例如，可以批量处理网络请求以同时全部运行，从而最大程度地利用与网络相关的开销。
2. **封装工作** &ndash; 应将用于执行后台工作的代码封装在一个独立的组件中，该组件可以独立于用户接口运行，并且如果由于某种原因而无法完成工作，重新计划起来也相对容易。

Firebase 作业调度程序是 Google 提供的一个库，它提供的 Fluent API 可以简化后台工作计划流程。 它旨在取代 Google Cloud Manager。 Firebase 作业调度程序包含以下 API：

- `Firebase.JobDispatcher.JobService`，它是一个抽象类，必须使用将在后台作业中运行的逻辑进行扩展。
- `Firebase.JobDispatcher.JobTrigger`，用于声明应何时启动作业。 通常以时间窗口的形式表示，例如，在启动作业之前等待至少 30 秒，但在 5 分钟内运行作业。
- `Firebase.JobDispatcher.RetryStrategy`，它包含在作业无法正确执行时应执行的操作的相关信息。 重试策略指定尝试再次运行作业之前要等待的时间。 
- `Firebase.JobDispatcher.Constraint`，这是一个可选值，用于描述在作业可以运行之前必须满足的条件，如设备处于非计量网络上或正在充电。
- `Firebase.JobDispatcher.Job`，此 API 将以前的 API 统一到可由 `JobDispatcher` 计划的工作单元中。 可以使用 `Job.Builder` 类实例化 `Job`。
- `Firebase.JobDispatcher.JobDispatcher`，它使用前面的三个 API 来计划操作系统的工作，并在必要时提供取消作业的方法。

若要计划使用 Firebase 作业调度程序，Xamarin.Android 应用程序必须将代码封装为一个可扩展 `JobService` 类的类型。 `JobService` 包含三种可以在作业生存期内调用的生命周期方法：

- **`bool OnStartJob(IJobParameters parameters)`** &ndash; 此方法用于执行工作，应始终实现此方法。 它在主线程上运行。 如果还有剩余工作未完成，此方法将返回 `true`，如果工作完成，则返回 `false`。 
- **`bool OnStopJob(IJobParameters parameters)`** &ndash; 出于某种原因停止作业时，将调用此方法。 如果应稍后重新计划作业，则它应返回 `true`。
- **`JobFinished(IJobParameters parameters, bool needsReschedule)`** &ndash; 当 `JobService` 完成任何异步工作时，将调用此方法。 

若要计划作业，应用程序将实例化 `JobDispatcher` 对象。 然后，使用 `Job.Builder` 创建 `Job` 对象，该对象将提供给尝试计划运行作业的 `JobDispatcher`。

本指南将介绍如何向 Xamarin.Android 应用程序添加 Firebase 作业调度程序，并使用它来计划后台工作。

## <a name="requirements"></a>要求

Firebase 作业调度程序需要 Android API 级别 9 或更高级别。 Firebase 作业调度程序库依赖于 Google Play Services 提供的某些组件；设备必须安装 Google Play Services。

## <a name="using-the-firebase-job-dispatcher-library-in-xamarinandroid"></a>在 Xamarin.Android 中使用 Firebase 作业调度程序库

要开始使用 Firebase 作业调度程序，请首先将 [Xamarin.Firebase.JobDispatcher NuGet 包](https://www.nuget.org/packages/Xamarin.Firebase.JobDispatcher) 添加到 Xamarin.Android 项目中。 在 NuGet 包管理器中搜索 Xamarin.Firebase.JobDispatcher  包（目前仍为预发行版）。

添加 Firebase 作业调度程序库后，创建一个 `JobService` 类，然后安排它随 `FirebaseJobDispatcher` 的实例一起运行。

### <a name="creating-a-jobservice"></a>创建 JobService

Firebase 作业调度程序库执行的所有工作都必须在扩展 `Firebase.JobDispatcher.JobService` 抽象类的类型中完成。 创建 `JobService` 的过程与使用 Android framework 创建 `Service` 非常相似： 

1. 扩展 `JobService` 类
2. 使用 `ServiceAttribute` 修饰子类。 尽管不是严格要求，但建议明确设置 `Name` 参数以辅助调试 `JobService`。 
3. 添加 `IntentFilter` 以声明 AndroidManifest.xml  中的 `JobService`。 这还有助于 Firebase 作业调度程序库找到并调用 `JobService`。

下面的代码示例是应用程序最简单的 `JobService`，它使用 TPL 异步执行一些工作：

```csharp
[Service(Name = "com.xamarin.fjdtestapp.DemoJob")]
[IntentFilter(new[] {FirebaseJobServiceIntent.Action})]
public class DemoJob : JobService
{
    static readonly string TAG = "X:DemoService";

    public override bool OnStartJob(IJobParameters jobParameters)
    {
        Task.Run(() =>
        {
            // Work is happening asynchronously (code omitted)
                       
        });

        // Return true because of the asynchronous work
        return true;  
    }

    public override bool OnStopJob(IJobParameters jobParameters)
    {
        Log.Debug(TAG, "DemoJob::OnStartJob");
        // nothing to do.
        return false;
    }
}
```

### <a name="creating-a-firebasejobdispatcher"></a>创建 FirebaseJobDispatcher

在计划任何工作之前，需要创建一个 `Firebase.JobDispatcher.FirebaseJobDispatcher` 对象。 `FirebaseJobDispatcher` 负责计划 `JobService`。 下面的代码片段演示一种创建 `FirebaseJobDispatcher` 实例的方法： 

 ```csharp
// This is the "Java" way to create a FirebaseJobDispatcher object
IDriver driver = new GooglePlayDriver(context);
FirebaseJobDispatcher dispatcher = new FirebaseJobDispatcher(driver);
```

在前面的代码片段中，`GooglePlayDriver` 类有助于 `FirebaseJobDispatcher` 与设备上 Google Play Services 中的某些计划 API 进行交互。 参数 `context` 是任意 Android `Context`，如 Activity。 目前，`GooglePlayDriver` 是 Firebase 作业调度程序库中唯一的 `IDriver` 实现。 

Firebase 作业调度程序的 Xamarin.Android 绑定提供了一种扩展方法，用于从 `Context` 创建 `FirebaseJobDispatcher`： 

```csharp
FirebaseJobDispatcher dispatcher = context.CreateJobDispatcher();
```

实例化 `FirebaseJobDispatcher` 后，便可以创建一个 `Job` 并在 `JobService` 类中运行代码。 `Job` 由 `Job.Builder` 对象创建，将在下一节中讨论。

### <a name="creating-a-firebasejobdispatcherjob-with-the-jobbuilder"></a>使用 Job.Builder 创建 Firebase.JobDispatcher.Job

`Firebase.JobDispatcher.Job` 类负责封装运行 `JobService` 所需的元数据。 `Job` 包含一些信息，如在作业可以运行之前必须满足的任何约束条件；`Job` 是否定期运行；或将导致运行作业的任何触发器。  最基本的要求是，`Job` 必须有一个标签  （一个可标识 `FirebaseJobDispatcher` 的作业的唯一字符串），以及一个应运行的 `JobService` 的类型。 在可以运行作业时，Firebase 作业调度程序将实例化 `JobService`。  `Job` 是使用 `Firebase.JobDispatcher.Job.JobBuilder` 类的实例创建的。 

下面的代码片段是使用 Xamarin.Android 绑定创建 `Job` 的最简单示例：

```csharp
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .Build();
```

`Job.Builder` 将对作业的输入值执行一些基本验证检查。 如果 `Job.Builder` 无法创建 `Job`，将会引发异常。  `Job.Builder` 将创建具有以下默认值的 `Job`：

- `Job` 的生存期  （计划运行的时间）在设备重新启动后结束 &ndash; 设备重新启动后，`Job` 将丢失。
- `Job` 不重复运行 &ndash; 它只运行一次。
- 将计划尽快运行 `Job`。
- `Job` 的默认重试策略是使用指数退避  （[设置 RetryStrategy](#Setting_a_RetryStrategy) 一节中进行了详细介绍）

### <a name="scheduling-a-job"></a>计划作业

创建 `Job`后，需要使用 `FirebaseJobDispatcher` 计划它的运行时间。 有两种方法可以计划 `Job`：

```csharp
// This will throw an exception if there was a problem scheduling the job
dispatcher.MustSchedule(myJob);

// This method will not throw an exception; an integer result value is returned
int scheduleResult = dispatcher.Schedule(myJob);
```

`FirebaseJobDispatcher.Schedule` 将返回以下整数值之一：

- `FirebaseJobDispatcher.ScheduleResultSuccess` &ndash; 已成功计划 `Job`。
- `FirebaseJobDispatcher.ScheduleResultUnknownError` &ndash; 发生了一些未知问题，导致无法计划 `Job`。
- `FirebaseJobDispatcher.ScheduleResultNoDriverAvailable` &ndash; 使用了无效的 `IDriver` 或因未知原因无法访问 `IDriver`。 
- `FirebaseJobDispatcher.ScheduleResultUnsupportedTrigger` &ndash; 不支持 `Trigger`。
- `FirebaseJobDispatcher.ScheduleResultBadService` &ndash; 服务配置不正确或不可用。

### <a name="configuring-a-job"></a>配置作业

可以自定义作业。 自定义作业的示例如下：

- [向作业传递参数](#Passing_Parameters_to_a_Job) &ndash; `Job` 可能需要其他值才能执行其工作，例如，下载文件。
- [设置约束](#Setting_Constraints) &ndash; 仅在满足某些条件时才有必要运行作业。 例如，仅在设备充电时运行 `Job`。 
- [指定 `Job` 应在何时运行](#Setting_Job_Triggers) &ndash; Firebase 作业调度程序允许应用程序指定应在何时运行作业。  
- [为失败的作业声明重试策略](#Setting_a_RetryStrategy) &ndash; 重试策略  为 `FirebaseJobDispatcher` 提供有关如何处理无法完成的 `Jobs` 的指导。 

以下各节将分别详细介绍这些主题。

<a name="Passing_Parameters_to_a_Job" />

#### <a name="passing-parameters-to-a-job"></a>向作业传递参数

通过创建与 `Job.Builder.SetExtras` 方法一起传递的 `Bundle`，向作业传递参数：

```csharp
Bundle jobParameters = new Bundle();
jobParameters.PutInt(FibonacciCalculatorJob.FibonacciPositionKey, 25);

Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetExtras(jobParameters)
                      .Build();

```

从 `OnStartJob` 方法上的 `IJobParameters.Extras` 属性访问 `Bundle`：

```csharp
public override bool OnStartJob(IJobParameters jobParameters)
{
    int position = jobParameters.Extras.GetInt(FibonacciPositionKey, DEFAULT_VALUE);
    
    // rest of code omitted
} 
```

<a name="Setting_Constraints" />

#### <a name="setting-constraints"></a>设置约束

约束有助于降低设备的成本或电池消耗。 `Firebase.JobDispatcher.Constraint` 类将这些约束定义为整数值：

- `Constraint.OnUnmeteredNetwork` &ndash; 仅在设备连接到非计量网络时才运行作业。 这有助于防止用户产生数据费用。
- `Constraint.OnAnyNetwork` &ndash; 在设备连接到的任何网络上运行作业。 如果与 `Constraint.OnUnmeteredNetwork` 一起指定，则优先使用此值。
- `Constraint.DeviceCharging` &ndash; 仅在设备充电时才运行作业。

使用 `Job.Builder.SetConstraint` 方法设置约束： 

```csharp
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetConstraint(Constraint.DeviceCharging)
                      .Build();
```

<a name="Setting_Job_Triggers" />

`JobTrigger` 可以让操作系统了解应启动作业的时间。 `JobTrigger` 有一个执行时段  ，用于定义应运行 `Job` 的计划时间。 执行时段包含“开始时段”  值和“结束时段”  值。 “开始时段”是设备在运行作业之前应等待的秒数，“结束时段”值是在运行 `Job` 之前要等待的秒数上限。 

可以使用 `Firebase.Jobdispatcher.Trigger.ExecutionWindow` 方法创建 `JobTrigger`。  例如，`Trigger.ExecutionWindow(15,60)` 表示作业应在计划后的 15 到 60 秒之间运行。 `Job.Builder.SetTrigger` 方法用于 

```csharp
JobTrigger myTrigger = Trigger.ExecutionWindow(15,60);
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetTrigger(myTrigger)
                      .Build();
```

作业的默认 `JobTrigger` 由 `Trigger.Now` 值表示，它指定在计划作业后尽快运行作业。

<a name="Setting_a_RetryStrategy" />

#### <a name="setting-a-retrystrategy"></a>设置 RetryStrategy

使用 `Firebase.JobDispatcher.RetryStrategy` 指定在尝试重新运行失败的作业之前，设备应使用的延迟量。 `RetryStrategy` 具有一个策略  ，该策略定义将使用哪种时间基准算法来重新计划失败的作业，还有一个执行时段，用于指定应在多久的时间内计划作业。 此重新计划时间段  由两个值定义。 第一个值是重新计划作业之前要等待的秒数（初始退避  值），第二个数字是在作业必须运行之前的秒数上限（最大退避  值）。 

这两种类型的重试策略均由以下 int 值标识：

- `RetryStrategy.RetryPolicyExponential` &ndash;  每次失败后，指数退避策略将以指数形式增加初始退避值。 作业第一次失败时，库将在重新计划作业之前等待指定的 _initial 时间间隔 &ndash; 例如 30 秒。 作业第二次失败时，库将等待至少 60 秒，然后再尝试运行作业。 第三次尝试失败后，库将等待 120 秒，依此类推。 Firebase 作业调度程序库的默认 `RetryStrategy` 由 `RetryStrategy.DefaultExponential` 对象表示。 它的初始退避值为 30 秒，退避值上限为 3600 秒。
- `RetryStrategy.RetryPolicyLinear` &ndash; 此策略为线性退避  ，定义应将作业重新计划为按设置的时间间隔运行（直到成功位置）。 线性退避最适用于必须尽快完成的工作，或者用于能够快速自行解决的问题。 Firebase 作业调度程序库定义了一个 `RetryStrategy.DefaultLinear`，其中的计划时段为最短 30 秒，最长 3600 秒。

使用 `FirebaseJobDispatcher.NewRetryStrategy` 方法可以定义自定义 `RetryStrategy`。 它使用三个参数：

1. `int policy` &ndash; 该策略  是以前的 `RetryStrategy` 值、`RetryStrategy.RetryPolicyLinear` 或 `RetryStrategy.RetryPolicyExponential` 之一。
2. `int initialBackoffSeconds` &ndash; 初始退避  是在尝试再次运行作业之前需要延迟的秒数。 此参数的默认值为 30 秒。 
3. `int maximumBackoffSeconds` &ndash; 最大退避  值声明尝试再次运行作业之前延迟的秒数上限。 默认值为 3600 秒。 

```csharp
RetryStrategy retry = dispatcher.NewRetryStrategy(RetryStrategy.RetryPolicyLinear, initialBackoffSeconds, maximumBackoffSet);

// Create a Job and set the RetryStrategy via the Job.Builder
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetRetryStrategy(retry)
                      .Build();
```

### <a name="cancelling-a-job"></a>取消作业

可以使用 `FirebaseJobDispatcher.CancelAll()` 方法取消已计划的全部作业，或使用 `FirebaseJobDispatcher.Cancel(string)` 方法方法取消已计划的单个作业：

```csharp
int cancelResult = dispatcher.CancelAll(); 

// to cancel a single job:

int cancelResult = dispatcher.Cancel("unique-tag-for-job");
```

每种方法都将返回一个整数值：

- `FirebaseJobDispatcher.CancelResultSuccess` &ndash; 已成功取消作业。
- `FirebaseJobDispatcher.CancelResultUnknownError` &ndash; 出现错误，无法取消作业。
- `FirebaseJobDispatcher.CancelResult.NoDriverAvailable` &ndash; `FirebaseJobDispatcher` 无法取消作业，因为没有可用的有效 `IDriver`。

## <a name="summary"></a>总结

本指南介绍了如何使用 Firebase 作业调度程序在后台智能地执行工作。 它介绍了如何封装要作为 `JobService` 执行的工作；如何使用 `FirebaseJobDispatcher` 来计划该工作，同时通过 `JobTrigger` 指定条件；以及如何使用 `RetryStrategy` 处理失败情况。

## <a name="related-links"></a>相关链接

- [NuGet 上的 Xamarin.Firebase.JobDispatcher](https://www.nuget.org/packages/Xamarin.Firebase.JobDispatcher)
- [GitHub 上的 firebase-job-dispatcher](https://github.com/firebase/firebase-jobdispatcher-android)
- [Xamarin.Firebase.JobDispatcher 绑定](https://github.com/xamarin/XamarinComponents/tree/master/Android/FirebaseJobDispatcher)
- [智能作业 - 计划](https://developer.android.com/topic/performance/scheduling.html)
- [Android 电池和内存优化 - Google I/O 2016（视频）](https://www.youtube.com/watch?v=VC2Hlb22mZM&feature=youtu.be)
