---
description: STL中map/multimap类的详细介绍
---

# 🗺 map / multimap - 映射

## map / multimap 基本概念

map 的特性是，所有的元素都会根据元素的键值自动排序；

map 的所有元素都是pair，同时拥有实值和键值。

* pair的第一元素被视为键值，第二元素被视为实值；
* map不允许两个元素有相同的键值；

和set类似的原因，我们不能通过迭代器改变map的键值，但我们可以任意修改实值。

map和list在增删元素的时候具有相似的性质。

map和multimap的操作类似，唯一的区别是multimap键值可重复。

map和multimap都是以红黑树作为底层实现机制。

> map和multimap包含在同一个头文件中。

![](https://images.unsplash.com/photo-1476973422084-e0fa66ff9456?crop=entropy\&cs=srgb\&fm=jpg\&ixid=MnwxOTcwMjR8MHwxfHNlYXJjaHw2fHxtYXB8ZW58MHx8fHwxNjQ0NTA0MjU4\&ixlib=rb-1.2.1\&q=85)

## map 的遍历

```cpp
for (map<T1, T2>::iterator it = m.begin(); it != m.end(); it++)
{
    cout << "key = " << it->first << " value = " << it->second << endl;
}
```







