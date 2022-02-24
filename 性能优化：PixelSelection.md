测试场景：City Park
测试对象数量：10k
目标：30FPS

结果：Opaque与Transparent物体都统一为单独的色块。
原始实现：MaterialPropertyBlock + RenderObject(3 Render Queue(Opaque x2 + Transparent x1))
新的实现：Custom RenderFeature (1 Render Queue(Opaque))

原始实现：Camera开启，～10FPS
	PixelSelection 
		本身DrawOpaqeObjects会每个对象都会渲染一次(numgo:2934)
		OpaqueQueue(AfterOpaqueObjects) 会渲染2次(5868=2934*2)
		Transparent Quque会执行 (154次,154个透明物体)
	EditCamera: 
		DepthPrepass是(2934，每物体一次)
		DrawOpaqueObjects(2934,每物体一次)
		DrawTransparentObjects (154,每透明物体一次)
（❓MaterialPropertyBlock也会影响EditCamera？）

新的RenderFeature：Camera开启，～30FPS
	PixelSelectionCamera: 
		DrawOpaqueObjects （SRP Batched 22，被SRP Batcher过）
		PixelSelectPass （5130，所有Renderer对象，每个单独Material）
		DrawTransparentObjects (SRP Batched 66)
	EditCamera:
		DepthPrepass (28, batched)
		DrawOpaqueObjects(28, Batched)
		DrawTransparentObjects(66, batched)


当屏幕中GameObject越多，性能下降越明显。原因：
1. 可渲染物体增多，则MaterialPropertyBlock影响越明显。不仅影响PixelSelectionCamera，还影响EditCamera。每个对象都会独立渲染。
2. 虽然使用CommandBuffer.DrawRender()对每个对象独立渲染，也会使SRP batcher对它不起作用，但它不会影响EditCamera的Batcher。（但所有Render都纳入了渲染，应该只是在相机范围内的物体才被渲染）

色块图可以存在一张RenderTexture中，当需要时，可以通过RenderTexture获取。色块图的用途仅用于屏幕坐标以获取颜色值。因此RenderTexture可以不用拷贝，只在需要时，通过计算获取值即可。


## 附录
SetPassCall：指定用什么shader，shader数据在哪儿等。
static batch：标明为static的物体，如果使用相同材质球，在Unity构建时，自动提取这些共享材质的静态模型的Vertex Buffer和Index Buffer，将模型定点存储在世界空间下大的Buffer中，并按偏移，在后续的绘制过程中，一次性提交整个合并模型的顶点数据，根据场景管理系统判断各子模型可见性，然后一次设置渲染状态。因为预先把子模型的顶点变换到了世界空间下，所以运行时，CPU不需要进行顶点变换，且它们共享材质，所以多次Draw call并不会有渲染状态切换。但副作用是运行时内存体积增大。

Dynamic Batch: 在进行场景绘制之前，将所有共享同一材质模型的顶点信息变换到世界空间中，然后通过一次Draw Call绘制多个模型，达到合批的目的。模型顶点变换是由CPU完成，所以会带来CPU性能消耗。所以对动态Batch有诸多限制：
- Mesh不能>900面
- 动态改变材质变量后，不算同一材质，不参与batch
- 改变Renderer.material会造成材质拷贝，会打断batch，因此应该使用sharedMaterial保证材质的共享状态。

GPU Instancing：使用相同材质球、相同Mesh情况下，Unity会再运行时对于正在视野中的所有符合要求的对象使用Constant Buffer将其位置、缩放、UV、Lightmap Index等放在buffer中，然后抽取一个对象作为instance送入渲染流程，在执行DrawCall后，从显存中取出实例部分共享信息与GPU Constant Buffer取出与对应对象相关信息一并传递到下一渲染阶段。与此同时，不同shader stage可从buffer中直接获取所需常量，而不用再次设置。GPU Instancing规避合并Mesh导致的内存和性能问题，单由于constant buffer每帧创建，buffer大小不得>64k。

SRP Batcher：

Gfx.WaitForPresent & Graphics.PresentAndSync :说明CPU在等待GPU，性能瓶颈在GPU或者CPU-GPU BUS带宽上。