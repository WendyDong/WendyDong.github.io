---
layout:     post
title:      "全排列(46) Permutations "
subtitle:   " Backtracking 经典问题 "
date:       2019-11-04 18:40:00
author:     "DHH"
header-img: "img/bg/leetcodebg.jpg"
catalog: true
tags:
    - leetcode
    - Backtracking
typora-root-url: ..\..
---

> “Yeah It's leetcode problem. ”

## 问题描述

问题很容易理解，给定一个不重复的数组，找出所有有可能的排列组合

 Given a collection of **distinct** integers, return all possible permutations. 

**Example:**

```
Input: [1,2,3]
Output:
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]
```

**Note:**

这个数组是不重复的，数组内的数字都是distinct的，如果是有distinct的，也要注意一下

## Backtracking总结

[discuss](https://leetcode.com/problems/permutations/discuss/18239/A-general-approach-to-backtracking-questions-in-Java-(Subsets-Permutations-Combination-Sum-Palindrome-Partioning)) 来自leetcode discuss的一个总结，可以看出一下模板套路

distinct的时候，一般都是这样的

```java
public List<List<Integer>> subsets(int[] nums) {
    List<List<Integer>> list = new ArrayList<>();
    Arrays.sort(nums);
    backtrack(list, new ArrayList<>(), nums, 0);
    return list;
}

private void backtrack(List<List<Integer>> list , List<Integer> tempList, int [] nums, int start){
    list.add(new ArrayList<>(tempList));
    for(int i = start; i < nums.length; i++){
        tempList.add(nums[i]);
        backtrack(list, tempList, nums, i + 1);
        tempList.remove(tempList.size() - 1);
    }
}
```

not distinct的时候呢，就是这样的

```java
for(int i = start; i < nums.length; i++){
		//多了一行排除重复的情况，而且最前面要排序。
        if(i > start && nums[i] == nums[i-1]) continue; // skip duplicates
        tempList.add(nums[i]);
        backtrack(list, tempList, nums, i + 1);
        tempList.remove(tempList.size() - 1);
    }
```

## 代码

至于本题代码， 求n个数的全排列”, 相当于第一个位置与其他位置交换, 然后对每一个交换的结果求n-1个数的全排列, o（n!）

 例如求“1234”的全排列, 

- 当第一个数与第一个数交换时,相当于求“234”的全排列的;
- 当第一个数与第二个数交换时,相当于求“134”的全排列;
- 当第一个数与第三个数交换时,相当于求“214”的全排列;
- 当第一个数与第四个数交换时,相当于求“231”的全排列;

```c++
class Solution {
public:
    vector<vector<int> > permute(vector<int> &num) {
	    vector<vector<int> > result;
	    
	    permuteRecursive(num, 0, result);
	    return result;
    }
    
    // permute num[begin..end]
    // invariant: num[0..begin-1] have been fixed/permuted
	void permuteRecursive(vector<int> &num, int begin, vector<vector<int> > &result)	{
		if (begin >= num.size()) {
		    // one permutation instance
		    result.push_back(num);
		    return;
		}
		
		for (int i = begin; i < num.size(); i++) {
		    swap(num[begin], num[i]);
		    permuteRecursive(num, begin + 1, result);
		    // reset
		    swap(num[begin], num[i]);
		}
    }
};
```



还有一种写法如下，感觉套路更清晰一点，不过理解起来也时间复杂度也更高了o（n^n）

```c++
class Solution {
public:
    vector<vector<int>> permute(vector<int>& nums) {
        vector<vector<int>> re;
        vector<int> temp;
        DFS(nums,re,temp);
        return re;
    }
private:
    void DFS(vector<int>& nums, vector<vector<int>>& re, vector<int>& temp) {
        if (temp.size()==nums.size()){
            re.push_back(temp);
            return;
        }
        for (int i=0;i<nums.size();i++){
            if(find(temp.begin(), temp.end(),nums[i])!=temp.end()) continue; // element already exists, skip
            temp.push_back(nums[i]);
            DFS( nums,re, temp);
            temp.pop_back();
        }        
    }
};
```

