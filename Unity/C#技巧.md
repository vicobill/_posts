C#中模拟Union：
```c#
public struct Union{
[FieldOffset(0)]
public int i;
[FieldOffset(0)]
public float f;
}
```

CullingMask LayerMask:
```
cullingMask = 1<<8; // 只渲染第8层
cullingMask = (1<<8) + (1<<9); // 只渲染第8、9层

cullingmask |= (1<<8); // 在原来基础上增加第8层
cullingMask &= ~(1<<8); // 在原来基础上移除第8层
cullingMask = ~(1<<8); // 渲染除第8层外所有层

```