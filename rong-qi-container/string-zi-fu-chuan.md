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

```cpp
string& insert(int pos, const char* s); // 在pos位置插入C风格字符数组
string& insert(int pos, const string& str); // 在pos位置插入字符串str
string& insert(int pos, int n, char c); // 在pos位置插入n个字符c
```

> 返回值是插入后的字符串结果，`erase`同理。其实就是指向自身的一个引用。

#### **`erase` 成员函数**

```cpp
string& erase(int pos, int n = npos); // 删除从pos位置开始的n个字符
```

> 默认一直删除到末尾。

### `string` 和 `C-Style` 字符串的转换

#### **`string` 转 `const char*`**

```cpp
string str = "demo";
const char* cstr = str.c_str();
```

#### **`const char*` 转 `string`**

```cpp
const char* cstr = "demo";
string str(cstr); // 本质上其实是一个有参构造
```

{% hint style="info" %}
在c++中存在一个从`const char*`到`string`类的隐式类型转换，但却不存在从一个`string`对象到`const char*`的自动类型转换。对于`string`类型的字符串，可以通过`c_str()`方法返回`string`对象对应的`const char*` 字符数组。

比如说，当一个函数的参数是`string`时，我们可以传入`const char*`作为参数，编译器会自动将其转化为`string`，但这个过程不可逆。

为了修改string字符串的内容，下标操作符`[]`和`at`都会返回字符串的引用，但当字符串的内存被重新分配之后，可能发生错误。（结合字符串的本质是动态字符数组的封装便不难理解了）
{% endhint %}

## 和 string 相关的全局函数

> 注：有的可能需要C++11标准。

### 大小写转换

```cpp
#include <cctype>
// 在iostream中已经包含了这个头文件，如果没有包含iostream头文件，则需手动包含cctype

int tolower(int c); // 如果字符c是大写字母，则返回其小写形式，否则返回本身
int toupper(int c); // 如果字符c是小写字母，则返回其大写形式，否则返回本身

/**
  * C语言中字符就是整数，这两个函数是从C库沿袭过来的，保留了C的风格
*/
```

如果想要对整个字符串进行大小写转化，则需要使用一个`for`循环，或者配合和`algorithm`库来实现。例如：

```cpp
#include <string>
#include <cctype>
#include <algorithm>

string str = "Hello, World!";
transform(str.begin(), str.end(), str.begin(), toupper); //字符串转大写
transform(str.begin(), str.end(), str.begin(), tolower); //字符串转小写
```

### 字符串和数字的转换

#### **`int`/`double` 转 `string`**

> c++11标准新增了全局函数`std::to_string`，十分强大，可以将很多类型变成`string`类型。

```cpp
#include <string>
using namespace std;

/** 带符号整数转换成字符串 */
string to_string(int val);
string to_string(long val);
string to_string(long long val);

/** 无符号整数转换成字符串 */
string to_string(unsigned val);
string to_string(unsigned long val);
string to_string(unsigned long long val);

/** 实数转换成字符串 */
string to_string(float val);
string to_string(double val);
string to_string(long double val);
```

#### **`string` 转 `double` / `int`**

```cpp
#include <cstdlib>
#include <string>
using namespace std;

/** 字符串转带符号整数 */
int stoi(const string& str, size_t* idx = 0, int base = 10);
long stol(const string& str, size_t* idx = 0, int base = 10);
long long stoll(const string& str, size_t* idx = 0, int base = 10);

/**
  * 1. idx返回字符串中第一个非数字的位置，即数值部分的结束位置
  * 2. base为进制
  * 3. 该组函数会自动保留负号和自动去掉前导0
 */

/** 字符串转无符号整数 */
unsigned long stoul(const string& str, size_t* idx = 0, int base = 10);
unsigned long long stoull(const string& str, size_t* idx = 0, int base = 10);

/** 字符串转实数 */
float stof(const string& str, size_t* idx = 0);
double stod(const string& str, size_t* idx = 0);
long double stold(const string& str, size_t* idx = 0);
```

与之类似的在同一个库里的还有一组基于字符数组的函数如下。

```cpp
// 'a' means array, since it is array-based. 

int atoi(const char* str); // 'i' means  int
long atol(const char* str); // 'l' means long
long long atoll(const char* str); // 'll' means long long

double atof(const char* str); // 'f' means double
```

{% hint style="success" %}
至此，读者应当详细了解了C++STL中string容器的各种用法以及其他一些字符串处理的常用函数。可能量有些大，无法一下子记住，可以暂时留个印象，待到使用时多翻一翻，慢慢就记住了。
{% endhint %}
