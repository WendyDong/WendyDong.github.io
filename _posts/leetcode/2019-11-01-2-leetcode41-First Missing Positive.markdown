---
layout:     post
title:      "第一个缺失的正整数(41) First Missing Positive"
subtitle:   " Array问题 类似剑指offer面试题3 "
date:       2019-11-01 16:58:00
author:     "DHH"
header-img: "img/bg/leetcodebg.jpg"
catalog: true
tags:
    - leetcode
    - Array
typora-root-url: ..\..
---

> “Yeah It's leetcode problem. ”

## 问题描述

问题很容易理解，给定一个不重复的数组，找到第一个缺失的正整数。

 Given an unsorted integer array, find the smallest missing positive integer.  

**Example:**

```
Input: [3,4,-1,1]
Output: 2
```

**Note:**

 Your algorithm should run in *O*(*n*) time and uses constant extra space. 

## 思路分析

这个题想让我们在const 空间内完成，就要思考一下了，最直观的想法是这样的，假设数组的长度为len, 那么这个first missing就一定小于len+1, 所以我们可以建立一个len+1大小的数组，按顺序遍历nums, 如果出现了len+1中的某一个数字，就给数组对应的位置置为true, 最后遍历这个数组，找到第一个false的位置就可以了。不过这个方法需要耗费o(len)的空间，太浪费了。所以可以换一个思路

当我们遍历到位置 i 的一个数字n时，如果i!=n-1, 且n>0,n<len我们就要把n换到它应该呆的位置，即n-1，所以就把n-1和i处的数字交换，直到满足条件位置。 空间复杂度降为1，时间复杂度仍然是o(n)。

这一题跟类似剑指offer面试题3思路相同，可以一起复习。

### 1.  vector 拷贝

   `b.assign(a.begin()+i, a.end());`

   以上代码将a中[i-结尾]的部分拷贝给了b

### 2. vector 排序

   `sort(candidates.begin(),candidates.end());`

### 3. vector 求和

## 代码

```c++
class Solution {
public:
    int firstMissingPositive(vector<int>& nums) {
        int n=nums.size();
        for(int i = 0; i < n; ++ i)
            while(nums[i] > 0 && nums[i] <= n && nums[nums[i] - 1] != nums[i])
                swap(nums[i], nums[nums[i] - 1]);
        
        for(int i = 0; i < n; ++ i)
            if(nums[i] != i + 1)
                return i + 1;
        
        return n + 1;
    }
};
```
