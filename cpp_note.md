# Google C++ 规范 学习笔记

在学习C++语言规范的同时，希望借助Typora熟练掌握MarkDown


参考文档

[Google开源项目风格指南(中文版)](https://zh-google-styleguide.readthedocs.io/en/latest/google-cpp-styleguide/headers/#acgtyrant) 

[Github Google Style Guide](https://github.com/google/styleguide)

先迅速过一遍，然后找出自己不懂的地方再去琢磨

不要死扣几个点

将所有看不懂的地方标志为 <font color= red>**WTF**</font>

## 头文件

1. 自包含 <font color= red>**WTF**</font>

2. `#define`保护

   防止多重包含，格式`<PROJECT_<PATH>_<FILE>_H_`

3. 前置声明

   定义：它是类，函数，模板的纯粹声明，并没有定义

   到底是否使用前置声明仍值得讨论，优缺点都非常明显

   可以降低编译依赖，但同时又会隐藏依赖项，极端情况会改变函数的执行

4. 内联函数（inline）

   10行之内，直接定义在头文件，加速代码的执行速度

   不要有循环、swtich、析构函数在里面

5. `#include`顺序

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


## 作用域

1. 命名空间

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

2. 匿名命名空间和静态变量

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

3. 非成员函数、静态成员函数和全局函数

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

4. 局部变量

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

5. 静态和全局变量 <font color=red>**WTF**</font>

6. 译者笔记

   - YuleFox 
     - 多线程中的全局变量 (含静态成员变量) 不要使用 `class` 类型 (含 STL 容器), 避免不明确行为导致的 bug.
     - 嵌套类符合局部使用原则, 只是不能在其他头文件中前置声明, 尽量不要 `public`;
     - 作用域的使用, 除了考虑名称污染, 可读性之外, 主要是为降低耦合, 提高编译/执行效率.
   - acgtyrant
     - 匿名命名空间说白了就是文件作用域



## 类

1. 构造函数的职责

   不能构造虚函数中调用虚函数

2. 隐式类型的转换 <font color=red> **WTF**</font>

   不要定义隐式类型转换. 对于转换运算符和单参数构造函数, 请使用 `explicit` 关键字

3. 可拷贝类型和可移动类型 <font color=red> **WTF**</font>

4. 结构体 VS. 类

   仅当只有数据成员时使用 `struct`, 其它一概使用 `class`

5. 继承

   使用组合常常比使用继承更合理. 如果使用继承的话, 定义为 `public` 继承

   必要的话, 析构函数声明为 `virtual`. 如果你的类有虚函数, 则析构函数也应该为虚函数.

6. 多重继承 <font color=red> **WTF**</font>

7. 接口 <font color=red> **WTF**</font>

   当一个类满足以下要求时, 称之为纯接口:

   - 只有纯虚函数 (“`=0`”) 和静态函数 (除了下文提到的析构函数)
   - 没有非静态数据成员
   - 没有定义任何构造函数，如果有，也不能带有参数，并且必须为 `protected`
   - 如果它是一个子类，也只能从满足上述条件并以 `Interface` 为后缀的类继承

   接口类不能被直接实例化，因为它声明了纯虚函数。为确保接口类的所有实现可被正确销毁，必须为之声明虚析构函数。

8. 运算符重载

9. 存取控制

   将 *所有* 数据成员声明为 `private`，除非是 `static const` 类型成员 (遵循 [常量命名规则](https://zh-google-styleguide.readthedocs.io/en/latest/google-cpp-styleguide/naming/#constant-names))。处于技术上的原因，在使用 [Google Test](https://github.com/google/googletest) 时我们允许测试固件类中的数据成员为 `protected`

10. 声明顺序

    顺序是public，protected，private

11. 译者笔记 YuleFox

    - 为避免隐式转换, 需将单参数构造函数声明为 `explicit`
    - 接口类类名以 `Interface` 为后缀, 除提供带实现的虚析构函数, 静态成员函数外, 其他均为纯虚函数, 不定义非静态数据成员, 不提供构造函数, 提供的话, 声明为 `protected`


## 函数

1. 参数顺序

   一般来说，函数的参数顺序为: 输入参数在先,，后跟输出参数

2. 编写简短函数

   倾向于编写简短, 凝练的函数

   我们承认长函数有时是合理的，因此并不硬性限制函数的长度 

   如果函数超过 40 行，可以思索一下能不能在不影响程序结构的前提下对其进行分割

3. 引用参数

   - 所有按引用传递的参数必须加上 `const`，引用参数对于拷贝构造函数这样的应用也是必需的，同时也更明确地不接受空指针

   - 唯一缺点就是容易引起误解，被当成指针

   - Google的硬性规定， 输入参数是值参或 `const` 引用, 输出参数为指针. 输入参数可以是 `const` 指针, 但决不能是非 `const` 的引用参数, 除非特殊要求

   ```c++
     void func(const string &in,string *out);
   ```

   - 对于`const T*`和`const T&`，前者有时比后者更加明智

     可能会传递空指针，函数要把指针或对地址的引用赋值给形参 <font color=red>**WTF**</font>

   总而言之，大多数行参是`const T&`，若用`const T*`则需要特殊说明，以方便读者理解

4. 函数重载

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

5. 缺省参数

   只允许在非虚函数中使用缺性参数，而且缺省参数值保持唯一，不能这么写：

   ```c++
   void if(int n = counter++);
   ```

   之中关于缺省函数缺点某些的解释是看不懂的 <font color=red>**WTF**</font>

6. 函数返回类型后置语法

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



## 来自Google的奇技

1. 所有权与智能指针

   上过老张的课，对智能指针有一些初步的了解

   > 所有权是一种登记／管理动态内存和其它资源的技术. 动态分配对象的所有主是一个对象或函数, 后者负责确保当前者无用时就自动销毁前者. 所有权有时可以共享, 此时就由最后一个所有主来负责销毁它. 甚至也可以不用共享, 在代码中直接把所有权传递给其它对象.

   可以大致理解，把智能指针当成一个类，有一个`refcount`的变量来记录指针所指对象被link了几次，在删除释放空间，如果`refcount`大于1，那么不删除指针所指的数据，只是删除指向数据的指针。通过对`=`运算符的重载，来改变`refcount`的值

   现在c++有现成的智能指针`std::unique_ptr`和 `std::shared_ptr`

   后者的原理和老张讲的那一种智能指针相似

   **文中所提到的所有权机制还是很有趣的**

2. Cpplint

   检查风格错误的工具

   没用过



## 其他C++特性

1. []()