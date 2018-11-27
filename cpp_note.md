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
   - 用命名空间吧文件包含（类前置声明以外的整个源文件封装起来）

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

3. 非成员函数、静态成员函数和全局函数

   > Prefer placing nonmember functions in a namespace; use completely global functions rarely. Do not use a class simply to group static functions. Static methods of a class should generally be closely related to instances of the class or the class's static data.

   使用静态成员函数或命名空间内的非成员函数, 尽量不要用裸的全局函数. 将一系列函数直接置于命名空间中，不要用类的静态方法模拟出命名空间的效果，类的静态方法应当和类的实例或静态数据紧密相关. <font color= red>**WTF**</font>

4. 局部变量

5. 静态和全局变量