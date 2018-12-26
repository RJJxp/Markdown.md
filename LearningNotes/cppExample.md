# Google C++规范 Example

[Google开源项目风格指南(中文版)](https://zh-google-styleguide.readthedocs.io/en/latest/google-cpp-styleguide/headers/#acgtyrant) 

[Github Google Style Guide](https://github.com/google/styleguide)

已经看了几遍语言规范, 懂倒是懂, 但总是忘

所以需要实例去更好的记背

本篇主要写实例去加深印象

这个md文档只是为了自己去看



[TOC]



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

```c++
int i = f();	// 局部变量定义最好跟着初始化
vector<int> v = {1, 2};	// vector可以不这么严格

// for循环 实现要高效, 主要指调用构造和析构的次数
Foo f;
for (int i = 0; i < 1000; i++){
    f.doSomething();
}
```



## 3.类

<font color=blue>构造函数的职责; 隐式类型转换; 结构体于类; 继承; 多重继承; 接口; 运算符重载; 存取控制; 声明顺序</font>

大多数接触的不是很深, 看不大懂

```c++
// 隐式类型转换
explicit Func();

// 声明顺序
class RjpShape{
public:
    ...
protected:
    ...
private:
    ...
}
```



## 4.函数

<font color=blue>参数顺序; 简短函数; 引用参数; 函数重载; 缺省参数; 后置语法</font>

```c++
// 参数顺序: 先输入后输出
// 函数最好最好不要超过40行
// 引用的参数一般加 const 限定词
void Foo(const string &in, string in2, string *out);

class MyClass {
public:
    // 由于复杂的机制, 缺省不如重载
    void Analyze(const string &text);
    void Analyze(const char *text, size_t textlen);
};
```



## 5.Google特技

<font color=blue>智能指针; Cpplint</font>

智能指针是一个通过重载 `*` 和 `->` 运算符以表现得如指针一样的类. 智能指针类型被用来自动化所有权的登记工作, 来确保执行销毁义务到位. [std::unique_ptr](http://en.cppreference.com/w/cpp/memory/unique_ptr) 是 C++11 新推出的一种智能指针类型, 用来表示动态分配出的对象的独一无二的所有权; 当 `std::unique_ptr` 离开作用域时, 对象就会被销毁. `std::unique_ptr` 不能被复制, 但可以把它移动（move）给新所有主. [std::shared_ptr](http://en.cppreference.com/w/cpp/memory/shared_ptr) 同样表示动态分配对象的所有权, 但可以被共享, 也可以被复制; 对象的所有权由所有复制者共同拥有, 最后一个复制者被销毁时, 对象也会随着被销毁.

Cpplint 检查风格错误



## 6.C++其他特性

<font color=blue>24个特性</font> 只挑几个能看懂的写一写

<font color=blue>类型转换; 前置自增自减; const用法; 0, nullptr, null; 列表初始化; 模板编程</font>

类型转换不要使用C语言 `y = int(x)` 这种写法, 应当使用 `static<>()` 的写法, 因为不清楚前者究竟在做强制转换还是类型转换.

对简单数值 (非对象), 两种都无所谓. 对迭代器和模板类型, 使用前置自增 (自减).

