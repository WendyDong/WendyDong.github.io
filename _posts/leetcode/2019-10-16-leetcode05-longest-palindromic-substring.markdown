---
layout:     post
title:      "最长的palindromic子字符串问题"
subtitle:   " DP问题 马拉车算法(MANACHER'S ALGORITHM)"
date:       2019-10-16 20:45:00
author:     "DHH"
header-img: "img/bg/leetcodebg.jpg"
catalog: true
tags:
    - leetcode
    - DP
    - MANACHER'S
typora-root-url: ..\..
---

> “Yeah It's leetcode problem. ”

## 问题描述
Given a string s, find the longest palindromic substring in s. You may assume that the maximum length of s is 1000.
## DP方案（n2复杂度）
此问题可以使用动态规划的方式解决，动态规划状态表达为

![05-dp](/img/leetcode/05-dp.png)

基于上图，c++实现代码为：

```c++
string longestPalindrome(string s) {
    vector<vector<bool>> a(s.length(),vector<bool>(s.length()));
    int maxv=-1;
    int start=0;
    for (int i=0;i<s.length();i++){
        for (int j=0;j<=i;j++){
            if(s[i] == s[j] && (i - j <= 2 || a[j+1][i-1])) {
                a[j][i]=true;
                if(maxv<i-j+1){
                    maxv=i-j+1;
                    start=j;
                }
            }                
        }
    }
    return s.substr(start,maxv);
}
```



## 中心点算法（n2复杂度）

具体做法为：遍历2n+1个中心点（包含每个字符以及字符之间的间隔），为每个中心点寻找他的最大回文字符串。

* 优点：计算简单

* 缺点：有重复计算的内容。例如已经算了以2为中心点的回文（半径为2），计算以3为中心点的回文半径时无法合理利用这一信息

java实现代码(来自solution)

```java
public String longestPalindrome(String s) {
    if (s == null || s.length() < 1) return "";
    int start = 0, end = 0;
    for (int i = 0; i < s.length(); i++) {
        int len1 = expandAroundCenter(s, i, i);
        int len2 = expandAroundCenter(s, i, i + 1);
        int len = Math.max(len1, len2);
        if (len > end - start) {
            start = i - (len - 1) / 2;
            end = i + len / 2;
        }
    }
    return s.substring(start, end + 1);
}

private int expandAroundCenter(String s, int left, int right) {
    int L = left, R = right;
    while (L >= 0 && R < s.length() && s.charAt(L) == s.charAt(R)) {
        L--;
        R++;
    }
    return R - L - 1;
}
```



## 马拉车算法 MANACHER'S ALGORITHM（线性复杂度）

参考自文章[blog](https://www.cnblogs.com/grandyang/p/4475985.html)

马拉车算法利用前面算过的回文字符串信息去算接下来的，主要几点思想是

由于回文串的长度可奇可偶，比如 "bob" 是奇数形式的回文，"noon" 就是偶数形式的回文，马拉车算法的第一步是预处理，做法是在每一个字符的左右都加上一个特殊字符，比如加上 '#'，那么

bob   -->   #b#o#b#

noon   -->   #n#o#o#n# 

* 处理之后得到的字符串的个数都是奇数个 
*  需要在前面增加一个字符 ，加上之后，规律为：最长子串的长度是半径减1，起始位置是中间位置减去半径再除以2。 例如 "noon"，中间的 '#' 在字符串 "$#n#o#o#n#" 中的位置是5，半径也是5，相减并除以2还是0 

求解p数组：

 需要新增两个辅助变量 mx 和 id，其中 id 为能延伸到最右端的位置的那个回文子串的中心点位置，mx 是回文串能延伸到的最右端的位置，需要注意的是，这个 mx 位置的字符不属于回文串，所以才能用 mx-i 来更新 p[i] 的长度而不用加1，由 mx 的更新方式 mx = i + p[i] 也能看出来 mx 是不在回文串范围内的，这个算法的最核心的一行如下： 

`p[i] = mx > i ? min(p[2 * id - i], mx - i) : 1;`

如果 mx > i, 则 p[i] = min( p[2 * id - i] , mx - i )

否则，p[i] = 1

当 mx - i > P[j] 的时候，以 S[j] 为中心的回文子串包含在以 S[id] 为中心的回文子串中，由于 i 和 j 对称，以 S[i] 为中心的回文子串必然包含在以 S[id] 为中心的回文子串中，所以必有 P[i] = P[j]，其中 j = 2*id - i，因为 j 到 id 之间到距离等于 id 到 i 之间到距离，为 i - id，所以 j = id - (i - id) = 2*id - i，参见下图。

![05-mlc-01](/img/leetcode/05-mlc-01.png)

 当 P[j] >= mx - i 的时候，以 S[j] 为中心的回文子串不一定完全包含于以 S[id] 为中心的回文子串中，但是基于对称性可知，下图中两个绿框所包围的部分是相同的，也就是说以 S[i] 为中心的回文子串，其向右至少会扩张到 mx 的位置，也就是说 P[i] = mx - i。至于 mx 之后的部分是否对称，就只能老老实实去匹配了，这就是后面紧跟到 while 循环的作用。 

![05-mlc-02](/img/leetcode/05-mlc-02.png)

 对于 mx <= i 的情况，无法对 P[i] 做更多的假设，只能 P[i] = 1，然后再去匹配了。 

c++实现代码：

```c++
string longestPalindrome(string s) {
	string t = "$#";
    for (int i = 0; i < s.size(); ++i) {
        t += s[i];
        t += "#";
    }
    // Process t
    vector<int> p(t.size(), 0);
    int mx = 0, id = 0, resLen = 0, resCenter = 0;
    for (int i=1; i<t.size(); ++i){
        p[i] = mx > i ? min(p[2 * id - i], mx - i) : 1;
        while (t[i + p[i]] == t[i - p[i]]) ++p[i];
        if (mx < i + p[i]) {
            mx = i + p[i];
            id = i;
        }
        if (resLen < p[i]) {
            resLen = p[i];
            resCenter = i;
        }
    }
    return s.substr((resCenter - resLen) / 2, resLen - 1);
}
```

