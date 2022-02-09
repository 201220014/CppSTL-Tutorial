---
description: 通过一个例子，复习巩固已经学习的STL的用法
---

# 💿 一个串联前文的例子

## 在“学生”容器中作统计

在这个例子里面，我们将利用STL的容器，创建一个“学生”容器，并在其中利用STL的算法，通过STL的迭代器作用于容器，从而作一些统计工作。

{% hint style="info" %}
下面这个例子能够完全看懂即可，如果想要上手写一遍，注意补全各种类型的定义。
{% endhint %}

## 学生的类型

一下是一个简单的学生类型，这将是我们要放在容器中的数据。下面的类的定义是不完备的，只列出了后续需要用到的部分，省略的部分可自行补充。

```cpp
class Student
{
private:
    int no;
    string name;
    Sex sex;
    string birth_place;
    Major major; 
    // ......
public:
    // ......
    int get_no() { return no; }
    string get_name() { return name; }
    int get_sex() { return sex; }
    Major get_major() { return major; }
    display();
    // ......
};
```

## 需求及实现

### 建立“学生”容器

```cpp
vector<Student> students; // 创建学生容器
while (...) // 循环输入每个学生信息并放入容器
{
   Student st;
   ...... // 输入一个学生信息到st
   students.push_back(st); // 把st数据放入容器
}
```

### 容器元素按“学号由小到大”排序

```cpp
bool compare_no(Student &st1, Student &st2)
{
    return st1.get_no() < st2.get_no();
}
```

```cpp
sort(students.begin(), students.end(), compare_no);
```

### 显示每个学生的信息

```cpp
void display(Student &st) //显示st的信息
{
    st.display();
    cout << endl; 
}
```

```cpp
for_each(students.begin(), students.end(), display);
```

### 统计“计算机”专业的人数

```cpp
bool match_major(Student &st) { return st.get_major() == COMPUTER; }
```

```cpp
cout <<  count_if(students.begin(), students.end(), match_major);
```

**统计物理专业的人数呢？**

```cpp
bool match_major2(Student &st) { return st.get_major() == PHYSICS; }
```

```cpp
cout <<  count_if(students.begin(), students.end(), match_major2);
```

**再统计“XXX”专业的人数呢？**

{% hint style="info" %}
这里稍微拓展一下**仿函数(Functor)**的使用，在后续会有详细的介绍。
{% endhint %}

定义函数对象类如下：

```cpp
class MatchMajor
{
    Major major;
public:
    MatchMajor (Major m) { major = m; }
    bool operator ()(Student& st) { return st.get_major() == major; }
};
```

下面就可以这样使用了：

```cpp
count_if(students.begin(), students.end(), MatchMajor(COMPUTER))；
count_if(students.begin(), students.end(), MatchMajor(PHYSICS))；

// 统计XXX专业的人数
count_if(students.begin(), students.end(), MatchMajor(XXX))；
```

类似的，**统计“XXX”专业男生/女生的人数**：

```cpp
class MatchMajorAndSex
{  
    Major major;
    Sex sex;
public:
    MatchMajorAndSex(Major m,Sex s) { major = m; sex = s; }
    bool operator ()(Student& st)
    { 
        return st.get_major() == major && st.get_sex() == sex;
    }
};
```

```cpp
// 计算机女生
count_if(students.begin(),students.end(), MatchMajorAndSex(COMPUTER,FEMALE));

// 物理男生
count_if(students.begin(),students.end(), MatchMajorAndSex(PHYSICS,MALE))；
```

如果还有更多的









