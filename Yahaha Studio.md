- [ ]  在框选物体时，矩形在变化时，会添加选不中的物体。
> 可能跟PixelSelection的最终图片有关

- [ ]  CameraLazyRender为什么GC高


场景中存在2个相机：EditCamera, PixelSelectionCamera
EditCamera正常渲染，PixelSelectionCamera进行特殊效果展示
EditCamera本身就会渲染所有物体。
PixelSelectionCamera使用RenderPipelineAsset中定义的渲染特性(RenderFeature)进行渲染。RenderFeature中的RenderObject可以在渲染队列中使用自定方式渲染。



性能剖析中，
