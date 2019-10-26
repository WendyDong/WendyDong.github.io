---
layout:     post
title:      "移除链表的倒数第N个节点(19) Remove Nth Node From End of List"
subtitle:   " 链表 两个指针 "
date:       2019-10-23 19:22:00
author:     "DHH"
header-img: "img/bg/leetcodebg.jpg"
catalog: true
tags:
    - leetcode
    - Linklist
typora-root-url: ..\..
---

> “Yeah It's leetcode problem. ”

## 问题描述

Given a linked list, remove the *n*-th node from the end of list and return its head.

**Example:**

```
Given linked list: 1->2->3->4->5, and n = 2.

After removing the second node from the end, the linked list becomes 1->2->3->5.
```

**Note:**

Given *n* will always be valid.

## 先计算长度

先遍历一遍这个链表，计算出来链表的长度，判断出应该正向删除第几个指针，将head移动到要删除的指针的前一个位置，删除要删除的指针即可。

注意：为了避免删除第一个元素，没有前一个元素的情况，可以先在第一个元素之前加一个辅助元素。

```c++
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        if (head==NULL or head->next==NULL){
            return NULL;
        }
        ListNode* dummy = new ListNode(0);
        dummy->next=head;
        ListNode* tmp=dummy;
        ListNode* re=dummy;
        int len=1;
        while (head->next!=NULL) {
            len++;
            head=head->next;
        }
        int zheng=len-n;
        int count=0;
        while (count<zheng){
            count++;
            tmp=tmp->next;
        }
        tmp->next=tmp->next->next;
        return re->next;
    }
};
```



## 使用两个指针 one pass

核心思想：

（同样加一个dummy）第一个指针先走N-1步，第二个指针再走，知道第一个指针走到头，这时第二个指针指向的位置就是要删除元素的前一个。

代码：

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* dummy = new ListNode(0);
        dummy->next=head;
        ListNode* p1=dummy;
        ListNode* p2=dummy;
        int count=0;
        while(count<n){
            p1=p1->next;
            count++;
        }
        while(p1->next!=NULL){
            p1=p1->next;
            p2=p2->next;
        }
        p2->next=p2->next->next;
        return dummy->next;
    }
};
```

