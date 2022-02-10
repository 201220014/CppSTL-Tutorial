---
description: STL中queue类的详细介绍
---

# 🏁 queue - 队列

## queue 容器基本概念

queue 是一种先进先出(First In First Out, FIFO)的数据结构，它有两个出口，queue容器允许从一端新增元素，从另一端移除元素。

![queue](../.gitbook/assets/IMG\_1586.jpeg)

## queue 没有迭代器

只有queue的顶端元素，才有机会被外界去用。queue不提供遍历功能，也不提供迭代器。

## queue 常用API

### queue 构造函数

```cpp
queue<T> queT; // queue对象的默认构造函数形式，采用模版类实现
queue(const queue& que); // 拷贝构造函数
```

### queue 存取、插入和删除操作

```cpp
void push(T elem); // 往队尾添加元素
void pop(); // 从队头移除第一个元素
T& back(); // 返回最后一个元素
T& front(); // 返回第一个元素
```

### queue 赋值操作

```cpp
queue& operator=(const queue& que); // 重载赋值操作符
```

### queue 大小操作

```cpp
bool empty(); // 判断队列是否为空
int size(); // 返回队列的大小
```

{% hint style="success" %}
至此，读者应当对queue的特点及基本操作有了较为全面的认识，使用时API记不清可以回头多看。
{% endhint %}
