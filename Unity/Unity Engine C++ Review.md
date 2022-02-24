命令行参数
gfx-debug-msg
renderdoc
force-glcore32
force-glcore33
force-glcore40
force-glcore45
force-glcore
force-opengl
force-gles20
force-gles31aep
force-gles32
force-gles
force-d3d11
force-d3d12
force-vulkan
force-device-index
enable-metal-validation
force-gfx-jobs
force-gfx-st
createProject temporary projectTemplate cloneFromTemplate
projectpath
openfile
openscene
ForceDisableReopenLastUsedProjectOnStartup
rebuildlibrary
vcsMode
vcsModeSession
buildTarget buildTargetGroup forceSwitchOnInvalidBuildTarget
il2cpp-backend libmono libil2cpp
api-profile NET_2_0_Subset NET_4_6 NET_Standard_2_0
cacheServerWaitForConnection
connectToMacroClient
executeMethod
buildWindowsPlayer
buildOSXUniversalPlayer
buildWindows64Player
buildLinux64Player
outputpath
buildBuiltinUnityResources
buildBuiltinExtras
buildBuiltinPreviews
buildEditorUnityResources
importPackage
exportPackage
openurl
quit quitTimeout

MonoFunctions.h 包含了所有mono API 

# 关于Math
https://gametorrahod.com/unity-mathematics-vs-mathf/

Unity.Mathematics 中的函数：
	select(float3 a,float3 b,bool3 c) 选择函数，按每个元素(component-wise)比较(c的每个component 比较)，如果满足条件选取满足条件的值。
	select(float a, float b, bool c) ==> c ? b : a;