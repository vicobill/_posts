- RTTI: 存储类型的类名、命名空间、TypeID、属性、大小、派生自的类型索引等信息。提供运行时类型信息。
```c
struct RTTI {
	int PersistentTypeID;
	DerivedFromInfo 
}
```
- Type： 封装一个`RTTI m_internal`
- TypeManager： 