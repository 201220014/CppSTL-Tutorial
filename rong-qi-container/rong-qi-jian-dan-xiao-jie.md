---
description: 简单小结一下各类容器的特点和选用条件。
---

# 🗒 容器简单小结

## container 的特点总结

|        | vector | deque | list | set | multiset | map       | multimap |
| ------ | ------ | ----- | ---- | --- | -------- | --------- | -------- |
| 典型内存结构 | 单端数组   | 双端数组  | 双向链表 | 二叉树 | 二叉树      | 二叉树       | 二叉树      |
| 可随机存储  | 是      | 是     | 否    | 否   | 否        | 对key而言，不是 | 否        |
| 元素搜索速度 | 慢      | 慢     | 非常慢  | 快   | 快        | 对key而言，快  | 对key而言，快 |
| 元素安插移除 | 尾端     | 头尾两端  | 任何位置 | -   | -        | -         | -        |

## container 的使用场景

vector 可以涵盖其他所有容器的功能，只不过实现特殊功能时效率没有其他容器高。但如果只是简单存储，vector效率是最高的。

deque 相比于 vector 支持头端元素的快速增删。

**vector 与 deque 的比较**

* `vector.at()`比`deque.at()`的效率高
  * 比如`vector.at(0)`是固定的，`deque`的开始位置是不固定的
* 如果有大量释放操作的话，`vector`花的时间更少
* `deque`支持头部的快速插入与快速删除，这是`deque`的优点

list 支持频繁的不确定位置元素的移除插入。

set 会自动排序。

map 是元素为键值对组并按键排序的set。

{% hint style="success" %}
结合概述一章中的关于容器，容器一章的前述内容以及上述简单小结，相信读者已经对C++STL中的容器有了十分清晰而又详尽的理解。恭喜，小可爱又变强了:smile:。
{% endhint %}