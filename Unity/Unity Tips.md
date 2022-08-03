## GPU Instancing
GPU Instancing用于共享相同mesh和材质。instance一个mesh和material：
- material shader必须支持GPU instancing
- mesh必须从来源于以下之一：
	- MeshRenderer组件或Graphics.DrawMesh调用。Unity添加这些mesh到list以一起instance。如果GameObject的MeshRenderer组件是SRP Batcher兼容，Unity不会instace此mesh。
	>Unity 不支持SkinnedMeshRenderer的instancing
	- Graphics.DrawMeshInstanced或Graphics.DrawMeshInstancedIndirect调用。这些方法使用相同shader在不同地方instance相同的mesh。Unity独立执行每个call意味着它不能一起从不同的call中instance mesh。

## SRP

SRP Batcher:减少Unity为使用相同shader variant的材质必须准备和分发的draw call的CPU时间。

> 🎗️如果希望渲染相同mesh使用相同material，GPU Instancing可以比SRP Batcher更高效。
> ⚠️ 当Mesh顶点少于256时，不要使用GPU Instancing
> If you want to render a mesh with a low number of vertices many times, best practice is to create a single buffer that contains all the mesh information and use that to draw the meshes


> 使用TryGetComponent，与GameObject.GetComponent相比，一个最大的区别是，当请求的Component不存在时，此方法不会在编辑器中分配内存。

SetPass Call指的是切换渲染状态（render state）的次数，比如你的shader中如果有多个pass，或者是场景中有不同的material，都会造成渲染状态切换。  
Drawcall的话，以gles为例，就是调用draw的实际次数，例如drawarray、drawelement，调用一次都会增加。  
Batch则是会在第一次调用draw行为的时候加1，如果之后渲染状态没有改变，则batch的数量不再增加，但是一次batch内可能会有多次drawcall调用，只是渲染状态没有改变。

话说Unity的Compute Shader传float数组一直有bug。比如传float[5]，C#里写ComputeShader.SetFloats是无法成功的，只有第一个float可以设置成功，这个bug官方已知吗？  
**A:** 这个不是bug。而是根据HLSL的规则，应该对数据进行对齐，以避免为计算偏移所导致的ALU开销。
> 内存对齐！
> 合组。分层。



RenderPipelineAsset: 

RenderFeature:
- RenderObject: 用于




## 小知识
Unity std 100k Primitive Instace ID to Color Perf Starsky:
![[Pasted image 20220211182642.png]]
Unity URP 100k Primitive Instance ID to Color Perf Starsky:
![[Pasted image 20220211182850.png]]