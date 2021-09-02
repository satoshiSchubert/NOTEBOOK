---
title: 算法刷题笔记
date: 2021-8-13 21:54:00
tags:
- 算法刷题笔记
categories:
- 算法
---

<p align='center'>
<a href="https://github.com/satoshiSchubert" target="_blank"><img src="https://img.shields.io/badge/Github-@SatoshiNg-f3e1e1.svg?style=flat-square&logo=GitHub"></a>
<img src='https://img.shields.io/badge/style-Chinese-c45a65.svg?style=flat-square' />


### Problems to be solve:
[并查集，洛谷P1551](https://zhuanlan.zhihu.com/p/93647900/)


# Notebook for Algorithm Ploblems
---
分类参考：
https://books.halfrost.com/leetcode/ChapterTwo/Linked_List/

### 目录：

LEETCODE:

  - [2. Add Two Numbers](#2-add-two-numbers)
  - [P1014 [NOIP1999 普及组] Cantor 表](#p1014-noip1999-普及组-cantor-表)
  - [82. Remove Duplicates from Sorted List II](#82-remove-duplicates-from-sorted-list-ii)

LUOGU:
- [P1014.Cantor表](#p1014-noip1999-普及组-cantor-表)

---
### 2. Add Two Numbers
https://leetcode.com/problems/add-two-numbers/

**[LEETCODE] [Medium] [LinkedList] **

date: 2021-8-14 0:27:00
> description:
> 
> You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order, and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list.
> You may assume the two numbers do not contain any leading zero, except the number 0 itself.

Example:
```cpp
Input: l1 = [2,4,3], l2 = [5,6,4]
Output: [7,0,8]
Explanation: 342 + 465 = 807.
```
Answer1:
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        /*
        这里有一个小Point，如果要同时新建多个结构体指针，你可以：
        1. ListNode *prev=NULL, *cloneHead=NULL, *clone;或者
        2. ListNode* prev=NULL, *cloneHead=NULL, *clone;也就是第一个prev不用加*，但之后的都要，
        就是不能：
        x. ListNode* prev=NULL, cloneHead=NULL, clone;或者
        x. ListNode* *prev=NULL, *cloneHead=NULL, *clone;这样第一个会变成**
        个人觉得还是1.这样定义比较容易理解一些。
        */
        ListNode *cur1 = l1, *cur2 = l2; //加*代表结构体指针
        ListNode *prev=NULL, *cloneHead=NULL, *clone;
        int s=0, carry=0;
        while(cur1||cur2)
        {
            if(cur1&&cur2){
                s = cur1->val+cur2->val+carry;
            }else if(!cur1){
                s = cur2->val+carry;
            }else{
                s = cur1->val+carry;
            }
            clone = new ListNode(s%10); //s对10取余的值生成一个新结点
            if(s>=10){
                carry = 1; //进位
            }else carry=0;
            
            //接下来就是重要的链接部分了
            if(!cloneHead){
                cloneHead = clone; //若是表头，则直接链接到新生成的clone结点上
                prev = clone; //这里prev指向的是和cloneHead**同一个**new出来的结点，因此后面只需延伸prev即可！
            }else{
                prev->next = clone; //上一步new出来的那个结点的next指向新new出来的结点，创造链接
                prev = clone; //prev指向新结点
            }
            if(cur1) cur1 = cur1->next;
            if(cur2) cur2 = cur2->next;
        }
        if(carry){ //循环之后，如果最后还有carry额外再加一位
            clone = new ListNode(carry);
            prev->next = clone;
            prev = clone;
        }
        prev->next = NULL; //结束链表
        return cloneHead;
        
    }
};
```
Comment:

学完链表之后做（抄）的第一道题，虽然是Medium难度。抄完感觉对linked list的认识加深了，尤其是如何处理新增加结点和原结点之间的链接指向关系。




### P1014 [NOIP1999 普及组] Cantor 表
https://www.luogu.com.cn/problem/P1014

**[LUOGU] [模拟] [枚举，暴力]**

date: 2021-8-14 0:27:00

Answer1:
```cpp
#include <stdio.h>
#include <stdlib.h>
#include "iostream"
using namespace std;
int main(void)
{
	int N, a, b, odd, sum = 0; // a/b
	cin >> N;
	// 可以直接计算N对应的是第几个循环的
	int count = 0;
	while (1) {
		for (int i = 1; i <= N; i++) {
			odd = i % 2; //even=1则从上往下：1/4,2/3,...
			sum = i + 1;
			if (odd) {
				for (int j = 0; j < i; j++) {
					a = 1 + j;
					b = i - j;
					count++;
					if (count == N) {
						cout << b << '/' << a << endl;
						goto outloop;
					}
				}
			}
			else {
				for (int j = 0; j < i; j++) {
					b = 1 + j;
					a = i - j;
					count++;
					if (count == N) {
						cout << b << '/' << a << endl;
					}
				}
			}
		}
	}
outloop:
	system("pause");
	return 0;
}
```
Comment:

是入门难度的题，虽然还是做了挺久。。。题目本身似乎没有什么技巧，只要找到规律模拟就行了。虽然很简单，但是还是贴上来纪念一下，毕竟万事开头难，希望将来能够坚持下去，不要再放弃了。

### 82. Remove Duplicates from Sorted List II
https://leetcode.com/problems/remove-duplicates-from-sorted-list/

**[LEETCODE] [Medium] [LinkedList]**

date: 2021-8-16 15:45:00

>Description:
>
>Given the head of a sorted linked list, delete all nodes that have duplicate numbers, leaving only distinct numbers from the original list. Return the linked list sorted as well.

Example:
```cpp
Input: head = [1,2,3,3,4,4,5]
Output: [1,2,5]
Input: head = [1,1,1,2,3]
Output: [2,3]
```

Answer1:
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */

class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        // sentinel Head, 即人为在链表前面添加一个value=0的头，以避免[1，1，1，1]这种edge case.
        ListNode *sentinel = new ListNode(0,head); //包含虚表头的完整链表
        
        // Predecessor = the last node outside the sublist of duplicates
        ListNode *pred = sentinel;
        
        while(head != NULL){
            if(head != NULL && head->next != NULL && head->val == head->next->val)
            {
                while(head != NULL && head->next && head->val == (head->next)->val){
                    head = head->next;
                }
                pred->next = head->next; //注意，这里只是给next赋值，pred本身并没有动，这样即使next是另外一列duplicate也没事
            }else{
                pred = pred->next; //前方没有duplicate，可以前移
            }
            head = head->next;
        }
        return sentinel->next; //这里又忽略了虚表头，这样若输入是[1,1,1]，加入虚表头后[0,1,1,1]，计算完[0,'null']，最终返回则是[‘null] 
    }
    
};
```
comment:

这一题是83题(Remove Duplicates from Sorted List)的加强版本，相较于83题（碰到重复的子列只需保留一个，比如[1,2,3,3,4,4,5]->[1,2,3,4,5]）这一题要求完全删去重复的子列，得到[1,2,5]。这在碰到极端情况时（比如[1,1,1,1]->[]）就特别难处理，用83题的方法时就得考虑很多的if，尤其是表头也属于重复子列的情况。非常丢脸，这道题前后卡了起码两个小时。。。后来还是看了solution。<br/>
Solution中也特意点出了[1，1，1，1]这种edge case，但是它用了一个极为巧妙的方法，就是设定一个 pseudo-head伪表头，值为0且链接指向input的链表的表头，这样就规避了输入样例中[1，1，1，1]这样棘手的情况，具体分析如下： <br/>
sentinel意为哨兵，在这里是一个虚的表头，可以从代码看到它接在input的链表前面<br/>
```cpp
// sentinel Head, 即人为在链表前面添加一个value=0的头，以避免[1，1，1，1]这种edge case.
ListNode *sentinel = new ListNode(0,head); //包含虚表头的完整链表
```
然后也创建了一个pred，代表重复子列前的最新一个结点。（在后面很巧妙的保证了它不会等于重复子列中的元素）
```cpp    
// Predecessor = the last node outside the sublist of duplicates
ListNode *pred = sentinel;
```
在这个while循环中，尤其要注意当head从重复子列出来时，pred只是把它的next连接到head->next，pred结点本身是没有更新的，这样在碰到[1,2,2,4,4]的情况就不会出现pred=4的错误。甚至，由于只是pred->next而不是pred本身改变，在head遍历到NULL时将会自动pred->next = NULL，非常方便。
```cpp
    while(head != NULL && head->next && head->val == (head->next)->val){
        head = head->next;
    }
    pred->next = head->next; //注意，这里只是给next赋值，pred本身并没有动，这样即使next是另外一列duplicate也没事
    }
```
![](pics/leetcodeP82.png)
早知道就早点看题解了，浪费了挺多时间的。。。做这些题主要还是熟练链表的操作，下次应该限时。


### 341. Flatten Nested List Iterator
https://leetcode.com/problems/flatten-nested-list-iterator/

**[LEETCODE] [Medium] [stack] [queue]**

date: 2021-8-31 10:43:00

>Description:
>
>You are given a nested list of integers nestedList. Each element is either an integer or a list whose elements may also be integers or other lists. Implement an iterator to flatten it.
>Implement the NestedIterator class:
>NestedIterator(List< NestedInteger > nestedList) Initializes the iterator with the nested list nestedList.
>int next() Returns the next integer in the nested list.
>boolean hasNext() Returns true if there are still some integers in the nested list and false otherwise.

Example:
```cpp
Input: nestedList = [[1,1],2,[1,1]]
Output: [1,1,2,1,1]
Explanation: By calling next repeatedly until hasNext returns false, the order of elements returned by next should be: [1,1,2,1,1].
```

Answer:
```cpp
/**
 * // This is the interface that allows for creating nested lists.
 * // You should not implement it, or speculate about its implementation
 * class NestedInteger {
 *   public:
 *     // Return true if this NestedInteger holds a single integer, rather than a nested list.
 *     bool isInteger() const;
 *
 *     // Return the single integer that this NestedInteger holds, if it holds a single integer
 *     // The result is undefined if this NestedInteger holds a nested list
 *     int getInteger() const;
 *
 *     // Return the nested list that this NestedInteger holds, if it holds a nested list
 *     // The result is undefined if this NestedInteger holds a single integer
 *     const vector<NestedInteger> &getList() const;
 * };
 */

// **这个回答运用了由指针构成的栈** 
class NestedIterator {
private:
    // 这里stack的类型背后要加上 iterator，可能是要强调vector是可迭代的数据类型，否则下面begins和ends没法用 .push() 等操作
    stack<vector<NestedInteger>::iterator> begins, ends; 
public:
    NestedIterator(vector<NestedInteger> &nestedList) {
        begins.push(nestedList.begin()); // 注意，这里的begin和end是一个指针，指向nestedList的起始位置
        ends.push(nestedList.end()); // 这个指针指向终止位置+1！
    }
    
    int next() {
        hasNext(); //如果hasNext = True，那么这一句不会造成什么影响。
        return (begins.top()++)->getInteger(); //这里的顺序是先begins.top()->getInterger返回值,再begins.top()+=1
    }
    
    bool hasNext() {
        while(begins.size()){
            if(begins.top()==ends.top()){ //这里若一层的list循环完了（begin和end位置相等），则脱去当前这一层list嵌套。下面会有进一步解释
                begins.pop();
                ends.pop();
            }else{
                auto x = begins.top(); //这里的auto会根据元素自动为其定义类型
                if(x->isInteger())
                    return true; //脱出该while循环
                
                begins.top()++; //这里的top实际上是一个指针(位置信息)，因此这里是指向的位置+=1
                begins.push(x->getList().begin()); //若指向的是嵌套list，则把这个嵌套list再加载到stack中进行处理
                ends.push(x->getList().end());
            }
        }
        return false;
    }
};

/**
 * Your NestedIterator object will be instantiated and called as such:
 * NestedIterator i(nestedList);
 * while (i.hasNext()) cout << i.next();
 */
```
comment:<br>
这种需要自己构造一个类的题目是第一次碰到，而且他是完全建立在抽象的数据结构之上的，因此第一次着实有点摸不着头脑了。不过看了这个题解之后发现实际上就是根据给定的上一层的抽象数据结构的说明，构造更高一层的、调用前一层的数据类型的一个新数据类型。

举例子来说明这个代码的主要流程：

![](pics/leetcode341.png)

只能说，脑子还不够灵光，想不到这么抽象的层面啊！















