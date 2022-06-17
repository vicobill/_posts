
文件位置：

| 平台             | 文件                                                                       |
| ---------------- | -------------------------------------------------------------------------- |
| Win Editor       | Editor/Platform/Windows/WinEditorMain.cpp                                  |
| Linux Editor     | Editor/Platform/Linux/main.cpp                                             |
| OSX Editor       | Editor/Platform/OSX/main.mm                                                |
| Win Player       | PlatformDependent/WinPlayer/WinMain.cpp                                    |
| Linux Standalone | PlatformDependent/LinuxStandalone/LinuxMain.cpp                            |
| OSX Player       | PlatformDependent/OSXPlayer/main.mm                                        |
| Android Player   | PlatformDependent/AndroidPlayer/Source/com/unity3d/player/UnityPlayer.java |
| Lumin            | PlatformDependent/Lumin/Source/Main.cpp                                    |
| WebGL            | PlatformDependent/WebGL/Source/Main.cpp                                                                           |

WinPlayer flow：
ParseCommandLine() 解析命令行参数，进行基本设置
LoadScriptingRuntime()
> 加载Mono运行环境、Debugger环境等 见Runtime/Scripting/MonoLoader.cpp
> il2cpp:加载GameAssembly.dll, api函数，在External/il2cpp/builds/libil2cpp-api-functions.h
- PlayerInitEngineNoGraphics()
	- InitializeEngineNoGraphics() 在Runtime/Misc/SaveAndLoadHelper.cpp，初始化JobSystem、注册类型、注册PlayerLoop回调
	- 

InitializeMainWindow()
PlayerInitEngineGraphics()
	InitializeEngineGraphics()
	PlayerLoadGlobalManagers()
	ShaderLab::UpdateGlobalShaderProperties
InitInput()
PlayerLoadFirstScene()
BeginSplashScreen


> X::StaticInitialize() new静态或全局实例
> X::InitializeClass() 也大多是创建静态实例
> X::Initialize() 创建实例，也有创建其他与之相关的必要的实例
> 注册的Type ID在ClassRegisteration.inc.h中
> 