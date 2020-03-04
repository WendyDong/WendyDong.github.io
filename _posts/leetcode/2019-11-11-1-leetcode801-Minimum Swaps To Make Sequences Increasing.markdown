---
layout:     post
title:      " 交换数组对应位置直到升序所需要的最小步数(801) Minimum Swaps To Make Sequences Increasing "
subtitle:   " DP问题 "
date:       2019-11-11 15:58:00
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

给定两个长度相同的数组，可以交换两个数组对应位置的值，问：最少需要交换多少次，才能使两个数组都完全升序（等于都不行，后一个绝对大于前一个）（一定存在解）。这个是在做google online的时候碰到的题目，一脸懵逼，google的题目没有给定说一定存在解，如果解不存在要返回-1，后来看到leetcode上有一样的题目，是用dp做的，而且看了答案都不太明白，特此记录。与leetcode1187题也类似。

Given two integer arrays `arr1` and `arr2`, return the minimum number of operations (possibly zero) needed to make `arr1` strictly increasing.

In one operation, you can choose two indices `0 <= i < arr1.length` and `0 <= j < arr2.length` and do the assignment `arr1[i] = arr2[j]`.

If there is no way to make `arr1` strictly increasing, return `-1`.

**Example:**

```
Input: arr1 = [1,5,3,6,7], arr2 = [1,3,2,4]
Output: 1
Explanation: Replace 5 with 2, then arr1 = [1, 2, 3, 6, 7].
```

## DP

![image-20191111160555375](/img/leetcode/801-dp.png)

这个题的思路是这样的，假设我们判断到第`i`个位置的时候，前面`i-1`个位置已经完全升序了，满足了我们的条件，第`i`个的时候，如果`A[i]>A[i-1], B[i]>B[i-1]`，那么我们可以不交换。就是右下角的情况1；当然你也可以换这两个数值，但是你要把前面一项也给换了，才能继续这个条件，就是情况二；如果满足`A[i]>B[i-1], B[i]>A[i-1]`,那么我们还能交换`i`项目，但是不交换前一项，就是情况3，同理，不交换这两项，只交换前两项也可以，这个对应情况4。

所以我们就不能用传统的一个数组来维护memo数组了，我们可以设置swap数组和keep数组，swap数组表示交换这一项时，所需要的最少步骤，keep就代表不交换，在这个的基础上，就有了上图左边的状态转移方程，代码如下：

```c++
class Solution {
public:
    int minSwap(vector<int>& A, vector<int>& B) {
        int n=A.size();
        if(n<2){
            return 0;
        }
        vector<int>swap(n,INT_MAX), keep(n,INT_MAX);
        swap[0]=1;
        keep[0]=0;
        for (int j=1;j<n;j++){
            if (A[j]>A[j-1] && B[j]>B[j-1]){
                keep[j]=keep[j-1];
                swap[j]=swap[j-1]+1;                                
            }
            if (A[j]>B[j-1] && B[j]>A[j-1]){
                keep[j]=min(keep[j],swap[j-1]);  
                swap[j]=min(swap[j],keep[j-1]+1);
            }
        }
        return min(swap[n-1],keep[n-1]);
    }
};
```

我们可以观察到，在上面的代码里，其实dp计算的时候也只用到了前一项，没必要保存整个数组，所以可以再优化一下空间复杂度，代码如下:

注意 swap keep的初始化都是在循环内部，不然keep会一直为0哦。

```c++
class Solution {
public:
    int minSwap(vector<int>& A, vector<int>& B) {
        int n=A.size();
        if(n<2){
            return 0;
        }        
        int swap_pre=1;
        int keep_pre=0;
        for (int j=1;j<n;j++){
            int swap=INT_MAX, keep=INT_MAX;
            if (A[j]>A[j-1] && B[j]>B[j-1]){
                keep=keep_pre;
                swap=swap_pre+1;                                
            }
            if (A[j]>B[j-1] && B[j]>A[j-1]){
                keep=min(keep,swap_pre);  
                swap=min(swap,keep_pre+1);
            }
            keep_pre=keep;
            swap_pre=swap;
        }
        return min(keep_pre,swap_pre);
    }
};
```
