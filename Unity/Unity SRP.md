## Unity.Rendering 供扩展类：
- `RenderPipeline`: Unity引擎内定义。定义一系列命令和设置来描述如何渲染一帧。可由`RenderPipelineAsset`定义数据描述。RenderPipeline定义了`Renderer(ScriptableRenderContext, Camera[])`接口函数，供派生类实现，是自定义渲染的入口方法。
- `ScriptableRenderContext`: 定义自定渲染管线使用的状态和绘制命令


##  URP核心类：
- `UniversalRenderPipeline`: 是整个URP入口类。
- `ScriptableRenderer`: 实现一个渲染策略。它描述如何裁切(culling)和光照(lighting)工作以及支持的效果。渲染器可用于所有相机或被每相机覆盖。渲染器通过添加`ScriptableRendererFeature`可被扩展以支持更多效果。渲染器资源被序列化为`ScriptableRendererData`。
> 目前URP中存在的渲染器（Renderer）有2个：
> - Renderer2D
> - ForwardRenderer: 是URP中默认的渲染器。此渲染器支持所有URP支持的平台。用于经典的每物体光照剪切（per-object light culling）的前向渲染策略.

- `ScriptabeRendererData`: 渲染器的配置。可通过此配置，创建对应的ScriptRenderer。
	
- `ScriptableRendererFeature`: 可附加到`ScriptableRenderer`，以在渲染器中注入渲染通道(render passes)。URP中存在的Renderer Feature：
	- RenderObjects：
	- ScreeSpaceAmbientOcclusion：

- `ScriptableRendererPass`: 实现逻辑渲染通道，可用于扩展URP渲染器。


## UniversalRenderPipeline Render工作流程:
```c#
void Render(ScriptableRenderContext context, Camera[] cameras){
	// BeginContextRendering(context, cameras); // Unity 2021+
	BeginFrameRendering(context, cameras);
	
	SortCameras(cameras); // 按深度(camera.depth)排序
	
	foreach(var cam in cameras) {
		if (IsGameCamera(cam)) { // CameraType.Game/VR
			RenderCameraStack(context, cam);
		}else {
			BeginCameraRendering(context, cam);
			VFXManager.PrepareCamera(camera);
			UpdateVolumeFramework(camera,null);
			RenderSingleCamera(context, cam);
			EndCameraRendering();
		}
	}
	EndFrameRendering(context, cameras);
	// EndContextRendering(context, cameras);
}

void RenderSingleCamera(ScriptableRenderContext context, CameraData cameraData, bool anyPostProcessingEnabled){
	Camera camera = cameraData.camera;
	ScriptableRenderer renderer = cameraData.renderer;
	TryGetCullingParameters(cameraData, out var cullingParams);
	
	ScriptableRenderer.current = renderer;
	
	CommandBuffer cmd = CommandBufferPool.Get(); // Sampler用？
	
	renderer.Clear(cameraData.renderType);
	renderer.SetupCullingParameters(ref cullingParams,ref cameraData);
	var cullResult = context.Cull(ref cullingParams);
	InitializeRenderingData(ref cameraData, ref cullResult,out var renderingData);
	
	ApplyAdaptivePerformance(ref renderingData);
	
	// 配置渲染器将要执行的渲染通道。每个相机在每帧执行。由派生类实现。
	// ForwardRenderer实现了此函数.
	renderer.Setup(context, ref renderingData); 
	// 执行入列的众多渲染通道。
	renderer.Execute(context, ref renderingData);
	
	context.ExecteCommandBuffer(cmd);
	
	cmd.Clear();
	CommandBufferPool.Release(cmd);
	
	context.Submit();
	
	ScriptableRenderer.renderer = null;
}
```

在ForwardRenderer.Setup中，主要负责将RenderPass入列：
```c#
void Setup(ScriptableRenderContext context, ref RenderingData renderingData){
	// 添加RendererFeature中的渲染通道，并移除空的渲染通道。
	// tips：可使用RendererFeature自定义RenderPass。
	AddRenderPasses(ref renderingData);
	
	ConfigureCameraTarget(...);
	
	foreach(var renderPass in m_renderPasses){
		renderPass.Setup(ref renderingData);
		// ForwardRenderer内部使用的RenderPass入列
		EnqueuePass(renderPass);
	}
}

void AddRenderPasses(ref RenderingData renderingData) {
	foreach(var rendererFeature in m_rendererFeatures) {
		// 由每个RenderFeature实现的抽象方法
		// Inject一个或多个RenderPass到渲染器中
		rendererFeature.AddRenderPasses(this, ref renderingPass);
	}
	
	RemoveAllNullRenderPasses();
}


```

Renderer管理ScriptableRendererFeatures和ScriptableRenderPasses。ForwardRenderer定义了许多固有的渲染通道，自定义的渲染通道则通过RendererFeature引入。

在ScriptableRenderer.Execute中：
```csharp
void Execute(ScriptableRenderContext context, ref RenderingData renderingData) {
	ref var cameraData = ref renderingData.cameraData;
	var camera = cameraData.camera;
	CommandBuffer cmd = CommandBufferPool.Get();
	
	{
		//每个RenderPass调用OnCameraSetup(cmd,ref renderingData);
	InternalStartRendering(context, ref renderingData);
		// 初始化相机渲染状态。{
	ClearRenderingState(cmd); // 重置每个相机的shader keywords。shader keyword会基于每个RenderPass执行时启用
	SetPerCameraShaderVariables(cmd, ref cameraData); // 设置相机位置、zbuffer、screen等等变量。
	SetShaderTimeValues(cmd, Time.time...); // 设置shader的时间相关变量
	// }
		
	context.ExecuteCommandBuffer(cmd);
	cmd.Clear();
	
	SortStable(m_ActiveRenderPassQueue); // 对队列中的渲染通道进行排序
		// 虚函数。设置光照，例如：可计算并上传光的CBUFFER等
	SetupLights(context, ref renderingData);
	
		// RenderBlock可以按RenderPassEvent顺序以块形式管理渲染事件
	using var renderBlocks = new RenderBlocks(m_ActiveRenderPassQueue);
		// 执行BeforeRenderering的RenderPass
	ExecuteBlock(RenderPassBlock.BeforeRendering, in renderBlocks,context, ref renderingData);
	
	context.SetupCameraProperties(camera);
	SetCameraMatrices(cmd, ref cameraData, true);
	SetShaderTimeValues(cmd, Time.time...);
	VFXManager.ProcessCameraCommand(camera, cmd);
	
	context.ExecuteComandBuffer(cmd);
	cmd.Clear();
	
	BeginXRRendering(cmd, context, ref cameraData);
	
	ExecuteBlock(RenderPassBlock.MainRenderingOpqeue, ...);
	ExecuteBlock(RenderPassBlock.MainRenderingTransparent, ...);
	
	DrawGizmos(context, camera, GizmoSubset.PreImageEffects);
	
	ExecuteBlock(RenderPassBlock.AfterRendering,...);
	
	EndXRRendering(cmd, context, ref cameraData);
	
	DrawWireOverlay(context, camrea);
	DrawGizmos(context, camera, GizmoSubset.PostImageEffects);
	
		// 调用RenderPass.FrameCleanup()、RenderPass.OnFinishCameraStackRendering(),以及ScriptableRenderer子类的.FinishRendering()函数，释放创建的资源。
	InternalFinishRendering(context, cameraData.resolveFinalTarget);
	}
	
	context.ExecuteCommandBuffer(cmd);
	CommandBufferPool.Release(cmd);
}

// 遍历Block中的RenderPass，执行每个RenderPass
void ExecuteBlock(int blockIndex, ...){
	foreach(int i in renderBlocks.GetRange(blockIndex)) {
		var renderPass = m_ActiveRenderPassQueue[i];
		// 执行对应的RenderPass
		ExecuteRenderPass(context, renderPass, ref renderingData);
	}
	context.Submit(); // 提交所有已安排的命令到渲染循环以便执行
}

void ExecuteRenderPass(context, renderPass, ref rendingData) {
	
	var cmd = CommandBufferPool.Get();

	// 配置render targets 及其 clear state，创建临时render target textures
	// 如果RenderPass不重载此方法，则渲染到激活的相机的render target。
	// 永远不硬调用CommandBuffer.SetRenderTarget，而应该调用ConfigureTarget和ConfigureClear
	// 可由派生类实现。
	renderPass.Configure(cmd,cameraData.cameraTargetDescriptor);
	// 设置额外附加的RenderTarget
	SetRenderPassAttachments(cmd, renderPass, ref cameraData);
	
	context.ExecuteCommandBuffer(cmd);
	CommandBufferPool.Release(cmd);
	
	// 执行RenderPass。这是自定义渲染发生的地方。
	// 由派生类实现。
	renderPass.Execute(context, ref renderingPass);
}
```

因此，对于外部而言，需要通过RenderFeature给Renderer添加新的渲染特性，渲染特性的实现由RenderPass实现。RenderPass实现Configure和Execute接口方法，Execute执行特定的渲染。

## UnityEditor中的URP流程
在常规的URP操作中，步骤如下：
1. 先创建URP `RenderPipelineAsset`，默认同时会创建匹配的`ScriptableRendererData`(ForwardRenderer),用于创建`ScriptableRenderer`
2. 对应的Forward Renderer Data中，可添加`ScriptableRendererFeature`。默认RendererFeature列表为空。目前可选的有`RenderObjects`，和`ScreenSpaceAmbientOcclusion`两个默认实现的RendererFeature。
3. 如果需要添加新的RendererFeature，需要新建RendererFeature脚本。
4. 如果需要添加新的渲染效果，则需要再新建RenderPass脚本。由自定义的RendererFeature管理此RenderPass。

### RendererFeature脚本
- RendererFeature脚本需要实现`Create()`和`AddRenderPasses(ScriptableRenderer renderer, ref RenderingData renderingData)`接口。
	- Create：初始化Feature的资源
	- AddRenderPasses：引入一个或多个RenderPass到Renderer中。一般内部是调用RenderPass.Setup()和renderer.EnqueuePass();
	> AddRenderPasses 在每帧都会被执行。

### RenderPass脚本
- RenderPass脚本可实现：
	-  Configure(CommandBuffer cmd, RenderTextureDescriptor desc);
	-  **Execute(ScriptRenderContext context, ref RenderingData renderingData)**; 执行自定义渲染逻辑。
	-  OnCameraSetup(CommandBuffer cmd, ref RenderingData data);
	-  OnCameraCleanup(CommandBuffer cmd);
	-  FrameCleanup(CommandBuffer cmd);
	-  OnFinishCameraStackRendering(CommandBuffer cmd);


## FAQ
- 在SRP中，通过RenderTargetIdentifiler引用RendetTexture，那么怎么从RenderTargetIdentifier访问RenderTexture呢？
- 