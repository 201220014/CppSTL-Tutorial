---
description: STL中容器的概述
---

# 🥣 关于容器

## 容器-Container

**容器**用于表示由同类型元素构成的、长度可变的元素序列。&#x20;

容器是由类模板来实现的，模板的参数是容器中元素的类型。&#x20;

STL中包含了很多种容器，虽然这些容器提供了一些相同的操作，但由于它们采用了不同的内部实现方法，因此，**不同的容器**往往适合于**不同的应用场合**。

![container](https://images.unsplash.com/photo-1493946740644-2d8a1f1a6aff?crop=entropy\&cs=srgb\&fm=jpg\&ixid=MnwxOTcwMjR8MHwxfHNlYXJjaHwxfHxjb250YWluZXJ8ZW58MHx8fHwxNjQ0NDAyNzk1\&ixlib=rb-1.2.1\&q=85)

## STL的主要容器

### vector<元素类型>

用于需要快速定位（访问）任意位置上的元素以及主要在元素序列的尾部增加/删除元素的场合。

在头文件`vector`中定义，用**动态数组实现。**

### list<元素类型>

用于经常在元素序列中任意位置上插入/删除元素的场合。&#x20;

在头文件`list`中定义，用**双向链表**实现。&#x20;

{% hint style="info" %}
在C++11标准中增加了`forward_list`容器，本质上是一个**单向链表**，定义在头文件`forward_list`中。
{% endhint %}

### deque<元素类型>

用于主要在元素序列的两端增加/删除元素以及需要快速定位（访问）任意位置上的元素的场合。

在头文件`deque`中定义，用**分段的连续空间结构**实现。

### stack<元素类型>

用于仅在元素序列的尾部增加/删除元素的场合。

在头文件`stack`中定义，可基于`deque`、`list`或`vector`来实现。

### queue<元素类型>

用于仅在元素序列的尾部增加、头部删除元素的场合。

在头文件`queue`中定义，可基于`deque`和`list`来实现。

### priority\_queue<元素类型>

它与`queue`的操作类似，不同之处在于：每次增加/删除元素之后，它将对元素位置进行调整，使得头部元素总是最大的。也就是说，每次删除操作总是把最大（优先级最高）的元素去掉。

在头文件`queue`中定义，可基于`deque`和`vector`来实现。

### map<关键字类型，值类型> 和 multimap<关键字类型，值类型>

用于需要根据关键字来访问元素的场合。容器中每个元素是一个`pair`结构类型，该结构有两个成员：`first`和`second`，关键字对应`first`，值对应`second`，元素是根据其关键字排序的。

对于`map`，不同元素的关键字不能相同；

对于`multimap`，不同元素的关键字可以相同。

它们在头文件`map`中定义，常常用某种**二叉树**来实现。

{% hint style="info" %}
有时候我们不需要排序，所以在C++11标准中新增加了`unordered_map`

和`unordered_multimap`容器。
{% endhint %}

### set<元素类型> 和 multiset<元素类型>

它们分别是`map`和`multimap`的特例，每个元素只有关键字而没有值，或者说，关键字与值合一了。

在头文件`set`中定义。

{% hint style="info" %}
C++11标准中增加了`unordered_set`和`unordered_multiset`容器。
{% endhint %}

### basic\_string<字符类型>

与`vector`类似，不同之处在于其元素为字符类型，并提供了一系列与**字符串**相关的操作。&#x20;

`string`和`wstring`分别是它的两个实例：

* `basic_string<char>`
* `basic_string<wchar_t>`

在头文件`string`中定义。

## 容器的基本操作

容器类模板提供了一些成员函数来实现容器的**基本操作**，其中包括：&#x20;

* 往容器中增加元素
* 从容器中删除元素
* 获取容器中指定位置的元素
* 在容器中查找元素
* 获取容器首/尾元素的迭代器
* ......

> 这些成员函数往往具有通用性（大部分容器类模板都包含它们）。

{% hint style="warning" %}
注意：如果容器的元素类型是一个类，则针对该类可能需要：

* 自定义拷贝构造函数和赋值操作符重载函数
  * 对容器进行的一些操作中可能会创建新的元素（对象的拷贝构造）或进行元素间的赋值（对象赋值）。
  * 重载小于操作符（<） 对容器进行的一些操作中可能要进行元素比较运算。
{% endhint %}

## 举例：利用`map`实现一个简单的电话簿

```cpp
#include <iostream>
#include <map>
#include <string>
using namespace std;
int main()
{
    map<string,int> phone_book; // 创建一个map类容器，用于存储电话号码簿
    
    // 创建电话簿
    phone_book["wang"] = 12345678; // 通过[]操作和关键字往容器中加入元素
    phone_book["li"] = 87654321;
    phone_book["zhang"] = 56781234;
    // ......  还可以添加更多的信息
    // 输出电话号码簿
    cout << "电话号码簿的信息如下：\n";
    for (pair<string, int> item: phone_book) 
    // C++11中引入的enhanced-for loop
        cout << item.first << ": " << item.second << endl; 
    // 输出元素的姓名和电话号码

    // 查找某个人的电话号码
    string name;
    cout << "请输入要查询号码的姓名：";
    cin >> name;
    map<string,int>::const_iterator it; // 创建一个不能修改所指向的元素的迭代器
    it = phone_book.find(name); // 查找关键字为name的容器元素
    if (it == phone_book.end()) // 判断是否找到
        cout << name << ": not found\n"; // 未找到
    else
        cout << it->first << ": " << it->second << endl; // 找到
    return 0;
}
```

{% hint style="success" %}
至此，读者应当对于STL中的容器有了一个整体上的把控，知晓各容器的基本特征。
{% endhint %}
