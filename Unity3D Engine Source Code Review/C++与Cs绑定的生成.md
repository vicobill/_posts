由工具Tools/BindingGenerator处理。
有2种工作方式：
- Patch：对已有Assembly打补丁
- CreateCpp：生成CPP绑定
## CreateCpp
- include:包含文件，依据IBindingsHeaderProviderAttribute的 NativeHeader,NativeType描述添加头文件。




C++调用C#：
ScriptingInvocation::Invoke() 中，调用C ABI: `scripting_method_invoke()`,位于ScriptBackend/Mono/ScriptingApi_Mono。最终调用 MONO API `mono_runtime_invoke` 调用函数。



CallStaticMonoMethod()