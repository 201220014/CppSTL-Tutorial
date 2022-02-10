---
description: STL中deque类的详细介绍
---

# ➿ deque - 双向队列

## duque 容器基本概念

vector 容器是单向开口的连续内存空间，deque 则是一种双向开口的连续线性空间

* 所谓的双向开口，意思是可以在头尾两端分别做元素的插入和删除操作

![deque](../.gitbook/assets/IMG\_1587.jpeg)

* vector 虽然也能在头尾插入元素，但是在头部插入元素的效率很低，需要大量进行移位操作

**deque 容器和 vector 最大的差异**

* deque 允许使用常数项时间在头部插入或删除元素
* deque 没有容量的概念，因为它是由动态的分段连续空间组合而成，随时可以增加一块新的空间并链接起来

虽然deque也提供了 Random Access Iterator，但其实现相比于vector要复杂得多，所以需要随机访问的时候最好还是用vector。

## deque 容器实现原理

![deque实现原理](../.gitbook/assets/IMG\_1584.jpeg)

### deque 的遍历

基本的遍历方式同 vector，不做赘述。这里提一下如何在遍历的时候防止内容被修改。

```cpp
#include <deque>
using namespace std;

const deque<T> d;
for (deque<T>::const_iterator it = d.begin(); it != d.end(); it++) 
// 要用const_iterator指向常量容器
{
  	// 如果在此处修改it指向空间的值，编译器会报错
  	cout << *it << endl;
}

/**
  * iterator 普通迭代器
  * reverse_iterator 反转迭代器
  * const_iteratoe 只读迭代器
*/
```

## deque 常用 API

### deque 构造函数

```cpp
deque<T> deqT; // 默认构造函数
deque(beg, end); // 构造函数将[beg, end)区间中的元素拷贝给本身
deque(int n, T elem); // 构造函数将n个elem拷贝给本身
deque(const deque& deq); // 拷贝构造函数
```

### deque 赋值操作

```cpp
assign(beg, end); // 将[beg, end)区间中的元素拷贝赋值给本身
assign(int n, T elem); // 将n个元素elem拷贝赋值给本身

deque& operator=(const deque& deq); // 重载赋值操作符

swap(deq); // 将deq与本身的元素互换
```

### deque 大小操作

```cpp
int size(); // 返回容器中元素的个数
bool empty(); // 判断容器是否为空

void resize(int num); 
// 重新指定容器的长度为num，若容器变长，则以默认值填充新位置，
// 如果容器变短，则末尾超出容器长度的元素被删除
void resize(int num, T elem);
// 重新指定容器的长度为num，若容器变长，则以elem填充新位置，
// 如果容器变短，则末尾超出容器长度的元素被删除
```

### deque 的双端插入和删除操作

```cpp
push_back(T elem); // 在容器尾部添加一个元素
push_front(T elem); // 在容器头部插入一个元素

pop_back(); // 删除容器最后一个数据
pop_front(); // 删除容器第一个数据
```

### deque 的数据存取

```cpp
T& at(int idx); // 返回索引idx所指的数据，如果idx越界，抛出out_of_range异常
T& operator[](int idx); // 返回索引idx所指的数据，如果idx越界，运行直接报错

T& front(); // 返回首元素的引用
T& back(); // 返回尾元素的引用
```

### deque 插入操作

```cpp
const_iterator insert(const_iterator pos, T elem); 
// 在pos位置处插入元素elem的拷贝，返回新数据的位置
void insert(const_iterator pos, int n, T elem); 
// 在pos位置插入n个元素elem，无返回值
void insert(pos, beg, end);
// 将[beg, end)区间内的元素插到位置pos，无返回值
```

### deque 删除操作

```cpp
clear(); // 移除容器的所有数据
iterator erase(iterator beg, iterator end);
// 删除区间[beg, end)的数据，返回下一个数据的位置
iterator erase(iterator pos)；
// 删除pos位置的数据，返回下一个数据的位置
```

{% hint style="success" %}
至此，读者应当对deque的特点及基本操作有了较为全面的认识，使用时API记不清可以回头多看。
{% endhint %}
