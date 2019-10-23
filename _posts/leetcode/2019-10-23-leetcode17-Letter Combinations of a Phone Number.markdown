---
layout:     post
title:      "Letter Combinations of a Phone Number(九宫格手机输入法)"
subtitle:   " DFS"
date:       2019-10-23 17:21:00
author:     "DHH"
header-img: "img/bg/leetcodebg.jpg"
catalog: true
tags:
    - leetcode
    - String
    - Backtracking
    - DFS
typora-root-url: ..\..
---

> “Yeah It's leetcode problem. ”

## 问题描述

输入为：在传统手机键盘按键0-9，输出可能打出的所有字母字符串组合。

![17-description](/img/leetcode/17-description.png)

**Example:**

```
Input: "23"
Output: ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
```

## DFS
也没什么可写的，就直接放代码吧

```c++
class Solution {
public:
    vector<string> letterCombinations(string digits) {
        vector<string>res;
        if(digits.empty()) return res;
        vector<string>letter({"", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"});
        string path = "";
        DFS(digits, path, res,letter);
        return res;
    }
    void DFS(string digits_next, string path, vector<string>& res, vector<string> letter){
        cout<<digits_next<<endl;
        if (digits_next.size()==0){
            res.push_back(path);
            cout<<path<<endl;
            return;
        }
        else{
            string hou=letter[digits_next[0] - '0'];
            for (int i=0;i<hou.size();i++){
                path.push_back(hou[i]);
                DFS(digits_next.substr(1),path,res,letter);
                path.pop_back();
            }
        }
    }
};
```

