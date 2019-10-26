---
layout:     post
title:      "生成所有的括号组合(22) Generate Parentheses"
subtitle:   " 非常典型的递归backtracking题目 "
date:       2019-10-26 19:54:00
author:     "DHH"
header-img: "img/bg/leetcodebg.jpg"
catalog: true
tags:
    - leetcode
    - String
    - Backtracking
typora-root-url: ..\..
---

> “Yeah It's leetcode problem. ”

## 问题描述

Given *n* pairs of parentheses, write a function to generate all combinations of well-formed parentheses.

For example, given *n* = 3, a solution set is:

**Example:**

```
[
  "((()))",
  "(()())",
  "(())()",
  "()(())",
  "()()()"
]
```

**Note:**

这个题很典型，类似的题目也非常多，这种题型一定要掌握，用递归写其实很简单，一定要再自己写一遍代码！！熟悉一下，很有可能一些就废。

其中需要注意的是dp写法，这里隐含的方程式是 第n个等于组合"("+a+")"+n-a的形式。记忆一下吧。也仔细看一下代码。

## 递归写法

```c++
class Solution {
public:
    vector<string> generateParenthesis(int n) {
        vector<string> re;
        if (n<1){
            return re;
        }
        int left_count=1;
        int right_count=0;
        string path="(";
        DFS(n, path, left_count, right_count, re);
        return re;
    }
    void DFS(int n, string path, int left_count, int right_count, vector<string>& re) {
        if (left_count==n and right_count==n){
            re.push_back(path);
            return;
        }
        if (left_count!=n){
            DFS(n, path+"(", left_count+1, right_count, re);
        }
        if (right_count<left_count){
            DFS(n, path+")", left_count, right_count+1, re);
        }
    }
};
```

## DP写法

```c++
class Solution {
public:
    vector<string> generateParenthesis(int n) {
        vector< vector<string> > dp(n+1, vector<string>());
        dp[0].push_back("");
        for (int i=0;i<=n;i++){
            for(int k=0; k<i; ++k){
                for(string s1: dp[k]){
                    for(string s2: dp[i-1-k]){
                        dp[i].push_back("("+s1+")"+s2);
                    }
                }
            }
        }
        return dp[n];
    }
};
```

