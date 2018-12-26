# Google C++规范 Example

[Google开源项目风格指南(中文版)](https://zh-google-styleguide.readthedocs.io/en/latest/google-cpp-styleguide/headers/#acgtyrant) 

[Github Google Style Guide](https://github.com/google/styleguide)

已经看了几遍语言规范, 懂倒是懂, 但总是忘

所以需要实例去更好的记背

本篇主要写实例去加深印象

这个md文档只是为了自己去看



## 1.头文件

<font color=blue>自包含; `#define` 保护; 前置声明; 内联函数;  `#include` 路径顺序</font>

```c++
#ifndef RJPSHAPE_H	// 头文件保护
#define RJPSHAPE_H

#include "shape.h"	// 直接与该头文件关联的.h 

#include "sys/types.h"	// C系统文件
#include "unistd.h"

#include "hash_map"	// C++系统文件
#include "vector"

#include "other/smartpointer.h"	// 其他项目.h

#include "include/RjpClass/rjpdataset.h"	// 本项目.h


#endif // RJPSHAPE_H

```

前置声明有争议; 内联函数要短, 无循环迭代递归和构造析构



## 2.作用域

<font color=blue>命名空间; 匿名命名空间和静态变量; 非成员函数...; 局部变量; 静态和全局变量</font>

```c++
// .h 文件
namespace mynamespace {
namespace np2{
    
// 所有声明都置于命名空间中
// 注意不要使用缩进
class MyClass {
    public:
    ...
    void Foo();
};

} // namespace np2
} // namespace mynamespace
```

禁止使用 `using namespace` 污染命名空间

头文件不要有命名空间的别名使用, 但是在 `.cc` 文件中可以

```c++
// .cpp 文件
namespace baz = mynamespace::np2;

namespace mynamespace {
namespace np2{
    
// 所有声明都置于命名空间中
// 注意不要使用缩进
void MyClass::Foo(){
	...        
}

} // namespace np2
} // namespace mynamespace
```

禁止使用内联命名空间

```c++
// 匿名命名空间 变量内部连接性, 外部无法访问
namespace{
    ...
}	// namespace
```



