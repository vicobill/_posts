## MaterialPropertyBlock
A block of material values to apply. 
用于绘制同材质但稍微属性不同的多个物体。不支持修改渲染状态。
Unity使用MaterialPropertyBlock绘制Tree，它们全部使用相同材质，但每个Tree有不同的颜色、缩放以及风力等。

SRP Batcher:
A renderering loop that speeds up your CPU rendering in Scenes with many Materials that use the same Shader Variant.
SRP Batcher并不会减少Draw Call, 它处理Draw call之前的CPU的设置工作，把材质属性数据永久放入显卡CBUFFER，只要数据不变，CPU就可不需要重新准备DrawCall数据。
SRP Batcher用于加速CPU渲染而不会影响GPU性能。


- SRP(Scriptable Render Pipeline):A thin API layer that lets you schedule and configure rendering commands using C# scripts. Unity passes these commands to its low-level graphics architecture, which then sends instructions to the graphics API.

A Unity feature that allow you to write C# scripts to control the way Unity render each frame.


- Render Graph System:基于SRP。Allow you to author a custom SRP in a maintanable and modular way. HDRP 使用RGS.

- Render Graph: A high-level representation of the custom SRP's render passes,which explicitly states how the render passes use resources.

- RTHandle System: 

RenderFeature里的RenderObject：
- Override/Material: 当渲染一个对象，Unity以此Material替换已分配的Material。





Gfx.WaitForPresentOnGfxThread
	Semaphore.WaitForSingal




ComputeShader


Dispatch定义了Thread Group的阵列，如：Dispatch(5,3,2) 定义了5x3x2个Thread Group（线程组）。每个阵列元素表示一个Thread Group。线程组的访问是无序的。
Thread Group定义了Group Thread ID 阵列。如(10,8,3)的Thread Group,则定义了10x8x3个Thread ID组。每个阵列元素都是一个Thread ID.

如果：访问Thread Group ID：(2,1,0), Group Thread ID： (7,5,0)
条件：
	Dispatch = (5,3,2)
	Thread Group = (10,8,3)
每项值：
	SV_GroupID = (2,1,0)
	SV_GroupThreadID = (7,5,0)
	SV_DispatchThreadID=(2,1,0)\*(10,8,3) + (7,5,0)  = (27,13,0)
	SV_GroupIndex = 0\*10\*8 + 5\*10 + 7 = 57

在实际应用中，将图像按Block进行划分，然后每个Compute线程对每个Block进行处理。
例如：一张4096x4096纹理，划分为8x8块进行处理：则可划分为512x512个工作组协同工作。
```c++
// number of work groups in x/y/z dimensions
// 定义每个维度上的工作组数量
ComputeShader.Dispatch(kernelIndex, 4096/8, 4096/8,1);

```

```hlsl
[numthreads(8,8,1)] // 定义每个工作组大小
void CSMain(uint3 id : SV_DispatchThreadID) 
{
}
```

如果要取(42,42)处的像素，则42/8=5, GroupID为(5,5)，即6行6列的工作组的第2x2个线程。
5,5 \*8,8 +2,2 = 42,42。则DispatchThreadID可视为像素的坐标。


## Material Property Block
对应C++中的ShaderPropertySheet概念。是Shader的属性合集。
SetPropertyBlock在
CommandBuffer的命令，会在`ScriptRenderContext::ExecuteScriptableRenderLoop()`中被调用。
RenderLoop开始之前，会调用PrepareDrawXXXCommand等类似函数，按批准备渲染任务。
其中，`PrepareDrawRenderersCommand`准备绘制渲染器任务。
当执行渲染时，会遍历每个命令，调用对应的ExecuteXXX执行对应的命令。
其中，`ExecuteDrawRenderersCommand`是对应的DrawRenderers命令。
执行完后，清理命令缓冲区。
RenderLoop最后，会调用`ScriptableBatchRenderer::UpdateUseSRPBatcher()`命令。
> 如果显式调用ScriptableRenderContext::Submit,则会立即执行一次ExecuteScriptableRenderLoop()。
### Prepare