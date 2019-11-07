---
layout:     post
title:      " 接雨水(42) Trapping Rain Water "
subtitle:   " Backtracking 类似于最大蓄水问题（11） "
date:       2019-11-07 16:08:00
author:     "DHH"
header-img: "img/bg/leetcodebg.jpg"
catalog: true
tags:
    - leetcode
    - DP
typora-root-url: ..\..
---

> “Yeah It's leetcode problem. ”

## 问题描述

问题很容易理解，给定一个数组，找到这个数组能够接到的最多的雨水，看图一下子就能理解题目

 Given *n* non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it is able to trap after raining. 

![image-20191107160918388](/img/leetcode/42-description.png)

**Example:**

```
Input: [0,1,0,2,1,0,1,3,2,1,2,1]
Output: 6
```

## DP

这个题的思路是这样的，判断当前这个位置能不能蓄水，能存多少水的思路：左边的最大柱子，右边的最大柱子，（包含自身），这两个柱子的最小值决定了能不能蓄水，能存多少水就是这个最小值在先去自身的高度，这就是这个位置所能存的水的数量。把每个位置都算出来即可。

用dp的话，就是从左到右 先依次算出来位置左侧最大的柱子，再从右到左，依次算出来右侧最大的柱子，再遍历每个柱子，计算蓄水的数量即可。

```c++
class Solution {
public:
    int trap(vector<int>& height) {      
        int size=height.size();
        int ans=0;
        if (size==0){
            return ans;
        }
        vector<int>left_max(size), right_max(size);
        left_max[0]=height[0];
        right_max[size-1]=height[size-1];
        for (int i=1;i<size;i++){
            left_max[i]=max(left_max[i-1],height[i]);
        }
        for (int i=size-2;i>=0;i--){
            right_max[i]=max(right_max[i+1],height[i]);
        }
        for (int i=1;i<size-1;i++){
            ans+=min(right_max[i],left_max[i])-height[i];
        }
        return ans;
    }
};
```

## Two pointers

这个的思想就跟[最大蓄水池]( https://wendydong.github.io/2019/10/21/leetcode11-Container-With-Most-Water/ )那个很像了，左右各一个指针，当左侧的指针位置高度较小的时候，决定因素在左侧，这个时候就比较当前值与left_max的大小，如果比left_max大，更新left_max，否则就可以蓄水啦，计算这个位置能蓄水的数量即可。反之同理。

```c++
class Solution {
public:
    int trap(vector<int>& height) {
        int size=height.size();
        int ans=0;
        if (size==0){
            return ans;
        }
        int left=0,right=size-1;
        int left_max=0,right_max=0;
        while (left<right){
            if (height[left]<height[right]){
                height[left]>=left_max?left_max=height[left]:ans+=(left_max-height[left]);
                left++;
            }
            else{
                height[right] >= right_max ? (right_max = height[right]) : ans += (right_max - height[right]);
                right--;
            }
        }
        return ans;
    }
};
```
