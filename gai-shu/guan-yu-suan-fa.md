---
description: STL中算法的概述
---

# 📱 关于算法

## 算法-Algorithm

在STL中，除了用容器类模板自身提供的成员函数来操作容器元素外，还提供了一系列通用的对容器中元素进行操作的**函数模板**，称为**算法**。&#x20;

STL算法实现了**对序列元素的一些常规操作**，用函数模板实现的，主要包括：&#x20;

* 调序算法
* 编辑算法
* 查找算法
* 算术算法
* 集合算法
* 堆算法
* 元素遍历算法

除了算术算法在头文件`numeric`中定义外，其它算法都在头文件`algorithm中`定义。

大部分STL算法都是**遍历**指定容器中某范围内的元素，对**满足条件**的元素执行**某项操作**。&#x20;

算法的内部实现往往隐含着循环操作，但这对使用者是隐藏的。&#x20;

* 使用者只需要提供：容器（迭代器）、操作条件以及可能的自定义操作。
* 算法的**控制逻辑则由算法内部实现**，这体现了一种**抽象**的编程模式。

![](https://images.unsplash.com/photo-1635335356074-5a9e45b47a14?crop=entropy\&cs=srgb\&fm=jpg\&ixid=MnwxOTcwMjR8MHwxfHNlYXJjaHw5fHxhbGdvcml0aG18ZW58MHx8fHwxNjQ0NDk2MDc2\&ixlib=rb-1.2.1\&q=85)

## 算法与容器之间的关系

在STL中，不是把容器传给算法，而是把容器的某些迭代器传给它们，在算法中通过迭代器来访问和遍历相应容器中的元素。

这样做的好处是：使得**算法不依赖于具体的容器，提高了算法的通用性**。&#x20;

{% hint style="info" %}
虽然容器各不相同，但它们的迭代器往往具有相容关系，一个算法往往可以接受相容的多种迭代器。
{% endhint %}

## 算法接受的迭代器类型

一个算法能接收的迭代器的类型是通过**算法模板参数的名字**来体现的。例如：

```cpp
template <class InIt, class OutIt>
OutIt copy(InIt src_first, InIt src_last, OutIt dst_first) {...}
```

* `src_first`和`src_last`是输入迭代器，算法中只能读取它们指向的元素。
* `dst_first`是输出迭代器，算法中可以修改它指向的元素。
* 以上参数可以接受其它相容的迭代器。

## 算法的操作范围

用算法对容器中的元素进行操作时，大都需要用两个迭代器来指出要操作的元素的范围。

例如：

```cpp
void sort(RanIt first, RanIt last);
```

* `first`是第一个元素的位置
* `last`是最后一个元素的下一个位置

有些算法可以有多个操作范围，这时，除第一个范围外，其它范围可以不指定最后一个元素位置，它由第一个范围中元素的个数决定。例如：

```cpp
OutIt copy(InIt src_first, InIt src_last, OutIt dst_first);
```

一个操作范围的两个迭代器必须属于同一个容器，而不同操作范围的迭代器可以属于不同的容器。

## 算法的自定义操作条件

有些算法可以让使用者提供一个**函数**或**函数对象**来作为**自定义操作条件**（或称为**谓词**），其参数类型为相应容器的元素类型，返回值类型为bool。

自定义操作条件可分为：

* **Pred**：一元“谓词”，需要一个元素作为参数
* **BinPred**：二元“谓词”，需要两个元素作为参数

### 一元谓词举例

例如，对于下面的“**统计**”算法：

```cpp
size_t count_if(InIt first, InIt last, Pred cond);
```

可以有如下使用方式：

```cpp
#include <vector>
#include <algorithm>
#include <iostream>
using namespace std;

bool f(int x) { return x > 0; }

int main() 
{
    vector<int> v;
    ...... // 往容器中放了元素
    cout << count_if(v.begin(), v.end(), f); // 统计v中正数的个数
    return 0;
}
```

### 二元谓词举例

例如，对于下面的“**排序**”算法：

```cpp
void sort(RanIt first, RanIt last); // 按“<”排序
void sort(RanIt first, RanIt last, BinPred comp); // 按comp返回true规定的次序
```

可以有如下用法：

```cpp
#include <vector>
#include <algorithm>
using namespace std;

bool greater2(int x1, int x2) { return x1 > x2; }

int main()
{
    vector<int> v;
    ...... // 往容器中放了元素
    sort(v.begin(), v.end()); // 从小到大排序
    sort(v.begin(), v.end(), greater2); // 从大到小排序
    return 0;
}
```

## 算法的自定义操作

有些算法可以让使用者提供一个函数或函数对象作为**自定义操作**，其参数和返回值类型由相应的算法决定。

自定义操作可分为：

* **Op**或**Fun**：一元操作，需要一个参数
* **BinOp**或**BinFun**：二元操作，需要两个参数

### 一元操作举例

例如，对于下面的“元素遍历”算法：

```cpp
Fun for_each(InIt first, InIt last, Fun f);
```

可以有如下用法：

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

void f(int x) { cout << ' ' << x; }

int main()
{
     vector<int> v;
     ...... // 往容器中放了元素
     for_each(v.begin(), v.end(), f); // 对v中的每个元素去调用函数f进行操作
     return 0;
}
```

### 二元操作举例

例如，对于下面的“累积”算法：

```cpp
T accumulate(InIt first, InIt last, T val); // 按“+”操作
T accumulate(InIt first, InIt last, T val, BinOp op); // 由op决定累积的含义
```

设元素为，$$e_1, e_2, ..., e_n$$，算法返回$$t_n$$：

$$
t_1 = op(val, e_1), \ t_2  = op(t1, e_2), \ ...... \ t_n = op(t_{n-1}, e_{n-1})
$$

可以有如下用法：

```cpp
#include <vector>
#include <numeric>
using namespace std;

int f1(int partial, int x) { return partial * x; }
int f2(int partial, int x) { return partial + x * x; }
double f3(double partial, int x) { return partial + 1.0 / x; }

int main
{
    vector<int> v;
    ...... // 往容器中放了元素
    
    int sum = accumulate(v.begin(), v.end(), 0); // 所有元素和
    int product = accumulate(v.begin(), v.end(), 1, f1); // 所有元素的乘积
    int sq_sum = accumulate(v.begin(), v.end(), 0, f2); // 所有元素平方和
    int rec_sum = accumulate(v.begin(), v.end(), 0.0, f3); // 元素倒数和
    
    return 0;
}
```

再例如，对于下面的元素“**变换/映射**”算法：

```cpp
OutIt transform(InIt src_first, InIt src_last, OutIt dst_first, Op f);
OutIt transform(InIt1 src_first1, InIt1 src_last1, InIt2 src_first2, OutIt dst_first, BinOp f);
```

可以有如下用法：

```cpp
#include <algorithm>
#include <vector>
using namespace std;

int f1(int x) { return x * x; }
int f2(int x1, int x2) { return x1 + x2; }

int main()
{
    vector<int> v1,v2,v3,v4;
    ...... // 往v1和v2容器中放了元素
    
    transform(v1.begin(),v1.end(),v3.begin(),f1); 
    // v3中的元素是v1相应元素的平方
    
    transform(v1.begin(),v1.end(),v2.begin(),v4.begin(),f2); 
    // v4中的元素是v1和v2相应元素的和
    
    return 0;
}
```

{% hint style="success" %}
至此，读者应当已经明白了STL中的算法是怎样通过迭代器作用于容器的。
{% endhint %}

