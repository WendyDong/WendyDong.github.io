---
layout:     post
title:      "求数组中满足和为sum的所有组合(39) Combination Sum"
subtitle:   " DFS的问题，Backtracking的套路 "
date:       2019-11-01 13:05:00
author:     "DHH"
header-img: "img/bg/leetcodebg.jpg"
catalog: true
tags:
    - leetcode
    - Array
    - Backtracking
typora-root-url: ..\..
---

> “Yeah It's leetcode problem. ”

## 问题描述

问题很容易理解，给定一个不重复的数组，求所有满足和为sum的数组可能性（返回的内容不重复）。

Given a **set** of candidate numbers (`candidates`) **(without duplicates)** and a target number (`target`), find all unique combinations in `candidates` where the candidate numbers sums to `target`.

The **same** repeated number may be chosen from `candidates` unlimited number of times.

 The solution set must not contain duplicate combinations. 

**Example:**

```
Input: candidates = [2,3,5], target = 8,
A solution set is:
[
  [2,2,2,2],
  [2,3,3],
  [3,5]
]

Input: candidates = [2,3,6,7], target = 7,
A solution set is:
[
  [7],
  [2,2,3]
]
```

**Note:**

这个问题吧，一看就知道使用backtracking的方法求解，不过这个题需要注意的是重复的问题，例如2-下面的再可能性应该为2,3,6,7 轮到3的时候，下一个就应该是3,6,7,因为之前已经考虑过包含2的情况了，所以这里不需要在考虑，否则返回的结果中就会有重复的数值。

## 知识储备

这个题的DP思路是这样的：构建一个dp数组，里面存的内容 dp[i] 是**从0到i的最长有效长度**。能想到这一点，其实底下就好办了一点。

如果s[i]为‘（ ’， 那么dp[i]就为0。   

如果s[i]为‘)’  那么如果s[i-1]为‘（ ’，也就是说是“...()...” 这样的形式，那dp[i]=2+dp[i-2]

### 1.  vector 拷贝

   `b.assign(a.begin()+i, a.end());`

   以上代码将a中[i-结尾]的部分拷贝给了b

### 2. vector 排序

   `sort(candidates.begin(),candidates.end());`

### 3. vector 求和

   `int sum=accumulate(a.begin(),a.end(),0);`

​	定义在#include<numeric>中 , accumulate带有三个形参：头两个形参指定要累加的元素范围，第三个形参则是累加的初值 

   `string sum = accumulate(v.begin() , v.end() , string(" "));`

   这个函数调用的效果是：从空字符串开始，把vec里的每个元素连接成一个字符串。

## Backtracking代码

每次都会忘记pop_back()注意一下

### 代码1： 使用vector记录已经运行到哪个位置了，浪费空间

```c++
class Solution {
public:
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        vector<vector<int>> re;
        
        for (int i =0;i<candidates.size();i++){
            vector<int> temp;
            temp.push_back(candidates[i]);
            vector<int> temlist;
            temlist.assign(candidates.begin()+i, candidates.end());
            DFS(temlist, temp, re,target);            
        }
        return re;
    }
private:
    void DFS(vector<int> temlist, vector<int>& temp, vector<vector<int>>& re, int target){
        int cursum=accumulate(temp.begin(),temp.end(),0);
        if (cursum==target){
            re.push_back(temp);
            return;
        }
        else if(cursum>target){
            return;
        }
        else{
            for (int i =0;i<temlist.size();i++){
                temp.push_back(temlist[i]);
                vector<int> temlist2;
                temlist2.assign(temlist.begin()+i, temlist.end());
                DFS(temlist2, temp, re,target); 
                //别忘记
                temp.pop_back();
            }
        }
    }
};
```

###  代码2，改用一个int指针 记录已经遍历到哪了

```c++
class Solution {
public:
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        vector<vector<int>> re;
        vector<int> temp;
        DFS(candidates, 0, temp, re,target);            
        
        return re;
    }
private:
    void DFS(vector<int> candidates, int next, vector<int>& temp, vector<vector<int>>& re, int target){
        int cursum=accumulate(temp.begin(),temp.end(),0);
        if (cursum==target){
            re.push_back(temp);
            return;
        }
        else if(cursum>target){
            return;
        }
        else{
            for (int i =next;i<candidates.size();i++){
                temp.push_back(candidates[i]);
                DFS(candidates, i, temp, re,target); 
                //别忘记
                temp.pop_back();
            }
        }
    }
};
```

以上两个代码都是直接传递的target, 中间就需要对vector求和判断是否等于target，过于复杂，其实可以每次传递target的时候传递target-curnum, 当target=0的时候就命中了。

### 代码三

```c++
class Solution {
public:

    void search(vector<int>& num, int next, vector<int>& pSol, int target, vector<vector<int> >& result)
    {
        if(target == 0)
        {
            result.push_back(pSol);
            return;
        }
        
        if(next == num.size() || target - num[next] < 0)
            return;
            
        pSol.push_back(num[next]);
        search(num, next, pSol, target - num[next], result);
        pSol.pop_back();
        
        search(num, next + 1, pSol, target, result);
    }

    
    vector<vector<int> > combinationSum(vector<int> &num, int target) 
    {
        vector<vector<int> > result;
        sort(num.begin(), num.end());
        vector<int> pSol;
        search(num, 0, pSol, target, result);
        return result;    
    }
};
```

## DP代码

DFS一般都可以用DP啦，这里也不例外，不过不太直观，我是看discuss看到的，参考一下。

思想如下，先找到target=i时的所有组合H, 当target=j的时候，如果当前num=H[j-i], 那就把这个num加进H, 最后这个H就是target=j的时候的组合。

```c++
class Solution {
public:
	vector<vector<int> > combinationSum(vector<int> &candidates, int target) {
		vector< vector< vector<int> > > combinations(target + 1, vector<vector<int>>());
		combinations[0].push_back(vector<int>());
		for (auto& score : candidates)
			for (int j = score; j <= target; j++)
				if (combinations[j - score].size() > 0)	{
					auto tmp = combinations[j - score];
					for (auto& s : tmp)
						s.push_back(score);
					combinations[j].insert(combinations[j].end(), tmp.begin(), tmp.end());
				}
		auto ret = combinations[target];
		for (int i = 0; i < ret.size(); i++)
			sort(ret[i].begin(), ret[i].end());
		return ret;
	}
};
```