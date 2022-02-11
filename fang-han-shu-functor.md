---
description: 在详细讲常用的算法之前补充一下函数对象的相关内容，后面会用到。
cover: >-
  https://images.unsplash.com/photo-1611117775350-ac3950990985?crop=entropy&cs=srgb&fm=jpg&ixid=MnwxOTcwMjR8MHwxfHNlYXJjaHw0fHxtYWNoaW5lfGVufDB8fHx8MTY0NDUwODcxNA&ixlib=rb-1.2.1&q=85
coverY: 0
---

# 🏭 仿函数(Functor)

## 函数对象 / 仿函数

1. **重载函数调用操作符的类，其对象**常称为**函数对象(function object)**，也叫**仿函数(functor)**，使得类对象可以像函数那样调用。
2. STL提供的算法往往有两个版本，一种是按照我们常规默认的运算来执行，另一种允许用户自己定义一些运算或操作，通常通过回调函数或模版参数的方式来实现，此时functor便派上了用场，特别是作为模版参数的时候，只能传类型。
3. 函数对象超出了普通函数的概念，其内部可以拥有自己的状态(其实也就相当于函数内的static变量)，可以通过成员变量的方式被记录下来。
4. 函数对象可以作为函数的参数传递。
5. 函数对象通常不定义构造和析构函数，所以在构造和析构时不会发生任何问题，避免了函数调用时的运行时问题。
6. 模版函数对象使函数对象具有通用性，这也是它的优势之一。
7. STL需要我们提供的functor通常只有一元和二元两种。
8. lambda 表达式的内部实现其实也是仿函数

![](https://images.unsplash.com/photo-1509475826633-fed577a2c71b?crop=entropy\&cs=srgb\&fm=jpg\&ixid=MnwxOTcwMjR8MHwxfHNlYXJjaHw2fHxmdW5jdGlvbnxlbnwwfHx8fDE2NDQ1MDcxMTk\&ixlib=rb-1.2.1\&q=85)

## 谓词

返回值为`bool`的普通函数或者函数对象，也就是我们离散数学中学习的predictor，比较常用的是一元谓词和二元谓词。

> 在概述中有提到过，这里再重复一下。

## 内建函数对象

> 使用时需要包含头文件`<functional>`

STL 内建了一些函数对象，分为：

* 算术类函数对象
* 关系运算类函数对象
* 逻辑运算类函数对象

### 算术类函数对象

```cpp
template<class T> T plus<T>; // 加法仿函数
template<class T> T minus<T>; // 减法仿函数
template<class T> T multiplies<T>; // 乘法仿函数
template<class T> T divides<T>; // 除法仿函数
template<class T> T modulus<T>; // 取模仿函数
template<class T> T negate<T>; // 取反函数
```

> negate 是一元运算，其他都是二元运算。

### 关系运算类函数对象

```cpp
template<class T> bool equal_to<T>; // 等于
template<class T> bool not_equal_to<T>; // 不等于
template<class T> bool greater<T>; // 大于
template<class T> bool greater_equal<T>; // 大于等于
template<class T> bool less<T>; // 小于
template<class T> bool less_equal<T>; // 小于等于
```

### 逻辑运算类运算函数

```cpp
template<class T> bool logical_and<T>; // 逻辑与
template<class T> bool logical_or<T>; // 逻辑或
template<class T> bool logical_not<T>; // 逻辑非
```

## 适配器

为了方便理解，我们以例子的形式来讲述这个部分的内容。

### 函数对象适配器

#### **原程序**

```cpp
#include <iostream>
#inlcude <algorithm>
#include <vector>
using namespace std;

class myPrint
{
public:
    void operator()(int val) { cout << val << endl; }
}

int main()
{
    vector<int> v;
    for (int i = 0; i < 10; i++) v.push_back(i);
    for_each(v.begin(), v.end(), myPrint());
    return 0;
}
```

#### **新需求**

我们希望在每个数据输出的时候加上一个基值，并且该基值由用户输入。

#### **使用函数适配器做的改进**

```cpp
#include <iostream>
#inlcude <algorithm>
#include <vector>
#include <functional>
using namespace std;

class myPrint: public binary_function<int, int, void> 
// 2.做继承 参数1类型 + 参数2类型 + 返回值类型 binary_function
{
public:
    void operator()(int val, int base) const // 3. 加const, 和父类保持一致
    {
        cout << val + base << endl;
    }
}

int main()
{
    vector<int> v;
    for (int i = 0; i < 10; i++) v.push_back(i);
    int n;
    cin >> n;
    for_each(v.begin(), v.end(), bind2nd(myPrint(), n)); 
    // 1. 将参数进行绑定 bind2nd
    // bind1st 功能类似，不过n会被绑定到第一个参数中
    return 0;
};
```

### 取反适配器

#### **原程序**

```cpp
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;

class GreaterThanFive
{
public:
    bool operator()(int val) { return val > 5; }
}

int main()
{
    vector<int> v;
    for (int i = 0; i < 10; i++) v.push_back(i);
    
    vector<int>::iterator pos = find_if(v.begin(), v.end(), GreaterThanFive());
    
    if (pos != v.end()) cout << *pos << endl;
    return 0;
}
```

#### **新需求**

我们希望找第一个不大于5的数，但又不想再写一个LessEqualThanFive。

#### **使用取反适配器做的改进**

```cpp
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;

class GreaterThanFive: public unary_function<int, bool> 
// 2. 做继承 参数类型 + 返回值类型 unary_function
{
public:
    bool operator()(int val) const // 3.加 const
    {
        return val > 5;
    }
}

int main()
{
    vector<int> v;
    for (int i = 0; i < 10; i++) v.push_back(i);
    
    vector<int>::iterator pos = find_if(v.begin(), v.end(), not1(GreaterThanFive()));  //1. 一元取反 not1
    
    if (pos != v.end()) cout << *pos << endl;
    return 0;
}
```

其实综合前面的内容会有更简便的方法：

```cpp
vector<int>::iterator pos = find_if(v.begin(), v.end(), not1(bind2nd(greater<int>(), 5)));
```

{% hint style="warning" %}
这一行代码如果还半知半解的话还请驻足思考一番，思考透彻了上述内容才算是真的懂了。
{% endhint %}

### 函数指针适配器

沿用函数对象适配器的例子，假设`myPrint`是一个全局函数

```cpp
for_each(v.begin(), v.end(), bind2nd(ptr_fun(myPrint), n));
// 函数指针适配器 ptr_fun 将函数指针适配成仿函数
```

### 成员函数适配器

我们假设有一个`Dog`类，`Dog`类内部有一个`bark()`成员方法，有一个装满了`Dog`的`vector`叫做`v`。

```cpp
for_each(v.begin(), v.end(), mem_fun_ref(&Dog::bark));
// 成员函数适配器 mem_fun_ref
// 如果容器中存放的不是对象实体，而是对象指针时，则需使用 ptr_fun
```

## 偏函数

对于一个多参数的函数，在某些应用场景下，它的一些参数往往取固定值，可以针对这样的函数，生成一个新函数，该新函数不包含原函数中已指定固定值的参数。（partial function application, 偏函数）

* 偏函数可缩小一个函数的适用范围，提高函数的针对性

例如，对于下面的print函数：

```cpp
void print(int n, int base); // 按base进制来输出n
```

由于它常常用来按十进制输出，因此，可以基于print生成一个新函数print10，只接受一个参数n，base固定为10：

```cpp
#include <functional>
using namespace std;
using namespace std::placeholders;

function<void(int)> print10 = bind(print, _1, 10);
print10(23); //相当于 print(23, 10)
```

{% hint style="info" %}
`function`类和`bind`的使用需要`c++11`标准
{% endhint %}

{% hint style="success" %}
至此，读者应当对于仿函数/函数对象有了一个稍微深入一点的理解了。如果还觉得有些云里雾里的话也没关系，以后熟悉了就好了。
{% endhint %}
