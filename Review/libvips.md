启动与关闭：`vips_init(const char*);vips_shutdown();`

VipsImage:基础数据对象。可从磁盘文件、内存块或C数组、甚至带格式对象如JPEG创建。`vips_image_new_from_file()`等类似函数。VIPS读取足够的信息，但并不立即解码，直到真正需要像素数据时才解码。当有一个Image，可通过`vips_iamge_get_width()`或`g_object_get()`获取`GObject`属性。所有VIPS对象都是immutable，意味着只能获取属性，但不可设置。

引用计数：VIPS基于`GObject`库，`vips_image_new_from_file()`返回计数为1的对象。当处理了image，使用`g_object_unref()`处理它。如果将image传给一个操作，操作需要保存image的拷贝，那么将会ref它。所以一旦不再需要它，可尽快立即unref。


绑定C API：C API大量使用varargs，对于其他语言绑定不是很好处理。因此可使用如下层：
1. 创建操作符。通过`vips_operation_new()`从昵称操作符如`add`创建`VipsOperation`对象。
2. 设置参数。可通过`vips_argument_map()`循环遍历操作以获得名称和输入参数类型。每个参数，可转成`GValue`，然后通过`g_object_set_property()`设置操作符的值。
3. 执行`vips_cache_operation_build()`
4. 提取结果。通过`vips_argument_map()`循环操作符参数，但现在查询输出参数。可通过`g_object_get_property()`提取值。


```c++
vips_image_new_from_file(filename,...): 提取文件的加载的操作名称，然后调用vips_call_split_option_string(operation_name, option_string, ...);

vips_call_split_option_string() 调用 vips_call_by_name(operation_name,option_string,...)

vips_call_by_name() 此函数是内部函数。其大致实现为：

	operation = vips_operation_new(operation_name);
	vips_object_set_from_string(operation,option_string); // 从string设置object的参数。
	result = vips_call_required_optional(&operation,...); // 
	g_object_unref(operation);


vips_call_required_optional(....) 的实现，大致类似：

	result = vips_operation_set_valist_required(operation,...) || vips_object_set_valist(operation,...);
	vips_cache_operation_buildp(operation);
	result = vips_operation_get_valist_required(operation,...) || vips_operation_get_valist_optional(operation,...);


vips_cache_operation_buildp(operations) 的实现，大致类似：

	if (hit = vips_cache_operation_lookup(operation)){
		g_object_unref(operation);
		operation = hit;
	} else {
		vips_object_build(operation);
		vips_cache_operation_add(operation);
	}

vips_object_build() 的大致实现：

	object_class = VIPS_OBJECT_GET_CLASS(object);
	object_class->build(object);
	vips_argument_map(object,vips_object_check_required,&result, &iomask);
	vips_object_postbuild(object);

object_class->build() 的大致实现：

	g_object_set(object,"nickname",object_class->nickname,"description",object_class->description);
	vips_argument_map(object,vips_object_check_required,&result, &iomask);

整体的流程：
- 从文件名称中获取文件名、操作名称、参数等
- 创建一个Operation,并设置其参数
- 遍历输入的参数信息，通过g_object_set_property()设置Operation参数值
- 查询是否已经存在Operation，如果存在，unref此Operation，设置为当前的Operation；如果未找到，构建此Operation,并添加进cache中。
- 遍历Operation参数，通过g_object_get()获取参数值，对遍历的参数arg->pspec对象调用g_object_unref
- 对Operation对象调用g_object_unref


```

Vips概念：
- Operation
- 
