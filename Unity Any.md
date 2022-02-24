SetLocalEulerAngles
	SetLocalEuler
		SetEulerHint
		SetLocalR(eulerToQuat(radians(euler)))




SharedMaterial: 改变属性，所有使用此材质的物体都会改变。
Material：在使用之前会生成一份拷贝Material(Instance)，可以应用单物体独特属性。
MaterialPropertyBlock: 绘制许多相同材质，但不同属性的对象时使用。MPB不会生成Material拷贝实例。



# URP 渲染流程
RenderPipelineAsset: 用于产出指定RenderPipeline的

UniversalRenderPipelineAsset 配置在ProjectSetting的Graphics选项中，对应UniversalRenderPipelineAssetEditor，用于资产的自定义Editor下配置的绘制。
```csharp
public partial class UniversalRenderPipelineAsset : RenderPipelineAsset {
	ScriptableRenderer[] m_Renderers;
	
	ScriptableRendererData[] m_RendererDataList;
	// ... light settings 
	// ... shadow settings
	
	// ... advanced settings
	bool m_UseSRPBatcher = true;
	bool m_SupportsDynamicBatching = false;
	bool m_MixedLightingSupported = true;
	
	// Post-processing settings
	ColorGradingMode m_ColorGradingMode = ColorGradingMode.LowDynamicRange;
	
	protected override RenderPipeline CreatePipeline() {
		CreateRenderers();
		return new UniversalRenderPipeline(this);
	}

	void CreateRenderers() {
		DestroyRenderers();

		for(int i = 0; i < m_RenderDataList.Length; ++i) {
			m_Renderers[i] = m_RenderDataList[i].InternalCreateRender();
		}
	}
}


```

ScriptableRenderData包含ScriptableRenderer的数据。与之对应的存在`ScriptableRendererDataEditor` 用于自定义绘制。
```c#
// 包含ScriptableRenderer资源（RenderFeature）的类
public abstract class ScriptableRendererData :ScriptableObject {
	List<ScriptableRendererFeature> m_RendererFeatures;

	ScriptableRenderer InternalCreateRenderer() {
		return Create();
	}
	// 创建ScriptableRenderer实例。
	// 由子类实现。
	protected abstract ScriptableRenderer Create();
}
```
URP中，默认ScriptableRendererData的子类为`ForwardRendererData`。它有对应的`ForwardRendererDataEditor`用于自定义Editor绘制。
```csharp
public class ForwardRendererData : ScriptableRendererData  {

	PostProcessData postProcessData;
	ShaderResources shaders; // 默认的shader资源

	LayerMask m_OpaqueLayerMask;
	LayerMask m_TransparentLayerMask;
	StencilStateData m_DefaultStencilState;
	bool m_ShadowTransparentReceive;
	RenderingMode m_RenderingMode = RenderingMode.Forward;
	bool m_AccurateGbufferNormals;

	protected override ScriptableRenderer Create() {
		return new ForwardRenderer(this);
	}
}
```

`ScriptableRenderer`实现一个渲染策略，它描述如何Cull、光如何工作、支持的效果等。一个渲染器可被用于所有Camera或 be overriden on a per-camera basis。它实现light culling、setup并描述在一帧内执行的`ScriptableRenderPass`列表。渲染器可通过`ScriptableRenderFeature`被扩展，以支持更多效果。渲染器所需的资源，在`ScriptableRendererData`中序列化。
```csharp
public abstract partial class ScriptableRenderer : IDisposable {
	// 当前渲染器已计划被执行的render pass列表
	List<ScriptableRenderPass> m_ActiveRenderPassQueue;
	// 添加到当前渲染器的render feature列表
	List<ScriptableRendererFeature> m_RendererFeatures;
	
	RenderTargetIdentifier m_CameraColorTarget;
	RenderTargetIdentifier m_CameraDepthTarget;

	public ScritableRenderer(ScriptableRendererData data) {

		foreach(var feature in data.renderFeatures) {
			feature.Create(); // 调用RenderFeature的Create函数，一般是添加RenderPass。
			m_RenderFeatures.Add(feature);
		}
		Clear(CameraRenderType.Base);
		m_ActiveRenderPassQueue.Clear();
	}

	// 对将要执行的render pass进行配置。此函数在每一帧每个camera都会被调用
	public abstract void Setup(ScriptableRenderContext context,ref RenderingData data);
	// 实现光照设置。例如可使用此函数计算并上传light CBUFFER
	public virtual void SetupLights(ScriptableRenderContext context,ref RenderingData data);
	// 对Culling 参数进行配置。例如可配置光是否需要对per-object进行cull，或者最大的shadow distance
	public virtual void SetupCullingParameters(ref ScriptableCullingParameters cullingParameters,

ref CameraData cameraData);
	// 完成camera stack 渲染后的调用。
	public virtual void FinishRendering(CommandBuffer cmd);

	// 执行已入列的render pass
	public void Execute(ScriptableRenderContext context, ref RenderingData data){
		CommandBuffer cmd = CommandBufferPool.Get();

		// 调用每个已计划的RenderPass的.OnCameraSetup
		InternalStartRendering(context, ref data);
		// 清理渲染状态，Disable Shader Keywords
		ClearRenderingState(cmd); 
		// 设置相机和屏幕shader变量（GlobalVector），如屏幕大小、zbuffer参数等
		SetPerCameraShaderVariables(cmd, ref cameraData);
		// 设置全局shader 时间变量（GlobalVector）， 如time, deltaTime等
		SetShaderTimeValues(cmd, time, deltaTime, smoothDeltaTime);

		context.ExecuteCommandBuffer(cmd);// 立即执行所有添加的Command
		cmd.Clear(); // 移出所有之前添加的所有Command

		// 按RenderPass的时间顺序排序
		SortStable(m_ActiveRenderPassQueue);

		// 调用子类SetupLights
		SetupLights(context, ref data);

		// before rendering
		using var renderBlocks = new RenderBlocks(m_ActiveRenderPassQueue);
		ExecuteBlock(RenderPassBlock.BeforeRendering, in renderBlocks, context, ref renderingData);

		// setup camera
		context.SetupCameraProperties(camera);
		SetCameraMatrices(cmd, ref cameraData, true);
		SetShaderTimeValues(cmd, time, deltaTime, smoothDeltaTime);

		context.ExecuteCommandBuffer(cmd);
		cmd.Clear();

		// opaque blocks
		ExecuteBlock(RenderPassBlock.MainRenderingOpaque, in renderBlocks, context, ref renderingData);
		// transparent blocks
		ExecuteBlock(RenderPassBlock.MainRenderingTransparent, in renderBlocks, context, ref renderingData);

		// 编辑器模式下，渲染Gizmo
		DrawGizmos(context, camera, GizmoSubset.PreImageEffects);

		// post processing, video player capture etc.
		ExecuteBlock(RenderPassBlock.AfterRendering, in renderBlocks, context, ref renderingData);

		// 编辑器模式下，渲染线框覆层
		DrawWireOverlay(context, camera);
		DrawGizmos(context, camera, GizmoSubset.PostImageEffects);

		// 调用每个已计划的RenderPass的.FrameCleanup,.OnFinishCameraStackRendering
		// 并调用子类的FinishRendering
		InternalFinishRendering(context, cameraData.resolveFinalTarget);

		context.ExecuteCommandBuffer(cmd);
		CommandBufferPool.Release(cmd);
	}

	void InternalStartRendering(context, ref data) {
		var cmd = CommandBufferPool.Get();
		foreach(var renderpass in m_ActiveRenderPassQueue){
			renderpass.OnCameraSetup(cmd, ref data);
		}
		context.ExecuteCommandBuffer(cmd);
		CommandBufferPool.Release(cmd);
	}
	void InternalFinishRendering(context, bool resolveFinalTarget) {
		var cmd = CommandBufferPool.Get();
		foreach(var renderpass in m_ActiveRenderPassQueue) {
			renderpass.FrameCleanup(cmd);//
		}

		foreach(var renderpass in m_ActiveRenderPassQueue) {
			renderpass.OnFinishCameraStackRenderering(cmd);
		}

		FinishRendering(cmd);
	
		m_ActiveRenderPassQueue.Clear();
	
		context.ExecuteCommandBuffer(cmd);
		CommandBufferPool.Release(cmd);
	}
	// 按块对RenderPass进行划分,执行每块内的RenderPass
	void ExecuteBlock(int blockIndex, in RenderBlocks renderBlocks,ScriptableRenderContext context, ref RenderingData renderingData,bool submit=false) {
		foreach (int currIndex in renderBlocks.GetRange(blockIndex)){
			var renderPass = m_ActiveRenderPassQueue[currIndex];
			ExecuteRenderPass(context, renderPass, ref renderingData);
		}

		if (submit) context.Submit();
	}

	// 配置RenderPass，设置RenderPass的渲染目标，并执行RenderPass。
	void ExecuteRenderPass(context, renderpass, ref renderingdata) {
		var cmd = CommandBufferPool.Get();

		renderpass.Configure(cmd, cameraData.cameraTargetDescriptor);
		SetRenderPassAttachments(cmd, renderPass, ref cameraData);

		context.ExecuteCommandBuffer(cmd);
		CommandBufferPool.Release(cmd);

		renderPass.Execute(context, ref renderingdata);
	}
}
```

`ForwardRenderer`则是UniversalRP中的默认渲染器。它使用按per-object light culling的传统的前向渲染策略。
```csharp
public sealed class ForwardRenderer : ScriptableRenderer {
	// 默认使用的RenderPass
	ColorGradingLutPass m_ColorGradingLutPass;
	DepthOnlyPass m_DepthPrepass;
	DepthNormalOnlyPass m_DepthNormalPrepass;
	MainLightShadowCasterPass m_MainLightShadowCasterPass;
	AdditionalLightsShadowCasterPass m_AdditionalLightsShadowCasterPass;
	GBufferPass m_GBufferPass;
	CopyDepthPass m_GBufferCopyDepthPass;
	TileDepthRangePass m_TileDepthRangePass;
	TileDepthRangePass m_TileDepthRangeExtraPass; 
	DeferredPass m_DeferredPass;
	DrawObjectsPass m_RenderOpaqueForwardOnlyPass;
	DrawObjectsPass m_RenderOpaqueForwardPass;
	DrawSkyboxPass m_DrawSkyboxPass;
	CopyDepthPass m_CopyDepthPass;
	CopyColorPass m_CopyColorPass;
	TransparentSettingsPass m_TransparentSettingsPass;
	DrawObjectsPass m_RenderTransparentForwardPass;
	InvokeOnRenderObjectCallbackPass m_OnRenderObjectCallbackPass;
	PostProcessPass m_PostProcessPass;
	PostProcessPass m_FinalPostProcessPass;
	FinalBlitPass m_FinalBlitPass;
	CapturePass m_CapturePass;
[[if]] ENABLE_VR && ENABLE_XR_MODULE
	XROcclusionMeshPass m_XROcclusionMeshPass;
	CopyDepthPass m_XRCopyDepthPass;
[[endif]]
[[if]] UNITY_EDITOR
	SceneViewDepthCopyPass m_SceneViewDepthCopyPass;
[[endif]]
	
	  
	// RenderTargetHandle可看作ShaderProperty的字符串哈希，即管理Shader.PropertyToID()及RenderTargetIdentifier
	RenderTargetHandle   m_ActiveCameraColorAttachment;
	RenderTargetHandle   m_ActiveCameraDepthAttachment;
	RenderTargetHandle   m_CameraColorAttachment;
	RenderTargetHandle   m_CameraDepthAttachment;
	RenderTargetHandle   m_DepthTexture;
	RenderTargetHandle   m_NormalsTexture;
	RenderTargetHandle[] m_GBufferHandles;
	RenderTargetHandle   m_OpaqueColor;
	RenderTargetHandle   m_AfterPostProcessColor;
	RenderTargetHandle   m_ColorGradingLut;
	// For tiled-deferred shading.
	RenderTargetHandle   m_DepthInfoTexture;
	RenderTargetHandle   m_TileDepthInfoTexture;
	  
	ForwardLights m_ForwardLights; // 计算和提交光照数据到GPU
	DeferredLights m_DeferredLights;

	RenderingMode m_RenderingMode;	
	StencilState m_DefaultStencilState; // 按配置的StencilState数据创建

	// 按配置中的ShaderResources创建的材质
	Material m_BlitMaterial;
	Material m_CopyDepthMaterial;
	Material m_SamplingMaterial;
	Material m_ScreenspaceShadowsMaterial;
	Material m_TileDepthInfoMaterial;
	Material m_TileDeferredMaterial;
	Material m_StencilDeferredMaterial;

	public ForwardRenderer(ForwardRendererData data) : base(data) {
		// 按照配置数据，创建材质
		// 创建RenderPass
		// 创建RenderTargetHandle
	}

	// 每个子Renderer都必须实现，且在每帧中，每个相机都会调用
	public override void Setup(context, ref renderingdata) {
		

		// 调用各Light Shadow Caster RenderPass的.Setup()

		// 创建临时RT，并配置Renderer的CameraColorTarget和CameraDepthTarget
		CreateCameraTarget();
		ConfigureCameraTarget();

		// 调用每个RenderFeature的.AddRenderPasses，入列自定RenderPass
		AddRenderPasses(ref renderingdata);

		// 为主光渲染一个shadow map
		EnqueuePass(m_MainLightShadowCasterPass);
		EnqueuePass(m_AdditionalLightsShadowCasterPass);
		EnqueuePass(m_DepthNormalPrepass);
		EnqueuePass(m_DepthPrepass);

		EnqueuePass(m_ColorGradingLutPass);
	
		EnqueuePass(m_RenderOpaqueForwardPass);

		EnqueuePass(m_DrawSkyboxPass);
		EnqueuePass(m_CopyDepthPass);
		EnqueuePass(m_CopyColorPass);
		EnqueuePass(m_TransparentSettingsPass);
		EnqueuePass(m_RenderTransparentForwardPass);
		EnqueuePass(m_OnRenderObjectCallbackPass);
		EnqueuePass(m_PostProcessPass);
		EnqueuePass(m_FinalPostProcessPass);
		EnqueuePass(m_CapturePass);
		EnqueuePass(m_FinalBlitPass);
	

	}

	public override void SetupLights(context, ref renderingdata) {
		m_ForwardLights.Setup(context, ref renderingdata);
		if (deferredMode) {
			m_DeferredLights.SetupLights(context, ref renderingdata);
		}
	}

	public override void SetupCullingParameters(ref cullingParameters, ref cameraData) {
		// 计算shadowDistance和maximumVisibleLights
	}

	public override void FinishRendering(CommandBuffer cmd) {
		cmd.ReleaseTemporaryRT()
	}
}
```

ScriptableRendererFeature，与之对应ScriptableRenderFeatureEditor用于自定义绘制。


ForwardRendererData:ScriptableRendererData
	Create(): 创建一个ForwardRenderer



## 附录
- RenderTargetIdentifier：CommandBuffer中用于标识一个RenderTexture。RenderTexture可被标识的方式有多种：一个RenderTexture对象、内置render texture（BuiltinRenderTextureType）中的一种、具名临时render texture(通过CommandBuffer.GetTemporaryRT())