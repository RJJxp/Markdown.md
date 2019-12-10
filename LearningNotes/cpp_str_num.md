# C++ 中字符串与数字之间的转换



## 数字2字符串

c++11标准增加了全局函数std::to_string用于将数值类型变量转换为string类型变量

```c++
string to_string (int val);
string to_string (long val);
string to_string (long long val);
string to_string (unsigned val);
string to_string (unsigned long val);
string to_string (unsigned long long val);
string to_string (float val);
string to_string (double val);
string to_string (long double val)
```



## 字符串2数字

```c++
std::string s;
int i = atoi(s.c_str());
double d = atof(s.c_str());
```

