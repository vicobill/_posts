由工具Tools/BindingGenerator处理。
有2种工作方式：
- Patch：对已有Assembly打补丁
- CreateCpp：生成CPP绑定
## CreateCpp
- include:包含文件，依据IBindingsHeaderProviderAttribute的 NativeHeader,NativeType描述添加头文件。
- 