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

# Notebook for DataStructure
---
[数据结构复习题](https://blog.csdn.net/weixin_43751983/article/details/103411097)
## 第一讲 基本概念



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
$$f(i,j) = max(0,\Sigma_{k=i}^jA_k)$$

对于O(n<sup>2</sup>)的算法，应考虑是否能化成O(nlogn)

**分治法**，分而治之
> 把一个大而复杂的问题切分成小的块，并分别解决他们，最后再进行综合。

---
## 第二讲 线性结构
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
   1.先构造一个新结点，用s指向
   2.再找到第i-1个结点，用p指向
   3.s->Next = p->Next
   4.p->Next = s
   */
   List Insert(ElementType X, int i, = List PtrL)
   {
       List p,s;
       if(i == 1){
           //新结点若插在表头，要单独考虑
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
- 多重链表中结点的指针域会有多个，如上一例中包含了Next和SubList两个指针。
- 然而，包含两个指针域并不一定就是多重链表，比如双向链表。

树结构，图结构可以用多重链表来实现存储。

举例：可以使用**十字链表**来存储稀疏矩阵：
- 只存储非零项，其中数据域包括：行R，列C，值V；
- 通过向右指针Right和向下指针Down来把这些结点串起来。

### 堆栈

后入先出

#### 栈的顺序存储实现
通过一个一维数组来实现

```cpp
#define MaxSize <12>
typedef struch SNode *Stack;
struct SNode{
    ElementType Data[MaxSize];
    int Top;
};
```
1. 入栈
   ```cpp
   void Push(Stack PtrS, ElemenType item)
   {
       if(PtrS->Top == MaxSize-1){
           printf("Stack full");
           return
       }else{
           PtrS->Data[++(PtrS->Top)] = item; // topp++
           return;
       }
   }
   ```
2. 出栈
   ```cpp
   ElementType Pop(Stack PtrS)
   {
       if(Ptrs->Top == -1){
           printf("stack empty");
           return ERROR; //ERROR是ElementType的特殊值，标志错误！
       }else{
           return(PtrS->Data[(Ptrs->Top)--]);
       }
   }
   ```
#### 栈的链式存储实现
注意：链表尾端无法作为栈顶，因为不能对尾端进行删除操作。
```cpp
typedef struct SNode *Stack;
struct SNode{
    ElementType Data;
    struct SNode *Next;
}
```

1. 构建一个空堆栈：
   ```cpp
    Stack CreateStack()
    { //构建一个堆栈的头结点，返回指针
        Stack S;
        S = (Stack)malloc(sizeof(struct SNode)); //这里也可以用new(c++)
        S = new SNode();
        S->Next = NULL;
        return S;
    }

    int IsEmpty(Stack S)
    {
        return (S->Next == NULL); //链式堆栈只能在链表头插入删除，因此如果后面是NULL则表示这个链表(堆栈)空了
    } 
    ```
    Tips：malloc和new的区别：
    - malloc/free是标准库函数，new/delete是C++运算符
    - new/delete会调用构造、析构函数，malloc/free不会，所以他们无法满足动态对象的要求。
    - new返回有类型的指针，malloc返回无类型的指针。malloc内存分配成功后返回void*，然后再强制类型转换为需要的类型；new操作符分配内存成功后返回与对象类型相匹配的指针类型；因此new是符合类型安全的操作符。
    - malloc内存分配失败后返回NULL；new分配内存失败则会抛异常（bac_alloc）。
    - reference: https://blog.csdn.net/qq_40840459/article/details/81268252


2. 插入
    ```cpp
    bool Push( Stack S, ElementType X )
    { /* 将元素X压入堆栈S */
        PtrToSNode TmpCell;

        TmpCell = (PtrToSNode)malloc(sizeof(struct SNode));
        TmpCell->Data = X;
        TmpCell->Next = S->Next;
        S->Next = TmpCell;
        return true;
    }
    ```
3. 删除
    ```cpp
    ElementType Pop( Stack S )  
    { /* 删除并返回堆栈S的栈顶元素 */
        PtrToSNode FirstCell;
        ElementType TopElem;

        if( IsEmpty(S) ) {
            printf("堆栈空"); 
            return ERROR;
        }
        else {
            FirstCell = S->Next; 
            TopElem = FirstCell->Data;
            S->Next = FirstCell->Next;
            free(FirstCell);
            return TopElem;
        }
    }
    ```
#### 表达式求值
详见浙大数据结构ppt
1. 中缀表达式利用堆栈转换为后缀表达式
    - 运算数：直接输出；
    - 左括号：压入堆栈；
    - 右括号：将栈顶的运算符弹出并输出，直到遇到左括号（出栈，不输出）；
    - 运算符：
        - 若优先级大于栈顶运算符时，则把它压栈；
        - 若优先级小于等于栈顶运算符时，将栈顶运算符弹出并输出；再比较新的栈顶运算符，直到该运算符大于栈顶运算符优先级为止，然后将该运算符压栈；
    - 若各对象处理完毕，则把堆栈中存留的运算符一并输出。

2. 利用堆栈求后缀表达式的值
    - 字符序列的后缀表达式 -> 分割 -> 对象序列的后缀表达式 -> 利用堆栈求值
    - 6 2 / 3 - 4 2 * + =  8:
    - 6/2=3,3-3=0,4*2=8,0+8=8


### 队列

是一种具有操作约束的线性表。<br>
只能在后端(rear)插入，前端(front)删除，因此是先进先出（FIFO）。
| data(front) | data | data(rear) | -> |
| --- | --- | --- | --- |

定义：
```cpp
typedef int Position;
struct QNode {
    ElementType *Data;     /* 存储元素的数组 */
    Position Front, Rear;  /* 队列的头、尾指针 */
    int MaxSize;           /* 队列最大容量 */
};
typedef struct QNode *Queue;
```

#### 队列的顺序存储实现
循环队列，善用取余操作
```cpp
Queue CreateQueue( int MaxSize )
{
    Queue Q = (Queue)malloc(sizeof(struct QNode));
    Q->Data = (ElementType *)malloc(MaxSize * sizeof(ElementType));
    Q->Front = Q->Rear = 0;
    Q->MaxSize = MaxSize;
    return Q;
}

bool IsFull( Queue Q )
{
    return ((Q->Rear+1)%(Q->MaxSize) == Q->Front);
}

bool AddQ( Queue Q, ElementType X )
{
    if ( IsFull(Q) ) {
        printf("队列满");
        return false;
    }
    else {
        Q->Rear = (Q->Rear+1)%(Q->MaxSize); 
        Q->Data[Q->Rear] = X;
        return true;
    }
}

bool IsEmpty( Queue Q )
{
    return (Q->Front == Q->Rear);
}

ElementType DeleteQ( Queue Q )
{
    if ( IsEmpty(Q) ) { 
        printf("队列空");
        return ERROR;
    }
    else  {
        Q->Front =(Q->Front+1)%Q->MaxSize;
        return  Q->Data[Q->Front];
    }
}
```
#### 队列的链式存储实现
注意，front只能是链表头，rear只能是链表尾，因为只有链表头可以删除结点

```cpp

typedef struct Node *PtrToNode;
struct Node { /* 队列中的结点 */
    ElementType Data;
    PtrToNode Next;
};
typedef PtrToNode Position;

struct QNode {
    Position Front, Rear;  /* 队列的头、尾指针 */
    int MaxSize;           /* 队列最大容量 */
};
typedef struct QNode *Queue;

bool IsEmpty( Queue Q )
{
    return ( Q->Front == NULL);
}

ElementType DeleteQ( Queue Q )
{
    Position FrontCell; 
    ElementType FrontElem;
    
    if  ( IsEmpty(Q) ) {
        printf("队列空");
        return ERROR;
    }
    else {
        FrontCell = Q->Front;
        if ( Q->Front == Q->Rear ) /* 若队列只有一个元素 */
            Q->Front = Q->Rear = NULL; /* 删除后队列置为空 */
        else                     
            Q->Front = Q->Front->Next;
        FrontElem = FrontCell->Data;

        free( FrontCell );  /* 释放被删除结点空间  */
        return  FrontElem;
    }
}

```
### 多项式加法运算

```cpp
Polynomial PolyAdd (Polynomial P1, Polynomial P2)
{
    Polynomial front, rear, temp;
    int sum;
    rear = (Polynomial) malloc(sizeof(struct PolyNode)); 
    front = rear; /* 由front 记录结果多项式链表头结点 */
    while ( P1 && P2 ) /* 当两个多项式都有非零项待处理时 */
        switch ( Compare(P1->expon, P2->expon) ) {
        case 1: 
            Attach( P1->coef, P1->expon, &rear);
            P1 = P1->link;
            break;
        case -1: 
            Attach(P2->coef, P2->expon, &rear); 
            P2 = P2->link;
            break;
        case 0: 
            sum = P1->coef + P2->coef;
            if ( sum ) Attach(sum, P1->expon, &rear);
            P1 = P1->link; 
            P2 = P2->link;
            break;
        }
    /* 将未处理完的另一个多项式的所有结点依次复制到结果多项式中去 */
    for ( ; P1; P1 = P1->link ) Attach(P1->coef, P1->expon, &rear);
    for ( ; P2; P2 = P2->link ) Attach(P2->coef, P2->expon, &rear);
    rear->link = NULL; 
    temp = front;
    front = front->link; /*令front指向结果多项式第一个非零项 */
    free(temp); /* 释放临时空表头结点 */
    return front;
```

## 第三讲 树（上）
###  树与树的表示

为什么要用树？<br>
因分层次的组织在管理上具有更高的效率。<br>
用树可以实现有效率的查找

#### 查找
查找分为**动态查找**和**静态查找**

- 静态查找
  - 集合中记录是固定的
  - 没有插入和删除操作
  - 方法：
    - 顺序查找。按顺序查找，平均找n/2次，复杂度O(n)
    - 二分查找。
      -  假设n个数据元素的关键字满足有序（比如：从小到大）并且是连续存放的（数组），那么可以进行二分查找
      -  二分查找流程(假设数据从小到大排列)：
         -  设定初始左右边界(left,right)
         -  迭代入口>>>while(left<=right)
         -  计算中间坐标(mid)对应元素的值
         -  若中间元素比给定值大，调整右边边界等于中间坐标-1，right=mid-1，并进行下一轮迭代
         -  若中间元素比给定值小，调整左边边界等于中间坐标+left=mid+1，并进行下一轮迭代
         -  若中间元素等于给定值，return中间元素的下标，寻找成功
         -  <<<若跳出while循环，代表左右边界错位，不存在待查早数据，return-1
      -  时间复杂度是O(logN)，事先进行有序化排序是效率提升的关键
      -  实际上这种二分查找流程就构成了一个树
- 动态查找
  - 集合中记录是动态变化的
  - 除了查找还可能发现插入和删除
  
技巧：在边界设定“哨兵”（sentinel），这样可以规避edge case，避免进行麻烦的边界情况的判断。参考[算法笔记](algorithmNotes.md)中的82. Remove Duplicates from Sorted List II

#### 树的基本概念
树(Tree) :n (n $\geq$ 0)个结点构成的有限集合。<br>
当n=0时，称为空树; <br>
对于任一棵非空树(n>0)，它具备以下性质:
- 树中有一个称为“根(Root) ”的特殊结点，用r表示;
- 其余结点可分为m(m>0)个**互不相交**的有限集T，T2, ...，Tm，其中每个集合本身又是一棵树，称为原来树的“子树(SubTree)”。<br>
  **子树是不相交的**
- 除了根结点外，**每个结点有且仅有一个父结点**;一棵N个结点的树有N-1条边，因此**树是保证链接数最小的结点连接方式**


关于树的基本术语：
- 结点的度(Degree)：结点的子树个数
- 树的度：树的所有结点中最大的度数
- 叶结点(Leaf)：度为0的结点
- 父结点(Parent)：有子树的结点是其子树的根结点的父结点
- 子结点(Child)：若A是B的父结点，则B是A的子结点
- 兄弟结点(Sibling)：具有同一父结点的各结点之间是兄弟结点
- 路径和路径长度：从结点n1到nk的路径为一个结点序列n1,n2,…,nk, ni是ni+1的父结点。路径所包含边的个数为路径的长度。
- 祖先结点(Ancestor)：沿树根到某一结点路径上的所有结点都是这个结点的祖先结点。
- 子孙结点(Descendant)：某一结点的子树中的所有结点是这个结点的子孙。
- 结点的层次（Level）：规定根结点在1层，其它任一结点的层数是其父结点的层数加1。
- 树的深度（Depth）：树中所有结点中的最大层次是这棵树的深度

#### 树的表示
树难以用数组表示，也不太好用链表表示(每个Node结构不同，因为next指针域的数量不同)

可以使用儿子-兄弟表示法：
这样每一个链表结点的构造都是相同的。<br>
这种树结构称为二叉树。

![](../pics/child_sibling_1.png)

![](../pics/child_sibling_2.png)



### 二叉树及存储结构

#### 几种特殊二叉树
1. 斜二叉树(实际上就是链表)
   ![](../pics/3.2_1.png)
2. 完美/满二叉树
   ![](../pics/3.2_2.png)
3. **完全二叉树**
   完全二叉树可以不是完美二叉树，可以缺结点，但是必须是按照一定规则（从上到下，从左到右）排序之后的尾端的连续结点序列。<br>
   比如，下面这个是完全二叉树：<br>
   ![](../pics/3.2_3.png)
   而下面这个不是：<br>
   ![](../pics/3.2_4.png)
   完全二叉树是很重要的一种树。

#### 二叉树的重要性质
- 一个二叉树第i层的最大结点数为：2<sup>i-1</sup>, i $\geq$ 1
- 深度为k的二叉树有最大结点总数为2<sup>k</sup>-1, k $\geq$ 1
- 对任何非空二叉树T，若n<sub>0</sub>表示叶结点的个数,n<sub>2</sub>是度为2的非叶结点个数，那么两者满足关系n<sub>0</sub>=n<sub>2</sub>+1。

#### 二叉树顺序存储结构
对于完全二叉树（需按照上面的规则编号），可以用数组实现，而且非常方便：
- 非根结点(序号i>1)的父结点的序号是int(i/2)（向下取整—）;
- 结点(序号为i)的左孩子结点的序号是2i,(若2i<=n，否则没有左孩子) ;
- 结点(序号为i )的右孩子结点的序号是2i+1,(若2i+1<=n， 否则没有右孩子);
一般二叉树也可以这样处理，但是在没有值的位置需要把对应的位置空出来。这样会造成空间浪费

#### 二叉树链式存储结构
| Left | Data | Right |
| -- | -- | -- |

```cpp
typedef struct TreeNode *BinTree;
typedef BinTree Position;
struct TreeNode{
    ElementType Data;
    BinTree Left;
    BinTree Right;
}
```

### 二叉树的遍历
#### **二叉树的递归遍历**
1. 先序遍历
   ![](../pics/3.3_1.png)
   1. 访问根结点
   2. 先序遍历其左子树
   3. 先序遍历其右子树
   ```cpp
   void PreOrderTraversal(BinTree BT)
   {
       if(BT){
           printf("%d", BT->Data);
           PreOrderTraversal(BT->Left);
           PreOrderTraversal(BT->Right);
       }
   }
   ```

2. 中序遍历
   ![](../pics/3.3_2.png)
   1. 中序遍历其左子树
   2. 访问根结点
   3. 中序遍历其右子树
   ```cpp
   void InOrderTraversal(BinTree BT)
   {
       if(BT){
           InOrderTraversal(BT->Left);
           printf("%d", BT->Data);
           InOrderTraversal(BT->Right);
       }
   }
   ```
3. 后序遍历
   ![](../pics/3.3_3.png)
   1. 后序遍历其左子树
   2. 后序遍历其右子树
   3. 访问根结点
   ```cpp
   void PostOrderTraversal(BinTree BT)
   {
       if(BT){
           PostOrderTraversal(BT->Left);
           PostOrderTraversal(BT->Right);
           printf("%d", BT->Data);
       }
   }
   ```
#### 层序遍历
**二叉树遍历的核心问题：二维结构的线性化**
![](../pics/3.3_4.png)
因此需要一个存储结构保存暂时不访问的结点（堆栈或者队列），否则扫描经过之后就无法访问了。

队列实现:遍历从根结点开始，首先将根结点入队，然后开始执行循环:1.结点出队、2.访问该结点、3.其左右儿子入队.

举例：对于下面的树：
![](../pics/3.3_5.png)<br>
有：<br>
| A    | out: <br>
| B C  | out:A<br>
| C D F | out:AB<br>
| D F G I| out:ABC<br>
| F G I E | out:ABCD<br>
| G I E H | out:ABCDF<br>
out: ABCDFGIEH<br>

代码：
```cpp
void LevelOrderTraversal ( BinTree BT )
{ Queue Q; BinTree T;
    if ( !BT ) return; /* 若是空树则直接返回 */
    Q = CreatQueue( MaxSize ); /*创建并初始化队列Q*/
    AddQ( Q, BT );
    while ( !IsEmptyQ( Q ) ) {
        T = DeleteQ( Q );
        printf(“%d\n”, T->Data); /*访问取出队列的结点*/
        if ( T->Left ) AddQ( Q, T->Left );
        if ( T->Right ) AddQ( Q, T->Right );
    }
}

```
#### 一些应用
1. 输出二叉树中的叶子结点
   只需要在二叉树遍历算法中增加检测结点的“左右子树是都否为空”即可。
   ```cpp
   void PreOrderPrintLeaves( BinTree BT )
    {
        if( BT ) {
            if ( !BT-Left && !BT->Right )
                printf(“%d”, BT->Data );
            PreOrderPrintLeaves ( BT->Left );
            PreOrderPrintLeaves ( BT->Right );
        }
    }
   ```
2. 求二叉树的高度
   假设左子树高度为HL，右子树高度为HR，则<br>
   Height=max(HL, HR)+1（根结点）
   ```cpp
   int PostOrderGetHeight( BinTree BT )
    {   int HL, HR, MaxH;
        if( BT ) {
            HL = PostOrderGetHeight(BT->Left); /*求左子树的深度*/
            HR = PostOrderGetHeight(BT->Right); /*求右子树的深度*/
            MaxH = （HL > HR）? HL : HR; /*取左右子树较大的深度*/
            return ( MaxH + 1 ); /*返回树的深度*/
        }
        else return 0; /* 空树深度为0 */
    }
   ```
3. 通过中序遍历+前序/后序遍历，唯一确定一颗二叉树

#### 判断树的同构
运用链表的思想，构造结构数组来表示下面的树：<br>
![](../pics/3.3_6.png)<br>
```cpp
// 来学习一下怎么构造结构数组吧！
#define MaxTree 10
#define ElementType char
#define Tree int
#define Null -1

struct TreeNode
{
    ElementType Element;
    Tree Left;
    Tree Right;
}T1[MaxTree], T2[MaxTree]
```
需要完成：1.读入两个二叉树；2.二叉树同构判别<br>
输入样例：
```
### 输入样例:
8
A 1 2
B 3 4
C 5 -
D - -
E 6 -
G 7 -
F - -
H - -
8
G - 4
B 7 6
F - -
A 5 1
H - -
C 0 -
D - -
E 2 -
```
结构：
```cpp
int main()
{
    Tree R1, R2;
    R1 = BuildTree(T1); // T1，T2就是上面的结构数组，返回的是根结点的index
    R2 = BuildTree(T2);
    if(Isomorphic(R1,R2)){
        printf("Yes\n");
    }else{
        printf("No\n");
    }
    return 0;
}

Tree BuildTree( struct TreeNode T[] )
{...
    scanf("%d\n", &N);
    if (N) {
        for (i=0; i<N; i++) check[i] = 0;
        for (i=0; i<N; i++) {
            scanf("%c %c %c\n", &T[i].Element, &cl, &cr);
            if (cl != '-') {
                T[i].Left = cl-'0';
                check[T[i].Left] = 1; //原理是，如果一个元素没有指向被指向，则它就是根结点
            }
            else T[i].Left = Null;
            ... /*对cr的对应处理 */
        }
        for (i=0; i<N; i++)
            if (!check[i]) break;
        Root = i;
    }
    return Root;
}

int Isomorphic ( Tree R1, Tree R2 )
{
    if ( (R1==Null )&& (R2==Null) ) /* both empty */
        return 1;
    if ( ((R1==Null)&&(R2!=Null)) || ((R1!=Null)&&(R2==Null)) )
        return 0; /* one of them is empty */
    if ( T1[R1].Element != T2[R2].Element )
        return 0; /* roots are different */
    if ( ( T1[R1].Left == Null )&&( T2[R2].Left == Null ) )
        /* both have no left subtree */
        return Isomorphic( T1[R1].Right, T2[R2].Right );
    if ( ((T1[R1].Left!=Null)&&(T2[R2].Left!=Null))&&
        ((T1[T1[R1].Left].Element)==(T2[T2[R2].Left].Element)) )
        /* no need to swap the left and the right */
        return ( Isomorphic( T1[R1].Left, T2[R2].Left ) &&
        Isomorphic( T1[R1].Right, T2[R2].Right ) );

    else /* need to swap the left and the right */
        return ( Isomorphic( T1[R1].Left, T2[R2].Right) &&
        Isomorphic( T1[R1].Right, T2[R2].Left ) );
}
```



## 第四讲 树（中）

### 二叉搜索树(BST, Binary Search Tree)
**也成为二叉排序树和二叉查找树**

满足以下性质：
1. 非空左子树的所有键值小于其根结点的键值 （运用二分查找的思想，需要先排序）
2. 非空右子树的所有键值大于于其根结点的键值
3. 左、右子树都是二叉搜索树

![](../pics/4.1_1.png)

#### 查找操作：Find
- 查找从根结点开始，如果树为空，返回NULL
- 若搜索树非空，则根结点关键字和X进行比较，并进行不同处理:
  - 若X小于根结点键值，只需在左子树中继续搜索;
  - 如果X大于根结点的键值，在右子树中进行继续搜索;
  - 若两者比较结果是相等，搜索完成，返回指向此结点的指针。
  ```cpp
  1.使用尾递归的方法：
  Position Find(ElementType X, BinTree BST)
  {
      if(!BST) return NULL; //failure
      if(X>BST->Data){
          return Find(X, BST->Right); //在右子树中继续查找
      }else if(X<BST->Data){
          return Find(X, BST->Left);
      }else //X == Bst->Data{
          return BST; //查找成功，返回找到的结点的位置
      }
  }

  2.使用迭代循环的方法（推荐）：
  Position IterFind(ElementType X, BinTree BST)
  {
      while(BST){
          if(X>BST->Data){
              BST = BST->Right; //向右移动
          }else if(X<BST->Data){
              BST = BST->Left; //向左移动
          }else{
              return BST；
          }
      }return NULL;
  }
  ```
  - 若树往一边倒，比如全都只有左儿子，或者都只有右儿子，那算法效率就很低，达不到二分的log2n。因此需要平衡二叉树

#### 查找最值操作：FindMin，FindMax2
FindMin:最小值肯定在最左边
```cpp
Position FindMin(BinTree BST)
{
    // 递归方法
    if(!BST)
        return NULL;
    else if(!BST->Left)
        return BST; //找到最左边结点并返回
    else
        return FindMin(BST->Left);
}
```
FindMin:最大值肯定在最右边
```cpp
Position FindMax(BinTree BST)
{
    // 迭代方法
    if(BST){
        while(BST->Right){
            BST = BST->Right;
        }
    }return BST;
}
```
#### 插入操作
找到一个数的排序位置，并将其作为叶结点插入
```cpp
BinTree Insert(ElementType X, BinTree BST)
{
    if(!BST){
        // 若原树为空，则生成并返回一个结点的二叉搜索树
        BST = malloc(sizeof(struct TreeNode))
        BST->Data = X;
        BST->Left = BST->Right = NULL;
    }else{ //开始找插入的位置
        if(X<BST->Data)
            BST->Left = Insert(X, BST->Left);
        else if(X>BST->Data)
            BST->Right = Insert(X,BST->Right);
    }return BST;
}
```
#### 删除操作
三种情况：
1. 要删除的是叶结点：直接删除，再修改其父结点指针为NULL
2. 要删除的结点**只有一个孩子结点**：将其父结点的指针指向要删除结点的孩子结点即可，也就是直接把删除结点的子结点挂在删除结点的父结点的对应位置
3. 要删除的结点有左、右、两棵子树：用另一结点替代被删除结点--右子树的最小元素，或者左子树的最大元素（因为左子树的最大值和右子树的最小值一定不会有两个儿子，这样只需参照1和2修改即可）

```cpp
BinTree Delete( BinTree BST, ElementType X ) 
{ 
    Position Tmp; 

    if( !BST ) 
        printf("要删除的元素未找到"); 
    else {
        if( X < BST->Data ) 
            BST->Left = Delete( BST->Left, X );   /* 从左子树递归删除 */
        else if( X > BST->Data ) 
            BST->Right = Delete( BST->Right, X ); /* 从右子树递归删除 */
        else { /* BST就是要删除的结点 */
            /* 如果被删除结点有左右两个子结点 */ 
            if( BST->Left && BST->Right ) {
                /* 从右子树中找最小的元素填充删除结点，也可以左子树找最大元素 */
                Tmp = FindMin( BST->Right );
                BST->Data = Tmp->Data; //用右子树中最小元素替换被删除结点
                /* 从右子树中删除最小元素 */
                BST->Right = Delete( BST->Right, BST->Data );
            }
            else { /* 被删除结点有一个或无子结点 */
                Tmp = BST; 
                if( !BST->Left )       /* 只有右孩子或无子结点 */
                    BST = BST->Right;  //把当前结点的下一个结点桥接上来，等于删除当前结点 
                else                   /* 只有左孩子 */
                    BST = BST->Left;
                free( Tmp );
            }
        }
    }
    return BST;
}
```

### 平衡二叉树
搜索树结点不同的插入次序，将导致不同的**深度**和**平均查找长度**ASL

平衡因子（BF）：左子树和右子树的高度差<br>
BF(T) = h<sub>L</sub> - h<sub>R</sub>

平衡二叉树(Balanced Binary Tree), 或AVL树（AVL是提出的科学家的名字缩写）：<br>
- 空树，或者;
- **任 一 结 点**左、右子树的高度差绝对值不超过1，即|BF(T)| $\leq$  1<br>
  ![](../pics/4.2_1.png)
- 给定结点数为n的AVL树的最大高度为O(log<sub>2</sub>n)
  
#### 平衡二叉树的调整

分为四种情况
1. RR插入，RR旋转
   ![](../pics/4.2_2.png)
2. LL插入，LL选择
   ![](../pics/4.2_3.png)
3. LR插入，LR选择
   ![](../pics/4.2_4.png)
4. RL插入，RL旋转
   ![](../pics/4.2_5.png)

#### 如何判断是同一颗二叉搜索树

给定一个插入序列就可以唯一确定一棵二叉搜索树，但反之则不然。<br>
尝试根据输入的队列来确定是否能生成一样的二叉搜索树

1. 搜索树的表示
   ```cpp
   typedef struct TreeNode *Tree;
   struct TreeNode{
       int v;
       Tree Left, Right;
       int flag; // 用来记录结点是否有被遍历到
   }
   ```
2. 程序主要框架：
   1. 读入N和L
   2. 根据第一个序列建树T
   3. 判别之后的L个序列是否能形成和T一样的树，并返回结果<br>
   构建部分：
   ```cpp
   int main()
   {
       int N,L,i;
       Tree T;

       scanf("%d", &N);
       while(N){
           scanf("%d",&L);
           T = MakeTree(N);
           for (i=0;i<L;i++){
               if(Judge(T,N)) printf("Yes\n");
               else printf("No\n");
               ResetT(T);
           }
           FreeTree(T);
           scanf("%d",&N);
       }
       return 0;
   }

   Tree MakeTree(int N)
   {
       Tree T;
       int i, V;

       scanf("%d",&V);
       T = NewNode(V);
       for(i=1;i<N;i++>){
           scanf("%d",&V);
           T = Insert(T,V);
       }
       return T;
   }

   Tree NewNode(int V)
   {
       Tree T = (Tree)malloc(sizeof(struct TreeNode));
       T->v = T;
       T->Left = T->Right = NULL;
       T->frag = 0;
       return T;
   }

   Tree Insert(Tree T, int V)
   {
       if(!T)
            T = NewNode(V);
       else{
           if(V>T->v)
                T->Right = Insert(T->Right, V);
           else
                T->Left = Insert(T->Left, V);
        }
        return T;
   }
   ```
3. **判别方法**
   - 在构建好的树T中按顺序搜索序列3 2 4 1中的每一个数：
   - **如果每次搜索所经过的结点在前面均出现过**，则一致；
   - 否则（某次搜索中遇到未经过，即flag=0的结点），则不一致
    ```cpp
    int check(Tree T, int V) // 单个元素的判别
    {
        if(T->flag){ // 目前遍历到的结点是之前有标记的，继续迭代
            if(V < T->v){
                return check(T->Left, V);
            }else if(V > T->v){
                return check(T->Right, V);
            }else
            return 0;
        }else{ 
            if(V == T->v){ // 碰到了新结点，康康值是否相等呢？
                T->flag = 1; // 是正确的结点，把他标记一下就return
                return 1;
            }
            else return 0; // 碰到了未经过的结点，判断出来不是同一棵树
        }
    }

    int Judge(Tree, int N) // 循环调用check来判断整个序列
    {
        int i, V, flag = 0; // flag=1:代表(可能序列还没检查完就已经)碰到不一致了
        scanf("%d", &V);
        if( V!=T->v) flag=1; // 根节点就不同了
        else T->flag = 1 // 注意区分flag和T的flag！
        for(i=1;i<N;i++）{
            scanf("%d", &V);
            if((!flag)&&(!check(T,V))) flag=1; // 如果check返回0，则flag=1
        }
        if( flag ) return 0;
        else return 1;
    }

    void ResetT(Tree T)
    {
        if (T->Left) Reset(T->Left);
        if (T->Right) Reset(T->Right);
        T->falg = 0;
    }

    void FreeTree(Tree T)
    {
        if(T->Left) FreeTree(T->Left);
        if(T->Right) FreeTree(T->Right;
        free(T))
    }
    ```

### （补充）红黑树
参考：https://zhuanlan.zhihu.com/p/143396578

为了解决二叉树多次插入结点后可能产生的不平衡现象，提出了红黑树。<br>
那么红黑树和平衡二叉树(AVL树)有什么区别呢？
- 红黑树放弃了追求完全平衡，追求大致平衡，在与平衡二叉树的时间复杂度相差不大的情况下，保证每次插入最多只需要三次旋转就能达到平衡，实现起来也更为简单。
- 平衡二叉树追求绝对平衡，条件比较苛刻，实现起来比较麻烦，每次插入新节点之后需要旋转的次数不能预知。

#### 红黑树的具体概念

**红黑树是一种自平衡的二叉查找树，它具有以下特点：**
1. **结点是红色或黑色。**
2. **根结点是黑色。**
3. **每个叶子结点都是黑色的空结点（NIL结点）。**
4. **每个红色结点的两个子结点都是黑色。(从每个叶子到根的所有路径上不能有两个连续的红色结点)**
5. **从任一结点到其每个叶子的所有路径都包含相同数目的黑色结点。**
![](../pics/BRT-1.png)

- 因为这五条规则的限制，红黑树能够保证自平衡，且**从根到叶子的最长路径不会超过最短路径的2倍。**
- 而ALVL树是严格平衡的二叉树，要求**每个节点的左右子树高度差不超过1**
- AVL树查找效率更高，需要频繁查找时，可以选用AVL树
- 频繁插入删除时，则可以使用红黑树

比如，这种情况插入之后就还是正常的：
![](../pics/BRT-2.png)

但是这种就需要做出调整了：
![](../pics/BRT-3.png)

调整的方法：
1. 变色
2. 旋转
   1. 左旋
   2. 右旋

##### 1. 变色
![](../pics/BRT-4.png)

但是凭空多出来一个黑色节点会打破规则5，因此需要进一步的调整。


##### 2.1. 左旋
父节点下沉，右孩子上浮，父节点的链接转移给右孩子；他们三的子节点继续按原顺序依次排列：
![](../pics/BRT-5.png)

##### 2.2. 右旋
也是同样地，父节点下沉，左孩子上浮，父节点的链接转移给右左孩子；他们三的子节点继续按原顺序依次排列：

就像下象棋一样，不同的局面对应着不同的搞法。而对于插入新结点来说，又可以归结为5种不同局面：

##### 局面1：新结点（A）位于树根，没有父结点。
![](../pics/BRT-6.png)
直接让新节点变成黑色即可，能够满足5条规则。

##### 局面2：新结点（B）的父结点是黑色。
![](../pics/BRT-7.png)
此时插入没有打破任何规则，不用做任何调整

##### 局面3：新结点（D）的父结点和叔叔结点都是红色。
![](../pics/BRT-8.png)
这种局面，两个红色结点B和D连续，违反了规则4。因此我们先让结点B变为黑色；但这样一来，结点B所在路径凭空多了一个黑色结点，打破了规则5。因此我们让结点A变为红色：
![](../pics/BRT-9.png)
这时候，结点A和C又成为了连续的红色结点，我们再让结点C变为黑色：
![](../pics/BRT-10.png)

即可。

##### 局面4：新结点（D）的父结点是红色，叔叔结点是黑色或者没有叔叔，且新结点是父结点的右孩子，父结点（B）是祖父结点的左孩子。
![](../pics/BRT-11.png)
做一次左旋，进入局面5.

##### 局面5：新结点（D）的父结点是红色，叔叔结点是黑色或者没有叔叔，且新结点是父结点的左孩子，父结点（B）是祖父结点的左孩子。
我们以结点A为轴，做一次右旋转，使得结点B成为祖父结点，结点A成为结点B的右孩子：
![](../pics/BRT-12.png)

接下来，我们让结点B变为黑色，结点A变为红色：
![](../pics/BRT-13.png)

即可。

如果局面4中的父结点B是右孩子，则成为了局面5的镜像，原本的右旋操作改为左旋；如果局面5中的父结点B是右孩子，则成为了局面4的镜像，原本的左旋操作改为右旋。

对于删除，也有三种不同的局面，这里就不再赘述



## 第五讲 树（下）

### 堆（Heap）
#### 什么是堆

**优先队列(Priority Queue)**: 特殊的“队列”，取出元素的顺序是依照元素的**优先权（关键字）**大小，而不是元素进入队列的先后顺序.

采用数组或链表实现优先队列，则他们的复杂度：
- 数组
  - 插入：元素总是插在尾部 -- O(1)
  - 删除：
    - 先找到最大、最小的关键字 -- O(n)
    - 数组删除还需要移动逐个元素 -- O(n)
- 链表
  - 插入：元素总是插在链表头部 -- O(1)
  - 删除：
    - 先找到最大、最小的关键字 -- O(n)
    - 链表删去结点不需逐个移动 -- O(1)
- 有序数组
  - 插入：
    - 找到合适的位置 -- O(n)或者O(log2n) (二分法？)
    - 移动元素并插入 -- O(n)
  - 删除：删去最后一个元素 -- O(1)
- 有序链表
  - 插入：
    - 找到合适位置 -- O(n)
      - 插入元素 -- O(1)
    - 删除：首个元素或最后的元素 -- O(1)

尝试使用二叉树来存储？

由于删除比插入更加复杂，因此可以把最大值放在树根，并且使得其他的根节点都是相对的子节点的最大值，并使用完全二叉树来构造，这样就构成了堆。<br>
堆的两个特性：
- 结构性：用数组表示的完全二叉树
- 有序性：任一结点的关键字是其子树所有节点的最大值或最小值
  - “最大堆（MaxHeap）”也成为“大顶堆”
  - “最小堆（MinHeap）”也成为“小顶堆”

堆的有序性在于它从上到下的任何一条路径都是单调的。

![](../pics/5.1_1.png)

#### 最大堆的实现
1. 最大堆的定义
```cpp
typedef struct HeapStruct *MaxHeap;
struct HeadStruct{
    ElementType *Elements;
    int Size;
    int Capacity;
};

MaxHeap Create (int MaxSize)
{
    MaxHeap H = malloc(sizeof(struct HeapStruct));
    H->Elements = malloc((MaxSize+1) * sizeof(ElementType)); //这里+1是因为下标为0处并不用来存放数据
    H->Size = 0;
    H->Capacity = MaxSize;
    H->Elements[0] = MaxData; // 定义‘哨兵’为大于堆中所有可能元素的值，便于以后更快操作。
    return H;
}
```
2. 最大堆的插入

假设需要在[6]的位置上插入一个新节点，根据最大堆的性质进行判断：若该结点值比它的上一个结点大，则他们需要交换，在这里58>31，交换一次；58>44，再交换一次，最后新插入的58被交换到了[1]的位置。

![](../pics/5.1_2.png)
```cpp
void Insert(MaxHeap H, ElementType item)
{   // O(logN)
    int i;
    if(IsFull(H)){
        printf("Heap is Full");
        return;
    }
    i = ++ H->Size; // i指向插入后堆中最后一个元素的位置
    for(; H->Elements[i/2]<item ; i/=2 ){
        H -> Elements[i] = H -> Elements[i/2]; //路径上碰到更小的元素将其逐个下移一位（如果item更大的话）
    }
    H -> Elements[i] = item; // item 插入移位后上面空出来的位置
    /*
    注意，如果比到树的第一个元素
    （比如说：将25 插入 1000[0,哨兵]->20[1]->15[2]->12[3]->2[4] 这棵树,)
    在0位置设定一个很大的数作为正式数据之外的哨兵，保证其比插入的元素大，这样可以保证插入的这个25不会越界，巧妙地规避edge case。
    */
}
```
3. 最大堆的删除

![](../pics/5.1_3.png)

假设要删除58(最大值，在根的位置)：
- 先把31移动到根的位置
- 然后找到31 的较大的孩子，交换位置
- 然后再往下迭代，交换位置

![](../pics/5.1_4.png)

```cpp
ElementType DeleteMax(MaxHeap H)
{
    //从最大堆H中取出键值最大的元素，并删除一个结点
    int Parent, Child;
    ElementType MaxIten, temp;
    if (IsEmpty(H)){
        print("MaxHeap is empty");
        return;
    }
    MaxItem = H->ELements[1]; // 取出根节点最大值
    // 然后，用最大堆中的最后一个元素从根节点开始向上过滤下层结点，也就是逐个比较，移位。
    temp = H->Elements[H->Size--] // 取出H的最后一个元素（要删除的），暂时储存，再让Size-1
    for(Parent=1; Parent*2 <= H->Size; Parent = Child){
        Child = Parent*2; //子节点位置,注意，父节点的两个子节点:i*2 and i*2+1
        if( (Child != H->Size) && (H->Element[Child] < H->Elements[Child+1]) ){
            Child +=1; //指向两个孩子中的较大者
        }
        if(temp >= H->Elements[Child]) break; // 移位完成
        else{
            H -> Elements[Parents] = H->Elements[Child] //更大的子节点上移
        }
    }
    H -> Elements[Parent] = temp;
    return MaxItem;
}
```
1. 最大堆的建立

建立最大堆是指将**给定的N个元素**按照最大堆的要求存放在一个一维数组中。

- 方法一：通过插入操作一个一个插入到初始为空的堆中去 -- O(nlongn)
- 方法二：在线性时间复杂度下建立最大堆：
  - 先将N个元素按顺序存入，先满足完全二叉树的结构特性
  - 再调节各节点的位置，以满足最大堆的有序特性
  
方法二的具体逻辑：<br>
在堆删除中，根节点的底下是两个最大堆，以此类推，往下每个结点的两边都是最大堆<br>
如果将其反过来，从最底层起，不断往上迭代地构筑最大堆，到根节点就是一个完整的最大堆了。

### 哈夫曼树与哈夫曼编码 Huffman

根据数据出现的频率来建立更具效率的匹配<br>
比如将100分制转化为5分制，可以score<60 ==grade 1; 60<=score<70 ==grade 2,以此类推，这样会形成如下一颗判定树：

![](../pics/5.2_1.png)

可以通过不同的方法构造搜索树，但不同搜索树的搜索效率又是不一样的（比如把频率最大的元素放在根节点）

#### 哈夫曼树的定义

**带权路径长度(WPL)**：设二叉树有n个叶子结点，每个叶子结点带有权值W<sub>k</sub>,从根结点到每个叶子结点的长度为l<sub>k</sub>，则每个叶子结点的带权路径长度之和就是:

$$\Sigma_{k=1}^{n} w_kl_k$$

**最优二叉树/哈夫曼树**：WPL最小的二叉树

例如下面的二叉树：
![](../pics/5.2_2.png)

WPL = 5x1+4x2+3x3+2x4+1x4 = 34

而下面这棵树：
![](../pics/5.2_3.png)

WPL = 1x3+2x3+3x2+4x2+5x2 = 33

#### 如何构造哈夫曼树

每次把**权值最小的两棵树合并**即可。（可以利用最小堆）
![](../pics/5.2_4.png)

```cpp
typedef struct TreeNode *HuffmanTree;
struct TreeNode{
    int Weight;
    HuffmanTree Left, Right;
}

HuffmanTree Huffman(MinHeap H)
{
    O(NlogN)
    int i; HuffmanTree T;
    BuildMinHeap(H); //将 H->Element[]按权值调整为最小堆
    for(i = 1; i; i< H->Size; i++){ //做H.Size - 1次的合并
        T = malloc(sizeof(struct TreeNode));
        T->Left = DeletMin(H); // 从最小堆中删除一个结点，作为新T的左子节点
        T->Right = DeletMin(H); // 从最小堆中删除一个结点，作为新T的右子节点
        T->Weight = T->Left->Weight+T->Right->Weight;

        Insert(H,T);
    }
    T = DeletMin(H);
    return T; //返回的是合并之后的树根
}
```

#### 哈夫曼树的特点
- **没有度为1的结点**
- n个叶子节点的哈夫曼树共有2n-1个结点
- 哈夫曼树的任意**非叶节点的左右子树交换后仍是哈夫曼树**;
- 对同一组权值{W<sub>1</sub>,.....,w<sub>n</sub>},存在不同构的两棵哈夫曼树

#### 哈夫曼编码

给定一段字符串，如何对字符进行编码，可以使得该字符串的编码存储空间最少?<br>
进行不等长编码，并避免其二义性。

前缀码(prefixcode):任何字符的编码都不是另一字符编码的前缀
- 可以无二义地解码

通过二叉树进行编码：
下图是两种二叉树的编码方式，第一种四个元素分别对应00，01，10，11，由于每个元素不是另外一个元素的前缀，因此没有二义性。但是第二个编码方式出现了某个元素在其他元素前面的情况，这就会产生二义性。<br>
因此，需要产生一棵二叉树，使得每个元素都在叶节点上，保证其不会出现前缀的情况。<br>

![](../pics/5.2_5.png)

生成一棵正常的二叉树之后，如果要让编码效率最高，实际上就等同于生成一颗哈夫曼树，而每条路径就代表它的编码。

![](../pics/5.2_6.png)

![](../pics/5.2_7.png)


#### 如何构造一个哈夫曼编码树

比如，要根据如下的字母和出现频数来构造一个最优的哈夫曼编码：
| a | e | i | s | t | sp | nl |
| - | - | - | - | - | - | - |
| 10|15 |12 |3 |4 |13 |1|

过程：不断找最小的来合并结点即可。

![](../pics/5.2_8.png)
![](../pics/5.2_9.png)
![](../pics/5.2_10.png)
![](../pics/5.2_11.png)
![](../pics/5.2_12.png)

最后得到：

![](../pics/5.2_13.png)

### 集合及运算，**并查集**
并查集运用：File Transfer<br>
https://www.icourse163.org/learn/ZJU-93001?tid=1464647442#/learn/content?type=detail&id=1243232243&cid=1266529407&replay=true


#### 集合的表示

- 集合运算：交、并、补、差，判定一个元素是否属于某一集合
- 并查集：集合并、查某元素属于什么集合
- 并查集问题中集合存储如何实现？

可以通过**树**来表示，树的每个结点代表一个集合元素


假设有三个整数集合：<br>
S1={1,2,4,7}<br>
S2={3,5,8}<br>
S3={6,9,10}<br>
他们可以表示成：
![](../pics/5.3.1.png)

这样一棵树，可以使用链表存储，但是使用数组存储更好。<br>
假设每个元素的类型描述为：
```cpp
typedef struct{
    ElementType Data;
    int Parent;
}SetType;
```
![](../pics/5.3.2.png)

如果没有父节点，则parent为-1，否则指向其父节点的下标

|index|Data|Parent|
|-|-|-|
|0|1|-1|
|1|2|0|
|2|3|-1|
|3|4|0|
|4|5|2|
|5|6|-1|
|6|7|0|
|7|8|2|
|8|9|5|
|9|10|5|

#### 集合运算
1. 查找某个元素所在的集合(用根节点表示)
   ```cpp
   int Find(SetType S[], ElementType X)
   {
       //在数组S中查找值为X的元素所属的集合
       //MaxSize是全局变量，为S的最大长度
       int i;
       for(i=0;i<MaxSize&&S[i].Data != X; i++>); //空转，直到找到X
       if(i>=NaxSize) return -1; //未找到，返回-1
       for( ;S[i].Parent>=0;i=S[i].Parent); //不断向上，直到找到根节点
       return i; //返回根节点在数组中的下标

   }
   ```
2. 集合的并操作
   分别找到X1和X2两个元素所在集合树的根节点，如果他们不同根，则将其中一个根节点的父节点指针设置成另一个根节点的数组下标
   ```cpp
   void Union(SetType S[], ElementType X1, ElementType X2)
   {
       int Root1, Root2;
       Root1 = Find(S, X1);
       Root2 = Find(S, X2);
       if(Root1 != Root2)   S[Root2].Parent = Root1;
   }
   ```
   为了改善合并以后的查找性能(防止树过高)，可使每一次合并都小的集合合并到大的集合中。但是要怎么表示每个集合中元素的个数（大小）呢？可以把根节点原来的Parent(-1)改成（负号+这个集合中元素的个数）

   ![](../pics/5.3.4.png)

## 图（上）
### 什么是图

图表示**多对多**的关系。因此，线性表和树都是图的一种特殊形式。<br>
包含：
- 一组顶点：用V(Vertex)表示顶点集合
- 一组边：用E(Edge)表示边的集合
  - 边是定点对：(v,w) $\in$ E，其中v，w $\in$ V
  - 有向边<v,w>表示从v指向w的单向边
  - 不考虑重边(两条互指的单向边)和自回路

#### 怎样表示一个图
1. 可以用邻接矩阵 G[N][N]，其中：
   $$G[i][j]= \begin{cases}1 & \text { 若< } v_{i}, v_{j}>\text { 是G中的边 } \\ 0 & \text { 否则 }\end{cases}$$
   它是对称矩阵，有一半空间是浪费的 <br>
   因此，将上面邻接矩阵压缩成一个长度为N(N+1)/2的一维数组A：{G<sub>00</sub>,G<sub>10</sub>,G<sub>11</sub>,...,G<sub>n-1 0</sub>,...,G<sub>n-1 n-1</sub>,}

   则G<sub>ij</sub>在G中对应的下标为(i*(i+1)/2+j)

   如果要构造一个网络，只要把G[i][j]的值定义为边<vi,vj>的权重即可

   缺点：<br>
   1. 存稀疏图的时候很浪费空间
   2. 浪费时间：需要每一个元素都扫过去

2. 邻接表：G[N]为指针数组，对应的矩阵每行一个链表，只存储非0元素<br>
   无所谓顺序
   ![](../pics/6.1.1.png)

   特点：
   1. 方便找任意顶点的连接点
   2. 节约稀疏图的空间
   3. **对于无向图**，方便计算任意顶点的度
   4. 很难检测任意一对顶点之间是否存在边

### 图的遍历

**深度优先搜索，Depth First Search, DFS**
一条路走到底，没路后**原路返回**，直到返回到这样一个岔路口（结点）：还有没被遍历的支路可以走。重复，指导最后原路返回至初始节点。

伪码描述：
```cpp
void DFS (Vertex V)
{   //类似树的先序遍历
    visited[V] = true;
    for(V的每个邻接点W)
        if(!visited)
            DFS(W);
}
```

若有N个顶点、E条边，时间复杂度是（主要耗在对“V的每个邻接点W”进行扫描）:
- 用邻接表存储图，有O(N+E) 
- 用邻接矩阵存储图，有O(N<sup>2</sup>)

**广度优先搜索，Breadth First Search, BFS**
类似树的层序遍历

伪码描述：
```cpp
void BFS(Vertex V)
{
    visited[V] = true;
    Enqueue(V, Q);
    while(!IsEmpty(Q)){
        V = Dequeue(Q);
        for(V的每个邻接点W){
            if(!visited[W]){
                visited[W] = true;
                Enqueue(W, Q);
            }
        }
    }
}
```
时间复杂度(主要耗在对“V的每个邻接点W”进行扫描)：
- 用邻接表存储图，有O(N+E)
- 用邻接矩阵存储图，有O(N<sup>2</sup>)

 #### 图的连通性

- 连通:如果从v到w存在一条(无向)路径,则称v和w是连通的。
- 路径：V到W的路径是一系列顶点{V, v1, v2, …, vn, W}的集合，其中任一对相邻的顶点间都有图中的边。路径的长度是路径中的边数（如果带权，则是所有边的权重和）。如果V到W之间的所有顶点都不同，则称简单路径。 
- 回路：起点等于终点的路径
- 连通图：图中任意两顶点均连通

无向图：
- 连通分量：无向图的**极大**连通子图
  - **极大**顶点数：再加1个顶点就不连通了
  - **极大**边数：包含子图中所有顶点相连的所有边

有向图：
- 强连通：有向图中顶点V和W之间存在**双向路径**（不一定是同一条，但是都存在），则称V和W是强连通的
  - 强连通图：有向图中任意两顶点均强连通
  - 强连通分量：有向图的极大强连通子图

如何遍历不连通的图呢？
```cpp
void ListComponents(Graph)
{
    for(each V in G){
        if(!visited[V]){
            DFS(V);//or BFS
            // 每调用一次DFS/BFS, 就把V所在的连通分量遍历了一遍
            // 类似于绘画工具中的油漆桶
        }
    }
}
```

### 如何建立图

#### 邻接矩阵表示
```cpp
typedef struct GNode *PtrToGNode;
struct GNode{
    int Nv; //顶点数
    int Ne; //边数
    WeightType G[MaxVertexNum][MaxVertexNum]
    Datatype Data[MaxVertexNum] //存顶点的数据
};
typedef PtrToGNOde MGraph; //以邻接矩阵存储的图类型
```
初始化一个有VertexNum个顶点但没有边的图
```cpp
MGraph CreateGraph(int VertexNum)
{
    MGraph Graph;
    Graph = (MGraph)malloc(sizeof(struct GNode));
    Graph->Nv = VertexNum;
    Graph->Ne = 0;

    for(V =0 ;V<Graph->Nv;V++){
        for(W=;W<Graph->Nv;W++){
            Graph->G[V][W] = ;
        }
    }
    return Graph;
}
```
向MGraph中插入边
```cpp
typedef struct ENode *PtrToENode;
struct ENode{
    Vertex V1,V2; //有向边<V1,V2>
    WeightType Weight; //权值
};
typedef PtrToENode Edge;

void InsertEdge(MGraph Graph, Edge E)
{
    //插入<v1,v2>
    Graph->G[E->V1][E->V2] = E->Weight;
    //若是无向图，还要插入<v2,v1>
    Graph->G[E->v2][E->v1] = E->Weight;
}
完整地建立一个MGraph<br>
输入格式：<br>
Nv Ne<br>
V1 V2 Weight<br>
... ...<br>
```cpp
MGraph BuildGraph()
{
    MGraph Graph;

    scanf("%d",&Nv);
    Graph = CreateGraph(NV); //initialization
    scanf("%d",&(Graph->Ne));
    if(Graph->Ne !=0){
        E = (Edge)malloc(sizeof(struct ENode));
        for(i=;i<Graph->Ne;i++){
            scanf("%d %d %d",&E->V1,&E->V2,&E->Weight);
            InsertEdge(Graph, E);
        }
    }

    //如果顶点有数据的话，读入数据
    for(V=;V<Graph->Nv;V++){
        scanf("%c"),&(Graph->Data[V]);
    }
    return Graph;
}
```

更简单的实现方法：
```cpp
int G[MAXN][MAXN],Nv,Ne;
void BuildGraph()
{
    int i,j,v1,v2,w;
    scanf("%d",&Nv);
    // CreateGraph
    for(i=;i<Nv;i++){
        for(j=;j<Nv;j++){
            G[i][j]=0;
        }
    }
    scanf("%d",&Ne);
    for(i=0;i<Ne;i++){
        scanf("%d %d %d",&v1,&v2,&w);
        G[v1][v2] = w;
        G[v2][v1] = w;
    }
}
```

#### 邻接表表示
![](../pics/6.1.5.png)
```cpp
typedef struct GNode *PtrToGNode;
struct GNode{
    int Nv; //顶点数
    int Ne; //边数
    AdjList G; //邻接表
}；
typedef PtrToGNode LGraph;
//以邻接表方式存储的图

typedef struct Vnode{
    PtrToAdjVNode FirstEdge;
    DataType Data; //存顶点的数据
} AdjList[MaxVertexNum];
// AdjList是邻接表类型

typedef struct AdjVNode *PtrToAdjVNode;
struct AdjVNode{
    Vertex AdjV; //邻接点下标
    WeightType Weight; // 边权值
    PtrToAdjVNode Next; // 指向下一个
}

```
初始化一个有VertexNum个顶点但没有边的图
```cpp
typedef int Vertex; //用顶点下标表示顶点，为整形
LGraph CreateGraph(int VertexNum)
{
    Vertex V,W;
    LGraph Graph;
    Graph = (LGraph)malloc(sizeof(struct GNode));
    Graph->Nv = VertexNum;
    Graph->Ne = 0;

    // 使得其指针为空
    for(V = 0;V<Graph->Nv;V++){
        Graph->G[V].FirstEdge = NULL;
    }
    return Graph;
}
```
向LGraph中插入边
![](../pics/6.1.6.png)
```cpp
void InsertEdge(LGraph Graph, Edge E)
{
    PtrToAdjVNode NewNode;
    //先为V2建立新的邻接点
    NewNode=(PtrToAdjVNode)malloc(sizeof(struct AdjVNode));
    NewNode->AdjV = E->V2;
    NewNode->Weight = E->Weight;
    //再将V2插入V1的表头
    NewNode->Next = Graph->G[E->V1].FirstEdge;
    Graph->[E->V1].FirstEdge = NewNode;

    //若是无向图，还要插入<V2,V1>
    //先为V1建立新的邻接点
    NewNode=(PtrToAdjVNode)malloc(sizeof(struct AdjVNode));
    NewNode->AdjV = E->V1;
    NewNode->Weight = E->Weight;
    //再将V1插入V2的表头
    NewNode->Next = Graph->G[E->V2].FirstEdge;
    Graph->[E->V2].FirstEdge = NewNode;
}
```
## 图（下）

### 最小生成树（Minimum Spanning Tree)

**最小生成树存在** 和 **图连通** 是一对充分必要条件
#### 定义
- 是一棵树
  - 无回路
  - V个顶点一定有V-1条边
- 是生成树
  - 包含全部顶点
  - V-1条边都在图里
  - 向生成树中任加一条边都一定构成回路
- 边的权重和最小

#### 贪心算法

- 每一步都要最好的
- 每次都要权重最小的边

**约束：**
- **只能用图里有的边**
- **只能正好用掉V-1条边**
- **不能有回路**

#### Prim(普利姆)算法-让一棵小树长大
**注意：构造时需要判断不能构成回路！**

适用于稠密图，算法复杂度为**O(V<sup>2</sup>**

算法描述：
1. 在一个加权连通图中，顶点集合V，边集合为E
2. 任意选出一个点作为初始顶点,标记为visit,计算所有与之相连接的点的距离，选择距离最短的，标记visit.
3. 重复以下操作，直到所有点都被标记为visit：
   在剩下的点中，计算与已标记visit点距离最小的点，标记visit,证明加入了最小生成树。

```cpp
void Prim()
{
    //初始化：dist[V] = E(s,V)或正无穷
    //        parent[s] = -1
    MST = {s};
    while(1){
        V = 未收录顶点中dist最小者;
        if(这样的V不存在){
            break; //MST完成了，或者剩下的结点和树之间的距离都是无穷大
        }
        //将V收录进MST
        for(V的每个邻接点W){
            if(dist[W]!=0,"即没有被收录"){
                if(E(V,W)<dist[W],"小树长大后，需要更新一下未收录结点和MST的距离"){
                    dist[W] = E(V,W);
                    parent[W] = V;
                }
            }
        }if(MST中收录的顶点不到V个)
         //说明图不是连通的
         {
             Error("生成树不存在");
         }
    }
}
```

#### Kruskal算法-将森林合并成树
**注意：构造时需要判断不能构成回路！**

适用于稀疏图，直接选出权重最小的边（每一条边都会连接两棵树，因此是树的合并），直到无法再选出符合条件的边为止。时间复杂度：O(ElogE)

```cpp
void Kruskal(Graph G)
{
    MST = {};
    while(MST中不到V-1条边&&E中还有边){
        从E中取一条权重最小的边E(V,W); //最小堆
        将E(V,W)从E中删除；
        if(E(V,W)不在MST中构成回路)
            将E(V,W)加入MST;
        else
            彻底无视E(V,W);
    }
    if(MST中不到V-1条边)
        ERROR("生成树不存在")；
}
```
### 拓扑排序

- AOV(activity on vertex)网络,活动仅发生在顶点处。
- 拓扑序：如果图中从V到W有一条有向路径，则V一定排在W之前。满足此条件的顶点序列称为一个拓扑序
- 获得一个拓扑序的过程就是拓扑排序
- AOV如果有合理的拓扑序，则必定是有向无环图（Directed Acyclic Graph, DAG)
![](../pics/8.2.1.png)

算法：
- 每次选择入度为0的顶点（没有前置结点）
- 每次输出后，要把被输出的点和他对应的边给抹除
- 不断这样一层一层输出，即可得到拓扑序
![](../pics/8.2.2.png)
![](../pics/8.2.3.png)

O(V<sup>2</sup>)
```cpp
void TopSort()
{
    for(cnt = 0; cnt<V;cnt++){
        V = 未输出的入度为0的顶点; //这一步的扫描很耗时（O(N)）
        if(这样的V不存在){  //即，外循环还没结束，就碰到入度始终不为0的结点
            Error("图中有回路");
            break;
        }
        输出V，或者记录V的输出序号;
        for(V的每个邻接点W){
            Indegree[W]--;//入度减少
        }
    }
}
// 更加聪明的算法：随时将入度变为0的顶点放到一个容器里，这样就不用扫描
void TopSort()
{
    for(图中每个顶点V){
        if(Indegree[V]==0){
            Enqueue(V,Q);
        }while(!IsEmpty(Q)){
            V = Dequeue(Q);
            输出V，或者记录V的输出序号;
            cnt++;
            for(V的每个邻接点W){
                if(--Indegree[W]==0){
                    Enqueue(W,Q); //碰到入度为0的直接加入队列，就不用额外重新扫描
                }
            }
        }
    }
    if(cnt != V){
        ERROR("图中有回路");
    }
}
```
这个算法还可以用来检测一个有向图是否是有向无环图(DAG)

#### 关键路径问题

AOE(Activity On Edge)网络

- 一般用于安排项目的工序
![](../pics/8.2.4.png)

工期安排举例：
![](../pics/8.2.5.png)

关键路径则是由**绝对不允许延误**的活动组成的路径


## 排序（上）

模板：

### 简单排序

#### 冒泡排序（稳定）
改进：设立一个flag标志一次循环中是否有做交换（swap）若没有做交换，则认为其余序列已经是有序的，则可以直接跳出，节省时间。

最好情况：顺序，T=O(N)<br>
最坏情况：逆序，T=O(N<sup>2</sup>)

优点：可与适配数组和链表，而且在由于只在值不同时才做交换，是稳定的。
```cpp
void Bubble_Sort(ElementType A[], int N)
{
    for (P=N-1;P>=0;P--){
        flag = 0;
        for(i=0;i<P;i++){
            if(A[i]>A[i+1]){
                swap(A[i], A[i+1]);
                flag = 1;
            }
        }
        if(flag==0) break;
    }
}
```

#### 插入排序（稳定）
最好情况：顺序，T=O(N)<br>
最坏情况：逆序，T=O(N<sup>2</sup>)
```cpp
void Insertion_Sort(ElementType A[], int N)
{
    for(P=1;P<N;P++){
        Tmp = A[P]; 
        for(i=P; i>0 && A[i-1] > Tmp; i--){
            A[i] = A[i-1]; //数据往后逐个拷贝移位
        }
        A[i] = Tmp; //插入
    }
}
```

由于只有在牌大小不同时才移位，这种排序也是稳定的

#### 时间复杂度下界
- 对于下标i<j，如果A[i]>A[j], 则称(i,j)是一对逆序对(inversion)
- 交换2个相邻元素正好消去1个逆序对!
- 因此插入和冒泡对于同一个数列的交换次数一样不是巧合
- 插入排序：T(N,I)=O(N+I)
  - 如果序列基本有序，则插入排序简单有效
- 任意N个不同元素组成的序列平均具有N(N-1)/4个逆序对。
- 定理:任何仅以交换相邻两元素来排序的算法，其平均时间复杂度为Ω(N<sup>2</sup>)
  - 因此，要提高效率，必须
    - 每次消去不止一个的逆序对
    - 每次交换相隔较远的2个元素

### 希尔排序（Shell sort）（不稳定）
即采取设定间隔来采样，并对采样值分别做插入排序.
![](../pics/9.1.1.png)

希尔排序为什么不稳定？
>希尔排序是按照不同步长对元素进行插入排序，当刚开始元素很无序的时候，步长最大，所以插入排序的元素个数很少，速度很快；当元素基本有序了，步长很小， 插入排序对于有序的序列效率很高。所以，希尔排序的时间复杂度会比O(n^2)好一些。由于多次插入排序，我们知道一次插入排序是稳定的，不会改变相同元素的相对顺序，但在不同的插入排序过程中，相同的元素可能在各自的插入排序中移动，最后其稳定性就会被打乱，所以shell排序是不稳定的。


流程：
1. 定义一个增量序列作为采样间隔DM>DM-1>...>D1=1，最后一位必须是1，比如{5, 3, 1}
2. 对每个采样间隔进行“Dk-间隔排序”，比如对按照5-间隔滑动采样出来的5个序列分别进行插入排序。
3. **注意:“Dk-间隔”有序的序列，在执行“Dk-1-间隔”排序后，仍然是“Dk间隔”有序的，这是希尔排序的关键**

注意：需要保证增量序列的互质！增量元素不互质，则小增量可能根本不起作用，如：
![](../pics/9.1.2.png)

因此增量序列的设计是关键的，如这里采用Sedgwick


```cpp
void ShellSort( ElementType A[], int N )
{ /* 希尔排序 - 用Sedgewick增量序列 */
     int Si, D, P, i;
     ElementType Tmp;
     /* 这里只列出一小部分增量 */
     int Sedgewick[] = {929, 505, 209, 109, 41, 19, 5, 1, 0};
     
     for ( Si=0; Sedgewick[Si]>=N; Si++ ) 
         ; /* 初始的增量Sedgewick[Si]不能超过待排序列长度 */

     for ( D=Sedgewick[Si]; D>0; D=Sedgewick[++Si] )
         for ( P=D; P<N; P++ ) { /* 插入排序*/
             Tmp = A[P];
             for ( i=P; i>=D && A[i-D]>Tmp; i-=D )
                 A[i] = A[i-D];
             A[i] = Tmp;
         }
}
```

### 选择排序（不稳定），堆排序（不稳定）

#### 选择排序

选择排序和插入排序的差别：插入排序是选择给定序列中最近的一组逆序对交换，而选择排序则是寻找全局最小元，并将其换到序列的最后位置

选择排序是不稳定的。
>举个例子，序列5 8 5 2 9，我们知道第一遍选择第1个元素5会和2交换，那么原序列中2个5的相对前后顺序就被破坏了，所以选择排序不是一个稳定的排序算法。

选择排序算法：
```cpp
void Selection_Sort(ElementType A[], int N)
{
    for(i = 0; i< N; i++){
        MinPosition = ScanForMin(A, i, N-1); //这一步scanformin需要O(N)的复杂度，可以用最小堆优化
        // 从到中寻找最小元，并将其位置赋给MinPosition
        Swap(A[i], A[MinPosition]});
        // 将未排序部分的最小元换到有序部分的最后位置
    }
}
```

#### 堆排序
（实际中，堆排序可能不如Sedgwick序列的希尔排序好用）

简单的情况：调用一个最小堆，使用时让其返回根节点（最小值）即可。

[堆排序不是稳定的](https://zhidao.baidu.com/question/589205766.html)
[为什么需要判断排序是否稳定](https://blog.csdn.net/weixin_39996908/article/details/111108431)
>在需要对多个(也意味着多次)具有优先级的关键字进行排序的场景下，稳定排序能利用上一次排序的结果服务于本次排序，从而保证对于值相同的元素的两次排序结果相同。

算法：
```cpp
void Heap_Sort (ElementType A[], int N)
{
    BuildHeap(A); // O(N)
    for(i=0;i<N;i++){
        TmpA[i] = DeleteMin(A); //O(logN)
    }
    for(i=0; i<N; i++){ //这一步需要额外的O(N)空间，且复制也需要时间
        A[i] = TmpA[i];
    }
}
```
另一种算法，直接在原数组上操作，不需额外开辟空间复制元素


```cpp
void PercDown( ElementType A[], int p, int N )
{ /* 改编代码4.24的PercDown( MaxHeap H, int p )    */
  /* 将N个元素的数组中以A[p]为根的子堆调整为最大堆 */
    int Parent, Child;
    ElementType X;

    X = A[p]; /* 取出根结点存放的值 */
    for( Parent=p; (Parent*2+1)<N; Parent=Child ) {
        Child = Parent * 2 + 1;
        if( (Child!=N-1) && (A[Child]<A[Child+1]) )
            Child++;  /* Child指向左右子结点的较大者 */
        if( X >= A[Child] ) break; /* 找到了合适位置 */
        else  /* 下滤X */
            A[Parent] = A[Child];
    }
    A[Parent] = X;
}

void HeapSort( ElementType A[], int N ) 
{ /* 堆排序 */
     int i;
      
     for ( i=N/2-1; i>=0; i-- )/* 建立最大堆 */
         PercDown( A, i, N );
     
     for ( i=N-1; i>0; i-- ) {
         /* 删除最大堆顶 */
         Swap( &A[0], &A[i] ); /* 见代码7.1 */
         PercDown( A, 0, i );
     }
```

### 归并排序（稳定）

是一种**稳定**的排序，算法复杂度也低。唯一的不好是需要额外的空间<br>

内排序：指在排序期间数据对象全部存放在内存的排序。
外排序：指在排序期间全部对象太多，不能同时存放在内存中，必须根据排序过程的要求，不断在内，外存间移动的排序。<br>
归并排序适用于外排序，不适用于内排序


#### 有序子列的归并
类似多项式相加的思想：
![](../pics/9.2.1.png)

算法：
```cpp
/* L = 左边起始位置, R = 右边起始位置, RightEnd = 右边终点位置*/
void Merge( ElementType A[], ElementType TmpA[], int L, int R, int RightEnd )
{ /* 将有序的A[L]~A[R-1]和A[R]~A[RightEnd]归并成一个有序序列 */
     int LeftEnd, NumElements, Tmp;
     int i;
     
     LeftEnd = R - 1; /* 左边终点位置 */
     Tmp = L;         /* 有序序列的起始位置 */
     NumElements = RightEnd - L + 1;
     
     while( L <= LeftEnd && R <= RightEnd ) {
         if ( A[L] <= A[R] )
             TmpA[Tmp++] = A[L++]; /* 将左边元素复制到TmpA */
         else
             TmpA[Tmp++] = A[R++]; /* 将右边元素复制到TmpA */
     }

     while( L <= LeftEnd )
         TmpA[Tmp++] = A[L++]; /* 直接复制左边剩下的 */
     while( R <= RightEnd )
         TmpA[Tmp++] = A[R++]; /* 直接复制右边剩下的 */
         
     for( i = 0; i < NumElements; i++, RightEnd -- )
         A[RightEnd] = TmpA[RightEnd]; /* 将有序的TmpA[]复制回A[] */
}
```

#### 递归算法
分而治之。
![](../pics/9.2.2.png)

```cpp
void Msort( ElementType A[], ElementType TmpA[], int L, int RightEnd )
{ /* 核心递归排序函数 */ 
     int Center;
     
     if ( L < RightEnd ) {
          Center = (L+RightEnd) / 2;
          Msort( A, TmpA, L, Center );              /* 递归解决左边 */ 
          Msort( A, TmpA, Center+1, RightEnd );     /* 递归解决右边 */  
          Merge( A, TmpA, L, Center+1, RightEnd );  /* 合并两段有序序列 */ 
     }
}
```

#### 非递归算法

```cpp

/* 归并排序 - 循环实现 */
/* 这里Merge函数在递归版本中给出 */

/* length = 当前有序子列的长度*/
void Merge_pass( ElementType A[], ElementType TmpA[], int N, int length )
{ /* 两两归并相邻有序子列 */
     int i, j;
      
     for ( i=0; i <= N-2*length; i += 2*length )
         Merge( A, TmpA, i, i+length, i+2*length-1 );
     if ( i+length < N ) /* 归并最后2个子列*/
         Merge( A, TmpA, i, i+length, N-1);
     else /* 最后只剩1个子列*/
         for ( j = i; j < N; j++ ) TmpA[j] = A[j];
}

void Merge_Sort( ElementType A[], int N )
{ 
     int length; 
     ElementType *TmpA;
     
     length = 1; /* 初始化子序列长度*/
     TmpA = malloc( N * sizeof( ElementType ) );
     if ( TmpA != NULL ) {
          while( length < N ) {
              Merge_pass( A, TmpA, N, length );
              length *= 2;
              Merge_pass( TmpA, A, N, length );
              length *= 2;
          }
          free( TmpA );
     }
     else printf( "空间不足" );
}
```
## 排序（下）
### 快速排序（不稳定）

算法思想：分而治之<br>
使用主元（pivot）将数据分成两块
![](../pics/10.1.1.png)

伪码描述：
```cpp
void Quicksort(ElementType A[], int N)
{
    if(N<2) return;
    pivot = 从A[]中选一个主元  <- 这一步很关键
    将S={A[]\pivot}分成两个独立子集： <- 这一步也很关键
        A1 = { a∈S | a≤pivot } 和
        A2 = { a∈S | a≥pivot };
    A[] = Quicksort(A1,N1)∪{pivot}∪Quicksort(A2,N2)
}
```

当每次正好中分时，是快排算法的最好情况，此时O(NlogN)

#### 怎么选择主元

- 经典方法：取头、中、尾的中位数（一共就3个元素）（如8、12、3的中位数是8）

```cpp
ElementType Median3( ElementType A[], int Left, int Right )
{ 
    int Center = (Left+Right) / 2;
    if ( A[Left] > A[Center] )
        Swap( &A[Left], &A[Center] );
    if ( A[Left] > A[Right] )
        Swap( &A[Left], &A[Right] );
    if ( A[Center] > A[Right] )
        Swap( &A[Center], &A[Right] );
    /* 此时A[Left] <= A[Center] <= A[Right] */
    Swap( &A[Center], &A[Right-1] ); /* 将基准Pivot藏到右边*/
    /* 只需要考虑A[Left+1] … A[Right-2] */
    return  A[Right-1];  /* 返回基准Pivot */
}
```
#### 怎么划分子集

快排之所以快的原因在于子集划分之后，它的主元被一次性地放到了正确的位置上，以后不再移动，而不像插入排序那样之后还可能要移动。

如果有元素正好等于pivot，则停下来交换，这样可以保证pivot最后会处于比较中心的位置，对于递归来说是有好处的

初始状态，6为pivot，初始化两个指针i和j，分别往右和往左移<br>
i所指向的值应该比6小，否则就发出红色警告，j反之。
![](../pics/10.1.2.png)
一边发出红色警告则停止，让另一边开始移动；如果两边都发出红色警告，则停止
![](../pics/10.1.3.png)
并交换他们所指向的元素。
![](../pics/10.1.4.png)
然后进行下一轮的比较，如果没有发出警报就一直右移/左移
![](../pics/10.1.5.png)
交换，
![](../pics/10.1.6.png)
直到检测到i，j错位，则结束，并将pivot交换到i的位置
![](../pics/10.1.7.png)


#### 对于小规模数据的处理：
- 快速排序的问题
  - 用到递归
  - 对小规模的数据（例如N不到100）可能还不如插入排序快
- 解决方案
  - 当递归的数据规模小于一个阈值，则停止递归，直接调用简单排序（例如插入排序）

```cpp
void Qsort( ElementType A[], int Left, int Right )
{ /* 核心递归函数 */ 
     int Pivot, Cutoff, Low, High;
      // Cutoff是阈值
     if ( Cutoff <= Right-Left ) { /* 如果序列元素充分多，进入快排 */
          Pivot = Median3( A, Left, Right ); /* 选基准 */ 
          Low = Left; High = Right-1;
          while (1) { /*将序列中比基准小的移到基准左边，大的移到右边*/
               while ( A[++Low] < Pivot ) ;
               while ( A[--High] > Pivot ) ;
               if ( Low < High ) Swap( &A[Low], &A[High] );
               else break;
          }
          Swap( &A[Low], &A[Right-1] );   /* 将基准换到正确的位置 */ 
          Qsort( A, Left, Low-1 );    /* 递归解决左边 */ 
          Qsort( A, Low+1, Right );   /* 递归解决右边 */  
     }
     else InsertionSort( A+Left, Right-Left+1 ); /* 元素太少，用简单排序 */ 
}

void QuickSort( ElementType A[], int N )
{ /* 统一接口 */
     Qsort( A, 0, N-1 );
}
```

### 表排序
每个元素是具体的，庞大的一个实体（如，一部5G的电影，一个3G的ZIP），这时对其进行任何频繁的swap都是不切实际的。<br>
表排序不移动元素本身，只移动指针，这种排序方法叫做**间接排序**。

间接排序
- 定义一个指针数组作为表(table)
- 只需修改指针数组即可
- 输出时直接输出排序后的指针数组所对应的元素
![](../pics/10.2.1.png)
如果仅仅要求按顺序输出，则输出：<br>
A[table[0]],A[table[1]],...,A[table[N-1]]

#### 物理排序
N个数字的排列由若干个独立的环组成，按顺序地移位每一个独立的环。<br>
如果table[i] == i，则说明这个物体已经在他该待的位置上了（环中只有他一个元素）


### 基数排序（Radix sort）（稳定）
只能用于有基数的东西。对于结构体排序，或者只有相对大小关系，没有具体数值（比如你要自己写判断大小的函数），那么基数排序就没法使用（因为它不是基于比较的）。<br>
基于比较的排序下界就是O(nlogn)，基数排序不是基于比较的排序，而是基于数据分配，所以可以突破下界
#### 桶排序

引例：假设我们有N个学生，他们的成绩是0到100之间的整数(于是有M=101个不同的成绩值)。如何在线性时间内将学生按成绩排序?<br>
由于只有101个不同的成绩值，可以直接分成101个不同的桶，数据便可扫描一遍就对号入座，接入对应的链表里。输出时只需直接按顺序输出链表即可
![](../pics/10.3.1.png)

```cpp
void Bucket_Sort(ElementType A[], int N) 
{  count[]初始化;
   while (读入1个学生成绩grade)
      将该生插入count[grade]链表;
   for ( i=0; i<M; i++ ) {
      if ( count[i] )
         输出整个count[i]链表;
   }
}
```

但是如果M比N大很多怎么办呢？加入M=1000，N=10，那就要建1000个桶，很不划算。这时可以使用基数排序。这里的基数指进制的基数，比如10进制则为10

如果按照**次位优先(Least Significant Digit, LSD)**，则：
![](../pics/10.3.2.png)

基数排序的算法复杂度取决于桶的个数。如果桶比较少的话，复杂度接近线性。

#### 多关键字的排序

引例：扑克牌的排序，花色是主关键字，面值是次关键字。
![](../pics/10.3.3.png)

运用基数排序，如果把主关键字作为高位，次关键字作为低位，就用到了**主位优先(Most Significant Digit, MSD)**。

1. 第一种办法：
   为4种花色建立4个桶，再在每个花色桶里对数值进行排序----这样效率不高
2. 第二种方法：用次位优先：
   先为面值建立13个桶，将结果合并，然后再为花色建立4个桶。---效率大大提高（实际上在现实生活中，也倾向于用第二种方法分扑克牌。但是相较于第一种方法，显然地，第二种需要占用的桌面，或者说空间，要大得多。）


### 排序算法的比较

[常见排序算法的代码实例](https://www.cnblogs.com/knull/archive/2012/12/11/2813724.html)

| 排序方法 | 平均时间复杂度 | 最坏情况下时间复杂度 | 额外空间复杂度 | 稳定性 |
| -- | -- | -- | -- | -- |
|简单选择排序|O(N<sup>2</sup>)|O(N<sup>2</sup>)|O(1)|不稳定|
|冒泡排序|O(N<sup>2</sup>)|O(N<sup>2</sup>)|O(1)|稳定|
|插入排序|O(N<sup>2</sup>)|O(N<sup>2</sup>)|O(1)|稳定|
|希尔排序|O(N<sup>d</sup>)|O(N<sup>2</sup>)|O(1)|不稳定|
|堆排序|O(NlogN)|O(NlogN)|O(1)|不稳定|
|快速排序|O(NlogN)|O(N<sup>2</sup>)|O(logN)|不稳定|
|归并排序|O(NlogN)|O(NlogN)|O(N)|稳定|
|基数排序|O(P(N+B))|O(P(N+B))|O(N+B)|稳定|


## 散列查找（哈希）

目前已经学过的查找方法：
1. 顺序查找 O(N)
2. 二分查找（静态查找），需要先把数据从小到大排好 O(log<sub>2</sub>N)
3. 二叉搜索树 O(h) h为二叉查找树的高度
4. 平衡二叉树(AVL) O(log<sub>2</sub>N)

对于二叉查找，对于 10亿 的用户，查找30次即可，且只需要1T的连续空间。但是因为账户数据是按大小有序存储的，插入和删除一个新的QQ号码将需要移动大量数据。

**查找的本质：**已知对象，查找位置
- 如果是有序安排对象：全序、半序
- 直接“算出”对象位置：散列/哈希

散列查找的两项基本工作：
- 计算位置：构造合适的散列函数(哈希函数)来确定关键词的存储位置
- 解决冲突：应用某种策略解决多个关键词位置相同的问题

哈希的时间复杂度几乎是常量：O(1),也就是说查找时间与问题的规模无关！

以空间换时间

### 哈希表
哈希表的数据结构描述：

类型名称:符号表（SymbolTable）

数据对象集：符号表是“名字(Name)-属性(Attribute)”对的集合。

操作集：Table$\in$SymbolTable，Name$\in$NameType，Attr$\in$AttributeType
1. SymbolTable InitializeTable( int TableSize )： 创建一个长度为TableSize的符号表；
2. Boolean IsIn( SymbolTable Table, NameType Name)： 查找特定的名字Name是否在符号表Table中；
3. AttributeType Find( SymbolTable Table, NameType Name)： 获取Table中指定名字Name对应的属性；
4. SymbolTable Modefy(SymbolTable Table, NameType Name, AttributeType Attr)： 将Table中指定名字Name的属性修改为Attr；
5. SymbolTable Insert(SymbolTable Table, NameType Name, AttributeType Attr)： 向Table中插入一个新名字Name及其属性Attr；
6. SymbolTable Delete(SymbolTable Table, NameType Name)： 从Table中删除一个名字Name及其属性。

“散列（Hashing）” 的基本思想是：
- 以关键字key为自变量，通过一个确定的函数 h（散列函数），计算出对应的函数值h(key)，作为数据对象的存储地址。
- 可能不同的关键字会映射到同一个散列地址上， 即h(key<sub>i</sub>) = h(key<sub>j</sub>)（当key<sub></sub> ≠key<sub>j</sub>），称为“冲突(Collision)”。
- 因此需要某种冲突解决策略

举例：<br>
有n = 11个数据对象的集合{18，23，11，20，2，7，27，30，
42，15，34}。<br>
符号表的大小用TableSize = 17，选取散列函数h如下：
h(key) = key mod TableSize (求余)
![](../pics/11.1.1.png)

1. 存放：
    h(18)=1, h(23)=6, h(11)=11, h(20)=3, h(2)=2, …….<br>
    如果新插入35， h(35)=1, 该位置已有对象！冲突！！(潜在解决办法：使用二维数组，先匹配到对应的行的第一列，如果有冲突，加到该行的后续的列)

2. 查找：
   key = 22, h(22)= 5，该地址空，不在表中<br>
   key = 30, h(30)= 13，该地址存放是30，找到！

装填因子（Loading Factor）：设散列表空间大小为m，填入表
中元素个数是n，则称α＝ n / m为散列表的装填因子α＝11 / 17 ≈ 0.65


### 哈希函数的构造方法

一个“好”的散列函数一般应考虑下列两个因素：
1. 计算简单，以便提高转换速度；
2. 关键词对应的地址空间分布均匀，以尽量减少冲突。

#### **数字关键词**的哈希函数构造
1. 直接定址法
   取关键词的某个线性函数值为散列地址，即h(key) = a · key + b (a、b为常数)
2. 除留余数法
   哈希函数为：h(key) = key mod p
3. 数字分析法
   分析数字关键字在各位上的变化情况，取比较随机的位作为散列地址<br>
   比如：取11位手机号码key的后4位作为地址：散列函数为：h(key) = atoi(key+7) (char *key），atoi是array to integer函数

4. 折叠法
   把关键词分割成位数相同的几个部分，然后叠加：
   ![](../pics/11.1.2.png)
5. 平方取中法
   ![](../pics/11.1.3.png)


#### **字符关键词**的哈希函数构造
通过求余mod操作来实现大整数向小整数的映射

1. 一个简单的散列函数——ASCII码加和法
   对字符型关键词key定义散列函数如下：
   $$ h(key) = ( \Sigma key[i]) mod TableSize $$
   但是这个方法在碰到这些字符时冲突严重：a3, b2, c1; eat, tea

2. 简单的改进——前3个字符移位法:
   $$ h(key)=(key[0] \times 27^2 + key[1] \times 27 + key[2])mod TableSize $$
   仍然冲突：string、 street、strong、structure等等；空间浪费：3000/263 ≈ 30%

3. 好的散列函数——移位法:
   涉及关键词所有n个字符，并且分布得很好：
   $$h(key) = (\Sigma_{i=0}^{n-1} key[n - i - 1]\times 32^i ) mod TableSize$$

快速计算：

$$h(“abcde”)=‘a’*324+’b’*323+’c’*322+’d’*32+’e’$$

巧妙的做法：<br>
( ( (a x 32 + b) x 32 + c) x 32 + d) ...<br>
而这里更巧妙的是，乘以32实际上就是向左移动5位
```cpp
Index Hash ( const char *Key, int TableSize )
{ 
    unsigned int h = 0; // 散列函数值，初始化为0 
    while ( *Key != '\0') // 这里\0代表字符串的末尾，为空
        h = ( h << 5 ) + *Key++; //左移5位就是乘32，因此虽然ascii码字母只有27，我们还是把进制设为32更方便
    return h % TableSize;
}
```
### 冲突的处理

#### 开放地址法（Open Addressing）
若发生了第 i 次冲突，试探的下一个地址将增加di，基本公式是：

$$h_i(key) = (h(key)+d_i) mod TableSize ( 1≤ i < TableSize )$$

di 决定了不同的解决冲突方案：线性探测($d_i=i$)、平方探测($d_i = \pmi^2$)、双散列($d_1 = i\timesh_2(key$)。

##### 线性探测法（Linear Probing）

线性探测法：以增量序列 1，2，……，TableSize -1）循环试探下一个存储地址。

[例] 设关键词序列为 {47，7，29，11，9，84，54，20，30}，
- 散列表表长TableSize =13 （装填因子 α = 9/13 ≈ 0.69）；
- 散列函数为：h(key) = key mod 11。
用线性探测法处理冲突，列出依次插入后的散列表，并估算查找性能

![](../pics/11.1.4.png)

![](../pics/11.1.5.png)

**哈希表查找性能分析**
- 成功平均查找长度(ASLs)
- 不成功平均查找长度(ASLu)

对于散列表：
![](../pics/11.1.6.png)

ASLs：查找表中关键词的平均查找比较次数（其冲突次数加1）<br>
ASLs= （1+7+1+1+2+1+4+2+4）/ 9 = 23/9 ≈ 2.56

ASLu：不在散列表中的关键词的平均查找次数（不成功）<br>
一般方法：将不在散列表中的关键词分若干类。
如：根据H(key)值分类<br>
ASL u= （3+2+1+2+1+1+1+9+8+7+6）/ 11 = 41/11 ≈ 3.73

##### 平方探测法（Quadratic Probing）--- 二次探测

平方探测法：以增量序列$1^2，-1^2，2^2，-2^2，……，q^2，-q^2$且$q ≤ floor(TableSize/2)$(向下取整)循环试探下一个存储地址。

[例]设关键词序列为 {47，7，29，11，9，84，54，20，30}，
- 散列表表长TableSize = 11，
- 散列函数为：h(key) = key mod 11。
用平方探测法处理冲突，列出依次插入后的散列表，并估算ASLs。
![](../pics/11.2.1.png)

ASLs = （1+1+2+1+1+3+1+4+4）/ 9 = 18/9 = 2
![](../pics/11.2.2.png)

但是，对于某些tablesize，平方探测（二次探测）并不能接触到全部的空间。<br>
如果散列表长度TableSize是某个4k+3（k是正整
数）形式的素数时，平方探测法就可以探查到整个散列表空间

```cpp
typedef struct
HashTbl *HashTable;
struct HashTbl{
    int TableSize;
    Cell *TheCells;
}H ;

HashTable InitializeTable( int TableSize )
{
    HashTable H;
    int i;
    if ( TableSize < MinTableSize ){
        Error( "散列表太小" );
        return NULL;
    }
    /* 分配散列表 */
    H = (HashTable)malloc( sizeof( struct HashTbl ) );
    if ( H == NULL )
        FatalError( "空间溢出!!!" );
    H->TableSize = NextPrime( TableSize );
        /* 分配散列表 Cells */
    H->TheCells=(Cell *)malloc(sizeof( Cell )*H->TableSize);
    if( H->TheCells == NULL )
        FatalError( "空间溢出!!!" );
    for( i = 0; i < H->TableSize; i++ )
        H->TheCells[ i ].Info = Empty;
    return H;
}

Position Find( ElementType Key, HashTable H ) /*平方探测*/
{ Position CurrentPos, NewPos;
    int CNum; /* 记录冲突次数 */
    CNum = 0;
    NewPos = CurrentPos = Hash( Key, H->TableSize ); 
    while( H->TheCells[ NewPos ].Info != Empty &&
    H->TheCells[ NewPos ].Element != Key ) {
    /* 字符串类型的关键词需要 strcmp 函数!! */
        if(++CNum % 2){ /* 判断冲突的奇偶次 */
            NewPos = CurrentPos + (CNum+1)/2*(CNum+1)/2;//这里的操作很巧妙，看下图！
            while( NewPos >= H->TableSize )
            NewPos -= H->TableSize;
        } else {
            NewPos = CurrentPos - CNum/2 * CNum/2;//这里的操作很巧妙，看下图！
            while( NewPos < 0 )
            NewPos += H->TableSize;
        }
    }
    return NewPos;
    }
```
![](../pics/11.2.3.png)

```cpp
void Insert( ElementType Key, HashTable H )
{ /* 插入操作 */
    Position Pos;
    Pos = Find( Key, H );
    if( H->TheCells[ Pos ].Info != Legitimate ) {
        /* 确认在此插入 */
        H->TheCells[ Pos ].Info = Legitimate;
        H->TheCells[ Pos ].Element = Key;
        /*字符串类型的关键词需要 strcpy 函数!! */
    }
}
```
在开放地址散列表中，删除操作要很小心。 通常只能“懒惰删除”，即需要增加一个“删除标记(Deleted)”，而并不是真正删除它。 以便查找时不会“断链”。其空间可以在下次插入时重用

##### 双散列探测法（Double Hashing）

di 为i*h<sub>2</sub>(key)，h<sub>2</sub>(key)是另一个散列函数探测序列成：h<sub>2</sub>(key)，2h<sub>2</sub>(key)，3h<sub>2</sub>(key)，……

- 对任意的key，h2(key) ≠ 0 
- 探测序列还应该保证所有的散列存储单元都应该能够被探测到。 选择以下形式有良好的效果：
  h<sub>2</sub>(key) = p - (key mod p)<br>
  其中：p < TableSize，p、TableSize都是素数。

##### 再散列（Rehashing）
当散列表元素太多（即装填因子 α太大）时，查找效率会下降；

实用最大装填因子一般取 0.5 <= α<= 0.85

当装填因子过大时，解决的方法是加倍扩大散列表，这个过程叫
做“再散列（Rehashing）”

注意：扩大散列表后，原来的元素需要全部重新计算

##### 分离链接法✔（Separate Chaining）
分离链接法：将相应位置上冲突的所有关键词存储在同一个单链表中

[例] 设关键字序列为 47, 7, 29, 11, 16, 92, 22, 8, 3, 50, 37, 89, 94, 21;<br>
散列函数取为：h(key) = key mod 11；<br>
用分离链接法处理冲突。
```cpp
struct HashTbl {
 int TableSize;
 List TheLists;
} *H;
```
![](../pics/11.2.4.png)

表中有9个结点只需1次查找，5个结点需要2次查找，查找成功的平均查找次数：<br>
 ASLs=（9+5*2）/ 14 ≈ 1.36

```cpp
typedef struct ListNode *Position, *List;
struct ListNode {
    ElementType Element;
    Position Next;
};
typedef struct HashTbl *HashTable; 
struct HashTbl {
    int TableSize;
    List TheLists;
};
Position Find( ElementType Key, HashTable H )
{ 
    Position P;
    int Pos;

    Pos = Hash( Key, H->TableSize ); /*初始散列位置*/
    P = H->TheLists[Pos]. Next; /*获得链表头*/
    while( P != NULL && strcmp(P->Element, Key) )
        P = P->Next;
    return P;
}
```






