---
layout:     post
title:      " 最小子串窗口(76) Minimum Window Substring "
subtitle:   " 滑动窗口，字符串问题，弱项 "
date:       2019-11-13 16:09:00
author:     "DHH"
header-img: "img/bg/leetcodebg.jpg"
catalog: true
tags:
    - leetcode
    - String
    - Sliding Window
typora-root-url: ..\..
---

> “Yeah It's leetcode problem. ”

## 问题描述

给定两个字符串，找到前一个字符串包含后一个字符串的所有字符的最小窗口。时间复杂度限制为o(n) 。

 Given a string S and a string T, find the minimum window in S which will contain all the characters in T in complexity O(n). 

**Example:**

```
Input: S = "ADOBECODEBANC", T = "ABC"
Output: "BANC"
```

## Sliding Window

思路如下，两个指针，右边的指针判断是否包含了所有，一旦包含了所有，左边的指针就开始收缩窗口，直到恰好包含，统计一下这个窗口的大小，然后继续移动右边的指针。

```c++
class Solution {
public:
    string minWindow(string s, string t) {
        int left=0,right=0,min_left=0,min_len=INT_MAX;
        unordered_map<char,int>count;
        for(char c:t){
            count[c]++;
        }
        int contain=t.size();
        while(right<s.size()){
            //右指针移动，移动的时候，如果这个字符在count里面的话，那我们就找到了一个，contain就可以减一了，当减到零的时候，就可以收缩左边的指针了。
            if(count[s[right++]]-->0){
                contain--;
            }
            //收缩左边的指针，直到恰好包含。
            while (contain==0){
                if(right-left<min_len){
                    min_len=right-left;
                    min_left=left;
                }
                //如果收缩的多了一个，那么久可以跳出这个收缩的条件了
                //注意这里的++,例如left++,其实使用的值是left,使用完之后才+1,count也是，是判断++之前是否就等于0了。
                if (count[s[left++]]++==0){
                    contain++;
                }
            }
        }
        return min_len==INT_MAX?"":s.substr(min_left,min_len);
    }
};
```
