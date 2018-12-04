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



