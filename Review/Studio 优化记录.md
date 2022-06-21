拣选操作的优化：
- 产品的要求：
	1. 根据鼠标拣选对象
	2. 鼠标在移动过程中，高亮处于鼠标可选中状态下的对象
	3. 鼠标在拣选对象时，可以穿透非Mesh遮挡的后面的物体
	4. 鼠标在拣选对象时，前面的透明对象可被选中。
- 现行方案：
	- 像素级选中：添加了一个PixelSelectionCamera，用于渲染到RenderTexture，将instaceID映射为颜色。对每个对象通过添加Render，SetPropertyBlock("SelectionColor")方式，将物体设为所有像素映射为instanceID对应的颜色。在选择时，通过ReadPixels方式，从RenderTexture中查找对应坐标的颜色值，映射为instanceID从而找到选中的物体。
- 敢死队员报告，性能消耗主要集中在：
	- ReadPixels方法和SetPropertyBlock方法太耗。
	- ReadPixels是因为要从GPU同步信息到CPU中，需要等待所有渲染目标被渲染完成，所以比较慢。
## 针对ReadPixels的优化
ReadPixels是在得到色块图后可读取。

- 尝试的解决方案：
	- 将ReadPixels用Graphics.CopyTexture,Graphic.ConvertTexture替换。
	> 性能消耗差不多 CopyTexture > ConvertTexture > ReadPixels
    - 改动ReadPixels函数像素框大小，不需要读取那么多数据【待试】
    - 拣选操作放在UniTask中进行
	- 使用AsyncGPUReadback异步等待GPU结果。
	> 异步读取GPU数据
    - 去掉PixelSelectionCamera，直接将主相机的数据通过Graphic.Blit复制到RenderTexture【待试】
    - 直接读取鼠标所处像素点：【待试】
	    - 通过SRP从C#传递参数，获取帧的指定像素点值
	    - NativeRenderPlugin，直接截取帧像素。
	- 重新捋一遍拣选算法【待定】

- 在尝试之前，需要验证：(10k)
	- ReadPixels和SetPropertyBlock是不是真的慢
	> ReadPixels依据需要读取的像素矩形框大小而有性能影响。全屏4k大概60-70ms。
	> SetPropertyBlock单对象大概0.8ms，100k对象大概80-120ms
	- 可使用10k个对象进行测试。
		- 结果：单在Update中进行ReadPixels函数测试，稳定在60-70ms

Camera.GetVisibleObjects
foreach visibleObjects
	get closest object to screen point



最终实现方案：CommandBuffer + AsyncGPUReadback。

问题：
- CommandBuffer.RequestAsyncReadbackIntoNativeArray 需要注意NativeArray释放时机。难以正确释放。

## 针对PixelSelectionCamera的优化
在相机中，在尚未渲染UI前，就已经有了几个相机：EditCamera、PixelSelectionCamera、UICamera。 EditCamera渲染完整场景、Gizmo等编辑时的对象；PixelSelectionCamera只用于渲染色块图；UICamera用于渲染UI。

色块的获取：
- 颜色值，是通过映射GameObject的InstanceID得到颜色。
- 色块，是通过MaterialPropertyBlock中对SelectionColor设置。色块是对完整物体涂色，因此类似于Unlit效果实现。
色块获取的限制：
- GameObject不可以添加可影响EditCamera的材质
- 所有的改变都不可影响EditCamera最终图像结果。
色块获取的可能优化：
- PixelSelectionCamera 是否可以完全去掉？



1. 去掉多余的相机
2. 使用CommandBuffer，在渲染色块后，保存到指定RenderTarget中
3. 其他部分正常渲染到相机

commandbuffer

# SRP中常见优化
| render                       | case | 
| ---------------------------- | ---- |
| 对象数量                     | 10k  |
| 每个对象静态/动态            | 动态 |
| 每个对象Mesh是否相同         | 不同 |
| 每个对象是否使用相同Material | 不同 | 
![](http://blog.coolcoding.cn/wp-content/uploads/2020/09/image5-3.png)


SRP Batcher限制：
- 不可定义在其他CBUFFER中定义的属性值，如\_BaseColor, \_MainTex等UnityPerMaterial常见名称，以及如unity_ObjectToWorld 等再UnityPerDraw中出现的名称。
- 不可使用MaterialPropertyBlock

在CPU成本中，最大的部分在于做DrawCall之前的设置（即，绘制之前的准备工作），而非DrawCall本身。
SRP Batcher会把材质数据放到CBUFFER中，并绑定属性的偏移量。使用相同的Shader，可以支持大量不同的Material。
SRP Batcher则会大幅减少CPU准备数据的时间。

> AsyncReadback有诸多限制：Depth graphics format 不允许readback；目标纹理大小需要一致。

在URP渲染流程中，Camera会DrawOpaqueObjects

以下项影响性能：
- PostProcess： -56%
- Depth ： -59%
- Opaque: -44%
- Depth & Opaque: -100%
- PostProcess & Depth & Opaque: -119%



RenderObjects RenderPass