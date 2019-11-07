---
layout:     post
title:      "最长的有效的括号(32) Longest Valid Parentheses"
subtitle:   " dp为主 可以奇思妙想 "
date:       2019-10-28 11:04:00
author:     "DHH"
header-img: "img/bg/leetcodebg.jpg"
catalog: true
tags:
    - leetcode
    - String
    - DP
    - Inspiration
typora-root-url: ..\..
---

> “Yeah It's leetcode problem. ”

## 问题描述

问题比较简单，就是给你一个由（）这两个字符组成的字符串，求其中有效的括号组成的字符串的最长长度。

Given a string containing just the characters '(' and ')', find the length of the longest valid (well-formed) parentheses substring.

**Example:**

```
Input: ")()())"
Output: 4
Explanation: The longest valid parentheses substring is "()()"
```

**Note:**

这种问题吧，一看求有效部分的最大长度，第一反应就应该是DP了，不过这个题最开始没想好怎么DP， 就卡住了，所以现在记住这题DP的方法！！！ 谢谢！！

另外 这个题还可以用stack 和一种比较神奇的思路去做，stack可以了解一下，那个比较神奇的方法看看能记住就行啦。DP的一定要记住！

## DP

这个题的DP思路是这样的：构建一个dp数组，里面存的内容 dp[i] 是**从0到i的最长有效长度**。能想到这一点，其实底下就好办了一点。

如果s[i]为‘（ ’， 那么dp[i]就为0。   

如果s[i]为‘)’  那么如果s[i-1]为‘（ ’，也就是说是“...()...” 这样的形式，那dp[i]=2+dp[i-2]

​					 如果s[i-1] 为‘ ）’， 就是“...))...” 这样的形式，这个就有点复杂啦，我们要再往前面考虑一下，如果前面能找到（（...））这种形式，其中第二个（ 是s[i-1]那个括号的最长有效对应的左括号，如果这个左括号左边还是左括号，那这个就能构成一个有效的字符串啦，第一个左括号的位置为i-1-dp[i-1]。 所以我们再判断一下这个，同时，说不定还能跟再前面一个有效的练起来，所以满足这些条件的时候dp[i]=dp[i - dp[i - 1] - 2]+2+dp[i-1]。

注意： 代码的时候，要注意范围，例如i-2之类的，先判断一下有没有超出范围，超出范围了就要另写一下。这是我实现的时候没有注意到的。

```c++
#include <stack>
class Solution {
public:
    int longestValidParentheses(string s) {
        // way 1 DP
        if(s.size()<1){
            return 0;
        }
        int re=0;
        vector<int> dp(s.size(),0);
        dp[0]=0;
        for (int i=1;i<s.size();i++){
            if(s[i]==')'){
                if (s[i-1]==')'){
                    if(i-dp[i-1]>0 && s[i-1-dp[i-1]]=='('){
                        dp[i] = dp[i - 1] + ((i - dp[i - 1]) >= 2 ? dp[i - dp[i - 1] - 2] : 0) + 2;
                    }
                }
                else{
                    if (i>1)
                        dp[i]=dp[i-2]+2;
                    else
                        dp[i]=2;
                }
            }
            re=max(re,dp[i]);
        }
        return re;      
    }
};
```

## Stack

这种方法呢，就比较考逻辑思维啦。直接看solution的图吧[solution]( https://leetcode.com/problems/longest-valid-parentheses/solution/ )

```c++
#include <stack>
class Solution {
public:
    int longestValidParentheses(string s) {
        // way 2 stack
        stack<int> sta;
        int maxAns=0;
        sta.push(-1);
        for (int i=0;i<s.size();i++){
            if  (s[i]=='('){
                sta.push(i);
            }
            else{
                sta.pop();
                if (sta.empty()){
                    sta.push(i);
                }
                else{
                    maxAns = max(maxAns, i-sta.top());
                }
            }
        } 
        return maxAns;
    }
};
```

## two pass(节省了空间 只用了o(1))

这种方法呢，我是第一次想不到，不过可以记住，面试官问到减少空间复杂度可以回答[solution]( https://leetcode.com/problems/longest-valid-parentheses/solution/ )

思路：先从左往右遍历一次，如果左括号个数大于右括号个数，不用管，如果相等了，代表找到了一个配对，此时len=两者个数。如果小于了，就代表违反规则了，两者置0接着判断。

但是还需要从右往左来一次，为什么呢，例如（（） 由于括号的个数不是偶数，左括号右括号个数不相等，如果只从左往右就会错过（），所以要从右往左再来一次。

```c++
#include <stack>
class Solution {
public:
    int longestValidParentheses(string s) {
        // way 3 Inspiration 一个很神奇的解法
        int left, right, maxAns;
	    left = right = maxAns = 0;
        for (int i=0;i<s.size();i++){
            if (s[i]=='(')
                left++;          
            else
                right++;
            if(left==right){
                maxAns=max(maxAns, left*2);
            }
            else if(right>left){
                left=right=0;
            }
        }
        left=right=0;
        for (int i=s.size()-1;i>-1;i--){
            if (s[i]=='(')
                left++;          
            else
                right++;
            if(left==right){
                cout<<left;
                maxAns=max(maxAns, left*2);
            }
            else if(right<left){
                left=right=0;
            }
        }
        return maxAns;
    }
};
```