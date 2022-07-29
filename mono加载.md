LoadMonoCallback(): 加载Mono时的回调函数。会从`mono-*.dll`中加载Mono的C ABI符号。
RegisterMonoPathRemapper();
LoadUnityMonoAssemblies();
LoadDomainAndUnityAssemblies();
LoadUserAssemblies();

```c++
void LoadUnityMonoAssemblies()
{
	UpdateTargetAssembliesInMonoManager(); // 加载UnityEngine.dll, UnityEditor.dll,以及UnityEngine.AI.Module.dll 等module的dlls
	LoadDomainAndUnityAssemblies();
}
```


```c++
void LoadDomainAndUnityAssemblies()
{
	// 1. 注册Domain Reloading/Reloaded/Unload 等事件
	// ...
	GetMonoManager().ReloadAssembly();

	// 去掉上面1.注册的回调
	ModuleManagerProxy()::InitializePlatformSupportModules(); // 调用mono脚本的initializePlatformSupportModules ScriptingMethodPtr的方法。 
	
	// 创建 Library/ScriptingAssemblies 目录
}

```


* ModuleManagerProxy: 是C++调脚本方法的代理。
- ScriptingMethodPtr：脚本方法指针
- ScriptingInvocation: 调用。通过ScriptingInvocation.Invoke()调用脚本方法。


MonoManager.ReloadAssembly