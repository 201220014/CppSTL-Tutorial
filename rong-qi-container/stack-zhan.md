---
description: STL中stack类的详细介绍
---

# 🍡 stack - 栈

## stack 容器基本概念

stack 是一种先进后出（First In Last Out, FILO）的数据结构，它只有一个出口。

stack 容器允许新增元素、移除元素、取得栈顶元素，但是除了最顶端外，没有任何其他方法可以存取stack的其他元素。

{% hint style="warning" %}
**换言之，stack不允许有遍历行为。**
{% endhint %}

![stack](../.gitbook/assets/IMG\_1585.jpeg)

## stack 没有迭代器

> 不允许遍历行为，自然也就不提供迭代器了。

## stack 常用API

### stack 构造函数

```cpp
stack<T> stkT; // 默认构造函数，stack采用模版类实现
stack(const stack& stk); // 拷贝构造函数
```

### stack 赋值操作

```cpp
stack& operator=(const stack& stk); // 重载赋值操作符
```

### stack 数据存取操作

```cpp
void push(T elem); // 向栈顶添加元素
void pop(); // 从栈顶移除第一个元素
T& top(); // 返回栈顶元素
```

### stack 大小操作

```cpp
bool empty(); // 判断堆栈是否为空
int size(); // 返回栈的大小
```

{% hint style="success" %}
至此，读者应当对stack的特点及基本操作有了较为全面的认识，使用时API记不清可以回头多看。
{% endhint %}
