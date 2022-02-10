---
description: 对于STL中各种类型的算法的详细介绍
cover: >-
  https://images.unsplash.com/photo-1510511459019-5dda7724fd87?crop=entropy&cs=srgb&fm=jpg&ixid=MnwxOTcwMjR8MHwxfHNlYXJjaHwyfHxhbGdvcml0aG18ZW58MHx8fHwxNjQ0NDk2MDc2&ixlib=rb-1.2.1&q=85
coverY: -634.3047830923248
---

# 💻 算法(Algorithm)

## 算法概述

算法主要由头文件`<algorithm><functional><numeric>`组成

* `<algorithm>` 是所有STL头文件中最大的一个，其中常用的功能涉及到比较、交换、查找、遍历、复制、修改、反转、排序、合并等
* `<numeric>` 体积很小，只包括在几个序列容器上进行简单运算的模版函数
* `<functional>` 定义了一些模版类，用以声明函数对象

自定义的类如果想要直接使用算法库，则需补全默认构造函数、拷贝构造函数、析构函数、赋值操作符、小于操作符、等于操作符。

## 常用遍历算法

### for\_each



