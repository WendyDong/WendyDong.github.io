---
layout:     post
title:      "有效括号(20) Valid Parentheses"
subtitle:   " 栈 需要例子试验一下 "
date:       2019-10-26 19:40:00
author:     "DHH"
header-img: "img/bg/leetcodebg.jpg"
catalog: true
tags:
    - leetcode
    - String
    - Stack
typora-root-url: ..\..
---

> “Yeah It's leetcode problem. ”

## 问题描述

Given a string containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.

An input string is valid if:

1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.

Note that an empty string is also considered valid.

**Example:**

```
Input: "()"
Output: true
Input: "([)]"
Output: false
Input: "{[]}"
Output: true
```

**Note:**

这个题挺简单的，就是要让给定的这三组括号成对即可，例如有了（下一个如果再碰到右括号就必须是）,左括号就可以无所谓，自然就能想到用栈来做这个题。直接上代码吧

## 代码

```c++
class Solution {
public:
    bool isValid(string s) {
        stack<char> paren;
        for (char& c : s) {
            switch (c) {
                //不同于if 会自动顺延到下一项，直到遇到break
                case '(': 
                case '{': 
                case '[': paren.push(c); break;
                case ')': if (paren.empty() || paren.top()!='(') return false; else paren.pop(); break;
                case '}': if (paren.empty() || paren.top()!='{') return false; else paren.pop(); break;
                case ']': if (paren.empty() || paren.top()!='[') return false; else paren.pop(); break;
                default: ; // pass
            }
        }
        return paren.empty() ;
        
    }
};
```

