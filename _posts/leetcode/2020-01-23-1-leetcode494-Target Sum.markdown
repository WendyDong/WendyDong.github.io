---
layout:     post
title:      " 目标和(494) Target Sum "
subtitle:   " dp, 背包问题 "
date:       2020-01-23 16:09:00
author:     "DHH"
header-img: "img/bg/leetcodebg.jpg"
catalog: true
tags:
    - leetcode
    - DP
    - 背包问题
typora-root-url: ..\..
---

> “Yeah It's leetcode problem. ”
>
> 好久没有刷题了，昨天刚刚交完ijcai2020的论文，希望我能一次过！谷歌冬令营也结束了，希望我能拿到它的offer, 实习offer和对应的转账offer ！！许愿许愿！！！

## 问题描述

给定一个非负整数列表,a1, a2,…、一个目标和S ,现在你有+符号和-符号。对于每一个整数,从两个符号中选择一个使得他们的和为S, 找出共有多少种选择符号的方法。

You are given a list of non-negative integers, a1, a2, ..., an, and a target, S. Now you have 2 symbols `+` and `-`. For each integer, you should choose one from `+` and `-` as its new symbol.

Find out how many ways to assign symbols to make sum of integers equal to target S.

**Example:**

```
Input: nums is [1, 1, 1, 1, 1], S is 3. 
Output: 5
Explanation: 

-1+1+1+1+1 = 3
+1-1+1+1+1 = 3
+1+1-1+1+1 = 3
+1+1+1-1+1 = 3
+1+1+1+1-1 = 3

There are 5 ways to assign symbols to make the sum of nums be target 3.
```

**Note:**

1. The length of the given array is **positive** and will not exceed **20**.
2. The sum of elements in the given array will not exceed **1000**.
3. Your output answer is guaranteed to be fitted in a 32-bit integer.

## 题目分析

​	拿到这个题吧，最直观的想法就是dfs，上来就是写。但是时间复杂度为可怕的2^n ,因为所有的情况都要遍历到最后才能知道对不对。而且最开始的时候，我的想法是在S的基础上不断的加减nums，最后target为0就好。不过这样会有个很大的问题，就是S是一个已将将要超出int范围的数字，例如27949949，此时再加上一个数就崩溃了。这个时候要利用好note中1000这个信息，不能再S的基础上叠加，最后的target就应该是S。

```c++
class Solution {
public:
    int result;
    int findTargetSumWays(vector<int>& nums, int S) {
        //way1 2^n的暴力解法
        if(S>1000){
            return 0;
        }
        dfs(nums, S, 0, 0);
        return result;
    }
private:    
    void dfs(vector<int>& nums, int S, int index, int curSum){
        if(index==nums.size()){
			if(curSum==S){
				result++;
			}
			return;
		}
        dfs(nums, S, index+1, curSum+nums[index]);
        dfs(nums, S, index+1, curSum-nums[index]);
        return;
    }
};
```

## DP解法

DFS的问题一定能转为一个dp问题，嗯现在开始思考！

可以将这个题看成是一个背包问题去解决，dp\[i]\[j]表示遍历到第i个nums时，和为j的数量  相应的状态转移方程为
`dp[i][j]=dp[i-1][j+nums[i]]+dp[i-1][j-nums[i]]`

或者

`dp[i][j+nums[i]]+=dp[i-1][j]  和 dp[i][j-nums[i]]+=dp[i-1][j]`
由于和最大为1000，所以将S+1000,全部变成正整数（因为有些语言不支持负数？）

```c++
class Solution {
public:
    int findTargetSumWays(vector<int>& nums, int S) {
        vector<vector<int>> resultVec(nums.size(),vector<int>(2001,0));
        resultVec[0][nums[0]+1000]=1;
        resultVec[0][-nums[0]+1000]+=1;
        for (int i=1;i<nums.size();i++){
            for(int j=-1000;j<=1000;j++){
                if(resultVec[i-1][j+1000]>0){
                    resultVec[i][j+nums[i]+1000]+=resultVec[i-1][j+1000];
                    resultVec[i][j-nums[i]+1000]+=resultVec[i-1][j+1000];
                }
            }
        }
        return S>1000?0:resultVec[nums.size()-1][S+1000];
    }
};
```

## DP+剪枝