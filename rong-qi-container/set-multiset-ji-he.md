---
description: STL中set/multiset类的详细介绍
---

# 🏵 set / multiset - 集合

## set/multiset 容器基本概念

### set 容器基本概念

set的特性是，所有的容器都会根据元素自身的键值进行自动被排序。

set的元素不像map那样可以同时拥有实值和键值，set的元素既是实值又是键值。

* set不允许两个元素有相同的键值

我们不可以通过set的迭代器改变set元素的值。

* 因为其元素值就是键值，任意改变会严重破坏set的组织
* 换句话说，set的iterator是一种const\_iterator

set和list拥有某些相同的性质，当对容器中的元素进行插入操作或者删除操作的时候，除了被删除的元素之外，操作之前的所有迭代器，操作之后仍然有效。

### multiset 基本概念

* multiset特性及用法和set完全相同，唯一的差别在于它允许键值重复。
* set和multiset的底层实现是**红黑树**，红黑树为平衡二叉树的一种

> 注意，multiset和set共用一个头文件。

![](https://images.unsplash.com/photo-1581361265618-8e33511bb9b7?crop=entropy\&cs=srgb\&fm=jpg\&ixid=MnwxOTcwMjR8MHwxfHNlYXJjaHwzfHxzZXRzfGVufDB8fHx8MTY0NDUwMzYwNQ\&ixlib=rb-1.2.1\&q=85)

## set 的遍历

```cpp
for (set<T>::iterator it = s.begin(); it != s.end(); it++)
{
  	cout << *it << endl;
}
```

## set 常用API

### set 构造函数

```cpp
set<T> st; // set 默认构造函数
multiset<T> mst; // multiset 默认构造函数
set(const set& st); // 拷贝构造函数
```

### set 赋值操作

```cpp
set& operator=(const set& st); // 重载等号操作符

swap(st); // 交换两个集合容器
```

### set 大小操作

```cpp
int size(); // 返回容器中元素的数目
bool empty(); // 判断容器是否为空
```

### set 插入和删除操作

```cpp
pair<iterator, bool> insert(T elem); 
// 在容器中插入元素，返回插入位置的迭代器（不成功则返回end()）和是否插入成功
// 如果是multiset，则返回值只有iterator
clear(); // 清除所有元素
iterator erase(pos); // 删除pos迭代器所指的元素，返回下一个元素的迭代器
iterator erase(beg, end); // 删除区间[beg, end)内的所有元素，返回下一个元素的迭代器
erase(T elem); // 删除容器中值为elem的元素
```

插入之前可以指定排序规则：

```cpp
//利用仿函数 指定set容器的排序规则
class MyCompare
{
public:
    bool operator()(int v1, int v2)
    {
        return v1 > v2;
    }
};

set<int, MyCompare> s;

//模版类也是可以有默认值的，第二个模版参数的默认值为less
```

> 自定义的数据类型需要指出排序规则。
>
> 当然，也可以通过重载小于操作符的方式指出。

### set 查找操作

```cpp
iterator find(T key); 
// 查找键key是否存在，若存在，返回该键的元素的迭代器；若不存在，返回set.end();
int count(T key);
// 查找键key的元素个数
iterator lower_bound(T keyElem);
// 返回第一个key>=keyElem元素的迭代器
iterator upper_bound(T keyElem);
// 返回第一个key>keyElem元素的迭代器
pair<iterator, iterator> equal_range(T keyElem);
// 返回容器中key与keyElem上相等的两个上下限迭代器
```

> 上述几个方法若不存在，返回值都是尾迭代器。

对组的构造和使用：

```cpp
//构造
pair<T1, T2> p(k, v);
//另一种构造方式
pair<T1, T2> p = make_pair(k, v);
//使用
cout << p.first << p.second << endl;
```

{% hint style="success" %}
至此，读者应当对set/multiset的特点及基本操作有了较为全面的认识，使用时API记不清可以回头多看。
{% endhint %}
