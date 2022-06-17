核心类：
- `ScriptableRenderer`: 实现一个渲染策略。它描述如何裁切(culling)和光照(lighting)工作以及支持的效果。渲染器可用于所有相机或被每相机覆盖。渲染器通过添加`ScriptableRendererFeature`可被扩展以支持更多效果。渲染器资源被序列化为`ScriptableRendererData`。
- `ScriptableRendererPass`:
- `ScriptableRendererFeature`: 可附加到`ScriptableRenderer`，以在渲染器中注入渲染通道(render passes)。


现在URP中存在的`ScriptableRendererFeature`：
- RenderObjects
- ScreeSpaceAmbientOcclusion
- ExecuteCommandBuffers
- TranslucentImageBlurSource