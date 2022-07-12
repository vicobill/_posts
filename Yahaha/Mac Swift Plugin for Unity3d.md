> 创建的项目是Framework&Library -> Bundle. 
> ❓ Unity 只认bundle，不认libNative.a(Framework&Library->Library)。很奇怪！
  
OC调用Swift代码：
-   OC头文件：在构建设置中，Targets -> Build settings -> Packing -> Product Module Name 设置模块名，那么会生成对应的`<ProductName>-swift.h`文件可供包含。

> 这个文件是隐藏的，在工程中不可见。但可以通过#import后，通过Ctrl+Click查看此文件。

> swift中需要导出给OC，则需要继承自NSObject，用`@objc`标记。

  

Swift调OC：
需要`<ProductName>-Bridging-Header.h`文件 ： 构建设置中，Targets -> Build Settings -> Object-C Bridging Header指定桥接的头文件。这样，可以把Swift中想要调用的OC头文件直接通过#import 写到这个桥接文件中，就可以在Swift中使用OC代码了。

OC-Swift通过头文件桥接。

> Swift导出的方法、类等，使用“@objc”修饰即可导出。

ObjectC函数传参数：
```cpp
int class::sum(int a, int b)
```

```objc
[class sum: 1 : 2]
```


## XCode 问题   

- code object is not signed at all . In subcomponent:
> Project的Build Settings -> Other Code Signing Flags : --deep 