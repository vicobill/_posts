Editor主窗口：是ContainerWindow。在ContainerWindow::Init()时创建窗口，在ContainerWindow::ContainerWndProc() 处理Windows平台下的消息。




IMGUI的绘制，在`GUIStyle::Draw(GUIState&, screenRect, GUIContent, isHover,isActive,on,hasKeyboardFocus)

先绘制背景GUIStyle::DrawBackground()，再绘制内容 GUIStyle::DrawContent(),

DrawBackground() |> DrawClippedTexture() |> DrawGUITexture() |> DynamicVBO::DrawChunk() |> GfxDevice::DrawBuffers()

```c++
void DrawGUITexture(...)
{
	InitializeGUIShaders();
	
	GetGfxDevice();
	FillGUITextureVBOChunkInverted(...); // 上下颠倒
	
}
```



GfxDeviceD3D11Base::DrawBuffers();


SceneView.cs定义了Unity编辑器主编辑视图。其渲染通过Repaint()函数。
SceneView <: SearchableEditorWindow <: EditorWindow
EditorWindow一部分来自C++中Editor/Src/ContainerWindow.bindings.h映射，一部分自行实现。
EditorWindow中，有个字段HostView，Repaint()调用的即是HostView.Repaint()。
HostView <: GUIView
GUIView一部分来自Editor/Src/GUIView.bindings.h，一部分自行实现。
```c++
struct MonoViewData{
	UnityEngineObjectMemoryLayout data;
    GUIView* m_ViewPtr;
};
void MonoGUIView::Repaint()
{
	ExtractMonoObjectData<MonoViewData>(this.GetInstance()).m_ViewPtr->RequestRepaint();
}

void GUIView::RequestRepaint()
{
	m_NeedsRepaint=true;
	SignalTick();
}
// 在下个循环中，调用GUIView::DoPaint()
void GUIView::DoPaint()
{
	m_GfxWindow->BeginRendering()
	m_AuxBackBufferManager.OnWindowBeginRender(this);
	
	RenderTexture::SetBackbufferActive();
	ContainerWindow* window = GetContainerFromView(m_View);
	UpdateScreenManager(); // 更新屏幕的大小
	
	GfxDevice& device = GetGfxDevice();
	device.SetViewport(RectInt(...)); // 调用ID3D11DeviceContext->RSSetViewports()
	device.DisableScissor();
	device.SetWireframe(false);
	InputEvent::RepaintEvent(m_View);
	ProcessInputEvent(repaintEvent);
	
	m_AuxBackBufferManager.OnWindowEndRender();
	m_GfxWindow->EndRendering(); // 调用SwapChain->Present() 呈现
}
```
