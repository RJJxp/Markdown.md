# Google C++ 规范 学习笔记

在学习C++语言规范的同时，希望借助Typora熟练掌握MarkDown


参考文档

[Google开源项目风格指南(中文版)](https://zh-google-styleguide.readthedocs.io/en/latest/google-cpp-styleguide/headers/#acgtyrant) 

[Github Google Style Guide](https://github.com/google/styleguide)

先迅速过一遍，然后找出自己不懂的地方再去琢磨

不要死扣几个点

将所有看不懂的地方标志为 <font color= red>**WTF**</font>



<font size=5>每一种方式都是各有利弊,没有最好, 只有更适合</font>



## 0.目录

[TOC]



## 1.头文件

### 1.1 自包含 

​	<font color= red>**WTF**</font>

### 1.2 `#define`保护

防止多重包含，格式`<PROJECT_<PATH>_<FILE>_H_`

### 1.3 前置声明

定义：它是类，函数，模板的纯粹声明，并没有定义

到底是否使用前置声明仍值得讨论，优缺点都非常明显

可以降低编译依赖，但同时又会隐藏依赖项，极端情况会改变函数的执行

### 1.4 内联函数（inline）

10行之内，直接定义在头文件，加速代码的执行速度

不要有循环、swtich、析构函数在里面

### １.5 `#include`顺序

```c++
// 总共分为五部分，如下所示
// 每一部分空行分割
#include "sensor/lidar.h"	// 对应的头文件

#include <sys/types.h>	// C系统文件
#include <unistd.h>

#include <vector>	// C++系统文件
#include <map>

#include "control/direction.h"	// 其他目录下的库
#include "control/speed.h"

#include "sensor/realsense.h"	// 本目录库
```


## 2.作用域

### 2.1 命名空间

- 遵守命名空间命名的规则 <font color= red>**WTF**</font>
- 在命名空间最后注释命名空间的名字
- 用命名空间把文件包含（类前置声明以外的整个源文件封装起来）

```c++
// .h file
namespace rjp{

// 不要使用缩进，所有声明置于命名空间中
class MyClass{
    public:
    void func();
    private:
    int a;
};
 
}	// namespace rjp
```

```c++
// .cpp file
namespace rjp{

// 函数定义都置与命名空间
void rjp::func(){
    ...
}
    
}	// namespace rjp
```

- 不要在`std`内声明任何东西 <font color= red>**WTF**</font>
- 不使用`using namespace`引入整个命名空间的标识符号
- 在.h文件中不要使用命名空间别名（变成公开API），除非显示标记内部命名空间使用 <font color= red>**WTF**</font>  

```c++
// 在.cpp中使用别名缩短常用的命名空间
namespace baz = ::foo::bar::baz;
```



   ```c++
   // 显示标记内部空间？
   // 在.h中使用别名缩短常用的命名空间
   namespace rjp {
   namespace impl {	//仅限在rjp命名空间的内部使用的命名空间impl	
   namespace temp = ::ns1::ns2:temp;
   }/namespace impl
   
   inline void func() {
   	// 将命名空间限制在一个函数内部
       namespace baz = ::foo:bar::baz;
   }    
   
   }	// namespace rjp
   ```

   - 禁止使用内联命名空间，它主要用来保持跨版本ABI兼容 <font color=red>**WTF**</font>

### 2.2 匿名命名空间和静态变量

在.cc文件中定义一个**不需要被外部引用**的变量，可将他们放在匿名命名空间或声明为`static`

使其拥有内部链接性，在其他文件无法访问他们，甚至命名一样也不行（比如前置声明？ <font color=red>**WTF**</font>)

```c++
// .cpp file
namespace {
...
}	// namespace 
```

在.h文件中就不要这么做了

在.cpp匿名命名空间中的变量可以该文件中访问，而具名命名空间则不可以

匿名命名空间可避免命名冲突, 限定作用域, 避免直接使用 `using` 关键字污染命名空间

### 2.3 非成员函数、静态成员函数和全局函数

> Prefer placing nonmember functions in a namespace; use completely global functions rarely. Do not use a class simply to group static functions. Static methods of a class should generally be closely related to instances of the class or the class's static data.

使用静态成员函数或命名空间内的非成员函数, 尽量不要用裸的全局函数. 将一系列函数直接置于命名空间中，不要用类的静态方法模拟出命名空间的效果，类的静态方法应当和类的实例或静态数据紧密相关. <font color= red>**WTF**</font>

非成员函数不应依赖于外部变量, 应尽量置于某个命名空间内. 相比单纯为了封装若干不共享任何静态数据的静态成员函数而创建类, 不如使用命名空间

对于头文件`myproject/rjp.h`应当使用

```c++
namespace myproject{
namespace rjp{
void func1();
void func2();
}	// namespace rjp
}	// namespace myproject
```

而非

```c++
namespace myproject{
class rjp{
    public:
    static void func1();
    static void func2();
};
}	// namespace myproject
```

### 2.4 局部变量

- C++可以再任何位置声明变量，在尽可能小的作用域里面声明变量，离第一次使用越近越好

  使用初始化的方式替代声明再赋值

  ```c++
  int i;
  i = f();	// 不建议：初始化和声明分离
  
  int j = g();	// 好：初始化的时候声明
  
  vector<int> v1;	// 其实用花括号初始化更好
  v1.push_back(1);	// 按照我的理解，这种初始化方式还是可以接受的	
  v1.push_back(2);
  
  vector<int> v2 = {1,2};	// 好：一开始就初始化
  ```

- 属于`if`，`while`和`for`语句的变量应当在这些语句中声明，可以将变量的作用域限制在这些语句中

  ```c++
  while (const char* p = strchr(str, '/')) str = p + 1;
  ```

  但如果变量是一个对象，那么每次进入作用域要调用构造函数，退出要调用析构函数，会降低代码效率

  ```c++
  // 低效的实现
  for(int i=0;i<1000000;i++){
      rjp f;
      f.doSth(i);
  }
  
  // 高效的实现
  rjp f;
  for(int i=0;i<1000000;i++){
      f.doSth(i);
  }
  ```

### 2.5 静态和全局变量 

<font color=red>**WTF**</font>

### 2.6 译者笔记

- YuleFox 
  - 多线程中的全局变量 (含静态成员变量) 不要使用 `class` 类型 (含 STL 容器), 避免不明确行为导致的 bug.
  - 嵌套类符合局部使用原则, 只是不能在其他头文件中前置声明, 尽量不要 `public`;
  - 作用域的使用, 除了考虑名称污染, 可读性之外, 主要是为降低耦合, 提高编译/执行效率.
- acgtyrant
  - 匿名命名空间说白了就是文件作用域



## 3.类

### 3.1 构造函数的职责

不能构造虚函数中调用虚函数

### 3.2 隐式类型的转换 

<font color=red> **WTF**</font>

不要定义隐式类型转换. 对于转换运算符和单参数构造函数, 请使用 `explicit` 关键字

### 3.3 可拷贝类型和可移动类型 

<font color=red> **WTF**</font>

### 3.4 结构体 VS. 类

仅当只有数据成员时使用 `struct`, 其它一概使用 `class`

### 3.5 继承

使用组合常常比使用继承更合理. 如果使用继承的话, 定义为 `public` 继承

必要的话, 析构函数声明为 `virtual`. 如果你的类有虚函数, 则析构函数也应该为虚函数.

### 3.6 多重继承 

<font color=red> **WTF**</font>

### 3.7 接口 

<font color=red> **WTF**</font>

当一个类满足以下要求时, 称之为纯接口:

- 只有纯虚函数 (“`=0`”) 和静态函数 (除了下文提到的析构函数)
- 没有非静态数据成员
- 没有定义任何构造函数，如果有，也不能带有参数，并且必须为 `protected`
- 如果它是一个子类，也只能从满足上述条件并以 `Interface` 为后缀的类继承

接口类不能被直接实例化，因为它声明了纯虚函数。为确保接口类的所有实现可被正确销毁，必须为之声明虚析构函数。

### 3.8 运算符重载

C++ 允许用户通过使用 `operator` 关键字 对内建运算符进行重载定义，只要其中一个参数是用户定义的类型

只有在意义明显, 不会出现奇怪的行为并且与对应的内建运算符的行为一致时才定义重载运算符，比如自己知道的拷贝构造函数`=`重载

```c++
class Ojb{};
Obj a;
Obj b = a;
```

只有在意义明显, 不会出现奇怪的行为并且与对应的内建运算符的行为一致时才定义重载运算符

<font color =red>**WTF**</font>

### 3.9 存取控制

将 ***所有*** 数据成员声明为 `private`，除非是 `static const` 类型成员 (遵循 [常量命名规则](https://zh-google-styleguide.readthedocs.io/en/latest/google-cpp-styleguide/naming/#constant-names))。处于技术上的原因，在使用 [Google Test](https://github.com/google/googletest) 时我们允许测试固件类中的数据成员为 `protected`

### 3.10 声明顺序

顺序是public，protected，private

在各个部分中, 建议将类似的声明放在一起, 并且建议以如下的顺序: 类型 (包括 `typedef`, `using` 和嵌套的结构体与类), 常量, 工厂函数, 构造函数, 赋值运算符, 析构函数, 其它函数, 数据成员.

### 3.11 译者笔记 YuleFox

- 为避免隐式转换, 需将单参数构造函数声明为 `explicit`
- 接口类类名以 `Interface` 为后缀, 除提供带实现的虚析构函数, 静态成员函数外, 其他均为纯虚函数, 不定义非静态数据成员, 不提供构造函数, 提供的话, 声明为 `protected`




## 4.函数

### 4.1 参数顺序

一般来说，函数的参数顺序为: 输入参数在先,，后跟输出参数

### 4.2 编写简短函数

倾向于编写简短, 凝练的函数

我们承认长函数有时是合理的，因此并不硬性限制函数的长度 

如果函数超过 40 行，可以思索一下能不能在不影响程序结构的前提下对其进行分割

### 4.3 引用参数

- 所有按引用传递的参数必须加上 `const`，引用参数对于拷贝构造函数这样的应用也是必需的，同时也更明确地不接受空指针

- 唯一缺点就是容易引起误解，被当成指针

- **Google的硬性规定**， 输入参数是值参或 `const` 引用, 输出参数为指针. 输入参数可以是 `const` 指针, 但决不能是非 `const` 的引用参数, 除非特殊要求

```c++
  void func(const string &in,string *out);
```

- 对于`const T*`和`const T&`，前者有时比后者更加明智

  可能会传递空指针，函数要把指针或对地址的引用赋值给形参 <font color=red>**WTF**</font>

总而言之，大多数行参是`const T&`，若用`const T*`则需要特殊说明，以方便读者理解

### 4.4 函数重载

函数重载一定要让读者一看调用点就知道你重载的到底是哪一种

构造函数也是这样

```c++
class rjpClass{
    public:
    void work(const string &text);
    void work(const char *text, int textlen);
};
```

- 可以在重载函数里加上参数信息，比如`appendString()`和`appendInt()`，而不是一口气重载很多（那这还叫重载吗？ <font color=red> **WTF**</font>）

- 如果重载的目的是为了支持不同数量的同一类型参数，可以优先考虑使用`std::vector`，方便读者使用列表初始化指定参数

### 4.5 缺省参数

只允许在非虚函数中使用缺性参数，而且缺省参数值保持唯一，不能这么写：

```c++
void if(int n = counter++);
```

之中关于缺省函数缺点某些的解释是看不懂的 <font color=red>**WTF**</font>

### 4.6 函数返回类型后置语法

```c++
int foo(int x);	// 常规写法
auto foo(int x) -> int;	// 类型后置写法 c++11的新特性
```

对于`int`这种简单的情况，两种写法没有太大区别

但是在某些复杂的情况中，写法不同会造成区别 （完全不懂Lambda表达式 <font color=red>**WTF**</font>）

```c++
template <class T, class U> auto add(T t, U u) ->decltype(t+u);
```


```c++
template <class T, class U> decltype (declval<T&>() + declval<U&>()) add(U u, T t);
```



## 5.来自Google的奇技

### 5.1 所有权与智能指针

上过老张的课，对智能指针有一些初步的了解

> 所有权是一种登记／管理动态内存和其它资源的技术. 动态分配对象的所有主是一个对象或函数, 后者负责确保当前者无用时就自动销毁前者. 所有权有时可以共享, 此时就由最后一个所有主来负责销毁它. 甚至也可以不用共享, 在代码中直接把所有权传递给其它对象.

可以大致理解，把智能指针当成一个类，有一个`refcount`的变量来记录指针所指对象被link了几次，在删除释放空间，如果`refcount`大于1，那么不删除指针所指的数据，只是删除指向数据的指针。通过对`=`运算符的重载，来改变`refcount`的值

现在c++有现成的智能指针`std::unique_ptr`和 `std::shared_ptr`

后者的原理和老张讲的那一种智能指针相似

**文中所提到的所有权机制还是很有趣的**

### 5.2 Cpplint

检查风格错误的工具

没用过



## 6.其他C++特性

### 6.1 引用参数

请参考[4.3 引用参数](#4.3 引用参数)

按照引用传递的参数必须加上`const`

### 6.2 右值引用

右值引用是一种只能绑定到临时对象的引用的一种, 其语法与传统的引用语法相似. 例如, `void f(string&& s)`; 声明了一个其参数是一个字符串的右值引用的函数<font color=red>**WTF**</font>

只在定义**移动构造函数**与**移动赋值操作**时使用右值引用，不要使用 `std::forward` 功能函数，可能会使用`std::move`函数

> 用于定义移动构造函数 (使用类的右值引用进行构造的函数) 使得移动一个值而非拷贝之成为可能. 例如, 如果 `v1` 是一个 `vector<string>`, 则 `auto v2(std::move(v1))` 将很可能不再进行大量的数据复制而只是简单地进行指针操作, 在某些情况下这将带来大幅度的性能提升

特性比较新，并未被广泛理解

### 6.3 函数重载

请参考[4.4 函数重载](#4.4 函数重载)

### 6.4 缺省参数

尽可能改用函数重载不要用缺省函数参数，除少数情况外

> 缺省参数会干扰函数指针，害得后者的函数签名（function signature）往往对不上所实际要调用的函数签名。即在一个现有函数添加缺省参数，就会改变它的类型，那么调用其地址的代码可能会出错，不过函数重载就没这问题了

由于缺点并不是很严重，有些人依旧偏爱缺省参数胜于函数重载。所以除了以下情况，我们要求必须显式提供所有参数（acgtyrant 注：即不能再通过缺省参数来省略参数了）。

- 位于 `.cc` 文件里的静态函数或匿名空间函数，毕竟都只能在局部文件里调用该函数了

- 可以在构造函数里用缺省参数，毕竟不可能取得它们的地址<font color=red>**WTF**</font>

- 可以用来模拟变长数组

```c++
// 通过空 AlphaNum 以支持四个形参
string StrCat(const AlphaNum &a,
              const AlphaNum &b = gEmptyAlphaNum,
              const AlphaNum &c = gEmptyAlphaNum,
              const AlphaNum &d = gEmptyAlphaNum);
```

### 6.5 变长数组和`alloca()`

不允许用变长数组，用`std::vector`就好了

### 6.6 友元

> 通常友元应该定义在同一文件内, 避免代码读者跑到其它文件查找使用该私有成员的类. 经常用到友元的一个地方是将 `FooBuilder` 声明为 `Foo` 的友元, 以便 `FooBuilder` 正确构造 `Foo` 的内部状态

builder设计模式？？？

友元扩大但是并没有打破类的边界

某些情况下，相对于把变量声明为`public`，使用友元是更好的选择

### 6.7 异常

坚决不使用异常

### 6.8 运行时类型识别

> RTTI 允许程序员在运行时识别 C++ 类对象的类型. 它通过使用 `typeid` 或者 `dynamic_cast` 完成.

不允许使用RTTI

在老张的课程上接触过这个东西，是在不同类之间的转换中见到过

好像是分一个`static_cast`和`dynamic_cast`

动态转换，把不同子类之间的不同数据转换为空，如果不识别的话。而静态会报错还是怎么样忘了

可以在测试的时候用，如果你自己都不知道自己的类型是什么，只能说明你的类需要重新设计

### 6.9 类型转换

使用 C++ 的类型转换, 如 `static_cast<>()`. 不要使用 `int y = (int)x` 或 `int y = int(x)` 等转换方式

C 语言的类型转换问题在于模棱两可的操作; 有时是在做强制转换 (如 `(int)3.5`), 有时是在做类型转换 (如 `(int)"hello"`)

另外, C++ 的类型转换在查找时更醒目，但就是语法太恶心233333

不要使用 C 风格类型转换. 而应该使用 C++ 风格.

- 用 `static_cast` 替代 C 风格的值转换, 或某个类指针需要明确的向上转换为父类指针时.
- 用 `const_cast` 去掉 `const` 限定符.
- 用 `reinterpret_cast` 指针类型和整型或其它指针之间进行不安全的相互转换. 仅在你对所做一切了然于心时使用.

### 6.10 流

不要使用流, 除非是日志接口需要. 使用 `printf` 之类的代替

流的最大优势是在于不需要关心打印的类型，但同时也很容易用错类型，编译器并不会报错（由于`<<`被重载）

```c++
cerr << "Error connecting to '" << foo->bar()->hostname.first
     << ":" << foo->bar()->hostname.second << ": " << strerror(errno);

fprintf(stderr, "Error connecting to '%s:%u: %s",
        foo->bar()->hostname.first, foo->bar()->hostname.second,
        strerror(errno));
```

### 6.11 前置自增和自减

对简单数值 (非对象), 两种都无所谓. 对迭代器和模板类型, 使用前置自增 (自减).

> 不考虑返回值的话, 前置自增 (`++i`) 通常要比后置自增 (`i++`) 效率更高. 因为后置自增 (或自减) 需要对表达式的值 `i` 进行一次拷贝. 如果 `i` 是迭代器或其他非数值类型, 拷贝的代价是比较大的. 既然两种自增方式实现的功能一样, 为什么不总是使用前置自增呢?

### 6.12 `const`用法

> 在声明的变量或参数前加上关键字 `const` 用于指明变量值不可被篡改 (如 `const int foo` ). 为类中的函数加上 `const` 限定符表明该函数不会修改类成员变量的状态 (如 `class Foo { int Bar(char c) const; };`).

- 如果函数不会修改传你入的引用或指针类型参数, 该参数应声明为 `const`
- 尽可能将函数声明为 `const`. 访问函数应该总是 `const`. 其他不会修改任何数据成员, 未调用非 `const` 函数, 不会返回数据成员非 `const` 指针或引用的函数也应该声明成 `const`
- 如果数据成员在对象构造之后不再发生变化, 可将其定义为 `const`

`const`的位置，不强制放在前面，但是要保持代码的一致性

### 6.13 `constexpr`用法

我怎么觉得不用管这个玩意

### 6.14 整型

> C++ 没有指定整型的大小. 通常人们假定`short`是16位, `int`是32位,`long`是32位,`long long`是64位

<font color=red>**WTF假定是几个意思**</font>

`<stdint.h>`定义了`int16_t`,`uint32_t`,`int64_t`

> 不要使用 `uint32_t` 等无符号整型, 除非你是在表示一个位组而不是一个数值

最近在解析超声波数据的时候用到了`uint32_t`的数据类型，大概意思是`uint32_t`一般用于位运算，也就是工业上和硬件解析时候要用，还在王卫安老师的课程中，使用过位运算

### 6.15 64位下的可移植性

暂时先不看，觉得还是用不到

### 6.16 预处理宏

<font color=red>**WTF**</font>

### 6.17 `0`,`nullptr`和`NULL`

整数用`0`，实数用`0.0`，字符串用`'\0'`

对于指针，C++11 项目用 `nullptr`; C++03 项目则用 `NULL`

### 6.18 `sizeof`

尽可能使用`sizeof(varname)`代替`sizeof(type)`

这样代码中变量类型变化会自动更新

```c++
rjpStruct data;
memset(&data,0,sizeof(data));	// 正常
memset(&data,0,sizeof(rjpStruct))	// Warning

```

### 6.19 `auto`

定义：

​	C++11 中，若变量被声明成 `auto`, 那它的类型就会被自动匹配成初始化表达式的类型。您可以用 `auto` 来复制初始化或绑定引用。

```c++
vector<string> v;
...
auto s1 = v[0];  // 创建一份 v[0] 的拷贝。
const auto& s2 = v[0];  // s2 是 v[0] 的一个引用。
```

- 优化命名

  ```c++
  // C++类型有时又臭又长，有模板和命名空间的时候
  sparse_hash_map<string, int>::iterator iter = m.find(val);
  // 使用auto简化
  auto iter = m.find(val);
  ```

  但有时候，声明离的太远，`auto`出来，别人不知道这是个什么玩意就很尴尬

  所以还是要在合适的时机使用`auto`

- 不要用来初始化

  ```c++
  auto x(3);	// x是int
  auto y{3};	// y是std::initializer_list<int>
  ```

  此处涉及到C++常见的坑[why is vector<bool> is not a STL container?](https://stackoverflow.com/questions/17794569/why-is-vectorbool-not-a-stl-container/17794965#17794965)

综上，`auto`尽量用在局部变量中，还有[之前](#4.6 函数返回类型后置语法)提到的`Lambda`表达式

### 6.20 列表初始化

全引用于[Google开源项目风格指南(中文版)](https://zh-google-styleguide.readthedocs.io/en/latest/google-cpp-styleguide/headers/#acgtyrant) 

```c++
// Vector 接收了一个初始化列表。
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

这有点强，但总感觉很多初始化的方式并没怎么用到，大概就是因为太过于灵活了吧

用户自定义类型也可以定义接收 `std::initializer_list<T>` 的构造函数和赋值运算符，以自动列表初始化：

```c++
class MyType {
 public:
  // std::initializer_list 专门接收 init 列表。
  // 得以值传递。
  MyType(std::initializer_list<int> init_list) {
    for (int i : init_list) append(i);
  }
  MyType& operator=(std::initializer_list<int> init_list) {
    clear();
    for (int i : init_list) append(i);
  }
};
MyType m{2, 3, 5, 7};
```

最后，列表初始化也适用于常规数据类型的构造，哪怕没有接收 `std::initializer_list<T>` 的构造函数。

```c++
double d{1.23};
// MyOtherType 没有 std::initializer_list 构造函数，
 // 直接上接收常规类型的构造函数。
class MyOtherType {
 public:
  explicit MyOtherType(string);
  MyOtherType(int, string);
};
MyOtherType m = {1, "b"};
// 不过如果构造函数是显式的（explict），您就不能用 `= {}` 了。
MyOtherType m{"b"};
```

注意不要用auto初始化

### 6.21 `Lambda`表达式

匿名函数，和回调机制有关

### 6.22 模板编程

不要使用复杂的模板

### 6.23 `Boost`库

只使用`Boost`被认可的库

### 6.24 C++11

新特性

### 6.25 译者acgtyrant笔记

- 把自带缺省参数的函数地址赋值给指针时，会丢失缺省参数信息
- `friend` 实际上只对函数／类赋予了对其所在类的访问权限，并不是有效的声明语句。所以除了在头文件类内部写 friend 函数／类，还要在类作用域之外正式地声明一遍，最后在对应的 `.cc`文件加以定义
- [滥用缺省参数会害得读者光只看调用代码的话，会误以为其函数接受的参数数量比实际上还要少。](http://www.zhihu.com/question/24439516/answer/27896004)
- 对使用 C++ 异常处理应具有怎样的态度？](http://www.zhihu.com/question/22889420) 非常值得一读
- 用断言`Assert()`代替无符号整型类型，深有启发，参考[6.14 整型](#6.14 整型)<font color=red>**WTF**</font>



## 7.命名约定

命名规则具有一定随意性，但相比按个人喜好命名，一致性更重要，所以无论你认为它们是否重要, 规则总归是规则

### 7.1 通用命名规则

- 函数、变量、文件的命名都要有描述性

- 少用缩写，不要只有项目开发者才能理解的缩写，也不要砍掉几个字母来缩写单词

- 一些特定的广为人知的缩写是允许的, 例如用 `i` 表示迭代变量和用 `T` 表示模板参数

```c++
int price_count_reader;    // 无缩写
int num_errors;            // "num" 是一个常见的写法
int num_dns_connections;   // 人人都知道 "DNS" 是什么
```

```c++
int n;                     // 毫无意义.
int nerr;                  // 含糊不清的缩写.
int n_comp_conns;          // 含糊不清的缩写.
int wgc_connections;       // 只有贵团队知道是什么意思.
int pc_reader;             // "pc" 有太多可能的解释了.
int cstmr_id;              // 删减了若干字母.
```

### 7.2 文件命名

- 文件名要**全部小写**, 可以包含下划线 (`_`) 或连字符 (`-`), 依照项目的约定. 如果没有约定, 那么 “`_`” 更好

  ```c++
  my_useful_class.cc	// 还可以接受的命名方式
  my-useful-class.cc
  myusefulclass.cc
  myusefulclass_test.cc // `_unittest` 和 `_regtest` 已弃用.
  ```

- 不要使用已经存在于 `/usr/include` 下的文件名 (Yang.Y 注: 即编译器搜索系统头文件的路径), 如 `db.h`.

- 通常应尽量让文件名更加明确. `http_server_logs.h` 就比 `logs.h` 要好. 定义类时文件名一般成对出现, 如 `foo_bar.h` 和 `foo_bar.cc`, 对应于类 `FooBar`

### 7.3 类型命名

类型名称的**每个单词首字母均大写**, 不包含下划线: `MyExcitingClass`, `MyExcitingEnum`

```c++
// 类和结构体
class UrlTable { ...
class UrlTableTester { ...
struct UrlTableProperties { ...

// 类型定义
typedef hash_map<UrlTableProperties *, string> PropertiesMap;

// using 别名
using PropertiesMap = hash_map<UrlTableProperties *, string>;

// 枚举
enum UrlTableErrors 
```

### 7.4 变量命名

变量 (包括函数参数) 和数据成员名一律小写, 单词之间用下划线连接. 类的成员变量以下划线结尾, 但结构体的就不用, 如: `a_local_variable`, `a_struct_data_member`, `a_class_data_member_`

```c++
int a_local_variable;	// 普通变量

struct RjpTestStruct{	// 结构体的命名，每个单词首字母大写，没有下划线和连字符
    string rjp_name;	// 结构体内部变量的命名
    int rjp_int;
}

class RjpClass{	// 类的命名，每个单词首字母大写，没有下划线和连字符
	int rjp_int_;
    string rjp_string_;	// 类变量的结尾要有下划线
}
```

### 7.5 常量命名

声明为 `constexpr` 或 `const` 的变量, 或在程序运行期间其值始终保持不变的, 命名时以 “k” 开头, 大小写混合. 例如:

```c++
const int kDaysInAWeek = 7;
```

### 7.6 函数命名 

- 驼峰命名：函数名的每个单词的首字母大写

> 同样的命名规则同时适用于类作用域与命名空间作用域的常量, 因为它们是作为 API 的一部分暴露对外的, 因此应当让它们看起来像是一个函数, 因为在这时, 它们实际上是一个对象而非函数的这一事实对外不过是一个无关紧要的实现细节

- 首字母缩写的单词, 更倾向于将它们视作一个单词进行首字母大写

### 7.7 命名空间命名 

> 命名空间以小写字母命名. 最高级命名空间的名字取决于项目名称. 要注意避免嵌套命名空间的名字之间和常见的顶级命名空间的名字之间发生冲突.

### 7.8 枚举命名 

枚举的命名应当和**常量**或**宏**一致: `kEnumName` 或是 `ENUM_NAME`

```c++
enum UrlTableErrors {
    kOK = 0,
    kErrorOutOfMemory,
    kErrorMalformedInput,
};
enum AlternateUrlTableErrors {
    OK = 0,
    OUT_OF_MEMORY = 1,
    MALFORMED_INPUT = 2,
};
```

### 7.9 宏命名 

不看

### 7.10 命名规则的特例 

？？？<font color=red>**WTF**</font>

### 7.11 译者acgtyrant笔记

感觉 Google 的命名约定很高明, 比如写了简单的类 QueryResult, 接着又可以直接定义一个变量 query_result, 区分度很好; 再次, 类内变量以下划线结尾, 那么就可以直接传入同名的形参, 比如 `TextQuery::TextQuery(std::string word) : word_(word) {}` , 其中 `word_` 自然是类内私有成员



## 8.注释

如何注释以及在哪儿注释. 当然也要记住: 注释固然很重要, 但最好的代码应当本身就是文档. 有意义的类型名和变量名, 要远胜过要用注释解释的含糊不清的名字.

你写的注释是给代码读者看的, 也就是下一个需要理解你的代码的人. 所以慷慨些吧, 下一个读者可能就是你!

### 8.1 注释风格

使用`//`或是`/* */`

注意风格的统一

### 8.2 文件注释

- 版权公告，类似license
- 文件内容，一两行说明文件足矣，每个概念的详细文档应当放在每个概念中，而不是文件注释
- 不要在`.h`和`.cc`文件中复制注释

### 8.3 类注释

每个类的定义都要附带一份注释, 描述类的功能和用法, 除非它的功能相当明显.

```c++
// Iterates over the contents of a GargantuanTable.
// Example:
//    GargantuanTableIterator* iter = table->NewIterator();
//    for (iter->Seek("foo"); !iter->done(); iter->Next()) {
//      process(iter->key(), iter->value());
//    }
//    delete iter;
class GargantuanTableIterator {
  ...
};
```

- 正确使用类需要考虑的因素
- 类的实例被多线程访问，特别注明
- 小段代码演示也可以放在注释里面

### 8.4 函数注释

函数声明处的注释描述函数功能

定义处的注释描述函数实现

注释使用叙述式 (“Opens the file”) 而非指令式 (“Open the file”)

1. **函数声明**
   - 函数的输入输出.

   - 对类成员函数而言: 函数调用期间对象是否需要保持引用参数, 是否会释放这些参数.
   - 函数是否分配了必须由调用者释放的空间.
   - 参数是否可以为空指针.
   - 是否存在函数使用上的性能隐患.
   - 如果函数是可重入的, 其同步前提是什么?

    ```c++
    // Returns an iterator for this table.  It is the client's
    // responsibility to delete the iterator when it is done with it,
    // and it must not use the iterator once the GargantuanTable object
    // on which the iterator was created has been deleted.
    //
    // The iterator is initially positioned at the beginning of the table.
    //
    // This method is equivalent to:
    //    Iterator* iter = table->NewIterator();
    //    iter->Seek("");
    //    return iter;
    // If you are going to immediately seek to another place in the
    // returned iterator, it will be faster to use NewIterator()
    // and avoid the extra seek.
    Iterator* GetIterator() const;
    ```

    - 注释函数重载时, 注释的重点应该是函数中被重载的部分, 而不是简单的重复被重载的函数的注释
    - 构造函数对参数做了什么，析构函数清理了哪些参数

2. **函数定义**

   - 使用的编程技巧
   - 实现的大致步骤
   - 实现的理由

   一定要注意在声明和定义处注释的区别，不要单纯的复制

### 8.5 变量注释

- 如果变量可以接受`NULL`或者`-1`这样的警戒值，需要注明
- 所有全局变量也要注释说明含义及用途, 以及作为全局变量的原因

### 8.6 实现注释

对于代码中巧妙的, 晦涩的, 有趣的, 重要的地方加以注释

- 代码前注释

  `//<space><YOUR_EXPLANATIONS>`

- 行注释

  `<tab>//<space><YOUR_EXPLANATIONS>`

- 函数参数注释

  > 1. 如果参数是一个字面常量, 并且这一常量在多处函数调用中被使用, 用以推断它们一致, 你应当用一个常量名让这一约定变得更明显, 并且保证这一约定不会被打破.
  >
  > 2. 考虑更改函数的签名, 让某个 `bool` 类型的参数变为 `enum` 类型, 这样可以让这个参数的值表达其意义.
  >
  > 3. 如果某个函数有多个配置选项, 你可以考虑定义一个类或结构体以保存所有的选项, 并传入类或结构体的实例. 这样的方法有许多优点, 例如这样的选项可以在调用处用变量名引用, 这样就能清晰地表明其意义. 同时也减少了函数参数的数量, 使得函数调用更易读也易写. 除此之外, 以这样的方式, 如果你使用其他的选项, 就无需对调用点进行更改.
  >
  > 4. 具名变量代替大段而复杂的嵌套表达式.
  >
  > 5. 万不得已时, 才考虑在调用点用注释阐明参数的意义.

不允许显而易见的注释

自文档化

### 8.7 标点、拼写和语法

风格一致最重要

### 8.8 TODO注释

`TODO`：还要做，还等待改进

对于临时的，短期的解决方案，已经可以运行，但是还不够好，可以写TODO注释

```c++
// TODO(kl@gmail.com): Use a "*" here for concatenation operator.
// TODO(Zeke) change this to use relations.
// TODO(bug 12345): remove the "Last visitors" feature
```

大写`TODO`在圆括号里面可以是你的名字，邮箱，bugID

### 8.9 弃用注释

不看<font color=red>**WTF**</font>

### 8.10 译者YuleFox笔记

1. 关于注释风格, 很多 C++ 的 coders 更喜欢行注释, C coders 或许对块注释依然情有独钟, 或者在文件头大段大段的注释时使用块注释;
2. 文件注释可以炫耀你的成就, 也是**为了捅了篓子别人可以找你**;
3. 注释要言简意赅, 不要拖沓冗余, 复杂的东西简单化和简单的东西复杂化都是要被鄙视的;
4. 对于 Chinese coders 来说, 用英文注释还是用中文注释, it is a problem, 但不管怎样, 注释是为了让别人看懂, 难道是为了炫耀编程语言之外的你的母语或外语水平吗；
5. 注释不要太乱, 适当的缩进才会让人乐意看. 但也没有必要规定注释从第几列开始 (我自己写代码的时候总喜欢这样), UNIX/LINUX 下还可以约定是使用 tab 还是 space, 个人倾向于 space;
6. TODO 很不错, 有时候, 注释确实是为了标记一些未完成的或完成的不尽如人意的地方, 这样一搜索, 就知道还有哪些活要干, 日志都省了.



## 9.格式

代码的风格与格式

### 9.1 行长度

每一行字符不超过80个字符

`#include`长路径可以超过80个字符

头文件保护可以无视该原则

### 9.2 非ASCII字符

必须使用`UTF-8`编码

编码的知识还是需要补一下<font color=red>**WTF**</font>

### 9.3 空格还是制表符

使用空格缩进，或者把`tab`设置成空格

谷歌推荐2个空格，但是实际由项目风格定

### 9.4 函数声明与定义

返回类型和函数名在同一行，参数也尽量放在同一行

```c++
int RjpClass::RjpFunction(int para_name_01, int para_name_02){
    DoSomething();
}
```

```c++
ReturnType LongClassName::ReallyReallyReallyLongFunctionName(
    Type par_name1,  // 4 space indent
    Type par_name2,
    Type par_name3) {
  DoSomething();  // 2 space indent
  ...
}
```

### 9.5 Lambda表达式

<font color=red>**WTF**</font>

### 9.6 函数调用

```c++
bool retval = DoSomething(argument1, argument2, argument3);
```

```c++
bool retval = DoSomething(averyveryveryverylongargument1,
                          argument2, argument3);
```

```c++
if (...) {
  ...
  ...
  if (...) {
    DoSomething(
        argument1, argument2,  // 4 空格缩进
        argument3, argument4);
  }
```

```c++
// 通过 3x3 矩阵转换 widget.
my_widget.Transform(x1, x2, x3,
                    y1, y2, y3,
                    z1, z2, z3);
```

### 9.7 列表初始化格式

与函数调用的格式一样

### 9.8 条件语句

不要再括号内使用空格，`if`和`else`关键字另起一行

```c++
if (condition) {  // 圆括号里没有空格.
  ...  // 2 空格缩进.
} else if (...) {  // else 与 if 的右括号同一行.
  ...
} else {
  ...
}
```

重要的是格式一致

```c++
if<space>(<condition>)<space>{
    ...	// 注意空格
}
```

如果语句中某个 `if-else` 分支使用了大括号的话, 其它分支也必须使用

### 9.9 循环和开关选择语句

空循环体应使用 `{}` 或 `continue`, 而不是一个简单的分号

```c++
switch (var) {
  case 0: {  // 2 空格缩进
    ...      // 4 空格缩进
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



```c++
for (int i = 0; i < kSomeNumber; ++i)
  printf("I love you\n");

for (int i = 0; i < kSomeNumber; ++i) {
  printf("I take it back\n");
}
```

### 9.10 指针和引用表达式

句点或箭头前后不要有空格. 指针/地址操作符 (`*, &`) 之后不能有空格

```c++
x = *p;
p = &x;
x = r.y;
x = r->y;
```

在声明指针变量或参数时, 星号与类型或变量名紧挨都可以:

```c++
// 好, 空格前置.
char *c;
const string &str;

// 好, 空格后置.
char* c;
const string& str;
```

### 9.11 布尔表达式

如果一个布尔表达式超过标准行宽, 断行方式要统一一下

下例中, 逻辑与 (`&&`) 操作符总位于行尾:

```c++
if (this_one_thing > this_other_thing &&
    a_third_thing == a_fourth_thing &&
    yet_another && last_one) {
  ...
}
```

### 9.12 函数返回值

```c++
return result;                  // 返回值很简单, 没有圆括号.
// 可以用圆括号把复杂表达式圈起来, 改善可读性.
return (some_long_condition &&
        another_condition);
```

### 9.13 变量及数组初始化

用 `=`, `()` 和 `{}` 均可.

```c++
int x = 3;
int x(3);
int x{3};
string name("Some Name");
string name = "Some Name";
string name{"Some Name"};
```

```c++
vector<int> v(100, 1);  // 内容为 100 个 1 的向量.
vector<int> v{100, 1};  // 内容为 100 和 1 的向量.
```



### 9.14 预处理指令

即使预处理指令位于缩进代码块中, 指令也应从行首开始.

```c++
// 好 - 指令从行首开始
  if (lopsided_score) {
#if DISASTER_PENDING      // 正确 - 从行首开始
    DropEverything();
# if NOTIFY               // 非必要 - # 后跟空格
    NotifyClient();
# endif
#endif
    BackToNormal();
  }
```

### 9.15 类格式

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



### 9.16 构造函数初始化列表

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



### 9.17 命名空间格式化

打死也不缩进

### 9.18 水平留白

水平留白的使用根据在代码中的位置决定. 永远不要在行尾添加没意义的留白.

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
  void Reset() { baz_ = 0; }  // 用括号把大括号与实现分开
```

添加冗余的留白会给其他人编辑时造成额外负担. 因此, 行尾不要留空格. 如果确定一行代码已经修改完毕, 将多余的空格去掉; 

```c++
// 循环和条件语句
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

```c++
// 操作符
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

### 9.19 垂直留白

这不仅仅是规则而是原则问题了: 不在万不得已, 不要使用空行. 尤其是: 两个函数定义之间的空行不要超过 2 行, 函数体首尾不要留空行, 函数体中也不要随意添加空行.

### 

### 10.结束语

运用常识和判断力, 风格要保持一致!!!!!!!!!!!!!