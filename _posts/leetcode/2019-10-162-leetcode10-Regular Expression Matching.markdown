---
layout:     post
title:      "正则字符完全匹配(10) Regular Expression Matching"
subtitle:   " DP问题 string 递归"
date:       2019-10-18 20:45:00
author:     "DHH"
header-img: "img/bg/leetcodebg.jpg"
catalog: true
tags:
    - leetcode
    - DP
    - string
typora-root-url: ..\..
---

> “Yeah It's leetcode problem. ”

## 问题描述

Given an input string (`s`) and a pattern (`p`), implement regular expression matching with support for `'.'` and `'*'`.

```
'.' Matches any single character.
'*' Matches zero or more of the preceding element.
```

The matching should cover the **entire** input string (not partial).

**Note:**

- `s` could be empty and contains only lowercase letters `a-z`.
- `p` could be empty and contains only lowercase letters `a-z`, and characters like `.` or `*`.

## 知识储备

c++中的substr

 `a = s.substr(0,5)` 

*  用途：一种构造string的方法 
*  形式：s.substr(pos, n) 
* 解释：返回一个string，包含s中从pos开始的n个字符的拷贝（pos的默认值是0，n的默认值是s.size() - pos，即不加参数会默认拷贝整个s） 
*  补充：若pos的值超过了string的大小，则substr函数会抛出一个out_of_range异常；若pos+n的值超过了string的大小，则substr会调整n的值，只拷贝到string的末尾 

## 递归

先看下状态转移公式

We define `dp[i][j]` to be `true` if `s[0..i)` matches `p[0..j)` and `false` otherwise. The state equations will be:

1. `dp[i][j] = dp[i - 1][j - 1]`, if `p[j - 1] != '*' && (s[i - 1] == p[j - 1] || p[j - 1] == '.')`;
2. `dp[i][j] = dp[i][j - 2]`, if `p[j - 1] == '*'` and the pattern repeats for 0 time;
3. `dp[i][j] = dp[i - 1][j] && (s[i - 1] == p[j - 2] || p[j - 2] == '.')`, if `p[j - 1] == '*'` and the pattern repeats for at least 1 time.

用递归的方法写出来

```c++
bool isMatch(string s, string p) {
    if (p.size()==0){
    	return s.size()==0;
    }
    bool initmatch=s.size()!=0 and ((s[0]==p[0]) or (p[0]=='.')) ;
    if (p.size()>1 and p[1]=='*'){
    	return ((initmatch and isMatch(s.substr(1),p)) or isMatch(s,p.substr(2)));
    }
    else{
    	return (initmatch and isMatch(s.substr(1),p.substr(1)));
    }
}
```

## DP

能用递归的方法写出来就能DP的方法写出来

```c++
bool isMatch(string s, string p) {
    int m = s.size(), n = p.size();
    vector<vector<bool>> dp(m + 1, vector<bool>(n + 1, false));
    dp[0][0] = true;
    for (int i = 0; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            if (p[j - 1] == '*') {
                dp[i][j] = dp[i][j - 2] || (i && dp[i - 1][j] && (s[i - 1] == p[j - 2] || p[j - 2] == '.'));
            } else {
                dp[i][j] = i && dp[i - 1][j - 1] && (s[i - 1] == p[j - 1] || p[j - 1] == '.');
            }
        }
    }
    return dp[m][n];
}
```

这种方法将时间复杂度为 o（TP）

如果想再次降低空间复杂度

参考： https://leetcode.com/problems/regular-expression-matching/discuss/5684/C%2B%2B-O(n)-space-DP 

