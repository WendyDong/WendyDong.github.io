---
layout:     post
title:      " C++基础知识 "
subtitle:   " C++基本用法 "
date:       2020-12-04 16:09:00
author:     "DHH"
header-img: "img/bg/leetcodebg.jpg"
catalog: true
tags:
    - leetcode
    - Basic
typora-root-url: ..\..
---

> “Yeah It's leetcode problem. ”

### 子数组问题

#### 前缀和解决+sliding window

​		[leetcode 862](https://leetcode.com/problems/shortest-subarray-with-sum-at-least-k/) Shortest Subarray with Sum at Least K（其中discuss里面有个[解法](https://leetcode.com/problems/shortest-subarray-with-sum-at-least-k/discuss/151169/my-solution-O(nlgn)-use-segment-tree)用[线段树](http://dongxicheng.org/structure/segment-tree/)做的）

​		[leetcode 325](https://leetcode.com/problems/maximum-size-subarray-sum-equals-k/) Maximum Size Subarray Sum Equals k

​		[leetcode 713](https://leetcode.com/problems/subarray-product-less-than-k/) Subarray Product Less Than K

​		[leetcode 1292](https://leetcode.com/problems/maximum-side-length-of-a-square-with-sum-less-than-or-equal-to-threshold/) Maximum Side Length of a Square with Sum Less than or Equal to Threshold (这个题是二维矩阵，跟上面俩差不多)

​		[leetcode 1074](https://leetcode.com/problems/number-of-submatrices-that-sum-to-target/) 二维矩阵的sliding window

​		[leetcode 239](https://leetcode.com/problems/sliding-window-maximum/) Sliding Window Maximum

#### 二分查找

​		[leetcode 410](https://leetcode.com/problems/split-array-largest-sum/) Split Array Largest Sum 将一个数组分为m个连续的子数组，最小化其最大子数组和（可以用dp或者二分解决）

​		[leetcode 1231](https://leetcode.com/problems/divide-chocolate/) Divide Chocolate 跟410题一样

​		[leetcode 300](https://leetcode.com/problems/longest-increasing-subsequence/)  Longest Increasing Subsequence 经典题目，求最长的递增子串LIS。可以用dp做，复杂度比较高o(n^2)。用一个candidate数组模拟这个最长子串（o(nlgn)）。例如已有的一个LIS为0 5 ，现在来了一个新的数字，3，因为3是小于5的，所以3的加入不能带来最终结果长度的增长，但是可以带来现有的优化，把现有的0 5 替换为0 3 ，如果以后来了一个新的数字4 ，就可以变成 0 3 4 ，带来增长，所以我们可以维护一个这样的candidate，这个candidate的最终的长度就是LIS的最终长度，但是candidate的内容并不有效。 如果现有candidate为 0 2 5 7，来了一个新的数字3 ，我们要把5换成3，所以candidate的内容其实并不是一个LIS,只是最长长度与LIS的最长长度相同。

​		[leetcode 1011 ](https://leetcode.com/problems/capacity-to-ship-packages-within-d-days/) *Capacity To Ship Packages Within D Days*

#### DP

​		[leetcode 659](https://leetcode.com/problems/split-array-into-consecutive-subsequences/) Split Array into Consecutive Subsequences, 判断数组能不能被拆分成几个连续的子数组，例如234456就可以。

​		[leetcode 717](https://leetcode.com/problems/minimum-window-subsequence/) Minimum Window Subsequence [最小窗口序列](https://www.cnblogs.com/grandyang/p/8684817.html)问题，判断S中的最小window W，使得T是W的子序列，即T中字符出现的顺序要跟W一样。如果不要求顺序一致的话，就是底下那个滑动窗口的第76题

​		[leetcode 494](https://leetcode.com/problems/target-sum/) Target Sum

​		[leetcode 1048](https://leetcode.com/problems/longest-string-chain/) Longest String Chain 从字符list中挑选出word，使得word依次比上一个多一个字符，求能组成的最大长度。跟300有点像。

​		[leetcode 416](https://leetcode.com/problems/partition-equal-subset-sum/)  Partition Equal Subset Sum 变相背包问题 

​		[leetcode 309](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/submissions/) Best Time to Buy and Sell Stock with Cooldown 状态转移的dp

​		[leetcode 152](https://leetcode.com/problems/maximum-product-subarray/) Maximum Product Subarray, 将DP分成正负来看。





#### 滑动窗口

​		[leetcode 76](https://leetcode.com/problems/minimum-window-substring/) Minimum Window Substring

​		[leetcode 30](https://leetcode.com/problems/substring-with-concatenation-of-all-words) Substring with Concatenation of All Words 和76很像

​		

### 图的问题

​		[leetcode1168](https://leetcode.com/problems/optimize-water-distribution-in-a-village/) 最小生成树（参考链接：[prim和Kruskal算法](https://www.cnblogs.com/ggzhangxiaochao/p/9070873.html) , [邻接表和邻接矩阵的区别](https://blog.csdn.net/curry___/article/details/81742727)）

​		[leetcode 465](https://leetcode.com/problems/optimal-account-balancing/) Optimal Account Balancing

​		[leetcode 207](https://leetcode.com/problems/course-schedule/) Course Schedule DFS的做法多看一下

​		[leetcode 444](https://leetcode.com/problems/sequence-reconstruction/) Sequence Reconstruction [拓扑排序](https://www.cnblogs.com/bigsai/p/11489260.html) 

​		[leetcode 737](https://leetcode.com/problems/sentence-similarity-ii/submissions/) Sentence Similarity II 一个DSU 问题 熟悉一下union-set的写法 [by-rank](https://www.jianshu.com/p/b5b8d488266e)

​		[leetcode 743](https://leetcode.com/problems/network-delay-time/) Network Delay Time 有向图

​		[Pacific Atlantic Water Flow](https://leetcode.com/problems/pacific-atlantic-water-flow) 一个比较复杂的图的遍历问题。

​		[leetcode 128](https://leetcode.com/problems/longest-consecutive-sequence/) Longest Consecutive Sequence 一个union find比较特殊的例子



### MinMax问题

​		[leetcode843](https://leetcode.com/problems/guess-the-word/) Guess the Word



### 不容易想

​		[leetcode 1153](https://leetcode.com/problems/string-transforms-into-another-string/) String Transforms Into Another String (注意最后判断是否小于26)

​		[leetcode 1088](https://leetcode.com/problems/confusing-number-ii/) Confusing Number II（dfs）

​		[leetcode 732](https://leetcode.com/problems/my-calendar-iii/) My Calendar III (用start区间和end区间解决)

### 二维矩阵

​		[leetcode 317](https://leetcode.com/problems/shortest-distance-from-all-buildings/)  Shortest Distance from All Buildings

​		[leetcode 363](https://leetcode.com/problems/max-sum-of-rectangle-no-larger-than-k/) Max Sum of Rectangle No Larger Than K：在矩阵中找到一个最大的和的子矩阵，并且和不能超过K(类似题目：在数组中找到子数组，和最大且不超过K。[o(nlgn)解法](https://www.quora.com/Given-an-array-of-integers-A-and-an-integer-k-find-a-subarray-that-contains-the-largest-sum-subject-to-a-constraint-that-the-sum-is-less-than-k))

​	[leetcode 48](https://leetcode.com/problems/rotate-image/) Rotate Image 图片顺时针旋转，可以先上下翻转，再转置，逆时针旋转：先左右翻转，再转置。



### 树的DFS

​		[leetcode 314](https://leetcode.com/problems/count-complete-tree-nodes/) Count Complete Tree Nodes 计算二叉完全树的节点个数

### 树的BFS

​		[leetcode 117](https://leetcode.com/problems/populating-next-right-pointers-in-each-node-ii) Populating Next Right Pointers in Each Node II

​		[leetcode 863](https://leetcode.com/problems/all-nodes-distance-k-in-binary-tree) All Nodes Distance K in Binary Tree

### String

​		[leetcode 726](https://leetcode.com/problems/number-of-atoms/) Number of Atoms 多个括号嵌套的问题，可以用dfs 或者stack解决。我觉得dfs更直观

​		**[leetcode 772](https://leetcode.com/problems/basic-calculator-iii/) Basic Calculator III** **自己做一遍！！！**

​		[leetcode 394](https://leetcode.com/problems/decode-string/) Decode String 跟上个题差不多 更基础点 也是嵌套问题。

​		[leetcode 642](https://leetcode.com/problems/design-search-autocomplete-system/) Design Search Autocomplete System

​		[leetcode 299](https://leetcode.com/problems/bulls-and-cows/) Bulls and Cows

​		[leetcode 336](https://leetcode.com/problems/palindrome-pairs/) Palindrome Pairs 可以用hashtable, 也可以用字典树

### SORT

 		[leetcode 148](https://leetcode.com/problems/sort-list/) Sort List 对一个链表进行排序，时间复杂度为o(nlgn)，空间复杂度为o(1) 。应该自然想到需要使用归并排序。

​		[leetcode 315](https://leetcode.com/problems/count-of-smaller-numbers-after-self/) Count of Smaller Numbers After Self

### 数组

​		[leetcode 846](https://leetcode.com/problems/hand-of-straights/)  Hand of Straights

​		[leetcode 4](https://leetcode.com/problems/median-of-two-sorted-arrays/) Median of Two Sorted Arrays

​		[leetcode 31](https://leetcode.com/problems/next-permutation/) Next Permutation

​		[largest rectangle in histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/solution/) 用栈解决

​		[Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals) merge intervals

### 链表

​		[leetcode 430](https://leetcode.com/problems/flatten-a-multilevel-doubly-linked-list/) *Flatten a Multilevel Doubly Linked List* 

​		[leetcode 2](https://leetcode.com/problems/add-two-numbers/) Add Two Numbers

​		[leetcode 23](https://leetcode.com/problems/merge-k-sorted-lists/) Merge k Sorted Lists

​		[leetcode 148](https://leetcode.com/problems/sort-list/) Sort List

​		[leetcode 143](https://leetcode.com/problems/reorder-list/) Reorder List

### backtracking

​		[leetcode 1219](https://leetcode.com/problems/path-with-maximum-gold/) Path with Maximum Gold

​		[leetcode 301](https://leetcode.com/problems/remove-invalid-parentheses/) Remove Invalid Parentheses



### Top K Elements

1. [Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array) 堆排序->**快排**
2. [Kth Smallest Element in a BST](https://leetcode.com/problems/kth-smallest-element-in-a-bst) 利用二叉树的中序遍历->大量数据时可参考**[146 题](https://leetcode.com/problems/lru-cache/) LRU用list解决**
3. [Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements) 用优先队列对频率排序
4. [Sort Characters By Frequency](https://leetcode.com/problems/sort-characters-by-frequency) 跟上一个类似
5. **[Course Schedule III](https://leetcode.com/problems/course-schedule-iii)** 用堆解决 跟[Maximum Number of Events That Can Be Attended](https://leetcode.com/problems/maximum-number-of-events-that-can-be-attended/) 有点相似
6. [Find K Closest Elements](https://leetcode.com/problems/find-k-closest-elements) 二分查找的感觉
7. [Reorganize String](https://leetcode.com/problems/reorganize-string) 先计算频率再利用优先队列
8. [Maximum Frequency Stack](https://leetcode.com/problems/maximum-frequency-stack) 两个map实现，一个放数字-频率，一个是频率-对应的stack
9. [K Closest Points to Origin](https://leetcode.com/problems/k-closest-points-to-origin) 跟第一个很像，相当于求最小的k个元素（可以无序），所以也是两种做法，找到第k个位置就行。
10. [Kth Smallest Element in a Sorted Matrix](https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix) 二分



### 递增栈

​	蓄水池 最大矩形面积，

​	[leetcode 84](https://leetcode.com/problems/largest-rectangle-in-histogram/) Largest Rectangle in Histogram直方图面积

​	[leetcode 85](https://leetcode.com/problems/maximal-rectangle/) Maximal Rectangle直方图面积的二维版本

