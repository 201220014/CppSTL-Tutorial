---
description: STL中vector类的详细介绍
---

# 🚃 vector - 向量

## vector 容器基本概念

`vector`的数据安排及操作方式，与`array`非常相似，两者的唯一差别在于空间的运用的灵活性。

`array`是静态空间，一旦配置了一般不能改变，如果要改变空间大小，需要自行完成以下三个步骤：

* 配置一块新的空间
* 将旧数据搬往新的空间
* 释放原来的空间

而`vector`是动态空间，但其实`vector`的动态也是对于上述过程的封装，并且`vector`配置空间的策略也考虑了运行成本，采用特定的扩展的策略（并不是简单的成倍扩展）。

![vector](<../.gitbook/assets/屏幕截图 06-07-2021 22.56.05.png>)

## vector 迭代器

vector维护一个线性空间，所以不论元素类型如何，普通指针都可以作为vector的迭代器。

* 因为vector迭代器所需要的行为，如`operator*`，`operator->`，`operator++`，`operator--`，`operator+`，`operator-`，`operator+=`，`operator-=`，普通指针天生具备。
* vector指针支持随机存取，而普通指针正有着这样的能力。

所以，vector提供的是随机访问迭代器（_Random Access Iterator_），其内部用普通指针实现。

### 使用迭代器进行正序遍历

```cpp
for (vector<T>::iterator it = v.begin(); it != v.end(); it++)
{
    cout << *it << endl;
}
/**
 * 1. 迭代器的声明方式:  容器类型::迭代器类型
 * 2. 顺序首尾迭代器由begin()和end()方法生成
*/
```

### 使用迭代器逆序遍历

```cpp
for (vector<T>::reverse_iterator it = v.rbegin(); it != v.rend(); it++)
{
    cout << *it << endl;
}
/**
  * 1. 逆向迭代器不再是iterator，而是reverse_iterator
  * 2. 逆序首位迭代器由rbegin()和rend()方法生成
*/
```

### 判断迭代器是否能随机访问的方法

用多了自然就背上了，下面给出一种现场测试的方法。

```cpp
iterator++；
iterator--；
//通过编译，至少是双向迭代器
  
iterator = iterator + 1；
//通过编译，则是随机访问迭代器
```

## vector 数据结构

vector采用的数据结构非常简单，线性连续空间，它以两个迭代器`_Myfirst`和`_Mylast`分别指向配置得来的连续空间中已被使用的范围，并以迭代器`Myend`指向整块连续内存空间的尾端。

为了降低空间配置时的成本，vector实际配置的大小可能比用户端需求大一些，以备将来可能的扩充，这便是**容量**的概念。

* 一个vector容器的容量永远大于等于其大小，一旦容量等于大小，便是满载，下次再有新增元素，整个vector容器就得另觅居所。

{% hint style="warning" %}
所谓动态增加大小，并不是在原空间之后续接新空间（因为无法保证原空间之后尚有可配置的空间），而是一块更大的内存空间，然后将原数据拷贝新空间，并释放原空间。

因此，**对vector的任何操作，一旦引起空间的重新配置，指向原vector的所有迭代器就都失效了**。
{% endhint %}

## vector常用API操作

> API = Applicational Programming Interface

### vector 构造函数

```cpp
vector<T> v; // 采用模版类实现，默认构造函数
vector<T> v(T* v1.begin(), T* v1.end()); // 将v1[begin(), end())区间中的元素拷贝给本身
vector<T> v(int n, T elem); // 将n个elem拷贝给本身
vector<T> v(const vector<T> v1); // 拷贝构造函数
```

下面对于第二种构造方式给出一个特殊的例子：

```cpp
int array[5] = {1, 2, 3, 4, 5};
vector<int> v(array, array + sizeof(array) / sizeof(int));
// 联系我们之前提到的vector迭代器本质上是指针就不难理解了
```

### vector 常用赋值操作

> 由于vector采用模版类实现，其完整的函数声明会稍显复杂，下面方法的演示会省略类型界定。

```cpp
assign(beg, end); // 将[beg, end)区间中的数据拷贝复制给本身
assign(n, elem); // 将n个elem拷贝给本身
vector& operator=(const vector& vec); // 重载赋值操作符
```

互换操作也可视为一种特殊的赋值：

```cpp
swap(vec); //将vec与本身的元素互换
```

> 巧用`swap`来收缩空间：
>
> ```cpp
> vector<int>(v).swap(v);
> // vector<int>(v): 利用拷贝构造函数初始化匿名对象
> // swap(v): 交换的本质其实只是互换指向内存的指针
> // 匿名对象指针指向的内存会由系统自动释放
> ```

### vector 大小操作

```cpp
int size(); // 返回容器中的元素个数
bool empty(); // 判断容器是否为空

void resize(int num); 
// 重新指定容器的长度为num，若容器变长，则以默认值填充新位置。
// 若容器变短，则末尾超出容器长度的元素被删除
void resize(int num, T elem); 
// 重新指定容器的长度为num，若容器变长，则以elem填充新位置。
// 若容器变短，则末尾超出容器长度的元素被删除

int capacity(); // 返回容器的容量
void reserve(int len); 
// 容器预留len个元素长度，预留位置不初始化，元素不可访问
```

### vector 数据存取操作

```cpp
T& at(int idx); // 返回索引idx所指的数据，如果idx越界，抛出out_of_range异常
T& operator[](int idx); // 返回索引idx所指的数据，如果idx越界，运行直接报错

T& front(); // 返回首元素的引用
T& back(); // 返回尾元素的引用
```

### vector插入和删除操作

```cpp
insert(const_iterator pos, T elem); // 在pos位置处插入元素elem
insert(const_iterator pos, int n, T elem); // 在pos位置插入n个元素elem
insert(pos, beg, end); // 将[beg, end)区间内的元素插到位置pos
push_back(T elem); // 尾部插入元素elem
pop_back(); // 删除最后一个元素

erase(const_iterator start, const_iterator end); // 删除区间[start, end)内的元素
erase(const_iterator pos); // 删除位置pos的元素

clear(); // 删除容器中的所有元素
```

{% hint style="success" %}
至此，读者应当对vector的特点及基本操作有了较为全面的认识，使用时API记不清可以回头多看。
{% endhint %}
