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
| unity_WorldToShader0/1/2/3          |                      |      |
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
| `_ScreenParams`                     |                      |      |
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
| 相机和屏幕                            |                      |                        |                                                                                                                                                 |
| `float3 _WorldSpaceCameraPos`         |                      |                        | 相机的世界空间内的位置                                                                                                                          |
| `float4 _ProjectionParams`            |                      |                        | x:1.0或-1，是否使用翻转projection矩阵渲染；y:相机近平面；z:相机远平面；w:1/far plane                                                            |
| `float4 _ScreenParams`                |                      |                        | x:相机target texture像素宽度；y:相机target texture像素宽度；z=1+1/width; w=1+1/height                                                           |
| `float4 _ZBufferParams`               |                      |                        | 用于线性化Z buffer值。x=1-far/near; y=far/near; z=x/far; w=y/far                                                                                |
| float4 unity_OrthoParams              |                      |                        | x:正交相机的宽；y:正交相机的高；z:未使用；w:1.0表示正交，0表示透视                                                                              |
| float4x4 unity_CameraProjection       |                      |                        | 相机 projection 矩阵                                                                                                                            |
| float4x4 unity_CameraInvProjection    |                      |                        | 相机 projection 矩阵的逆矩阵                                                                                                                    |
| float4 unity_CameraWorldClipPlanes[6] |                      |                        | 世界空间内，左右下上近远的平截体平面                                                                                                            |
|                                       |                      |                        |                                                                                                                                                 |
| 时间                                  |                      |                        |                                                                                                                                                 |
| `float4 _Time`                        |                      |                        | 自关卡加载的时间(t/20,t,2t,3t),用于animate things inside the shaders                                                                            |
| `float4 _SinTime`                     |                      |                        | sin(time):(t/8,t/4,t/2,t)                                                                                                                       |
| `float4 _CosTime`                     |                      |                        | cos(time):(t/8,t/4,t/2,t)                                                                                                                       |
| float4 unity_DeltaTime                |                      |                        | Delta time:(dt, 1/dt, smoothDt, 1/smoothDt)                                                                                                     |
|                                       |                      |                        |                                                                                                                                                 |
| 照明                                  |                      |                        |                                                                                                                                                 |
| `fixed4 _LightColor0`                 |                      |                        | 灯光颜色                                                                                                                                        |
| `float4 _WorldSpaceLightPos0`         |                      |                        | 平行光：(世界方向，0)，其他光:(世界位置,1)                                                                                                      |
| float4x4 unity_WorldToLight           |                      |                        | 世界到灯光的矩阵。用于采样 cookie和attenuation 纹理                                                                                             |
| float4 unity_4LightPos[XYZ]0          |                      |                        | （前向only） 首要4个非重要点光的世界空间位置                                                                                                    |
| `float4 unity_4LightAtten0`           |                      |                        | （前向only）首要4个非重要点光的衰减因子(attenuation factors)                                                                                    |
| `half4 unity_LightColor[4]`           |                      |                        | （前向only）首要4个非重要点光的颜色                                                                                                             |
| `float4x4 unity_WorldToShadow[4]`     |                      |                        | world-to-shadow 矩阵。第一个用于Spot光，到4用于平行光cascade                                                                                    |
|                                       |                      |                        |                                                                                                                                                 |
| 光照图                                |                      |                        |                                                                                                                                                 |
| Texture2D unity_LightMap              |                      |                        | 包含光照图信息                                                                                                                                  |
| float4 unity_LightmapST[8]            |                      |                        | 缩放和平移UV信息到正确的范围以对光照图纹理采样。                                                                                                |
|                                       |                      |                        |                                                                                                                                                 |
| 雾和环境(Fog & Ambient)               |                      |                        |                                                                                                                                                 |
| `fixed4 unity_AmbientSky`             |                      |                        | 渐变环境照明情况下的天空环境光照颜色，                                                                                                          |
| `fixed4 unity_AmbientEquator`         |                      |                        | 渐变环境照明情况下的赤道环境照明色，                                                                                                            |
| `fixed4 unity_AmbientGround`          |                      |                        | 渐变环境照明情况下的地面环境照明色                                                                                                              |
| `fixed4 UNITY_LIGHTMODEL_AMBIENT`     |                      |                        | 环境照明色(渐变环境情况下的天空颜色)。Legacy 变量                                                                                               |
| `fixed4 unity_FogColor`               |                      |                        | 雾色                                                                                                                                            |
| `fixed4 unity_FogParams`              |                      |                        | 雾计算种的变量：(density/sqrt(ln(2)), density/ln(2),-1/(end-start), end/(end-start))。x在Exp2雾模式中非常有用。y用于Exp模式。z和w用于线性模式。 |
|                                       |                      |                        |                                                                                                                                                 |
| 杂项                                  |                      |                        |                                                                                                                                                 |
| `float4 unity_LODFade`                |                      |                        | 当使用LODGroup是的细节层次渐隐。x为fade(0..1),y被量化到16级。z和w为用到                                                                         |
| `float4 _TextureSampleAdd`            |                      |                        | 只用于UI，由Unity自动设置，基于纹理是否使用Alpha8格式(其值设为(1,1,1,0))或否(值设为(0,0,0,0))                                                                                                                                                |
