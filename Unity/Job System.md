```C#
struct MyJob : IJob{
	void Execute(){}
}

MyJob job = new MyJob();
job.Schedule();
job.Complete();
job.Run();
```

`job.Schedule()` 
```C#
JobHandle Schedule<T>(this T jobData) {
var param = new JobScheduleParameters(&jobData, JobStruct<T>.Initialize());
return JobsUtility.Schedule(ref param);
}

struct JobScheduleParameters {
	IntPtr JobDataPtr; // MyObject实例的指针
	IntPtr ReflectionData;	// JobStruct<MyJob>.Initialize() 创建的Reflection Data。
	JobHandle Dependency;
	int ScheduleMode;
}

struct JobStruct<T> {
	static IntPtr jobReflectionData;

	static IntPtr Initialize() {
		jobReflectionData = JobsUtility.CreateJobReflectionData(typeof(T),new JobStruct<T>.Execute);
		return jobReflectionData;
	}
	static void Execute(...) {
		data.Execute(); // 调用MyJob.Execute()
	}
}
```