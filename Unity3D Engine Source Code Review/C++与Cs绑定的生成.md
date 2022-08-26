由工具Tools/BindingGenerator处理。
有2种工作方式：
- Patch：对已有Assembly打补丁
- CreateCpp：生成CPP绑定
## CreateCpp
- include:包含文件，依据IBindingsHeaderProviderAttribute的 NativeHeader,NativeType描述添加头文件。




## C++调用C#：
ScriptingInvocation::Invoke() 中，调用C ABI: `scripting_method_invoke()`,位于ScriptBackend/Mono/ScriptingApi_Mono。最终调用 MONO API `mono_runtime_invoke` 调用函数。
> IL2CPP也有类似的il2cpp_runtime_invoke函数。

C++调用C#中的函数，由生成器，生成类似<ClassName>Proxy::<MethodName>的实现。典型实现：

``` C++
void Scripting::UnityEngine::Rendering::RenderPipelineManagerProxy::DoRenderLoop_Internal(ScriptingObjectPtr pipe, void* loopPtr, ScriptingObjectPtr renderRequests, ScriptingExceptionPtr *outException))
{
    ScriptingMethodPtr method = GetCoreScriptingClasses().doRenderLoop_Internal;
    ScriptingInvocation invocation(method);
	// 下面依次给脚本的方法传入参数
	// 对应RenderPipelineManager.cs中实现的 DoRendererLoop_Internal(RenderPipelineAsset pipe, IntPtr loopPtr, List<Camera.RenderRequest> renderRequests)方法参数。
    invocation.AddObject(pipe); 
    invocation.AddIntPtr(loopPtr);
    invocation.AddObject(renderRequests);

    if (outException != NULL)
    {
        invocation.logException = false;
        invocation.Invoke<void>(outException);
    }
    else
    {
        invocation.Invoke<void>();
    }
}
// 脚本方法指针，也是由codegenerator生成
// RequireMethod 参数为 Assembly, Namespace, Class, Method  
// 定义在CommonScriptingClasses.cpp中。
s_CoreScriptingClasses->doRenderLoop_Internal = RequireMethod ("UnityEngine.CoreModule.dll", "UnityEngine.Rendering", "RenderPipelineManager", "DoRenderLoop_Internal");
```

## CS调用C++

CallStaticMonoMethod()