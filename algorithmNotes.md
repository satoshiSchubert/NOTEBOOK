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
            clone = new ListNode(s%10); //s对10取余的值生成一个新节点
            if(s>=10){
                carry = 1; //进位
            }else carry=0;
            
            //接下来就是重要的链接部分了
            if(!cloneHead){
                cloneHead = clone; //若是表头，则直接链接到新生成的clone结点上
                prev = clone; //这里prev指向的是和cloneHead**同一个**new出来的结点，因此后面只需延伸prev即可！
            }else{
                prev->next = clone; //上一步new出来的那个结点的next指向新new出来的结点，创造链接
                prev = clone; //prev指向新节点
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

学完链表之后做（抄）的第一道题，虽然是Medium难度。抄完感觉对linked list的认识加深了，尤其是如何处理新增加节点和原节点之间的链接指向关系。




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




![](https://github.com/satoshiSchubert/NOTEBOOK/blob/main/pics/leetcodeP82.png)

