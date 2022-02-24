-   现行实现的Pixel-Perfect Selection依照 [https://medium.com/@philippchristoph/pixel-perfect-selection-using-shaders-16070d3094d](https://medium.com/@philippchristoph/pixel-perfect-selection-using-shaders-16070d3094d) 中所描述的实现。  
    
-   Studio的需求：可以正常点选、框选；可以透过前方透明物体(Transparent)选中后面的物体(Opaque | Semi-Transparent)；  
    
-   Studio使用的Pixel-Perfect Selection，使用的技术：URP, MaterialPropertyBlock, Camera自定RenderFeature。在优化中的主要冲突：  
    
    -   SRP Batcher会优化DrawCall之前的所有CPU准备工作，这在一定程度上减少CPU时间，直接反应就是帧率。<font color=yellow>但MaterialPropertyBlock与SRP Batcher ，会打断SRP的Batcher</font>。  
        
    
    -   Camera自定的RendererFeature，在实现中使用了RenderObject 的Render Feature。
	    1. 用在AfterRenderingOpaques,调用PixelSelection材质的第1个通道渲染物体；(❓用于渲染Opaque物体？） 
	    2.  用在AfterRenderingOpaques，调用PixelSelection材质的第2个通道渲染物体；(❓用于渲染Semi-Transparent物体？和1的shader pass唯一差别是1是ZTest off, 2 是ZTest on) 
	    3.  用在AfterRenderingTransparents，调用PixelSelection材质的第2个通道渲染物体。  (❓渲染Transparent物体？)
   
-   在剖析中:
	- OnReadback()回调中，Texture2D.SetPixelDataImp() 单次调用，耗4ms，占6.8%.在使用完之后，内存的释放LargeAllocation.Free()单次调用，耗1.68ms
	- UniversalRenderPipeline.RenderSingleCamera()则是最大消耗部分，调用4次，耗21.44ms，GC 7.6kb 占37%，总耗23ms。主要消耗在：
		- ScriptableRenderContext.Submit() 调用4次，耗13.67ms，GC 7.3kb， 占23.3%
			- UniversalRenderPipeline.RenderSingleCamera():PixelSelectionCamera  调用1次，耗6.67ms, 占12.4%
			- UniversalRenderPipeline.RenderSingleCamera():EditCamera 调用1次，耗 6.63ms, 占 10.8%
		- ScriptableRender.Execute() 调用4次，耗3.88ms，GC 320b, 占6.6%

首先分析PixelSelectionCamera。PixelSelectionCamera的渲染都是来自手动调用Camera.Render()![[Pasted image 20220211154309.png]]
PixelSelectionCamera 本身渲染Opaque对象（场景对象）会耗时，在使用RenderObject Feature时，会再将对象渲染。
- 可以跳过DrawOpaqueObjects吗？我们最终需要的结果是使用PixelSelection shader渲染的色块图，DrawOpaqueObjects 会按照原始Shader绘制一遍？
> 根据https://forum.unity.com/threads/getting-low-fps-drawopaqueobjects.1035367/ 中的信息，PostProcess, Depth, Opaque 三项会降低Performance。

性能测试用例：

URP 渲染管线
10k Primitive 对象
shader: URP默认shader，PixelSelectionShader

A: 对象使用Material，进行颜色设置，每个颜色属性都不同。使用URP Shader，URP RenderPipelineAsset默认启用了SRP Batcher.
![[Pasted image 20220211174803.png]]
B: 对象使用MaterialPropertyBlock(MPB)进行颜色设置，每个颜色属性都不同。使用URP Shader，URP RenderPipelineAsset默认启用了SRP Batcher.。
![[Pasted image 20220211174855.png]]

> URP中，使用MPB显著降低Renderer性能。对比中，默认开启了SRP Batcher的传统Material较优。
> 原因：MPB与URP不兼容，当使用MPB后，SRP Batcher 不起作用。

A2: 对A测试中，禁用SRP Batcher。
![[Pasted image 20220211175538.png]]
B2: 对B测试中，禁用SRP Batcher。
![[Pasted image 20220211175521.png]]

> 差异不大。



A: 使用A测试。
![[Pasted image 20220211180846.png]]
A3: 使用A，URP shader，使用RenderFeature中的RenderObject对物体着色。
![[Pasted image 20220211181019.png]]

> 使用RenderFeature也能享受SRB Batcher带来的好处。



A4: 使用A， PixelSelection Shader， 使用RenderFeature中的RenderObject对物体着色。
A5: 使用A, PixelSelection Shader, 使用RenderFeature中的RenderObject对物体着色，使用MPB。

> 根据单独使用PixelSelection Shader，貌似得不到色块图？色块图在哪一步获得的？PixelSelection Shader并非直接得到色块？
> 
> 在PixelSelection Shader中，直接返回颜色为红色，也并不能work？RenderObject 的Override Material不起作用？



要得到色块图，唯一重要的信息就是渲染的GameObject颜色。将GameObject的信息，传给Shader，必定是通过Buffer。此Buffer是在Shader中，但可被访问。
- Shader.SetFloatArray
- ComputeBuffer