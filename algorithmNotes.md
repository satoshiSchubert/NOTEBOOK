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
- [# Notebook for Algorithm Ploblems](#-notebook-for-algorithm-ploblems)
  - [目录：](#目录)
  - [2. Add Two Numbers](#2-add-two-numbers)
  - [P1014 [NOIP1999 普及组] Cantor 表](#p1014-noip1999-普及组-cantor-表)
  - [83. Remove Duplicates from Sorted List](#83-remove-duplicates-from-sorted-list)

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

Example1:
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

### 83. Remove Duplicates from Sorted List
https://leetcode.com/problems/remove-duplicates-from-sorted-list/

**[LEETCODE] [Medium] [LinkedList] **

date: 2021-8-16 15:45:00

Answer1:
```cpp
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        int copy;
        ListNode *prev, *clonehead, *clone;
        clonehead = head; 
        while(1){
            if(!head){
                break;//need debug
            }
            copy = head->val;//当前值
            prev = head->next;//prev探路
            if(!prev){
                goto outloop;
            }
            if(copy == prev->val){
                head->next = prev->next;
                prev = prev->next;
                if(!prev || !head){
                        goto outloop;
                        }
                while(copy == prev->val){
                    if(!prev || !head){
                        goto outloop;
                        }
                    head->next = prev->next;
                    prev = prev->next;
                    if(!prev || !head){
                        goto outloop;
                        }
                }
            }
            head = head->next;
        }
    outloop:
    return clonehead;
    }
};
```
comment:

很简单的链表题，判断一下改变链表的next绕过下一个重复的结点就行。这个代码写的很不整齐，即使这样也还是想记录一下嘿嘿。



