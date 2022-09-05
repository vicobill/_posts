PPtr: 管理了对象的`instanceID`，可以通过指针方式找到对象。

C项目如果没有bool，包含`#include <stdbool.h> `

conan可通过`conan copy package/version local/version` 复制一份本地版本，再通过`package/version@local/version`形式本地加载。