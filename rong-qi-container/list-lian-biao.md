---
description: STL中list类的详细介绍
---

# 📜 list - 链表

## list 容器基本概念

链表是一种物理存储单元上非连、续非顺序的储存结构，数据元素的逻辑顺序是通过链表中的指针链接次序实现的。

* 链表由一系列结点（链表中每一个元素称为结点）组成
* 结点可以在运行时动态生成
* 每个结点包括两个部分
  * 储存数据元素的数据域
  * 储存下一个结点地址的指针域

相较于vector的连续线性空间，list就显得复杂许多

* 它的好处是每次插入或者删除一个元素，就是配置或者释放一个元素的空间
* 因此，list对于空间的运用有绝对的精准，一点也不浪费
* 而且，list对于任何位置插入或删除元素都是常数项时间

list 容器是一个**双向链表**

![list](../.gitbook/assets/IMG\_1587.jpeg)

链表灵活，但是空间和时间的额外消耗会比较大。

## list 容器的迭代器

list 容器不能像vector一样以普通指针作为迭代器，因其节点不能保证在同一块连续的内存空间上。

list 迭代器必须有能力指向list的结点，并有能力进行正确的递增、递减、取值、成员存取操作。

由于list是一个双向链表，迭代器必须能够具备前移、后移的能力，所以list容器提供的是Bidirectional Iterators，双向迭代器。

list 有一个重要的性质，插入和删除操作都不会造成原有list迭代器的失效

* 这在vector是不成立的，因为vector的插入操作可能会造成内存的重新配置，导致原有的迭代器全部失效
* 而list元素的删除只会使得被删除元素的迭代器失效

### list 容器的顺序遍历

```cpp
for(list<T>::iterator it = lst.begin(); it != lst.end(); it++)
{
    cout << *it << endl;
}
```

### list 容器的逆序遍历

```cpp
for(list<T>::reverse_iterator it = lst.rbegin(); it != lst.rend(); it++)
{
    cout << *it << endl;
}
```

## list 容器的数据结构

list不仅仅是一个双向链表，而且是一个**循环的双向链表**。

## list 常用API

### list 构造函数

```cpp
list<T> lstT; // 默认构造形式，list采用模版类实现
list(beg, end); // 构造函数将[beg, end)区间内的元素拷贝给本身
list(int n, T elem); // 构造函数将n个elem拷贝给本身
list(const list& lst); // 拷贝构造函数
```

### list 数据元素插入和删除操作

```cpp
void push_back(T elem); // 在容器尾部加入一个元素
void pop_back(); // 删除容器中最后一个元素

void push_front(T elem); // 在容器开头插入一个元素
void pop_front(); // 从容器开头移除第一个元素

insert(iterator pos, elem); // 在pos位置插入elem元素的拷贝，返回新数据的位置
insert(iterator pos, n, elem); // 在pos位置插入n个elem元素的拷贝，无返回值
insert(iterator pos, beg, end); // 在pos位置插入[beg, end)区间内的数据，无返回值

void clear(); // 移除容器的所有数据

erase(beg, end); // 删除[beg, end)区间内的所有数据，返回下一个数据的位置
erase(pos); // 删除pos位置的数据，返回下一个数据的位置

remove(elem); // 删除容器中所有与elem匹配的元素
```









