---
title: Android 作业计划程序
description: 本指南介绍了如何使用 Android 作业计划程序 API 来计划后台工作。
ms.prod: xamarin
ms.assetid: 673BB8C3-C5CC-43EC-BA8F-758F15D986C9
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/19/2018
ms.openlocfilehash: 10d2ae6ac35f02d75ef6e04a0531ec3f5dafd668
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/13/2020
ms.locfileid: "76940818"
---
# <a name="android-job-scheduler"></a>Android 作业计划程序

本指南介绍了如何使用 Android 作业计划程序 API 来计划后台工作，此 API 在运行 Android 5.0（API 级别 21）及更高版本的 Android 设备上可用。 

## <a name="overview"></a>概述 

让 Android 应用程序持续响应用户的最佳方法之一是，确保在后台执行复杂或长期工作。 然而，重要的是，后台工作不会对用户使用设备的体验产生负面影响。 

例如，后台作业可能每三到四分钟轮询一个网站，以查询对特定数据集的更改。 这似乎是良性的，但它会对电池使用时间产生灾难性影响。 应用程序将反复唤醒设备，将 CPU 提升到更高的功率状态，启动无线电，发出网络请求，然后处理结果。 由于设备不会立即断电，也不会回到低功耗空闲状态，因此情况变得更糟。 安排不当的后台工作可能会无意中让设备处于一种不必要的过度电源需求状态。 这种看似无害的活动（轮询网站）会导致设备在相对较短的时间内无法使用。

Android 提供了以下 API 来帮助在后台执行工作，但它们本身并不足以进行智能地计划作业。 

- **[意向服务](~/android/app-fundamentals/services/creating-a-service/intent-services.md)** &ndash; 意向服务非常适合执行工作，但无法计划工作。
- **[AlarmManager](https://developer.android.com/reference/android/app/AlarmManager.html)** &ndash; 这些 API 只能计划工作，而无法实际执行工作。 此外，AlarmManager 只允许基于时间的约束，即在特定时间或在特定时间段过后发出警报。 
- **[广播接收器](~/android/app-fundamentals/broadcast-receivers.md)** &ndash; Android 应用可以设置广播接收器来执行工作，以响应系统范围内事件或意向。 不过，广播接收器无法控制作业应何时运行。 此外，Android 操作系统更改也会限制广播接收器的运行时间或可以响应的工作类型。 

高效执行后台工作（有时亦称为“后台作业”  或“作业”  ）的关键功能有两个：

1. **智能计划工作** &ndash; 重要的是，应用程序在后台以良好的状态执行工作。 理想情况下，应用程序不得要求运行作业。 相反，应用程序应指定作业可以运行前必须满足的条件，然后使用在满足条件时执行工作的操作系统来计划此作业。 这样，Android 就可以运行作业，以确保设备达到最高效率。 例如，可以将网络请求批处理为全部同时运行，以最大限度地利用与网络相关的开销。
2. **封装工作** &ndash; 应将用于执行后台工作的代码封装在独立组件中，此组件可以独立于用户界面运行，并且如果工作由于某种原因而无法完成，重新计划起来也相对容易。

Android 作业计划程序是内置在 Android 操作系统中的框架，提供了简化如何计划后台工作的 Fluent API。  Android 作业计划程序由以下类型组成：

- `Android.App.Job.JobScheduler`：系统服务，用于代表 Android 应用程序计划、执行和（如果需要）取消作业。
- `Android.App.Job.JobService`：抽象类，必须使用在应用程序主线程中运行作业的逻辑来扩展它。 也就是说，`JobService` 负责如何异步执行工作。
- `Android.App.Job.JobInfo` 对象：保留指导 Android 何时应运行作业的条件。

若要使用 Android 作业计划程序计划工作，Xamarin.Android 应用程序必须将代码封装在扩展 `JobService` 类的类中。 `JobService` 包含三种可以在作业生存期内调用的生命周期方法：

- **bool OnStartJob(JobParameters 参数)** &ndash; 此方法由 `JobScheduler` 调用来执行工作，并在应用程序主线程中运行。 `JobService` 负责异步执行工作，并在有剩余工作时返回 `true`，或在工作完成时返回 `false`。
    
    当 `JobScheduler` 调用此方法时，它会在作业期间向 Android 请求并保留唤醒锁。 在作业完成时，`JobService` 负责通过调用 `JobFinished` 方法（如下文所述）将此事实告知 `JobScheduler`。

- **JobFinished(JobParameters 参数, bool needsReschedule)** &ndash; 必须由 `JobService` 调用此方法，以告知 `JobScheduler` 工作已完成。 如果 `JobFinished` 未获调用，`JobScheduler` 就不会删除唤醒锁，导致不必要的电池消耗。 

- **bool OnStopJob(JobParameters 参数)** &ndash; 此方法在 Android 过早停止作业时调用。 如果应根据重试条件重新计划作业（下文将更详细地讨论），它应返回 `true`。

可以指定约束  或触发器  ，用于控制作业何时可以或应该运行。 例如，可以将作业约束为，仅在设备充电时运行，或在拍摄照片时启动作业。

本指南将详细介绍如何实现 `JobService` 类，并使用 `JobScheduler` 计划它。

## <a name="requirements"></a>要求

Android 作业计划程序要求使用 Android API 级别 21 (Android 5.0) 或更高版本。 

## <a name="using-the-android-job-scheduler"></a>使用 Android 作业计划程序

使用 Android JobScheduler API 有三个步骤：

1. 实现 JobService 类型来封装工作。
2. 使用 `JobInfo.Builder` 对象来创建 `JobInfo` 对象，后者包含 `JobScheduler` 运行作业所需满足的条件。 
3. 使用 `JobScheduler.Schedule` 计划作业。

### <a name="implement-a-jobservice"></a>实现 JobService

Android 作业计划程序库执行的所有工作都必须在扩展 `Android.App.Job.JobService` 抽象类的类型中完成。 创建 `JobService` 非常类似于使用 Android 框架创建 `Service`： 

1. 扩展 `JobService` 类。
2. 使用 `ServiceAttribute` 修饰子类，并将 `Name` 参数设置为由包名称和类名组成的字符串（见以下示例）。
3. 将 `ServiceAttribute` 的 `Permission` 属性设置为字符串 `android.permission.BIND_JOB_SERVICE`。
4. 重写 `OnStartJob` 方法，同时添加执行工作的代码。 Android 会在应用程序主线程中调用此方法来运行作业。 应在线程上执行耗时长于几毫秒的工作，以免应用程序受阻。
5. 在工作完成后，`JobService` 必须调用 `JobFinished` 方法。 `JobService` 通过此方法告知 `JobScheduler` 作业已完成。 无法调用 `JobFinished` 会导致对 `JobService` 设备产生不必要的需求，缩短电池使用时间。 
6. 最好还是重写 `OnStopJob` 方法。 当作业在完成之前关闭时，此方法由 Android 调用，并为 `JobService` 提供了正确释放任何资源的机会。 如果需要重新计划作业，此方法应返回 `true`；如果不需要重新运行作业，此方法应返回 `false`。

下面的代码示例展示了应用程序的最简单 `JobService`，它使用 TPL 异步执行某工作：

```csharp
[Service(Name = "com.xamarin.samples.downloadscheduler.DownloadJob", 
         Permission = "android.permission.BIND_JOB_SERVICE")]
public class DownloadJob : JobService
{
    public override bool OnStartJob(JobParameters jobParams)
    {            
        Task.Run(() =>
        {
            // Work is happening asynchronously
                      
            // Have to tell the JobScheduler the work is done. 
            JobFinished(jobParams, false);
        });

        // Return true because of the asynchronous work
        return true;  
    }

    public override bool OnStopJob(JobParameters jobParams)
    {
        // we don't want to reschedule the job if it is stopped or cancelled.
        return false; 
    }
}
```

### <a name="creating-a-jobinfo-to-schedule-a-job"></a>创建 JobInfo 以计划作业

Xamarin Android 应用程序不直接实例化 `JobService`，而是将 `JobInfo` 对象传递到 `JobScheduler`。 `JobScheduler` 会实例化请求的 `JobService` 对象，同时根据 `JobInfo` 中的元数据来计划和运行 `JobService`。 `JobInfo` 对象必须包含以下信息：

- **JobId** &ndash; 这是用于向 `JobScheduler` 标识作业的 `int` 值。 重用此值会更新全部现有作业。 对于应用程序，此值必须是唯一的。 
- **JobService** &ndash; 此参数为 `ComponentName`，显式标识 `JobScheduler` 应该用来运行作业的类型。 

此扩展方法展示了如何使用 Android `Context` 创建 `JobInfo.Builder`（如 Activity）：

```csharp
public static class JobSchedulerHelpers
{
    public static JobInfo.Builder CreateJobBuilderUsingJobId<T>(this Context context, int jobId) where T:JobService
    {
        var javaClass = Java.Lang.Class.FromType(typeof(T));
        var componentName = new ComponentName(context, javaClass);
        return new JobInfo.Builder(jobId, componentName);
    }
}

// Sample usage - creates a JobBuilder for a DownloadJob and sets the Job ID to 1.
var jobBuilder = this.CreateJobBuilderUsingJobId<DownloadJob>(1);

var jobInfo = jobBuilder.Build();  // creates a JobInfo object.
```

Android 作业计划程序的一项强大功能是，能够控制作业何时运行或在什么条件下运行。 下表介绍了 `JobInfo.Builder` 的一些方法，这些方法可便于应用影响作业何时可以运行：  

|  方法 | 描述   |
|---|---|
| `SetMinimumLatency`  | 指定在作业运行前应观察到的延迟（以毫秒为单位）。 |
| `SetOverridingDeadline`  | 声明作业必须在此时间（以毫秒为单位）结束前运行。 |
| `SetRequiredNetworkType`  | 指定作业的网络要求。 |
| `SetRequiresBatteryNotLow` | 仅当设备不向用户显示“电池电量低”警告时，作业才运行。 |
| `SetRequiresCharging` | 仅当电池充电时，作业才运行。 |
| `SetDeviceIdle` | 作业在设备忙时运行。 |
| `SetPeriodic` | 指定作业应定期运行。 |
| `SetPersisted` | 作业应跨设备重新启动暂留。 | 

`SetBackoffCriteria` 提供了一些指导，可便于了解 `JobScheduler` 应等待多长时间才能尝试重新运行作业。 退避条件包括两个部分：以毫秒为单位的延迟（默认值为 30 秒），以及应使用的退避类型（有时亦称为“退避策略”  或“重试策略”  ）。 下面两个策略封装在 `Android.App.Job.BackoffPolicy` 枚举中：

- `BackoffPolicy.Exponential` &ndash; 指数退避策略在每次失败后以指数方式增加初始退避值。 当作业第一次失败时，库会先等待指定的初始时间间隔（例如 30 秒），再重新计划作业。 当作业第二次失败时，库会先等待至少 60 秒，再尝试运行作业。 在第三次尝试失败后，库会等待 120 秒，依此类推。 这是默认值。
- `BackoffPolicy.Linear` &ndash; 此策略为线性退避，即应将作业重新计划为按设置的时间间隔运行（直到成功为止）。 线性退避最适合必须尽快完成的工作，或很快就能自行解决的问题。 

若要详细了解如何创建 `JobInfo` 对象，请参阅 [`JobInfo.Builder` 类的 Google 文档](https://developer.android.com/reference/android/app/job/JobInfo.Builder.html)。

#### <a name="passing-parameters-to-a-job-via-the-jobinfo"></a>通过 JobInfo 向作业传递参数

通过创建与 `Job.Builder.SetExtras` 方法一起传递的 `PersistableBundle`，向作业传递参数：

```csharp
var jobParameters = new PersistableBundle();
jobParameters.PutInt("LoopCount", 11);

var jobBuilder = this.CreateJobBuilderUsingJobId<DownloadJob>(1)
                     .SetExtras(jobParameters)
                     .Build();
```

`PersistableBundle` 是从 `JobService` 的 `OnStartJob` 方法的 `Android.App.Job.JobParameters.Extras` 属性访问：

```csharp
public override bool OnStartJob(JobParameters jobParameters)
{
    var loopCount = jobParams.Extras.GetInt("LoopCount", 10);
    
    // rest of code omitted
} 
```

### <a name="scheduling-a-job"></a>计划作业

为了计划作业，Xamarin Android 应用程序会获取对 `JobScheduler` 系统服务的引用，并使用上一步中创建的 `JobInfo` 对象来调用 `JobScheduler.Schedule` 方法。 `JobScheduler.Schedule` 会立即返回以下两个整数值之一：

- **JobScheduler. ResultSuccess** &ndash; 已成功计划作业。 
- **JobScheduler. ResultFailure** &ndash; 无法计划作业。 这通常是由冲突的 `JobInfo` 参数造成的。

下面的代码示例展示了如何计划作业，并向用户通知作业计划尝试结果：

```csharp
var jobScheduler = (JobScheduler)GetSystemService(JobSchedulerService);
var scheduleResult = jobScheduler.Schedule(jobInfo);

if (JobScheduler.ResultSuccess == scheduleResult)
{
    var snackBar = Snackbar.Make(FindViewById(Android.Resource.Id.Content), Resource.String.jobscheduled_success, Snackbar.LengthShort);
    snackBar.Show();
}
else
{
    var snackBar = Snackbar.Make(FindViewById(Android.Resource.Id.Content), Resource.String.jobscheduled_failure, Snackbar.LengthShort);
    snackBar.Show();
}
```

### <a name="cancelling-a-job"></a>取消作业

可以使用 `JobsScheduler.CancelAll()` 方法取消已计划的全部作业，也可以使用 `JobScheduler.Cancel(jobId)` 方法取消已计划的单个作业：

```csharp
// Cancel all jobs
jobScheduler.CancelAll(); 

// to cancel a job with jobID = 1
jobScheduler.Cancel(1)
```
  
## <a name="summary"></a>总结

本指南讨论了如何使用Android 作业计划程序在后台智能地执行工作。 具体介绍了如何将要执行的工作封装为 `JobService`；如何使用 `JobScheduler` 计划此工作，同时通过 `JobTrigger` 指定条件；以及应如何使用 `RetryStrategy` 处理失败尝试。

## <a name="related-links"></a>相关链接

- [智能作业 - 计划](https://developer.android.com/topic/performance/scheduling.html)
- [JobScheduler API 参考](https://developer.android.com/reference/android/app/job/JobScheduler.html)
- [使用 JobScheduler 像专业人员一样计划作业](https://medium.com/google-developers/scheduling-jobs-like-a-pro-with-jobscheduler-286ef8510129)
- [Android 电池和内存优化 - Google I/O 2016（视频）](https://www.youtube.com/watch?v=VC2Hlb22mZM&feature=youtu.be)
- [Android JobScheduler-René Ruppert](https://www.youtube.com/watch?v=aSjBBPYjelE)
