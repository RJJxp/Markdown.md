# Google C++规范 Example

[Google开源项目风格指南(中文版)](https://zh-google-styleguide.readthedocs.io/en/latest/google-cpp-styleguide/headers/#acgtyrant) 

[Github Google Style Guide](https://github.com/google/styleguide)

已经看了几遍语言规范, 懂倒是懂, 但总是忘

所以需要实例去更好的记背

本篇主要写实例去加深印象

这个md文档只是为了自己去看



注意, 有些代码直接拷贝, 所以缩进习惯和自己的不太一样



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

有时, 系统内用 `<lib.h>` , 自己写的用 `"lib.h"` 



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

- 类型转换不要使用C语言 `y = int(x)` 这种写法, 应当使用 `static<>()` 的写法, 因为不清楚前者究竟在做强制转换还是类型转换.

- 对简单数值 (非对象), 两种都无所谓. 对迭代器和模板类型, 使用前置自增 (自减).

  ```c++
  const int a = 1;	// const一般放在前面, 还是要看具体项目的代码风格
  ```

- 整数用 `0` , 实数用 `0.0` , 字符串用 `'\0'` , 指针C++11用 `nullptr` 

- 尽可能用 `sizeof(varname)` 代替 `sizeof(type)` 

	```c++
	Struct data;
	Struct data;
	memset(&data, 0, sizeof(data));	// 而非 memset(&data, 0, sizeof(Struct))
	```

-  `auto` 的使用

  注意陷阱 `auto x(3)` 和 `auto x{3}` 的区别

  不要用 `auto` 进行初始化列表

- 列表初始化

  ```c++
  // vector 接收了一个初始化列表。
  vector<string> v{"foo", "bar"};
  
  // 不考虑细节上的微妙差别，大致上相同。
  // 您可以任选其一。
  vector<string> v = {"foo", "bar"};
  
  // 可以配合 new 一起用。
  auto p = new vector<string>{"foo", "bar"};
  
  // map 接收了一些 pair, 列表初始化大显神威。
  map<int, string> m = {{1, "one"}, {2, "2"}};
  
  // 初始化列表也可以用在返回类型上的隐式转换。
  vector<int> test_function() { return {1, 2, 3}; }
  
  // 初始化列表可迭代。
  for (int i : {-1, -2, -3}) {}
  
  // 在函数调用里用列表初始化。
  void TestFunction2(vector<int> v) {}
  TestFunction2({1, 2, 3});
  ```



## 7.命名约定

<font color=blue>通用; 文件; 类型; 变量; 常量; 函数; 命名空间; 枚举; 特殊</font>

- 有描述性, 尽量少用缩写

- 文件

  ```c++
  // 小写, 连字符, 下划线
  my_useful_class.cc
  my-useful-class.cc
  myusefulclass.cc 
  ```

- 类型

  ```c++
  // 每个单词首字母大写, 无下划线或是连字符
  class UrlTable { ...
  class UrlTableTester { ...
  struct UrlTableProperties { ...
  
  // 类型定义
  typedef hash_map<UrlTableProperties *, string> PropertiesMap;
  
  // using 别名
  using PropertiesMap = hash_map<UrlTableProperties *, string>;
  
  // 枚举
  enum UrlTableErrors { ..
  ```

- 变量

  ```c++
  // 一律小写, 下划线连接, 类的成员以下划线结尾(但我更喜欢以下划线开头)
  
  // 普通变量
  string table_name;
  
  // 类数据变量
  class TableInfo{
  private:
      stirng _table_name;
      static Pool<TableInfo>* _pool;
  }
  ```

- 常量

  ```c++
  // 以k开头, 但我更喜欢全部大写
  const int kDaysInAWeek = 7;
  ```

- 函数

  ```c++
  // 大小写混合, 取值设值与变量名匹配
  // 但我更喜欢第一个单词小写, 之后每个单词首字母大写
  void setValue();
  int getValue();
  ```

- 命名空间

  小写; 不要使用缩写, 



## 8.注释

<font color=blue>文件; 类; 函数; 变量; 实现; 标点; TODO; 弃用</font>

注意注释不要冗余

- 文件

  1-2行即可, 对于每个概念的详细文档应当放在具体概念处

  不要在 `.h` 和 `.cpp` 之间复制注释

- 类

  描述类的功能和用法

- 函数

  声明处, 描述函数功能: 输入输出; 保持引用参数, 可否为空指针......

  定义处, 描述函数实现的方法, 编程技巧之类.......

- 实现

  代码中晦涩难懂的地方进行注释

- TODO

  TODO注释: 对于代码不完美的地方, 可以写上TODO注释


## 9.格式

<font color=blue>行长度; tab; 函数; 列表初始化; if, for, switch; 指针, 引用; 类; 命名空间; 水平留白; 垂直留白</font>

- 行长度与tab

  80个字符最大值; 注释或是URL可以超过80个字符, `#include` 与头文件保护

  tab每次缩进2个空格, 但我更喜欢4个

- 函数

  返回类型和函数名在同一行, 参数尽量同一行, 过长就要对齐分行

  ```c++
  ReturnType ClassName::FunctionName(Type par_name1, Type par_name2) {
      DoSomething();
      ...
  }
  
  ReturnType LongClassName::ReallyReallyReallyLongFunctionName(
      	Type par_name1,  
      	Type par_name2,
      	Type par_name3) {
    	DoSomething();  
    	...
  }
  ```

  函数变量的注释

  ```c++
  class Shape {
   public:
    virtual void rotate(double radians) = 0;
  };
  
  class Circle : public Shape {
   public:
    void rotate(double radians) override;
  };
  
  // 参数不明显, 一定要注释起来
  void Circle::rotate(double /*radians*/) {}
  ```

  函数调用的格式

  ```c++
  // 普通的调用
  bool retval = doSomething(arg1, arg2, arg3);
  // 通过 3x3 矩阵转换 widget. 注意参数的格式
  my_widget.Transform(x1, x2, x3,
                      y1, y2, y3,
                      z1, z2, z3);
  ```

  函数返回值

  ```c++
  // 在返回一个变量时, 不用加括号
  // 在返回一个表达式的时候, 最好加上括号
  return val;
  return (some_long_condition &&
         another_condition);
  ```

  构造函数

  ```c++
  // 如果所有变量能放在同一行:
  MyClass::MyClass(int var) : some_var_(var) {
    DoSomething();
  }
  
  // 如果不能放在同一行,
  // 必须置于冒号后, 并缩进 4 个空格
  MyClass::MyClass(int var)
      	: some_var_(var), some_other_var_(var + 1) {
      DoSomething();
  }
  
  // 如果初始化列表需要置于多行, 将每一个成员放在单独的一行
  // 并逐行对齐
  MyClass::MyClass(int var)
      	: some_var_(var),             // 4 space indent
        	  some_other_var_(var + 1) {  // lined up
      DoSomething();
  }
  
  // 右大括号 } 可以和左大括号 { 放在同一行
  // 如果这样做合适的话
  MyClass::MyClass(int var)
        : some_var_(var) {}
  ```

- if, for, switch ...

  if

  ```c++
  // 标准格式
  if (condition) {  // 圆括号里没有空格.
    ...  // 2 空格缩进.
  } else if (...) {  // else 与 if 的右括号同一行.
    ...
  } else {
    ...
  }
  
  // 没有else可以写到一行, 最好写成一行
  if (x == kFoo) return new Foo();
  
  // if用不用大括号, 看具体项目
  // 如果用了大括号, if和else的大括号保持一致
  if (condition) {
    foo;
  } else {
    bar;
  }
  ```

  switch

  ```c++
  // 缩进按照自己的来
  switch (var) {
    case 0: {  
      break;
    }
    case 1: {
      ...
      break;
    }
    default: {
      assert(false);
    }
  }
  ```

  for & while

  ```c++
  for (int i = 0; i < kSomeNumber; ++i)
    printf("I love you\n");
  
  for (int i = 0; i < kSomeNumber; ++i) {
    printf("I take it back\n");
  }
  
  while (condition) {
    // 反复循环直到条件失效.
  }
  for (int i = 0; i < kSomeNumber; ++i) {}  // 可 - 空循环体.
  while (condition) continue;  // 可 - contunue 表明没有逻辑.
  ```

- 指针与引用表达式

  ```c++
  // * 和 & 后面没有空格
  x = *p;
  p = &x;
  
  // 声明指针变量, 星号和类型名紧挨与否都可以, 重要的是格式的统一
  char* c;
  const string& str;
  char *c;
  const string &str;
  
  // 决不允许出现这样的情况
  int x, *y;	// 多重声明中不允许有 * 或是 &
  char * c;	// 不能两边都有空格
  const string & str;
  ```

- 类格式

  谷歌推荐如下

  ```c++
  class MyClass : public OtherClass {
   public:      // 注意有一个空格的缩进
    MyClass();  // 标准的两空格缩进
    explicit MyClass(int var);
    ~MyClass() {}
  
    void SomeFunction();
    void SomeFunctionThatDoesNothing() {
    }
  
    void set_some_var(int var) { some_var_ = var; }
    int some_var() const { return some_var_; }
  
   private:
    bool SomeInternalFunction();
  
    int some_var_;
    int some_other_var_;
  };
  ```

  但我更喜欢

  ```c++
  class MyClass : public OtherClass {
  public:      // 无缩进
      MyClass();  // 4空格缩进
      explicit MyClass(int var);
      ~MyClass() {}
  
      void SomeFunction();
      void SomeFunctionThatDoesNothing() {
      }
  
      void set_some_var(int var) { some_var_ = var; }
      int some_var() const { return some_var_; }
  
  private:
     bool SomeInternalFunction();
  
     int some_var_;
     int some_other_var_;
  };
  ```

- 命名空间

  ```c++
  namespace np1{	// 声明嵌套命名空间, 每个命名空间都独立成行
  namespace np2{
  
  void foo(){	// 命名空间内没有额外的缩进
  	...        
  }
      
  }	// namespace np2
  }	// namespace np1
  ```

- 水平留白

  ```c++
  void f(bool b) {  // 左大括号前总是有空格.
    ...
  int i = 0;  // 分号前不加空格.
  // 列表初始化中大括号内的空格是可选的.
  // 如果加了空格, 那么两边都要加上.
  int x[] = { 0 };
  int x[] = {0};
  
  // 继承与初始化列表中的冒号前后恒有空格.
  class Foo : public Bar {
   public:
    // 对于单行函数的实现, 在大括号内加上空格
    // 然后是函数实现
    Foo(int b) : Bar(), baz_(b) {}  // 大括号里面是空的话, 不加空格.
    void Reset() { baz_ = 0; }  // 用括号把大括号与实现分开.
  ```

  循环和条件语句

  ```c++
  if (b) {          // if 条件语句和循环语句关键字后均有空格.
  } else {          // else 前后有空格.
  }
  while (test) {}   // 圆括号内部不紧邻空格.
  switch (i) {
  for (int i = 0; i < 5; ++i) {
  switch ( i ) {    // 循环和条件语句的圆括号里可以与空格紧邻.
  if ( test ) {     // 圆括号, 但这很少见. 总之要一致.
  for ( int i = 0; i < 5; ++i ) {
  for ( ; i < 5 ; ++i) {  // 循环里内 ; 后恒有空格, ;  前可以加个空格.
  switch (i) {
    case 1:         // switch case 的冒号前无空格.
      ...
    case 2: break;  // 如果冒号有代码, 加个空格.
  ```

  操作符

  ```c++
  // 赋值运算符前后总是有空格.
  x = 0;
  
  // 其它二元操作符也前后恒有空格, 不过对于表达式的子式可以不加空格.
  // 圆括号内部没有紧邻空格.
  v = w * x + y / z;
  v = w*x + y/z;
  v = w * (x + z);
  
  // 在参数和一元操作符之间不加空格.
  x = -5;
  ++x;
  if (x && !y)
    ...
  ```

- 垂直留白

  不要有太多空行即可