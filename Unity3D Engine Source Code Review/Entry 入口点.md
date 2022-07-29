
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

# WinEditorMain
```c++
int WinMain(...) 
{
	winutils::CrashHandler crashHandler;
	crashHandler.Install();

	...
	if (AssetDatabase::IsAssetImportWorkerProcess())
		AssetDatabase::InitializeLicenseInWorker();
	
	ConfigureCrashHandler();
	CurlRequestInitialize();

	InitializeCurrentDirectory();
	CPUInfo::VerifySSE2Support();
	TrySetupLogFile();

	GiveDebuggerChanceToAttachIfRequired();
	WaitForPixTimingCaptureIfRequired();

	RunNativeTestsIfRequiredAndExit();

	g_SingleAppInstance.FindOtherInstance();

	ILicensing::BootstrapLicensing();
	GetILicensing()->ProcessLicenseOperation();

	ShowSplashScreen();

	InitializeDragAndDrop();

	BeginBusyCursor();

	DisplayBleedingEdgeWarningIfRequired();

	GetApplicationFolder();
	GetApplicationContentsPath();

	ShouldStartBugReporterOnCrash();

	DisableWindowGhosting();

	SetupDefaultPreferences();

	SetLoadMonoCallback(LoadMonoCallback, NULL);

	EnsureBusyCursor();

	ParseGfxDeviceArgs();

	InitializeContainerWindowClasses();
    InitializeEditorWindowClasses();

	new Application(); // ...

	SetupInternetReachability();

	GetApplication().InitializeProject(); // 初始化项目，内部会调用InitializeEngineNoGraphics()函数。

	GetApplication().FinishLoadingProject();

	GetMainEditorWindow();
	if(batchMode)
	{
		mainEditorWindow = CreateMainWindowForBatchMode();
		SetMainEditorWindow(mainEditorWindow);
	}else {
		SetSplashScreenParent(mainEditorWindow);
		GetScreenManager().SetWindow(mainEditorWindow);
		InputInit(mainEditorWindow);
		UpdateMainMenu();
		HideSplashScreen();
		gInitialized=true;
		SetActiveWindow(mainEditorWindow);
	}

	GetApplication().AfterEverythingLoaded();

	ForceShowCursor();

	GlobalCallbacks::Get().editorTerminateCanceled.Register(ResetRequestedRelaunchArguments);

	MainMessageLoop(); // 主循环

	winutils::UninstallInternalCrashHandler();
	GlobalCallbacks::Get().editorTerminateCanceled.Unregister(ResetRequestedRelaunchArguments);

	Shutdown();
	UnloadMono();
	ShutdownDragAndDrop();
	RegisterLogPreprocessor(NULL);
}
```

## Application::InitializeProject() 加载并初始化项目

```c++
void Application::InitializeProject()
{
	PackageManager::StartServer();

	Thread::mainThreadId = CurrentThread::GetID();
	ThreadAndSerializationSafeCheck::InitFromMainThread();

	GetSceneTracker().AddSceneInspector(this);
	ActiveEditorTracker::InitializeSharedTracker();
	EditorMonoConsole::RegisterAsConsoleLogTarget();
	RegisterRemoveImportErrorFromConsole(RemoveLogImplementation);
	RegisterShowErrorWithMode(ShowFirstErrorWithMode);

	PrintARGV();
	ConnectMacroClient(port);

	InitProjectSettings();
	GetActualAbsolutePathSlow(GetProjectPath());

	while(true)
	{
		SetProjectPath(absoluteProjectPath, forgetProjectPath);
		SetCocoaCurrentDirectory(GetProjectPath());
		GetCocoaCurrentDirectory();
		GetFileSystem().SetCurrentDirectory(cocoaCurrentDirectory);

		EnsureSuitableFileSystem();
		DeleteFileOrDirectory(kTempFolder);
		CreateDirectorySafe(kTempFolder);
	}

	autoprogress::AutoBackgroundProgressBarInit();
	CreateDirectory(kLogsFolder);

    AssetDatabase::SelectVersionFromCurrentProject(IsCreatingProject());

    AssetDatabase::InitGUIDPersistentManager();
    RegisterAPIUpdaterCallbacksForAssetAndPackageImporting();

	InitializeEngineNoGraphics(); // 初始化引擎中非Graphics部分

	CloneProjectTemplate(GetProjectCreationParams().cloneFromTemplate, GetProjectPath(), errorMessage)

	PackageManager::Initialize();
	
	CreateDirectory(AssetDatabase::kUserSettingsFolder);
	MoveFileOrDirectory(kOldEditorUserSettingsPath, kEditorUserSettingsPath);
	ScriptCompilationStartUp();

	//! CRITICAL
	AssetDatabase::InitializeAssetDatabase(...); // 创建项目所需的各项.asset以及项目目录如Assets,Library,ProjectSettings等
	UpgradeProjectAssetsIfNecessary();

	Virtualization_Initialize(GetProjectPath(), IsBatchmode()); // ???

	ProcessVCSCmdArgs(openProjectProgressIterator);
	GetPlayerSettingsPtr();
	GetPersistentManager().WriteFile(AssetDatabase::kProjectSettingsPath, BuildTargetSelection::NoTarget(), kAllowTextSerialization);

	EditorMonoConsole::RegisterScriptUpdateCallbacks();
	SetLastOpenedSceneForTemplatedProject();
	BuildTargetDiscovery::GetInstance().FindAvailableBuildTargets();
	GetEditorUserBuildSettings().GetActiveBuildTarget();
	GetEditorUserBuildSettings().GetActiveBuildTargetGroup();
	GetBuildTargetGroup(originalActiveBuildTarget);

	ValidateBuildTargetSupport(originalActiveBuildTarget, originalActiveBuildTargetGroup,...);
	GetEditorUserBuildSettings().SetActiveBuildTargetAndGroup(originalActiveBuildTargetGroup, originalActiveBuildTarget);
	GetEditorUserBuildSettings().SetSelectedBuildTargetGroup(originalActiveBuildTargetGroup);

	GlobalCallbacks::Get().editorBuildTargetAvailable.Invoke();

	PluginManager::Get().RefreshPlugins(true);
	INVOKE_GLOBAL_CALLBACK(initializedPreloadedPlugins);

	GetGpuDeviceManager().SetDevice(...);

	// CRITICAL
	InitializeEngineGraphics(); // 初始化图形
	EnsureSupportedGfxDevice(graphicsInitialized);

	ShaderLab::UpdateGlobalShaderProperties(); // 更新全局Shader中的_Time相关数据

	BuiltinResourceManager::SetPreventLoadingBuiltinResources(preventResources); 
	AddCleanLogEntryHandler(&ResourceBuildCleanLogHandler);
	SetNeedToPerformRendering(false, NeedToStopRenderingReasons::kBuildingResources);

	InitializeAutoDocumentation(); // 自动生成的文档
	CustomLighting::Initialize();

	GetSceneManager().PrepareNewBootstrapScene(); // 创建一个新的`UnityScene*`
	gi::InitializeManagers(); // 实例化GI相关的类
	BuiltinResourceManager::LoadAllDefaultResourcesFromEditor(); // 加载默认的Shader和MonoScript

	RegisterUnityExtensions(); // UnityExtensions注册UnityEngine.PerformanceTesting.dll

	GetPersistentManager().WriteFile(AssetDatabase::kProjectSettingsPath);

	// 初始化MONO
	CallLoadMonoCallback();
	RegisterMonoPathRemapper(); // mono路径重新映射

	// 更新Shader包含目录 如 ., Shaders/Includes, CGInclude,以及shadercompiler命令注册
	ShaderImporter::UpdateShaderIncludePaths(); 

	
	AssetDatabase::AllowImporting();
	APIUpdaterManager::Initialize();

	/* 加载UnityEngine.dll,UnityEditor.dll以及各个*Module.dll并纳入MonoManager管理，
	注册mono assembly 的 Load,Reload,Unload等等事件
	重新构建Assembly Classes
	*/
	LoadUnityMonoAssemblies(); 

	CleanupAfterLoad();

	LoadUserAssemblies();
	AssetDatabase::RunAssetImporter();

	AssetDatabase::RegisterPostprocessCallback(WindowTitlePostprocess);

	ReadLicenseInfo();

	GlobalCallbacks::Get().didReloadMonoDomain.Register(MonoDomainWasReloaded);
    OnCompilationPipelineCompileStarted.Register(OnCompileScriptsStarted);

	GetShaderCompilerPerThread();
	UpgradeShadersIfNeeded();

	graphicsSettings.SetDefaultShaderReferences();
	GetPersistentManager().WriteFile(graphicsSettingsPath, BuildTargetSelection::NoTarget(), options);

	graphicsSettings.SetDefaultAlwaysIncludeduGUIShaders();
	graphicsSettings.AwakeFromLoad(kDefaultAwakeFromLoad);

	AssetDatabase::InitialScriptRefresh();

	AssetDatabase::Refresh(AssetDatabase::kForceSynchronousImport);

	GetResourceManager().SetResourcesNeedRebuild();

	ShaderImporter::RegisterAllKnownShaders();

	ComputeShaderImporter::LoadAllKnownComputeShaders();

	GetGraphicsSettings().UpdateTierSettingsForCurrentBuildTarget();

	AssetDatabase::ImportAssetsWithMismatchingAssetImporterVersion(AssetDatabase::kForceSynchronousImport);

	ImportPackagesAndSetTemplateWhenCreatingProject();
	UpdateRuntimeHashes();
	
	
	AssetDatabase::VerifyAssetsForBuildTarget(false, AssetDatabase::kNoCancel);

	GetPlayerSettings().SyncCurrentColorSpace(true);

	OnUsbDevicesChanged();

	EditorProfilerConnection::Get().SetupTargetSpecificConnection(GetEditorUserBuildSettings().GetActiveBuildTarget());

	AssetDatabase::SaveAssets();

	AssetDatabaseDeprecated::AssetInstanceCacheUpdate();

	UnityExtensions::Get().Initialize();

	InitializeManagedCompilationPipeline();

	CallStaticMonoMethod("SyncVS", "SyncVisualStudioProjectIfItAlreadyExists");

	ExecuteStartups();

	MenuController::RegisterDidExecuteMenuItem(&RecordMacroMenuItemAndAnalytics);
	MenuController::UpdateAllMenus();
	RebuildOSMenu();

	INVOKE_GLOBAL_CALLBACK(editorInitializingProject);

	WriteEditorInstanceJsonFile();

	CheckForAvailableDiskSpaceInProjectLocationAndEmitWarning();

	GetProjectVersion().SetEditorVersionAndWrite();
	AssetDatabase::GetRefreshAnalytics(refreshAnalytics);
	AssetDatabase::SendRefreshAnalytics(refreshAnalytics);

	GetMonoScriptManager().InitializeRegisteredScripts();

	AssetDatabase::FullConsistencyCheck(doReimport);

	UpdateInteractionModeSettings();

}
```

### InitializeEngineGraphics()
```c++
bool InitializeEngineGraphics(bool batch)
{
	InitializeGfxDevice();
	
	InitScalableBufferManager();

	ShaderLab::InitShaderLab();
	ShaderPassContextInitialize();

	TypeManager::Get().CallPostInitializeTypes();
	MessageHandler::Get().ResolveCallbacks();

	BuiltinResourceManager::InitializeAllResources();
	RayTracingAccelerationStructureManager::Initialize();
	InitializeVirtualTexturing();
	InitializeMeshVertexFormatManager();
	Shader::LoadDefaultShaders();
	InitializeRecordGlobalShaderVariantCollection();
	InitializeShaderCompilationManager();
	GetGraphicsSettings().InitShaderDefines();
	REGISTER_GLOBAL_CALLBACK(exitPlayModeAfterOnEnableInEditMode, "RendererScene.ClearIntermediateRenderers", GetRendererScene().ClearIntermediateRenderers());
	g_sIsEngineGraphicsInitialized = true;
	GetGfxDevice().SetSRGBWrite(GetActiveColorSpace() == kLinearColorSpace);
	INVOKE_GLOBAL_CALLBACK(initializedEngineGraphics);
}
```


## Application::FinishLoadingProject()
```c++
void Application::FinishLoadingProject()
{
	RestoreLastOpenedScenes();
	CopyDefaultSearchFiltersIfNeeded();

	Scripting::UnityEditor::WindowLayoutProxy::LoadDefaultWindowPreferences();

	m_IsInitialized = true;

	INVOKE_GLOBAL_CALLBACK(editorFinishedLoadingProject);

	UpdateSettingsTask::RebuildVCMenus();

	ParseARGVCommands();

	OpenAssetStoreURLAfterLaunch();
	Scripting::UnityEditor::EditorApplicationProxy::Internal_ProjectWasLoaded();
	UpdateMainWindowTitle();
	EditorUpdateCheck();
}
```

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


> X::StaticInitialize() 创建静态或全局实例
> X::InitializeClass() 也大多是创建静态实例
> X::Initialize() 创建实例，也有创建其他与之相关的必要的实例
> 注册的Type ID在ClassRegisteration.inc.h中


Application.cpp是Application主要逻辑部分。
- UnityHub的打开：在`Application.InitProjectSettings()`时会打开UnityHub
- 项目的打开：
	- Application会先删除Temp再创建Temp文件夹。
	- Application使用Temp/UnityLockfile能否被创建，判断项目是否被另外的Unity实例使用。
	- 创建Logs目录。如果存在Logs目录，则删除`shadercompiler-`前缀的日志文件。
	- 从`adb2.txt`文件读取配置

### InitializeEngineNoGraphics()
- InitializeEngineNoGraphics():
  - PlayerConnection::Initialize()
  - InstallPlayerConnectionLogging
  - EditorConnection::Initialize()
  - ProfilerConnection::Initialize()
  - EditorProfilerConnection::Initialize()
  - FrameDebugger::Initialize(): 注册FrameDebugger回调。
  - 
	- 创建JobSystem，创建Worker JobQueue、Background JobQueue以及JobBatchDispatcher
	- 注册所有类型ID


```c++
/** @ SaveAndLoadHelper.cpp */

void InitializeEngineNoGraphics()
{
	EditorAssetGarbageCollectManager::StaticInitialize();
	Thread::mainThreadId = CurrentThread::GetID();
	PlayerConnection::Initialize(GET_DATA_FOLDER);
	InstallPlayerConnectionLogging(true);
	EditorConnection::Initialize();
	ProfilerConnection::Initialize();
	EditorProfilerConnection::Initialize();
	FrameDebugger::Initialize();
	PlayerConnection::Get().Poll();
	EditorConnection::Get().PollWithCustomMessage();
	EditorShaderLivelinkManager::Instance()->Initialise();
	ShaderLivelinkManager::Instance()->Initialise();
	JobSystem::CreateJobSystem();
	InitializeBatchDelete(); // 创建 BatchDeleteManager, 并启动线程 “BatchDeleteObjects” 执行 BatchDeleteStep2Threaded 任务。

	RegisterAllClasses(); // 注册所有类型ID，包括废弃的、预留的、内置的、模块中的等等类型的ID
	TypeManager::Get().InitializeAllTypes(); // 初始化所有类型的派生信息？

	ExecutionOrderManager::InitializeClass(); // 创建 ExecutionOrderManager
	AwakeFromLoadQueue::InitializeClass(); // 设置一些类型的默认执行顺序
	ManagedAttributeManager::InitializeClass(); // 创建 ManagedAttributeManager

	SerializedFileManager::Initialize(); // 用于RuntimeRemap的typeid，对 LegacyPersistentTypeIDAttribute、RuntimeRemapFromPersistentTypeIDAttribute 属性进行remap

	InitializeAsyncReadManager();// 创建 AsyncReadManagerThreaded，并创建线程"Loading.AsyncRead" 执行 StaticThreadEntry 任务。 创建 AsyncReadManagerMetrics 并初始化度量。度量器在进入PlayMode开启，推出PlayMode关闭。

	MessageHandler::Get().Initialize(TypeOf<Object>());

	ManagerContextInitializeClasses(); // ManagerContext 查找所有以 Manager 结尾的类型及其派生类型。初始化MessageHandler，MessageHandler是事件系统处理中心。

	RenderBufferManager::InitRenderBufferManager(); // 创建 RenderBufferManager 和 ConcurrentQueue

	// This has to be called after Object::InitializeAllClasses
    ModuleManager::Get().InvokeSetup(); // 触发Setup回调，1. 调用 ModuleManager.Load() 确保所有模块被加载？ 2. 调用所有模块的 InitializeModule_* 函数，处理模块初始化

	InitScreenManager(); // new ScreenManagerPlatform

	InitFloatExceptions();

	TypeManager::Get().CallInitializeTypes(); // 调用类型的 IntializeClass 函数， 如GameObjectManager,Transform，Behaviour 等类初始化。回调：在注册所有类型ID时存下

	InitPlayerLoopCallbacks(); // 注册PlayerLoop的回调函数
}
```



## Mono Assemly 的加载 LoadUnityMonoAssemblies
// TODO:
``` c++
void LoadUnityMonoAssemblies() 
{
	UpdateTargetAssembliesInMonoManager();
	LoadDomainAndUnityAssemblies();
}
```





# 附录
比较耗时的几个操作：
- InitializeEngineGraphics