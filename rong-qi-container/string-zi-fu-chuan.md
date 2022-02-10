---
description: STL中string类的详细介绍
---

# 🎶 string - 字符串

## string 容器基本概念

> C风格字符串（以`\0`结尾的字符数组）太过复杂，难于掌握，不太适合大程序的开发，所以C++STL中定义了一种string类，在头文件`<string>`中。

### string 和 C风格字符 串对比

*   `char*`是一个指针，`string`是一个类

    `string`封装了`char*`，管理这个字符串，是一个`char*`型的容器。
*   `string` 封装了很多实用的成员方法

    查找`find`，拷贝`copy`，删除`erase`，替换`replace`，插入`insert`......
*   不用考虑内存释放和越界

    `string`管理`char*`所分配的内存，每一次`string`的复制/赋值，取值都由`string`类负责维护，不用担心复制越界和取值越界等。

> `string` 本质上是一个动态的char数组。

## string 容器常用操作

### string 构造函数

```cpp
string();
// 默认构造函数，创建一个空的字符串
string(const string& str);
// 拷贝构造函数，使用一个string对象初始化另一个string对象
string(const char* s);
// 含参构造函数，使用C风格字符串初始化
string(int n, char c);
// p含参构造函数，使用n个字符c初始化
```

### string 基本赋值操作

#### &#x20;** `=` 赋值操作符**

```cpp
string& operator=(const char* s);
// C风格字符串赋值给当前的字符串
string& operator=(const string& s);
// 把字符串s赋给当前的字符串
string& operator=(const char c);
//字符赋值给当前的字符串
```

#### &#x20;** `assign`成员函数**

```cpp
string& assign(const char* s); 
// C风格字符串赋值给当前的字符串
string& assign(const char* s, int n); 
// 把C风格字符串s的前n个字符赋给当前的字符串
string& assign(const string& s); 
// 把字符串s赋给当前字符串
string& assign(int n, char c); 
// 把n个字符c赋给当前的字符串
string& assign(const string& s, int start, int n); 
// 将字符串s中从start开始的n个字符赋值给当前字符串
```

### string 存取字符操作

#### **`[]`下标获取操作符**

```cpp
char& operator[](int n); 
// 通过[]下标方式获取字符
```

使用下标操作符获取字符时，如果下标越界，程序将会强制终止。

#### `at`成员函数

```cpp
char& at(int n); 
// 通过at方法获取字符
```

使用at方法获取字符时，如果下标越界，at方法内部会抛出异常（`exception`），可以使用`try-catch`捕获并处理该异常。示例如下：

```cpp
#include <stdexception> 
//标准异常头文件
#incldue <iostream>
using namespace std;

int main()
{
    string s = "hello world";
    try
    {
        //s[100]不会抛出异常，程序会直接挂掉
        s.at(100);
    }
    catch (out_of_range& e) 
        //如果不熟悉异常类型，可以使用多态特性， catch(exception& e)。
    {
        cout << e.what() << endl; 
        //打印异常信息
    }
    return 0;
}
```

{% hint style="warning" %}
关于C++的异常处理用法可以查看相关教程了解，参考教程：

[https://www.runoob.com/cplusplus/cpp-exceptions-handling.html](https://www.runoob.com/cplusplus/cpp-exceptions-handling.html)

后续教程将默认读者了解C++异常处理的相关知识点。
{% endhint %}

### string 拼接操作

#### **`+=`复合操作符**

```cpp
string& operator+=(const string& str); 
// 将字符串str追加到当前字符串末尾
string& operator+=(const char* str); 
// 将C风格字符数组追加到当前字符串末尾
string& operator+=(const char c); 
// 将字符c追加到当前字符串末尾
/* 上述操作重载了复合操作符+= */
```

#### **`append`成员函数**

```cpp
string& append(const char* s); 
// 把C风格字符数组s连接到当前字符串结尾
string& append(const char* s, int n); 
// 把C风格字符数组s的前n个字符连接到当前字符串结尾
string& append(const string &s); 
// 将字符串s追加到当前字符串末尾
string& append(const string&s, int pos, int n); 
// 把字符串s中从pos开始的n个字符连接到当前字符串结尾
string& append(int n, char c); 
// 在当前字符串结尾添加n个字符c
```

### string 查找和替换

#### **`find`成员函数**

```cpp
int find(const string& str, int pos = 0) const; 
// 查找str在当前字符串中第一次出现的位置，从pos开始查找，pos默认为0
int find(const char* s, int n = 0) const; 
// 查找C风格字符串s在当前字符串中第一次出现的位置，从pos开始查找，pos默认为0
int find(const char* s, int pos, int n) const; 
// 从pos位置查找s的前n个字符在当前字符串中第一次出现的位置
int find(const char c, int pos = 0) const; 
// 查找字符c第一次出现的位置，从pos开始查找，pos默认为0
```

> 当查找失败时，`find`方法会返回`-1`，`-1`已经被封装为string的静态成员常量`string::npos`。
>
> ```cpp
> static const size_t nops = -1;
> ```

#### **`rfind`成员函数**

```cpp
int rfind(const string& str, int pos = npos) const; 
// 从pos开始向左查找最后一次出现的位置，pos默认为npos
int rfind(const char* s, int pos = npos) const; 
// 查找s最后一次出现的位置，从pos开始向左查找，pos默认为npos
int rfind(const char* s, int pos, int n) const; 
// 从pos开始向左查找s的前n个字符最后一次出现的位置
int rfind(const char c, int pos = npos) const; 
// 查找字符c最后一次出现的位置
```

{% hint style="info" %}
`find`方法通常查找字串第一次出现的位置，而`rfind`方法通常查找字串最后一次出现的位置。

`rfind(str, pos)`的实际的开始位置是`pos + str.size()`，即从该位置开始（不包括该位置字符）向前寻找匹配项，如果有则返回字符串位置，如果没有返回`string::npos`。

`-1`其实是`size_t`类的最大值（学过补码的同学应该不难理解），所以`string::npos`还可以表示“直到字符串结束”，这样的话rfind中pos的默认参数是不是就不难理解啦？
{% endhint %}

#### **`replace`成员函数**

```cpp
string& replace(int pos, int n, const string& str); 
// 替换从pos开始n个字符为字符串s
string& replace(int pos, int n, const char* s);
// 替换从pos开始的n个字符为字符串s
```

### string 比较操作

#### **`compare`成员函数**

```cpp
int compare(const string& s) const; // 与字符串s比较
int compare(const char* s) const; // 与C风格字符数组比较
```

`compare`函数依据字典序比较，在当前字符串比给定字符串小时返回`-1`，在当前字符串比给定字符串大时返回`1`，相等时返回`0`。

#### **比较操作符**

```cpp
bool operator<(const string& str) const;
bool operator<(const char* str) const;
bool operator<=(const string& str) const;
bool operator<=(const char* str) const;
bool operator==(const string& str) const;
bool operator==(const char* str) const;
bool operator>(const string& str) const;
bool operator>(const char* str) const;
bool operator>=(const string& str) const;
bool operator>=(const char* str) const;
bool operator!=(const string& str) const;
bool operator!=(const char* str) const;
```

`string`类重载了所有的比较操作符，其含义与比较操作符本身的含义相同。

### string 子串

#### **`substr`成员函数**

```cpp
string substr(int pos = 0, int n = npos) const;
// 返回由pos开始的n个字符组成的字符串
```

### string 插入和删除操作

#### **`insert` 成员函数**

















