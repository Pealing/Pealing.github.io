---
title: C++新手村攻略
date: 2017-03-27 23:19:36
tags: C++
---
# 前言
    最近在准备一些面试的东西，做了一些笔试题之后发现。自己的基础真的是忘得一干二净。别说
    什么友元这种平时基本看不到的东西了。连最基本的指针、引用都混乱得一塌糊涂。于是赶紧收
    拾东西开始给自己补课。为了防止再次遗忘，会把东西写在这里。有需要跟我一起重温新手村的
    小伙伴们欢迎自取~欢迎补充~

<!--more-->

# 引用

## 引用的基本形式
```cpp
int a = 3;
int &b = a;
//此时的b等同于a
```

*不能只有引用，没有实体，所以以下是错误的*
```cpp
int &b;
```

### Example
```cpp
#include <string.h>
#include <iostream>
using namespace std;
int main(void)
{
    int a = 3;
    int &b = a;
    b = 10;
    cout<<a<<endl;
    return 0;
}
```
结果为：*10*，对b的改变等同于对a的改变。

## 指针类型的引用
```cpp
#include <string.h>
#include <iostream>
using namespace std;
int main(void)
{
    int a = 3;
    int *p = &a;
    int *&q = p;
    *q = 20;
    cout<<a<<endl;
    return 0;
}
```
结果为*20*，在这里，q是对p的引用，等同于p，p是指向a地址的指针。

"*指针 == 值"

"&变量 == 地址"

## 引用作为函数参数
### 对于C语言：
```c
void fun(int *a,int*b)
{
    int c = *b;
    *b = *a;
    *a = c;
}
int main()
{
    int x = 10;y = 20;
    fun(&x,&y);
    return 0;
}
```

### 对于C++:
```cpp
void fun(int &a,int &b)
{
    int c = b;
    int b = a;
    int a = c;
}
int main()
{
    int x = 10,y = 20;
    fun(x,y);
    return 0;
}
```

# const关键字
## 作用
    * 定义const常量，具有不变性
    * 便于类型检查，使编译器对处理内容有更多的了解
    * 避免意义模糊的字段出现
    * 防止意外修改
    * 为函数重载提供了一个参考
    * 节省空间，避免不必要的内存分配
    * 提高效率。一般被保存在符号表中，不另外分配空间。

## Example
```cpp
const int x = 3;
x = 5//出错，x为常量，不可以修改
```

```cpp
int x = 3;
const int *p = &x; //*p为常量
p = &y //正确
*p = 5//错误
```

```cpp
int x = 3;
int * const p = &x;//p为常量
p = &y;//错误
*p = 5;//正确
```

```cpp
int x = 3;
const int &y = x;
y = 10;//错误，y是不可变的
x = 10;//正确，x是可变的
```

```cpp
const int x = 3;
int *y = &x;//错误，可能通过*y改变常量x
```

# 函数特性
## 函数参数
* 带默认值的参数一定要放在参数列表的最右边。
```cpp
void fun(int i = 0,int j = 5,int k )//错误
void fun(int i,int j = 5,int k = 10)//正确
```

## 函数重载
* 相同作用域
* 用同一函数名定义多个函数
* 参数个数和参数类型不同

## 内联函数
    编译时将函数体代码和实参代替函数调用语句。
```cpp
inline int max(int a,int b,int c)
int main()
{
    int i = 10,j = 20,k = 5;
    m = max(i,j,k);
    return 0;
}
```

* 内联函数是建议性的，是否内联由编译器决定
* 内联函数逻辑必须简单，不能包含for,while
* 调用频繁的函数建议使用内联函数
* 递归函数不能作为内联函数

# 内存管理
    申请/归还内存就是内存管理

```cpp
    int *p = new int[10];//申请内存
    delete []p;//释放内存
```

```cpp
    int main()
    {
        int a[10];
        int *b = new int[10];
        cout<<sizeof(a)<<" "<<sizeof(b)<<endl;
        return 0;
    }
```
结果：*40 4*，int的长度为4，new没有长度的概念，需要自己记住长度。new分配首地址给指针。

```cpp
int *p = new int;
if(p == NULL)
{
    //内存分配失败
}
delete p
p = NULL;//需要将指针置空，避免再次回收出错。
```

## Example
```cpp
char str1[] = "abc";
char str2[] = "abc";
const char str3[] = "abc";
const char str4[] = "abc";
char *str5 = "abc";
char *str6 = "abc";
cout<<boolapha<<(str1 == str2)<<endl;
cout<<boolapha<<(str3 == str4)<<endl;
cout<<boolapha<<(str5 == str6)<<endl;
```
结果：*FALSE FALSE TRUE*，前4个为数组，数组地址不同。str5和str6为指向常量池中"abc"地址的指针。

## 内存分区
* 栈区：定义变量(int x = 0; int *p = NULL;),内存由系统分配。
* 堆区: int *p = new int[20]; 必须使用delete回收，堆区需要程序员管理。
* 全局区: 存储全局变量和静态变量
* 常量区：string str = "hellow";存储字符串和常量。
* 代码区: 存储逻辑代码的二进制

# typedef和define

* typedef用来定义一个标识符及关键字的别名，是语言编译的一个过程，并不实际分配空间。
* #define是宏定义语句，通常用来定义常量，不在编译过程中进行，而是在预处理完成的，但也因此难以发现潜在的错误和其他代码的维护问题。

## 区别
/#define 是简单的字符串代替（原地扩展），
```cpp
typedef (int*) pINT
#define pINT2 int*

pINT a,b;//等同于 int *a,int*b
pINT2 a,b;//等同于 int *a,b;表示定义了一个指针a和整型变量b
int *a,b;//只声明了一个指针a,b为int
```

## 注意
```cpp
//注意1：
typedef char* PSTR
int function(const PSTR,const PSTR);
//const PSTR不是cont char*,而是char* const,typedef不是简单的字符串替换，const在这里给指针本身常量性。

//注意2：
typedef static int INT2;//错误
//auto、extern、mutable、static、register等关键字不能放入typedef中，因为typedef的本质也是一个存储类的关键字。
```

# String类

## 可以输入为空和带有空格的string：
```cpp
#include <string.h>
#include <iostream>
using namespace std;
int main(void)
{
    string a;
    getline(cin,a);
    cout<<a<<endl;
    return 0;
}
```

## 非法情况
```cpp
string str6 = "hellow" + "world";//非法输入
```

# 类
## 默认构造函数
```cpp
class student
{
    public:
    student();//形式1
    student(string name = "Jim");//形式2
}
```

## 初始化列表
```cpp
class student
{
    student():m_strName("Jim"),m_iAge(10);//初始化列表，
}
```
* 初始化列表先于构造函数执行
* 初始化列表只能用于构造函数
* 初始化列表可以同时初始化多个数据成员
### 初始化的必要性
```cpp
class circle
{
public:
    circle()
    {m_dipi = 3.14}//语法错误。相当于对常量第二次赋值。
private:
    const double m_dipi;

}
```

```cpp
class circle
{
public:
    circle():m_dipi(3.14){}//使用初始化的方式，正确。
private:
    const double m_dipi;
}
```

## 拷贝构造函数
    定义格式：类名(const 类名 &变量名)

```cpp
class student
{
public:
    student(){m_strName = "jim";}
    student(const student &stu){}
private:
    string m_strName;
}
```

# 对象成员
```cpp
#include <iostream>
using namespace std;
class point
{
public:
    point(){cout<<"123"<<endl;}
    ~point(){cout<<"~123"<<endl;}
private:
    int x;
};
class line
{
    public:
        line(){cout<<"456"<<endl;}
        ~line(){cout<<"~~"<<endl;}
    private:
        point a;
};
int main()
{
    line li;
    return 0;
}
//先实例化point，再实例化line。先销毁line，再销毁point
```
结果：123 456 ~~ ~123

# 深拷贝和浅拷贝
## 浅拷贝
    不同的指针指向同一块地址

## 深拷贝
    不同的指针指向不同地址，并将地址中的值复制过来

# 对象指针
```cpp
class coordinate
{
public:
    coordinate(){}
private:
    int x;
    int y;
};
int main()
{
    coordinate *p = new coordinate;
    delete p;
    p = NULL;
    return 0;
}
```
在上述的例子中，
p保存的是x的地址，指向对象中的第一个元素
访问方法：
p->x,p->y
(*p).x
(*p).y

# 排序
## 时间复杂度
* 冒泡排序 O(N^2)
* 快速排序 O(nlog2n)
* 插入排序 最好O(n),最差O(n^2)  链表最差为O(n(n+1)/2)
* 希尔排序 
* 简单选择排序 O(N^2)
* 堆排序 O(nlog2n)

## 稳定性
* 稳定的排序算法：
    - 插入
    - 冒泡排序
    - 归并算法
* 不稳定的排序算法：
    - 希尔排序
    - 选择
    - 快排
    - 堆排

```cpp
//快速排序 
void quicksort(int data[],int length,int start,int end)
{
    if(data == null || start<0 || end >= length)
        return;
    int low = start;
    int height = end;
    int index = data[low];
    while(height > low)
    {
        while(height>low && index<data[height])
        {
            --height;
        }
        if(height > low)
            data[low ++ ] = data[height];
        while(height>low && index>data[low])
        {
            ++low;
        }
        if(height > low)
            data[height--] = data[low];
    }
    data[low] = index;
    if(start < low-1) quicksort(data,low,start,low-1);
    if(low+1 < end) quicksort(data,length-low,low+1,end);
}

//归并排序
void Nerfe(int data[],int low,int mid,int height)
{
    int* temp = new int [hight + 1];
    int i = low,j = mid+1,k = low;
    while(i<=mid && j<=height)
    {
        if(data[i] <= data[j])
            temp[k++] = data[i++];
        else
            temp[k++] = data[j++];
    }
    while(i<=mid)
        temp[k++] = data[i++];
    while(j<=height)
        temp[k++] = data[j++];
    for(int i=0;i<height;i++)
        data[i] = temp[i];
    delete temp;
    temp = NULL;
}

//二分查找
int BinSearch(int elem[],int low,int height,int key)
{
    int i = low;
    int j = height;
    if(low > height)
        mid = -1;
    else
    {
        int mid = (i+j)/2;
        if(key < elem[mid])
            mid = BinSearch(elem,i,mid-1,key);
        else if(key > elem[mid])
            mid = BinSearch(elem,mid+1,j,key);
    }
    return mid;
}

//二分查找的，旋转数组的最小数字
int min(int *numbers,int length)
{
    int low = 0;
    int height = length-1;
    int mid = (low + height) / 2;
    while(height > low)
    {
        if(numbers[mid] > numbers[height])
            low = mid + 1;
        else if(numbers[mid] < numbers[height])
            height = mid;
        else
        {
            if(numbers[mid + 1] >= numbers[mid])
                height = mid;
            else
                height = mid + 1;
        }
        mid = (low + height) / 2;
    }
    return numbers[mid];
}

// 斐波那契优化
long long Fibonacci(unsigned n)
{
    int result[2] = {0,1};
    if(n < 2)
        return result[n];
    long long first = 0;
    long long second = 1;
    long long fibN = 0;
    for(unsigned int i = 2;i<n;i++)
    {
        fibN = first + second;
        first = second;
        second = fibN;
    }
}

```
