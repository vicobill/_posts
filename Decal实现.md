1. 基于面片：直接用一个Quad的Mesh，加上一张贴图，简单直观。
> 缺点：只能贴在平面上

2. 基于SubMesh：先获取跟目标投影相交的mesh，然后将Mesh根据投影框进行裁切
	1. 获取所有可能和投影框相交的Mesh（通过使用AABB相交）
	2. 将Mesh顶点数据变换到投影框的3D空间中。方便裁切，裁切后可将变换后的坐标值直接作为UV使用。
	3. 得到相交的三角形片：判断每个点是否在投影框内，如果有则认为三角形与投影框相交。但也可能有三角形顶点都不在投影框内，但与框相交。如果需要严谨的求交算法，参考SAT：https://gdbooks.gitbooks.io/3dcollisions/content/Chapter4/aabb-triangle.html.
	4. 将所有相交的三角形片，合成新的IndexBuffer，使用新的Decal纹理重新渲染一次。UV可直接取映射到框中的xy值。需要把UV中0-1之外的部分clip掉。
	5. 也可以对处在边界、不完全在框内的三角形进行裁剪，最后整理成新的mesh。
	 缺点：比较适用于静态物体，创建过程可能耗时较长
3. 基于Multi-Pass：
	1. 获取所有相交的Mesh
	2. 在Mesh正常渲染结束后，再渲染一次，使用decal的shader，传入ClipToDecal矩阵（`ClipToWorld*WorldToDecal`)，在FS中计算映射到Decal框的坐标。取决于具体实现，可以将xy坐标作为uv，以及clip掉UV0-1外的部分，渲染出decal。
>Unity builtin 管线中的Projector就是使用这种方式。
>缺点：如果投影框与多个Mesh相交，或者Mesh很大，则会产生很大性能消耗

4. 基于后处理：将Decal整体作为一个长方体渲染2次来对目标进行贴花
	1. 先正常渲染其他物体，拿到正常渲染的Buffer和DepthBuffer
	2. 将投影框作为一个长方体进行渲染，关闭FaceCull，将DepthTest设置为GreatEqual，输出一个标志位到StencilBuffer（或任一其他可以标记像素点的方式），不需要输出颜色值
	3. 再次将投影框作为长方体渲染，打开FaceCull（只绘制长方形靠前的3个面），再渲染上一步中StencilBuffer测试通过的位置，当前像素点的WorldPosition通过从DepthBuffer中读取，然后反变换获得（后处理中非常常用的方法）
> 缺点：不支持光照

5. Deferred实现：大致和上述方法相同。
	1. 在Defferred渲染管线渲染所有GBuffer之后
	2. 也是先渲染长方体，写入StencilBuffer，然后再次渲染长方体，根据StencilBuffer改变GBuffer中的数据，根据需要修改BaseColor、Normal等
	3. 因为GBuffer被修改，后面光照计算会产生Decal效果
> 缺点：只能用于Defferred，不支持烘焙光（因为烘焙光是在渲染GBuffer时加上的）

6. DBuffer：
	1. 先进行Depth Prepass渲染深度图
	2. 用上面的方法将Decal渲染到类似GBuffer的DBuffer上，然后在渲染GBuffer时（或Forward渲染时），直接应用同样位置对DBuffer进行采样，融合到GBuffer中，可支持烘焙光、Defferred、Forward管线。
> Unity HDRP以及UE中DefferredDecal（使用DBuffer），使用该方法
> 缺点：很大的性能消耗