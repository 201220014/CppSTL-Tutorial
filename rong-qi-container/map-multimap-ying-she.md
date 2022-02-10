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

## map/multimap 常用API

### map 构造函数

```cpp
map<T1, T2> mapTT; // map默认构造函数
map(const map& mp); // 拷贝构造函数
```

### map 赋值操作

```cpp
map& operator=(const map& mp); // 重载等号操作符
swap(mp); // 交换两个集合容器
```

### map 大小操作

```cpp
int size(); // 返回容器中元素的数目
bool empty(); // 判断容器是否为空
```

### map 插入元素操作

```cpp
pair<iterator, bool> insert(pair<T1, T2> p); // 通过pair的方式插入对象
/*
1. 参数部分可以用pair的构造函数创建匿名对象
2. 也可以使用make_pair创建pair对象
3. 还可以用map<T1, T2>::value_type(key, value)来实现
*/

T2& operator[](T1 key); // 通过下标的方式插入值
// 如果通过下标访问新的键却没有赋值，会自动用默认值填充
```

> map指定排序规则的方式和set类似，都是利用functor在模版类型表的最后一个参数处指定。

### map 删除操作

```cpp
void clear(); // 删除所有元素
iterator erase(iterator pos); // 删除pos迭代器所指的元素，返回下一个元素的迭代器
iterator erase(beg, end); // 删除区间[beg, end)内的所有元素，返回下一个元素的迭代器
erase(keyElem); // 删除容器中key为keyElem的对组
```

### map 查找操作

```cpp
iterator find(T1 key); 
// 查找键key是否存在，若存在，返回该键的元素的迭代器；若不存在，返回map.end()
int count(T1 keyElem);
// 返回容器中key为keyElem的对组个数，对map来说只可能是0或1，对于multimap可能大于1

iterator lower_bound(T keyElem);
// 返回第一个key>=keyElem元素的迭代器
iterator upper_bound(T keyElem);
// 返回第一个key>keyElem元素的迭代器
pair<iterator, iterator> equal_range(T keyElem);
// 返回容器中key与keyElem上相等的两个上下限迭代器
```



