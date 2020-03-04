---
layout:     post
title:      " 解码字符串(394) Decode String "
subtitle:   " 字符串问题，弱项 "
date:       2020-01-24 12:30:00
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

给定一个编码后的字符串，对其进行解码

The encoding rule is: `k[encoded_string]`, where the *encoded_string* inside the square brackets is being repeated exactly *k* times. Note that *k* is guaranteed to be a positive integer.

You may assume that the input string is always valid; No extra white spaces, square brackets are well-formed, etc.

Furthermore, you may assume that the original data does not contain any digits and that digits are only for those repeat numbers, *k*. For example, there won't be input like `3a` or `2[4]`.

**Example:**

```
s = "3[a]2[bc]", return "aaabcbc".
s = "3[a2[c]]", return "accaccacc".
s = "2[abc]3[cd]ef", return "abcabccdcdcdef".
```

这个例子里面没有提到的，但是我在做的过程中碰到的一些常用错误例子

```
s = "3[a2[bc]]", return "abcbcabcbcabcbc".
s = "100[lee]", 我的错误返回是100lee
```

## 单纯的一个一个解析字符串

这是我的第一个思路，对每个字符解析，如果是数字就\*\*，是【就\*\*, 直接导致了错误示例中的第一个例子，返回的结果乱七八糟。后来想到想要解决这种嵌套的【【】】问题，可能还是得靠栈。所以写了如下代码

```c++
class Solution {
public:
    string decodeString(string s) {
        string result="";
        string temp="";
        stack<char> st;
        for(char c:s){
            if(c!=']'){
                st.push(c);                
            }
            else{                
                bool noEnd=true;
				while(noEnd){
					char t=st.top();
					st.pop();
					if(t!='['){
						if(isdigit(t)!=0){
							int times=t-'0';
							if(st.empty()){
								for(int i=0;i<times;i++){
									result+=temp;
								}
								temp="";
							}
							else{
                                string tt=temp;
								for(int i=0;i<times-1;i++){
									temp+=tt;
								}
							}
							noEnd=false;
						}
						else{
							temp=t+temp;
						}
					}
				}
            }             
        }
        while(!st.empty()){
            temp=st.top()+temp;
            st.pop();
        }
        return result+temp;
    }
};
```

这个代码复杂到自己看第二遍都看不明白了，虽然成功的解决了第一个错误示例，但是第二个错误又出现了，我才意识到我解析的数字是一位数字，当数字是多位的时候就会有问题。于是看了答案怎么处理字符串的，以下是正确解法。

## stack

```c++
class Solution {
public:

    string decodeString(string s) {
        stack<char> st;
        for(int i = 0; i < s.size(); i++){
            if(s[i] != ']') {
                st.push(s[i]);
            }
            else{
                string curr_str = "";
                
                while(st.top() != '['){
                    curr_str = st.top() + curr_str ;
                    st.pop();
                }
                
                st.pop();   // for '['
                string number = "";
                
                // for calculating number
                
                while(!st.empty() && isdigit(st.top())){
                    number = st.top() + number;
                    st.pop();
                }
                int k_time = stoi(number);    // convert string to number
                
                while(k_time--){
                    for(int p = 0; p < curr_str.size() ; p++)
                        st.push(curr_str[p]);
                }
            }
        }
        
        s = "";
        while(!st.empty()){
            s = st.top() + s;
            st.pop();
        }
        return s;
        
    }
};
```



和上面一份代码最大的不同就是，遇到一个中间的tempstring，如果栈还不为空，要把temp压回栈里面去，上面一个碰到“**"3[a]2[b4[F]c]"**”就会出现错误，解析成**"aaabcFFFFbcFFFF"**，应该是**"aaabFFFFcbFFFFc"**。

如果把FFFF压回去，再压入c, 就不会出现这个错误

## DFS

相应的DFS解法，也要注意，这种嵌套的时候的问题

```c++
class Solution {
public:
    string decodeString(string s) {
        string result;
        
        for(auto i=0; i < s.size(); i++) {
            if(isdigit(s[i])) result += processGroup(s, i);
            if(isalpha(s[i])) result += s[i];
        }
        return result;
    }
    
    string processGroup(const string &s, int &position) {
        string count, result, group;
        while(isdigit(s[position])) {
            count += s[position];
            position++;
        }
        int multiplier = stoi(count);
        
        position++;

        while(s[position] != ']') {
            if(isdigit(s[position])) {
                group += processGroup(s, position);
            }
            else if(isalpha(s[position]))
            {
                group += s[position];
            }
            position++;
        }

        while(multiplier > 0) {
            result += group;
            multiplier--;
        }
        
        return result;        
    }
};
```



