---
description: STL中迭代器的概述
---

# ⬇ 关于迭代器

## 迭代器-Iterator

迭代器实现了抽象的指针（智能指针）功能，它们用于指向容器中的元素，对容器中的元素进行访问和遍历。&#x20;

在STL中，迭代器是作为类模板来实现的（在头文件`iterator`中定义），它们可分为以下几种类型：&#x20;

### 根据访问修改权限分类

* 输出迭代器（output iterator，记为：**OutIt**）
  * 可以修改它所指向的容器元素
  * 间接访问操作（`*`）
  * `++`操作
* 输入迭代器（input iterator，记为：**InIt**）
  * 只能读取它所指向的容器元素
  * 间接访问操作（`*`）和元素成员间接访问（`->`）
  * `++`、`==`、`!=`操作。

### 根据迭代方式分类

* 前向迭代器（forward iterator，记为：**FwdIt**）
  * 可以读取/修改它所指向的容器元素
  * 元素间接访问操作（`*`）和元素成员间接访问操作（`->`）
  * `++`、`==`、`!=`操作
* 双向迭代器（bidirectional iterator，记为：**BidIt**）
  * 可以读取/修改它所指向的容器元素
  * 元素间接访问操作（`*`）和元素成员间接访问操作（`->`）
  * `++`、`--`、`==`、`!=`操作
* 随机访问迭代器（random-access iterator，记为：**RanIt**）
  * 可以读取/修改它所指向的容器元素
  * 元素间接访问操作（`*`）、元素成员间接访问操作（`->`）和下标访问元素操作（`[]`）
  * `++`、`--`、`+`、`-`、`+=`、`-=`、`==`、`!=`、`<`、`>`、`<=`、`>=`操作

## 各容器的迭代器类型

* 对于`vector`、`deque`以及`basic_string`容器类，与它们关联的迭代器类型为**随机访问迭代器（RanIt）**
* 对于`list`、`map/multimap`以及`set/multiset`容器类，与它们关联的迭代器类型为**双向迭代器（BidIt）**

{% hint style="danger" %}
`queue`、`stack`和`priority_queue`容器类，不支持迭代器！
{% endhint %}

## 迭代器之间的相融关系

![迭代器之间的相融关系](<../.gitbook/assets/截屏2022-02-09 19.58.20.png>)

{% hint style="info" %}
在需要箭头左边迭代器的地方可以用箭头右边的迭代器去替代。
{% endhint %}

除了上面五种基本迭代器外，STL还提供了一些迭代器的适配器，用于一些特殊的操作，如：&#x20;

### 反向迭代器（reverse iterator）

用于对容器元素从尾到头进行反向遍历，可以通过容器类的成员函数`rbegin`和`rend`可以获得容器的尾和首元素的反向迭代器。

需要注意的是，对反向迭代器，`++`操作是往容器首部移动，`--`操作是往容器尾部移动。&#x20;

### 插入迭代器（insert iterator）

用于在容器中指定位置插入元素，其中包括：

* &#x20;`back_insert_iterator`（用于在尾部插入元素）
* &#x20;`front_insert_iterator`（用于在首部插入元素）
* &#x20;`insert_iterator`（用于在任意指定位置插入元素）

&#x20;它们可以分别通过函数`back_inserter`、`front_inserter`和`inserter`来获得，函数的参数为容器。

## 举例：使用`list`和`iterator`求解约瑟夫问题

```cpp
#include <iostream>
#include <list>
using namespace std;
int main()
{ 
   int m,n; // m用于存储要报的数，n用于存储小孩个数
   cout << "请输入小孩的个数和要报的数：";
   cin >> n >> m;
   // 构建圈子
   list<int> children; // children是用于存储小孩编号的容器
   for (int i=0; i<n; i++) // 循环创建容器元素
      children.push_back(i); // 把i（小孩的编号）从容器尾部放入容器
   // 开始报数
   list<int>::iterator current; // current为指向容器元素的迭代器
   current = children.begin(); // current初始化成指向容器的第一个元素
   while (children.size() > 1) // 只要容器元素个数大于1，就执行循环
   {
      for (int count = 1; count < m; count++)  //报数，循环m-1次
      {
         current++; //current指向下一个元素
         // 如果current指向的是容器末尾，current指向第一个元素
         if (current == children.end()) current = children.begin();
       }
       // 循环结束时，current指向将要离开圈子的小孩
       current = children.erase(current);  // 小孩离开圈子，current指向下一个元素
       // 如果current指向的是容器末尾，current指向第一个元素
       if (current == children.end()) current = children.begin();
   } // 循环结束时，current指向容器中剩下的唯一的一个元素，即胜利者
   // 输出胜利者的编号
   cout << "The winner is No." << *current << "\n";
   return 0;
}
```

> 上面程序中的`list`也可以换成`vector`，但由于程序中**需要经常在容器的任意位置上删除元素**，而`list`容器的**双向链表**结构比较适合这个操作！

{% hint style="success" %}
到此为止，读者应当了解了C++中迭代器所扮演的角色，迭代器的类型及其基本用法。
{% endhint %}
