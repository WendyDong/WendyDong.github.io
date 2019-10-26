---
layout:     post
title:      "合并两个排序链表(21) Merge Two Sorted Lists"
subtitle:   " 经典题目 "
date:       2019-10-26 19:46:00
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

 Merge two sorted linked lists and return it as a new list. The new list should be made by splicing together the nodes of the first two lists. 

**Example:**

```
Input: 1->2->4, 1->3->4
Output: 1->1->2->3->4->4
```

**Note:**

问题很简单，就是两个已经排序好的链表合成一个。

思路其实也挺简单，一个一个比较呗，先比较两个的head, 谁小就构成新的，head向后移动，继续比较head。

根据上面的说法就可以想到，我们可以直接写出来这样的代码，也可以递归的写出来。

## 直接合并

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
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        //创建一个dummy头指针，这是比较常见的操作，注意一下
        ListNode* dummy = new ListNode(0);
        ListNode* cur=dummy;
        while (l1 and l2){
            if (l1->val < l2->val){
                cur->next = l1;
                l1 = l1->next;
            }
            else{
                cur->next = l2;
                l2 = l2->next;
            }
            cur = cur->next;
        }
        if (l1) cur->next = l1;
        if (l2) cur->next = l2;
        return dummy->next;
    }
};
```

## 递归

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
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
         if (not l1 or not l2){
             if(not l1){
                 return l2;
             }
             if (not l2){
                 return l1;
             }
         }
         if (l1->val <l2->val){
             l1->next=mergeTwoLists(l1->next, l2);
             return l1;
         }
         else{
             l2->next=mergeTwoLists(l2->next, l1);
             return l2;
         }
    }
};
```

