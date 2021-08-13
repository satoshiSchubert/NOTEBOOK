---
title: 浙江大学数据结构笔记
date: 2021-8-10 21:26:00
tags:
- 浙江大学数据结构笔记
categories:
- 数据结构
---

<p align='center'>
<a href="https://github.com/satoshiSchubert" target="_blank"><img src="https://img.shields.io/badge/Github-@SatoshiNg-f3e1e1.svg?style=flat-square&logo=GitHub"></a>
<img src='https://img.shields.io/badge/style-Chinese-c45a65.svg?style=flat-square' />



# 第一讲 基本概念



### 数据结构是什么？

个人认为比较确切的定义：
>“数据结构是数据对象，以及存在于该对象的实例和组成实例的数据元素之间的各种联系。这些联系可以通过定以相关的函数来给出。” --Sartaj Sahni, 《数据结构、算法与应用》

#### 举例：如何摆放图书？
- 随便放
  - 找书的时候累死
- 按照书名的拼音字母顺序排放
  - 新书要如何插入？
    - 比如《阿Q正传》
  - 怎么找到指定的书？
    - 二分查找？
- 把书架分成几块，每块区域摆放指定类别的书；每种类别内按照拼音顺序排序
  - 新书怎么插入？
    - 先定类别，二分查找确定位置，**移出空位**
  - 怎么找到某本指定的书？
    - 先定类别，再二分查找
  - 空间如何分配？类别应该分多细？
  
**解决问题的效率，跟数据的组织方式有关。** 

相较于循环算法，递归算法很占空间，效率很低。
<!--补充二分法的知识-->

**计时模板：**
```cpp
#include <stdio.h>
#include <time.h>
/* clock_t是clock()函数返回的变量类型*/
clock_t start,stop;
double duration;

int main()
{
    start = clock();
    MyFunction();
    stop = clock();
    duration = ((double)(stop - start))/CLK_TCK;

    return 0;
}
```
#### 数据对象的组织方式
- 逻辑结构
  - 线性结构，一对一
  - 树，一对多
  - 图，多对多
- 物理存储结构
  - 数组
  - 链表

#### 抽象数据类型
- 数据类型（面向对象语言中对象集和操作集是可以合并的，比如类）
  - 数据对象集
  - 数据集合相关联的操作集
- 抽象：**描述**数据类型的方法**不依赖于具体实现**
  - 与存放的机器无关
  - 与数据存储的物理结构无关
  - 与实现操作的算法和编程语言无关

**只描述**数据对象集和相关操作集**是什么**，并不涉及**如何做到**的问题。比如定义**抽象**数据类型：``ElementType``，之后在具体调用时只要``define ElementType xxx``即可，方便很多。

### 算法是什么
#### 选择排序算法的伪码描述
```cpp
void SelectionSort(int List[],int N)
{/*非递减排序*/
    for (i=0;i<N;i++>){
        MinPosition = ScanForMin(List,i,N-1);
        /*找list中从i到N-1的index中的最小元，并将其index返回*/
        /*注意，这里是伪码，List看起来很香数组，但是它也可以是链表*/
        Swap（List[i],List[MinPosition]);
    }
}
```
#### 什么是好的算法
- 空间复杂度S(n)
  - 根据算法写成的程序在执行时**占用存储单元的长度**
  - S(n)过高的算法可能导致内存超限，导致非正常中断
  - 比如递归算法，为了执行下一个递归需暂存目前函数的状态，比如为了``PrintN(9999)``需要先暂存``PrintN(10000)``的状态，而为了``PrintN(9998)``又需要暂存``PrintN(9999)``的状态，以此递推，需要占用很大的空间来暂存状态，S(n) = C·N 
- 时间复杂度T(n)
  - 根据算法写成的程序在执行时**耗费时间的长度**
  - T(n)过高可能导致有生之年都没法执行完程序
  - 比如计算多项式的值，若使用简单的循环``p += (a[i] * pow(x,i))``则需要(1+2+···+n = C<sub>1</sub>·n<sup>2</sup>+C<sub>2</sub>·n 次乘法)

#### 时间复杂度：

| 函数| 1 | 2 | 4 | 8 |
| ----- |----|----|----|----|
| 1 | 1 | 1 | 1 | 1 |
| log n | 0 |1| 2| 3 |
| n | 1 |2| 4 |8|
| n log n | 0| 2 |8 |24| 
| n<sup>2</sup> | 1 |4 |6 |64|
| 2<sup>n</sup> | 2 |4| 16| 256|
| n! | 1 |2 |24| 40326|

**复杂度分析窍门**
- 对于加性的组合，算法复杂度取两个中最大的，T<sub>1</sub>+T<sub>2</sub> = max(O(f<sub>1</sub>(n)),O(f<sub>2</sub>(n)))
- 对于嵌套的组合（乘性），算法复杂度为两个的积的复杂度，T<sub>1</sub>xT<sub>2</sub> = O(f<sub>1</sub>(n)xf<sub>2</sub>(n))
- 若T(n)是关于n的k阶多项式，那么T(n)=O(n<sup>k</sup>)，也就是去最大的那一项
- 一个for循环的时间复杂度等于循环次数乘以循环体代码的复杂度
- if-else结构的复杂度取决于if的条件判断复杂度和两个分支部分的复杂度，总体复杂度取三者中最大

#### 最大子列和
如下：
$$
f(i,j) = max(0,\Sigma_{k=i}^jA_k)
$$
对于O(n<sup>2</sup>)的算法，应考虑是否能化成O(nlogn)

**分治法**，分而治之
> 把一个大而复杂的问题切分成小的块，并分别解决他们，最后再进行综合。


# 第二讲 线性结构
### 线性表及其实现
#### 多项式的表示

1. **顺序存储结构直接表示**
  - 值为系数，下标为指数
  - 对于x+3x<sup>2000</sup>，则需要用2001个分量的数组来表示，效率很低
  - 是否能只保存非零项？
2. **顺序存储结构表示非零项**
  - 使用结构数组P<sub>1</sub>(x) = (a<sub>i</sub>,i),其中a<sub>i</sub>是系数，i是指数
  - 为了计算方便，应按照指数大小有序存储
  - 相加时只需从高到低分别滑动比较两个二元组的指数，copy指数更大的；若两个指数相同，则相加即可。
3. **链表结构存储非零项**
  - 链表中每个**结点**存储多项式中的一个非零项，包括系数和指数两个数据域和一个指针域。
  
  | coef | expon | link |
  | ---- | ----- | ---- |

  定义一个结构体：
```cpp
typedef struct PolyNode *Polynominal;
struct PolyNode{
    int coef;
    int expon;
    Polynominal link;
};
```
#### 线性表，List
> 由同类型**数据元素**构成**有序序列**的线性结构
- 表中元素个数称为其长度
- 没有元素时，成为空表
- 起始位置是表头，结束位置是表尾
  
#### 线性表的抽象数据类型描述
- 类型名称：线性表（List）
- 数据对象集：线性表是n（>=0）个元素构成的有序序列
- 操作集：对于L $\in$ List,整数i表示位置，元素X $\in$ ElementType，线性表基本操作如下：
    1. List MakeEmpty():返回一个初始化的空线性表
    2. ElementType FindKth(int K, List L):返回第K位元素
    3. int Find(ElementType X, List L):在L中查找X**第一次**出现的位置
    4. void Insert(ElementType X, int i, List L):在第i位前插入X
    5. void Delete(int i, List L):删除第i位元素
    6. int Length(List L):返回长度
   
#### 线性表的顺序存储实现
```cpp
typedef struct LNode *List;
struct LNode{
    //这里定义一个数组Data，最大长度是MAXSIZE，元素类型是ElementType
    ElementType Data[MAXSIZE]
    //我们还要知道这个数组的末端在哪里噢
    int Last;
};
struct LNode L;//构建了一个结构体L
List PtrL;//构建了一个结构体指针PtrL

//若要访问下标为i的元素，下面两种方法都可以：
L.Data[i];//或者
PtrL->Data[i];
//若要访问线性表的长度，则：
L.Last+1;//或者
Ptrl->Last+1;
```

结构体和结构体指针的理解和使用：
https://blog.csdn.net/weixin_36209467/article/details/82261286

#### 线性表顺序存储的主要操作的实现

（这里为什么要用结构体指针呢？可能是因为使用结构体进行操作的时候，比如作为函数参数，就要**复制**整个结构体，占内存？如果用指针，只需要复制4个字节（指针长度）即可，节省性能？）
1. 初始化
   ```cpp
    List MakeEmpty()
    {
        List PtrL;
        PtrL = (List)malloc(sizeof(struct LNode));//分配空间
        PtrL->Last = -1;
        return PtrL;
    }
   ```
2. 查找
   ```cpp
    int Find(ElementType X, List PtrL)
    {
        int i = 0;
        while(i<=PtrL->Last&&Ptrl->Data[i]!=X)
            i++;
        if(i>PtrL->Last) return -1;//如果没找到
        else 
        return i;
    }
   ```
3. 插入
   ```cpp
    //要先把每个元素往后挪
    void Insert(ElementType X, int i, List PrtL)
    {
        int j;
        if(PtrL->Last == MAXSIZE-1){
            printf("表已经满了");
            return;
        }
        if(i<1||i>PtrL->Last+2){
            printf("位置不合法");
            return;
        }
        for(j=PtrL->Last;j>=i-1;j++)
            PtrL->Data[j+1]=PtrL->Data[j];
        PtrL->Data[i-1] = X;
        PtrL->Last++;
        return;
    }
   ```
4. 删除
   ```cpp
    void Delete(int i, List PtrL)
    {
        inr j;
        if(i<1||i>PtrL->Last+1){
            printf("元素不存在");
            return;
        }
        for(j=i;j<=PtrL->Last;j++)
            PtrL->Data[j-1]=PtrL->Data[j];
        PtrL->Last--;
        return;
    }
   ```
#### 线性表链式存储的实现
> 只需逻辑上相邻，不需物理上相邻。通过**链**建立起数据元素之间的逻辑关系。
> 因此，插入或删除不需要移动元素，只需要修改**链**即可。

| $\sigma_1$ | -> | $\sigma_2$ | -> |
| --- | --- | --- | --- |

**链表定义**
```cpp
typedef struct LNode *List;
struct LNode{
    ElementType Data;
    List Next
};
struct LNode L;
List PtrL;
```

#### 链表主要操作的实现
1. 求表长
   ```cpp
   int Length(List PtrL)
   {
       List p = PtrL; //p指向链表的第一个结点
       int j = 0;
       while(p){
           //对链表进行一次遍历，当遍历到最后一个元素时，指针p=NULL，就会终止循环。
           p = p->Next;
           j++;
       }
       return j++;
   }
   ```
2. 查找
   ```cpp
   //1.按序号查找
   List FindKth(int K, List PtrL)
   {
       List p = PtrL;
       int i = 1;
       while(p !=NUll&& i<K){
           p = p->Next;//指向链表下一个
           i++;
       }
       if(i == K)
            return p;
       else return NULL;//找到第K个的指针值，返回p，否则返回空。
   }

   //2.按值查找
   List Find(ElementType X, List PtrL)
   {
       List p = PtrL;
       while(p!=NULL && p->Data!=X){
           p=p->Next;
       }
       return p;
   }
   ```
3. 插入
   ```cpp
   /*
   1.先构造一个新节点，用s指向
   2.再找到第i-1个结点，用p指向
   3.s->Next = p->Next
   4.p->Next = s
   */
   List Insert(ElementType X, int i, = List PtrL)
   {
       List p,s;
       if(i == 1){
           //新节点若插在表头，要单独考虑
           s = (List)malloc(sizeof(struct LNode));//先分配空间
           s->Data = X;
           s->Next = PtrL;//插在表头
           return s;//同时，返回新表的表头（已经变了）
       }
       p = FindKth(i-1, PtrL);//找到指向这个序号所对应结点的指针值
       if(p = NULL){
           printf("argument error.");
           return NULL;
       }
       else{
           s = (List)malloc(sizeof(struct LNode));
           s->Data = X;
           s->Next = p->Next;
           p->Next = s;
           return PtrL;//表头位置不变
       }
   }
   ```
4. 删除
   ```cpp
   /*
   1.若要删除第i个结点，先p指向第i-1个结点
   2.然后s指向第i个结点
   3.修改指针，p->Next=s->Next,第i个元素被丢出链表
   4.free(s),释放空间
   */
   List Delete(int i,List PtrL)
   {
       List p,s;
       if(i==1){
           //单独处理删除表头的情况
           s = PtrL;
           if(PtrL!=NULL){
               PtrL = PtrL->Next;
           }else return NULL;
           free(s);
           return PtrL;
       }
       p = FindKth(i-1,PtrL);
       if(p = NULL){
           printf("argument error:%dth",i-1);
           return NULL;
       }else if(p->Next == NULL){
           printf("argument error:%dth",i);
           return NULL
       }else{
           s = p->Next;
           p->Next = s->Next;
           free(s);
           return PtrL;
       }
   }
   ```
#### 广义表
广义表中，元素不仅可以是单元素也可以是另一个广义表。

定义：

```cpp
typedef struct GNode *GList;
struct GNode{
    int Tag; //需要一个标签来标注这个结点是单元素(0)还是广义表(1)
    union{ //cpp用union来组合不同类型的数据，这个数中可以是不一样的类型的数据
    ElementType Data;//既可以是这种
    GList SubList;//也可是这种指针
    }URegion；
    GList Next;//指向后续结点
}；
```

#### 多重链表
链表中的结点可能同时隶属于多个链。
- 多重链表中节点的指针域会有多个，如上一例中包含了Next和SubList两个指针。
- 然而，包含两个指针域并不一定就是多重链表，比如双向链表。

树结构，图结构可以用多重链表来实现存储。

举例：可以使用**十字链表**来存储稀疏矩阵：
- 只存储非零项，其中数据域包括：行R，列C，值V；
- 通过向右指针Right和向下指针Down来把这些结点串起来。

### 堆栈

xxxxxxxx


