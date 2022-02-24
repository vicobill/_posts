Array of Structure(AoS):
```c
struct Entity {
	Vector3 position;
	Quaternion orientation;
	...
};
Entity all_entities[1024];
```

<font color = green> Structure of Array (SoA): </font> 
> 比AoS在cache miss上有很大性能提升

```c
struct Entity {
	Vector3 position[1024];
	Quaternion orientation[1024];
	...
};
Entity all_entities;
```

> Jai 有struct SOA 标注