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

```cpp
/**
  * 遍历算法 遍历容器元素
  * @param beg 开始迭代器
  * @param end 结束迭代器
  * @param _callback 函数回调或者函数对象
  * @return 函数对象
*/
for_each(iterator beg, iterator end, _callback);
```

### transform

```cpp
/**
  * transform算法 将指定容器内的元素搬运到另一个容器中
  * 注意：transform不会给目标容器分配内存，所以需要我们提前分配好内存
  * @param beg1 源容器开始迭代器
  * @param end1 源容器结束迭代器
  * @param beg2 目标容器开始迭代器
  * @param _callback 回调函数或者函数对象
  * @return 返回目标容器迭代器
*/
iterator transform(iterator beg1, iterator end1, iterator beg2, _callback);
```

> 注意：目标容器一定要提前分配好内存。

## 常用查找算法

### find

```cpp
/**
  * find 算法 查找元素
  * @param beg 容器开始迭代器
  * @param end 容器结束迭代器
  * @param value 查找的元素
  * @return 返回查找元素的位置
*/
iterator find(iterator beg, iterator end, _callback);
```

### find\_if

```cpp
/**
  * find_if 算法 条件查找
  * @param beg 容器开始迭代器
  * @param end 容器结束迭代器
  * @param _callback 回调函数或者谓词(返回 bool 类型的函数对象)
  * @return 返回查找元素的位置
*/
iterator find_if(iterator beg, iterator end, _callback);
```

> 利用find\_if实现自定义类的find操作的时候，之前的函数适配器可能会派上用场。

### adjacent\_find

```cpp
/**
  * adjacent_find 算法 查找相邻重复元素
  * @param beg 容器开始迭代器
  * @param end 容器结束迭代器
  * @param _callback 回调函数或者谓词(返回 bool 类型的函数对象)
  * @return 返回相邻元素的第一个位置的迭代器
*/
iterator adjacent_find(iterator beg, iterator end, _callback);
```

### binary\_search

```cpp
/**
  * binary_search 算法 二分法查找
  * 注意：在无序序列中不可用
  * @param beg 容器开始迭代器
  * @param end 容器结束迭代器
  * @param value 查找的元素
  * @return bool 查找返回true，否则false
*/
bool binary_search(iterator beg, iterator end, value);
```

### count

```cpp
/**
  * count 算法 统计元素出现次数
  * @param beg 容器开始迭代器
  * @param end 容器结束迭代器
  * @param value 待计数的元素
  * @return int 返回元素个数
*/
int count(iterator beg, iterator end, value); 
```

### count\_if

```cpp
/**
  * count_if 算法 统计元素出现次数
  * @param beg 容器开始迭代器
  * @param end 容器结束迭代器
  * @param _callback 回调函数或者谓词
  * @return int 返回元素个数
*/
int count_if(iterator beg, iterator end, _callback);
```

## 常用排序算法

### merge

```cpp
/**
  * merge 算法 容器元素合并，并储存到另一个容器中
  * 注意：两个容器必须是有序的
  * @param beg1 容器1开始迭代器
  * @param end1 容器1结束迭代器
  * @param beg2 容器2开始迭代器
  * @param end2 容器2结束迭代器
  * @param dest 目标容器开始迭代器
*/
merge(iterator beg1, iterator end1, iterator beg2, iterator end2, iterator dest);
```

### sort

```cpp
/**
  * sort 算法 容器元素排序
  * @param beg 容器开始迭代器
  * @param end 容器结束迭代器
  * @param _callback 回调函数或者谓词
*/
sort(iterator beg, iterator end, _callback);
```

### random\_shuffle

```cpp
/**
  * random_shuffle 算法 对指定范围内的元素随机调整次序
  * @param beg 容器开始迭代器
  * @param end 容器结束迭代器
*/
random_shuffle(iterator beg, iterator end);
```

如果想要每次打乱不同，需要自己设置随机数种子：

```cpp
#include <ctime>
using namespace std;

int main()
{
    srand((unsigned int)time(NULL));
    ......//random_shuffle
}
```

### reverse

```cpp
/**
  * reverse 算法 反转指定范围的元素
  * @param beg 容器开始迭代器
  * @param end 容器结束迭代器
*/
reverse(iterator beg, iterator end);
```

## 常用拷贝和替换算法

### copy

```cpp
/**
  * copy算法 将容器内指定范围的元素拷贝到另一容器当中
  * @param beg 容器开始迭代器
  * @param end 容器结束迭代器
  * @param dest 目标容器开始迭代器
*/
copy(iterator beg, iterator end, iterator dest);
```

使用copy算法快速打印容器：

```cpp
vector<int> v = {1, 2, 3, 4, 5};
for_each(v.begin(), v.end(), [](int val){cout << val << " ";});
// 等价于
copy(v.begin(), v.end(), ostream_iterator<int>(cout, " "));
// 需要#include <iterator>
```

> 其实第一种方法已经够用了，不过为了提升一下逼格，不妨也了解一下第二种。

### replace

```cpp
/**
  * replace算法 将容器内指定范围的旧元素修改为新元素
  * @param beg 容器开始迭代器
  * @param end 容器结束迭代器
  * @param oldvalue 旧元素
  * @param newvalue 新元素
*/
replace(inerator beg, iterator end, oldvalue, newvalue);
```

### replace\_if

```cpp
/**
  * replace_if 算法 将容器内指定范围满足条件的元素替换为新元素
  * @param beg 容器开始迭代器
  * @param end 容器结束迭代器
  * @param _callback 回调函数或者谓词（返回bool类型的函数对象）
  * @param newvalue 新元素
*/
replace_if(inerator beg, inerator end, _callback, newvalue);
```

### swap

```cpp
/**
  * swap 算法 互换两个容器元素
  * @param c1 容器1
  * @param c2 容器2
*/
swap(container c1, container c2);
```

## 常用算数生成算法

### accumulate

```cpp
#include <numeric> // 注意头文件不是algorithm了
/**
  * accumulate 算法 计算容器元素累计总和
  * @param beg 容器开始迭代器
  * @param end 容器结束迭代器
  * @param value 起始累加值
*/
accumulate(iterator beg, iterator end, value);
```

### fill

```cpp
/**
  * fill 算法
  * @param beg 容器开始迭代器
  * @param end 容器结束迭代器
  * @param value 填充元素
*/
fill(iterator beg, iterator end, value);
```

## 常用集合算法

### set\_intersection

```cpp
/**
  * set_intersection 算法 求两个set集合的交集
  * 注意：两个集合必须是有序序列
  * @param beg1 容器1开始迭代器
  * @param end1 容器1结束迭代器
  * @param beg2 容器2开始迭代器
  * @param end2 容器2结束迭代器
  * @param dest 目标容器开始迭代器
  * @return 目标容器最后一个元素的迭代器地址
*/
set_intersection(iterator beg1, iterator end1, iterator beg2, iterator end2, iterator dest);
```

### set\_union

```cpp
/**
  * set_union 算法 求两个set集合的并集
  * 注意：两个集合必须是有序序列
  * @param beg1 容器1开始迭代器
  * @param end1 容器1结束迭代器
  * @param beg2 容器2开始迭代器
  * @param end2 容器2结束迭代器
  * @param dest 目标容器开始迭代器
  * @return 目标容器最后一个元素的迭代器地址
*/
set_union(iterator beg1, iterator end1, iterator beg2, iterator end2, iterator dest);
```

### set\_difference

```cpp
/**
  * set_difference 算法 求两个set集合的差集
  * 注意：两个集合必须是有序序列
  * @param beg1 容器1开始迭代器
  * @param end1 容器1结束迭代器
  * @param beg2 容器2开始迭代器
  * @param end2 容器2结束迭代器
  * @param dest 目标容器开始迭代器
  * @return 目标容器最后一个元素的迭代器地址
*/
set_difference(iterator beg1, iterator end1, iterator beg2, iterator end2, iterator dest);
```







