
```csharp
struct MyJob : IJob {
	void Execute();
}
```

Job创建后，需要被调度。Job.Schedule()，调用IJob.cs中的扩展函数，会先创建JobScheduleParameters，它保存了MyJob的地址、以及以MyJob类型和Execute回调创建的jobReflectionData IntPtr。最后调用JobsUtility.Schedule(ref parameters)，它映射C++的`ScheduleManagedJob`函数(JobsBindings.cpp)
在调用之前会将数据copy一份。
