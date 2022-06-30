# 内置变量
| 内置                                | 说明                 | 用途 |
| ----------------------------------- | -------------------- | ---- |
| unity_ObjectToWorld                 | 当前 model 矩阵      |      |
| unity_WorldToObject                 | 当前世界矩阵的逆矩阵 |      |
| unity_MatrixV                       |                      |      |
| unity_MatrixInvV                    |                      |      |
| glstate_matrix_projection           |                      |      |
| unity_MatrixVP                      |                      |      |
| unity_WorldToCamera                 |                      |      |
| unity_CameraToWorld                 |                      |      |
| unity_WorldToShader[0-3]          |                      |      |
| unity_LightmapMatrix                |                      |      |
| unity_Projector                     |                      |      |
| unity_ProjectorDistance             |                      |      |
| unity_ProjectorClip                 |                      |      |
| unity_GUIClipTextureMatrix          |                      |      |
| unity_WorldToLight                  |                      |      |
| unity_CameraProjection              |                      |      |
| unity_CameraInvProjection           |                      |      |
| unity_ProbeVolumeWorldToObject      |                      |      |
| unity_matrixPreviousM               |                      |      |
| unity_matrixPreviousMI              |                      |      |
|                                     |                      |      |
| **vectors**                         |                      |      |
| unity_LightColor[0-7]               |                      |      |
| unity_LightPosition[0-7]            |                      |      |
| unity_SpotDirection[0-7]            |                      |      |
| unity_LightAtten[0-7]               |                      |      |
| unity_VertexLightParams             |                      |      |
| glstate_lightmodel_ambient          |                      |      |
| `_WorldSpaceLightPos0`              |                      |      |
| `_LightColor0`                      |                      |      |
| `_WorldSpaceCameraPos`              |                      |      |
| unity_WorldTransformParams          |                      |      |
| `_ProjectionParams`                 |                      |      |
| `_ScreenParams`                     |     屏幕参数，x:width,y:height, z:1+1.0/width,w:1+1.0/height                 |      |
| `_ZBufferParams`                    |                      |      |
| unity_OrthoParams                   |                      |      |
| `_LightPositionRange`               |                      |      |
| `_LightProjectionParams`            |                      |      |
| unity_Ambient                       |                      |      |
| unity_LightmapFade                  |                      |      |
| unity_OcclusionMaskSelector         |                      |      |
| unity_ProbesOcclusion               |                      |      |
| `_ShadowOffsets[0-3]`               |                      |      |
| `_LightShadowData`                  |                      |      |
| unity_LightShadowBias               |                      |      |
| `_LightSplitsNear`                  |                      |      |
| `_LightSplitsFar`                   |                      |      |
| unity_ShadowSplitSphere[0-3]        |                      |      |
| unity_ShadowSplitSqRadii            |                      |      |
| unity_ShadowCascadeScales           |                      |      |
| unity_ShadowFadeCenterAndType       |                      |      |
| unity_ShadowColor                   |                      |      |
| unity_LODFade                       |                      |      |
| unity_RenderingLayer                |                      |      |
| unity_LightmapST                    |                      |      |
| unity_DynamicLightmapST             |                      |      |
| unity_SHA[rgb]                      |                      |      |
| unity_SHB[rgb]                      |                      |      |
| unity_SHC                           |                      |      |
| `_Time`                             |                      |      |
| `_LastTime`                         |                      |      |
| `_SinTime`                          |                      |      |
| `_CosTime`                          |                      |      |
| `_PiTime`                           |                      |      |
| unity_DeltaTime                     |                      |      |
| unity_4LightPos[XYZ]0               |                      |      |
| unity_4LightAtten0                  |                      |      |
| unity_FogStart                      |                      |      |
| unity_FogEnd                        |                      |      |
| unity_FogDensity                    |                      |      |
| unity_FogColor                      |                      |      |
| unity_FogParams                     |                      |      |
| unity_CameraWorldClipPlanes[0-5]    |                      |      |
| unity_AmbientSky                    |                      |      |
| unity_AmbientEquator                |                      |      |
| unity_AmbiendGround                 |                      |      |
| unity_IndirectorSpecColor           |                      |      |
| `unity_SpecCube[0-1]_HDR`           |                      |      |
| `unity_SpecCube[0-1]_BoxMax `       |                      |      |
| `unity_SpecCube[0-1]_BoxMin `       |                      |      |
| `unity_SpecCube[0-1]_ProbePosition` |                      |      |
| unity_BillboardNormal               |                      |      |
| unity_BillboardTangent              |                      |      |
| unity_BillboardCameraParams         |                      |      |
| unity_ProbeVolumeMin                |                      |      |
| unity_ProbeVolumeSizeInv            |                      |      |
| unity_ProbeVolumeParams             |                      |      |
| unity_StereoEyeIndex                |                      |      |
| unity_LightData                     |                      |      |
| unity_LightIndices[0-1]             |                      |      |
| unity_ReflectionProbeData           |                      |      |
| unity_MotionVectorsParams           |                      |      |
| unity_HalfStereoSeparation          |                      |      |
|                                     |                      |      |
| **texenvs**                         |                      |      |
| white                               |                      |      |
| black                               |                      |      |
| red                                 |                      |      |
| gr[ae]y                             |                      |      |
| linearGr[ae]y                       |                      |      |
| gr[ae]yscaleRamp                    |                      |      |
| bump                                |                      |      |
| blackCube                           |                      |      |
| lightmap                            |                      |      |
| unity_Lightmap                      |                      |      |
| unity_LightmapInd                   |                      |      |
| unity_ShadoeMask                    |                      |      |
| unity_DynamicLightmap               |                      |      |
| unity_DynamicDirectionality         |                      |      |
| unity_DynamicNormal                 |                      |      |
| unity_DitherMask                    |                      |      |
| `_DitherMaskLOD`                    |                      |      |
| `_DitherMaskLOD2D`                  |                      |      |
| unity_RandomRotation16              |                      |      |
| unity_NHxRoughness                  |                      |      |
| unity_SpecCube[01]                  |                      |      |
| unity_ProbeVolumeSH                 |                      |      |
|                                     |                      |      |

# 内置映射URP
| name                                  | Built-in             | SRP                    | 说明                                                                                                                                            |
| ------------------------------------- | -------------------- | ---------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| UNITY_MATRIX_MVP                      | UnityObjectToClipPos | TransformObjectToHClip | 当前 model * view * projection 矩阵                                                                                                             |
| UNITY_MATRIX_MV                       |                      |                        | 当前 model * view 矩阵                                                                                                                          |
| UNITY_MATRIX_V                        |                      |                        | 当前 view 矩阵                                                                                                                                  |
| UNITY_MATRIX_P                        |                      |                        | 当前 projection 矩阵                                                                                                                            |
| UNITY_MATRIX_VP                       |                      |                        | 当前 view * projection 矩阵                                                                                                                     |
| UNITY_MATRIX_T_MV                     |                      |                        | model * view 矩阵的转置(Transpose)                                                                                                              |
| UNITY_MATRIX_IT_MV                    |                      |                        | model * view矩阵的逆转置(Inverse Transpose)                                                                                                     |
|                                       |                      |                        |                                                                                                                                                 |
| **相机和屏幕**                            |                      |                        |                                                                                                                                                 |
| `float3 _WorldSpaceCameraPos`         |                      |                        | 相机的世界空间内的位置                                                                                                                          |
| `float4 _ProjectionParams`            |                      |                        | x:1.0或-1，是否使用翻转projection矩阵渲染；y:相机近平面；z:相机远平面；w:1/far plane                                                            |
| `float4 _ScreenParams`                |                      |                        | x:相机target texture像素宽度；y:相机target texture像素宽度；z=1+1/width; w=1+1/height                                                           |
| `float4 _ZBufferParams`               |                      |                        | 用于线性化Z buffer值。x=1-far/near; y=far/near; z=x/far; w=y/far                                                                                |
| float4 unity_OrthoParams              |                      |                        | x:正交相机的宽；y:正交相机的高；z:未使用；w:1.0表示正交，0表示透视                                                                              |
| float4x4 unity_CameraProjection       |                      |                        | 相机 projection 矩阵                                                                                                                            |
| float4x4 unity_CameraInvProjection    |                      |                        | 相机 projection 矩阵的逆矩阵                                                                                                                    |
| float4 unity_CameraWorldClipPlanes[6] |                      |                        | 世界空间内，左右下上近远的平截体平面                                                                                                            |
|                                       |                      |                        |                                                                                                                                                 |
| **时间**                                  |                      |                        |                                                                                                                                                 |
| `float4 _Time`                        |                      |                        | 自关卡加载的时间(t/20,t,2t,3t),用于animate things inside the shaders                                                                            |
| `float4 _SinTime`                     |                      |                        | sin(time):(t/8,t/4,t/2,t)                                                                                                                       |
| `float4 _CosTime`                     |                      |                        | cos(time):(t/8,t/4,t/2,t)                                                                                                                       |
| float4 unity_DeltaTime                |                      |                        | Delta time:(dt, 1/dt, smoothDt, 1/smoothDt)                                                                                                     |
|                                       |                      |                        |                                                                                                                                                 |
| **照明**                                  |                      |                        |                                                                                                                                                 |
| `fixed4 _LightColor0`                 |                      |                        | 灯光颜色                                                                                                                                        |
| `float4 _WorldSpaceLightPos0`         |                      |                        | 平行光：(世界方向，0)，其他光:(世界位置,1)                                                                                                      |
| float4x4 unity_WorldToLight           |                      |                        | 世界到灯光的矩阵。用于采样 cookie和attenuation 纹理                                                                                             |
| float4 unity_4LightPos[XYZ]0          |                      |                        | （前向only） 首要4个非重要点光的世界空间位置                                                                                                    |
| `float4 unity_4LightAtten0`           |                      |                        | （前向only）首要4个非重要点光的衰减因子(attenuation factors)                                                                                    |
| `half4 unity_LightColor[4]`           |                      |                        | （前向only）首要4个非重要点光的颜色                                                                                                             |
| `float4x4 unity_WorldToShadow[4]`     |                      |                        | world-to-shadow 矩阵。第一个用于Spot光，到4用于平行光cascade                                                                                    |
|                                       |                      |                        |                                                                                                                                                 |
| **光照图**                                |                      |                        |                                                                                                                                                 |
| Texture2D unity_LightMap              |                      |                        | 包含光照图信息                                                                                                                                  |
| float4 unity_LightmapST[8]            |                      |                        | 缩放和平移UV信息到正确的范围以对光照图纹理采样。                                                                                                |
|                                       |                      |                        |                                                                                                                                                 |
| **雾和环境(Fog & Ambient)**               |                      |                        |                                                                                                                                                 |
| `fixed4 unity_AmbientSky`             |                      |                        | 渐变环境照明情况下的天空环境光照颜色，                                                                                                          |
| `fixed4 unity_AmbientEquator`         |                      |                        | 渐变环境照明情况下的赤道环境照明色，                                                                                                            |
| `fixed4 unity_AmbientGround`          |                      |                        | 渐变环境照明情况下的地面环境照明色                                                                                                              |
| `fixed4 UNITY_LIGHTMODEL_AMBIENT`     |                      |                        | 环境照明色(渐变环境情况下的天空颜色)。Legacy 变量                                                                                               |
| `fixed4 unity_FogColor`               |                      |                        | 雾色                                                                                                                                            |
| `fixed4 unity_FogParams`              |                      |                        | 雾计算种的变量：(density/sqrt(ln(2)), density/ln(2),-1/(end-start), end/(end-start))。x在Exp2雾模式中非常有用。y用于Exp模式。z和w用于线性模式。 |
|                                       |                      |                        |                                                                                                                                                 |
| **杂项**                                  |                      |                        |                                                                                                                                                 |
| `float4 unity_LODFade`                |                      |                        | 当使用LODGroup是的细节层次渐隐。x为fade(0..1),y被量化到16级。z和w为用到                                                                         |
| `float4 _TextureSampleAdd`            |                      |                        | 只用于UI，由Unity自动设置，基于纹理是否使用Alpha8格式(其值设为(1,1,1,0))或否(值设为(0,0,0,0))                                                                                                                                                |

> UnityInput.hlsl中定义了部分从Unity的内置参数
## 内置宏
| 宏                                                             | 说明                                                                                                                                              |
| -------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| SHADER_API_D3D11                                               | D3D11 目标平台                                                                                                                                    |
| SHADER_API_GLCORE                                              | Desktop OpenGL平台                                                                                                                                |
| SHADER_API_GLES                                                | GLES2.0                                                                                                                                           |
| SHADER_API_GLES3                                               | GLES 3.0+                                                                                                                                         |
| SHADER_API_METAL                                               | ios/mac                                                                                                                                           |
| SHADER_API_VULKAN                                              | vulkan                                                                                                                                            |
| SHADER_API_D3D11_9X                                            | D3D11 9.x特性，UWP平台                                                                                                                            |
| SHADER_API_MOBILE                                              | GLES,GLES3,METAL等                                                                                                                                |
| SHADER_TARGET_GLSL                                             | GL,GLES等                                                                                                                                         |
|                                                                |                                                                                                                                                   |
| Shader Model 版本                                              |                                                                                                                                                   |
| SHADDER_TARGET                                                 | 30对应3.0，匹配`#pragma target`指令                                                                                                               |
|                                                                |                                                                                                                                                   |
| UNITY_VERSION                                                  | 如501对应5.0.1                                                                                                                                    |
|                                                                |                                                                                                                                                   |
| 不同平台辅助宏                                                 |                                                                                                                                                   |
| UNITY_BRANCH                                                   | 放在条件语句前，告诉编译器应该编译成具体分支                                                                                                      |
| UNITY_FLATTEN                                                  | 放在条件语句钱，告诉编译器应该平整，以避免具体分支指令                                                                                            |
| UNITY_NO_SCREENSPACE_SHADOWS                                   | 平台不使用cascaded 屏幕空间阴影图(mobile)                                                                                                         |
| UNITY_NO_LINEAR_COLORSPACE                                     | 平台不支持线性色彩空间(mobile)                                                                                                                    |
| UNITY_NO_RGBM                                                  | 平台不使用RGBM 压缩阴影图(mobile)                                                                                                                 |
| UNITY_NO_DXT5nm                                                | 平台不使用DXT5nm Normal Map 压缩(mobile)                                                                                                          |
| UNITY_FRAMEBUFFER_FETCH_AVAILABLE                              | 平台`抓取帧缓冲颜色`功能可被启用(ios-GLES2,3,Metal)                                                                                               |
| UNITY_USE_RGBA_FOR_POINT_SHADOWS                               | 平台点光阴影图使用RGBA纹理编码depth（其他平台使用单通道浮点纹理）                                                                                 |
| UNITY_ATTEN_CHANNEL                                            | 平台光衰减纹理使用的通道。用于per-pixel光照代码。r或a                                                                                             |
| UNITY_HALF_TEXEL_OFFSET                                        | 平台需要half-texel偏移校正以映射texel到pixel(例如D3D9)                                                                                            |
| UNITY_UV_START_AT_TOP                                          | 如果纹理V坐标在上为0，此值则为1.D3D-like平台为1，OpenGL-like此值为0                                                                               |
| UNITY_MIGHT_NOT_HAVE_DEPTH_Texture                             | 平台可模拟阴影图或深度图，通过自行渲染深度到Texture                                                                                               |
| UNITY_PROJ_COORD(a)                                            | 给出4组件向量，返回用于project的合适的纹理坐标                                                                                                    |
| UNITY_NEAR_CLIP_VALUE                                          | 定义的近平面值，D3D-like下为0.0，OpenGL-like下为-1.0                                                                                              |
| UNITY_VPOS_TYPE                                                | 定义像素位置输入(pixel position input)的类型：D3D9下为`float2`，其他为`float4`                                                                    |
| UNITY_CAN_COMPILE_TESSELLATION                                 | 当Shader编译器不理解tessellation Shader HLSL 语法(only D3D11)时被定义                                                                             |
| UNITY_INITIALIZE_OUTPUT(type,name)                             | 初始化变量为0                                                                                                                                     |
| UNITY_COMPILER_HLSL,UNITY_COMPILER_HLSL2GLSL,UNITY_COMPILER_CG | 指示用于编译Shader的编译器。                                                                                                                      |
| UNITY_REVERSED_Z                                               | 在使用反转Z buffer的平台定义。保存的Z值1..0替换0..1                                                                                               |
|                                                                |                                                                                                                                                   |
| 阴影映射宏                                                     |                                                                                                                                                   |
| UNITY_DECLARE_SHADOWMAP(tex)                                   | 声明阴影图纹理变量                                                                                                                                |
| UNITY_SAMPLE_SHADOW(tex,uv)                                    | 对阴影图纹理采样。返回0..1单float值                                                                                                               |
| UNITY_SAMPLE_SHADOW_PROJ(tex,uv)                               | 如上，但做一次project。uv是float4, 其他组件都会除以`.w`                                                                                           |
|                                                                |                                                                                                                                                   |
| 常量缓冲(Constant Buffer)                                      | D3D11将所有Shader变量组到常量缓冲中。绝大多数unity内置变量已经被组织，但也可以把自定义的变量加入以优化查询频率                                    |
| CBUFFER_START(name), CBUFFER_END                               |                                                                                                                                                   |
|                                                                |                                                                                                                                                   |
| 纹理/采样器(Texture/Sampler)声明                               | 在很多平台，Texture和Sampler是不同对象，Sampler数量是受限的                                                                                       |
| UNITY_DECLARE_TEX2D(name)                                      | 声明一个纹理和采样器对                                                                                                                            |
| UNITY_DECLARE_TEX2D_NOSAMPLER(name)                            | 声明一个纹理，但不声明采样器                                                                                                                      |
| UNITY_DECLARE_TEX2DARRAY(name)                                 | 声明一个纹理数组采样器变量                                                                                                                        |
| UNITY_SAMPLE_TEX2D(name, uv)                                   | 从纹理和采样器对中采样，使用给定纹理坐标                                                                                                          |
| UNITY_SAMPLE_TEX2D_SAMPLER(name, samplername, uv)              | 从使用一个其他纹理的采样器(samplername)进行纹理(name)采样                                                                                         |
| UNITY_SAMPLE_TEX2DARRAY(name, uv)                              | 从一个纹理数组采样，通过float3 uv，坐标的z是数组元素索引                                                                                          |
| UNITY_SAMPLE_TEX2DARRAY_LOD(name,uv,lod)                       | 从纹理数组以显式mipmap level采样                                                                                                                  |
|                                                                |                                                                                                                                                   |
| Surface Shader通道指示器                                       | 当Surface Shader被编译，它们生成很多通道做照明。当编译每个通道，以下宏其中一个被定义                                                              |
| UNITY_PASS_FORWARDBASE                                         | 前向渲染基础通道（主平行光，光照图，SH）                                                                                                          |
| UNITY_PASS_FORWARDADD                                          | 前向渲染附加通道（one light per pass）                                                                                                            |
| UNITY_PASS_DEFERRED                                            | 延迟渲染通道（G-Buffer）                                                                                                                          |
| UNITY_PASS_SHADOWCASTER                                        | Shadow caster和depth Texture 渲染通道                                                                                                             |
| UNITY_PASS_PREPASSBASE                                         | 旧的延迟照明基础通道（渲染normals 和 specular exponent）                                                                                          |
| UNITY_PASS_PREPASSFINAL                                        | 旧的延迟照明最终通道（应用光照和纹理）                                                                                                            |
|                                                                |                                                                                                                                                   |
| 禁用自动更新                                                   |                                                                                                                                                   |
| UNITY_SHADER_NO_UPGRADE                                        | 允许禁止Unity自动升级并修改shader文件                                                                                                             |
|                                                                |                                                                                                                                                   |
| 深度纹理辅助宏                                                 | 深度图用于从相机渲染Depth                                                                                                                         |
| UNITY_TRANSFER_DEPTH(o)                                        | 计算顶点的eye space depth，并输出到o(必须为float2)。用在vertex program渲染到depth纹理。在native 深度纹理，此宏不做任何事，因为Zbuffer值默认被渲染 |
| UNITY_OUTPUT_DEPTH(i)                                          | 从i(float2)中返回eye space深度。用在fragment program渲染到深度纹理。在netive深度纹理，此宏返回0。因为z隐式被渲染。                                |
| COMPUTE_EYEDEPTH(i)                                            | 计算顶点的eye space深度，并输出到o。用在vertex program当不渲染到深度纹理时                                                                        |
| DECODE_EYE_DEPTH(i), LinearEyeDepth(i)                         | 给出从深度纹理i的高精度值，返回对应的eye space 深度                                                                                                 |
| Linear01Depth(i)                                               | 给出从深度纹理i的高精度值，返回对应的0..1的线性深度                                                                                                                                                  |

## 内置函数(@see code)
```c
// 将点从对象空间转到相机裁切空间的齐次坐标。等同于 `mul(UNITY_MATRIX_MVP,float4(pos,1.0))`
float4 UnityObjectToClipPos(float3 pos);
// 将点从对象空间转到视图空间。这等同于 `mul(UNITY_MATRIX_MV, float4(pos,1.0)).xyz`
float3 UnityObjectToViewPos(float3 pos);

// 返回从给定对象空间顶点位置到相机的世界空间方向（非归一化）
float3 WorldSpaceViewDir(float4 v);
// 返回从给定对象空间顶点位置到相机的对象空间方向（非归一化）
float3 ObjSpaceViewDir(float4 v);
float2 ParrallaxOffset(half h)

```

## Shader数据类型和精度
高精度：float, 32bits，用于世界空间坐标、纹理坐标系、三角几何或幂次计算等
中精度：half, 16bits，-60000-+60000,3个数值精度，用于方向、物体空间位置、HDR颜色等。
低精度：fixed, 11bits, -2.0-+2.0，1/256精度，用于常规颜色、纹理，及其简单计算。
纹理和采样器：
- sampler2D,samplerCUBE:
- 可后缀精度，如sampler2D_float ,sampler2D_half等

## 不同图形API的区别
- D3D-like：Metal/Console: 0在上，增至下
- OpenGL/ES: 0在下，增至上。
### 渲染至RenderTexture
在渲染至RenderTexture时，D3D-like，Unity内部渲染时上下翻转。
```C
#if UNITY_UV_STARTS_AT_TOP
if (_MainTex_TexelSize.y < 0)
	uv.y = 1 - uv.y
#endif
```
当使用Image effects处理一个Render Texture时，Graphics.Blit会处理不一致的坐标系。但，如果有多余1个RenderTexture一起处理，RenderTexture则在D3D-like平台下的AntiAliasing时会有不同的垂直方向。为了标准化，则需要在Vertex Shader翻转屏幕Texture（如上述代码）以匹配OpenGL-like的坐标系标准。
类似的也发生在GrabPass中，则需要使用`ComputeGrabScreenPos`函数。
### 渲染至UV 空间
当渲染到Texture(UV)坐标系空间，需要调整Shader以保持一致。内部的`ProjectionParams.x`包含`+1`和`-1`，`-1`表示projection已经上下翻转以匹配OpenGL projection坐标系，`+1`表示还没翻转。如：
```hlsl
float4 vert(float2 uv : TEXCOORD) :SV_POSITION
{
	float4 pos;
	pos.xy = uv;
	if(_ProjectionParams.x < 0) 
		pos.y = 1 - pos.y;
	pos.z = 0;
	pos.w = 1;
	return pos;
}
```
### 裁切空间坐标系(Clip space)
类似Texture(UV)坐标系，裁切坐标系(也称为post-projection 空间坐标系)在D3D和OpenGL平台不同：
- D3D-like: 深度从+1近平面到0.0远平面。
- OpenGL-like: 深度从-1.0近平面到+1.0远平面。
可获取`UNITY_NEAR_CLIP_VALUE`获取近平面值。在脚本代码，`GL.GetGPUProjectionMatrix`可从Unity坐标系统(遵循OpenGL约定)转换到D3D-like坐标系。
### Shader计算精度
为了避免精度问题，确保在目标平台测试过shader。PC GPU支持所有浮点类型（以32位精度计算），但Mobile GPU却不是。
### `const`声明
- HLSL中的`const`类似C#和C++中定义，使变量只读，但可以以任何方式初始化。
- GLSL中`const`表示变量时编译期常量，且必须在编译期被初始化（字面量或其他const计算结果）（推荐）
### 语义(Semantics)
- Vertex Shader输出(clip space)位置：`SV_POSITION`。有时可用POSITION语义，但它在Sony PS4的tessellation中不工作。
- Fragment Shader 输出颜色：`SV_Target`。有时可使用COLOR或COLOR0,但它们在Sony PS4中不工作。
当渲染Mesh为Point，Vertex Shader输出`PSIZE`语义，在OpenGLES或Metal点大小被当作未定义。
### D3D Shader编译期语法
- Surface Shader 顶点变化有一个`out`参数必须初始化：
```c
void vert(inout appdata_full v, out Input o) {
	UNITY_INITIALIZE_OUTPUT(Input, o);
	//...
}
```
- 部分初始化的值。例如函数返回`float4`，但只设置为`.xyz`。`w`也必须初始化。
- 在Vertex Shader使用`tex2D`。 这是无效的，因为UV衍生在vertex shader中不存在。你需要显式采样mip level，例如使用`tex2Dlod(tex,float4(uv,0,0))`。并且需要添加`#prgram target 3.0`，因为`tex2Dlod`是Shader model 3.0特性。
### DX11 HLSL 语法
很多Surface shader不理解DX11 HLSL的语法。例如`StructuredBuffer`s,`RWTexture`s等
### 使用Shader framebuffer fetch
framebuffer fetcher用于应用程序化blending，通过提供当前fragment颜色作为输入传入到Fragment Shader中。Unity可以使用此功能：需要在参数中提供`inout`参数。
```c
#pragma only_renderers framebufferfetch
void frag(v2fi, inout half4 ocol : SV_Target) {
	//ocol可读（当前framebuffer颜色）也可写（改变颜色）
}
```
### Depth(Z)的方向
- DX11,DX12,Metal: 逆反方向
	- depth(Z) buffer 1.0为近平面，0.0为远平面。
	- Clip（裁切空间）范围为[near,0]
- 传统方向：
	- depth(Z) buffer 0.0为近平面，1.0为远平面
	- Clip space基于不同平台：
		- D3D-like: [0,far],0为近平面，1为远平面
		- OGL-like: [-near,far]
当在逆反方向平台使用Shader时：
- `UNITY_REVERSED_Z`被定义
- `_CameraDepthTexture` 纹理从1(near)到0(far)
- clip space 方位在near到0(far)
以下宏和函数可工作在任何depth(Z)方向:
- `Linear01Depth(float z)`
- `LinearEyeDepth(float z)`
- `UNITY_CALC_FOG_FACTOR(coord)`
### fetching the depth buffer
如果手动fetching深度缓冲值，则需要手动检测方向，如：
```
float z = tex2D(_CameraDepthTexture, uv);
#if defined(UNITY_REVERSED_Z)
 z = 1.0f - z;
#endif
```
### 使用clip space
如果手动使用clip spase(Z)深度，也需要抽象平台差异：
```
float clipSpaceRange01 = UNITY_Z_0_FAR_FROM_CLIPSPACE(rawClipSpace);
```
此函数在OpenGL平台返回1*-near到far。
### Projection 矩阵
`GL.GetGPUProjectionMatrix`在depth(Z)反转的平台上返回z-反转的矩阵。如果手动组合projection 矩阵（例如自定义shadow或深度渲染），你需要反转depth(z)方向：
```csharp
var shadowProjection = Matrix4x4.Ortho(...); // 阴影相机projection矩阵
var shadowViewMat = ... // 阴影相机view矩阵
var shadowSpaceMatrix = ... // clip到shadowMap纹理空间的矩阵

// 当引擎计算从相机projection到设备projection矩阵时， m_shadowCaemra.projectionMatrix被隐式反转
m_shadowCamera.projectionMatrix = shadowProjection;

// 手动反转
if (SystemInfo.useReversedZBuffer) {
shadoeProjection[2,0] = -shadoeProjection[2,0];
shadoeProjection[2,1] = -shadoeProjection[2,1];
shadoeProjection[2,2] = -shadoeProjection[2,2];
shadoeProjection[2,3] = -shadoeProjection[2,3];
}
m_shadowMatrix = shadowSpaceMatrix * shadowProjection * shadowViewMat;
```
### Depth(Z) bias（偏差）
Unity自动处理深度偏差以匹配Unity深度方向，如果使用Native渲染插件，则需要手动处理反转深度。
可使用`SystemInfo.usesReversedZBuffer`查看是否使用反转的深度(Z)


## Shader 性能提议
- 尽量使用`half`精度，在世界空间和纹理坐标系中使用`float`
- 超越函数（如pow,exp,log,cos,sin,tan)等，尽量不在低端硬件上使用。考虑使用查询texture作为替换。
- 避免编写自己的操作如normalize,dot,inversesqrt等，使用Unity内置操作以生成最佳代码。
- Alpha Test(discard)操作通常会降低fragment shader运行
- 固定管线的AlphtTest或其程序化等式`clip()`在不同平台有不同性能表现：
	- 移除全部透明像素时，使用它会带来小幅益处
	- 在ios和android设备，alpha test会大幅降低性能
- ColorMask用于提取颜色通道，在大多数ios和android设备性能低下。

## 调试Shader
### Visual Studio
1. 需要通过Visual Studio调试，可对每个需要调试的shader启用`#pragma enable_d3d11_debug_symbols`。（此指令极大降低性能，所以在最后构建时一定要移除它）
> 用PIX调试时，也需要此指令
3. Unity构建时，同时生成Visual Studio项目。也可以手动创建空的Visual Studio项目，在调试命令中，将`$(TargetPath)`改成游戏.exe，在命令行参数中，输入`-force-d3d11`
4. 然后可以用Visual Studio的图形诊断器剖析Shader。


# 常见的Shader内置函数
