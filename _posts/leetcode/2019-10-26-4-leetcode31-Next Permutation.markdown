---
layout:     post
title:      "下一个更大的全排列(31) Next Permutation"
subtitle:   " arrary 需要例子试验一下 "
date:       2019-10-26 20:00:00
author:     "DHH"
header-img: "img/bg/leetcodebg.jpg"
catalog: true
tags:
    - leetcode
    - Array
    - Inspiration
typora-root-url: ..\..
---

> “Yeah It's leetcode problem. ”

## 问题描述

这一题最开始理解题意就花了一段时间，没看懂什么意思，这里先把英文题目放上来

Implement **next permutation（排列）**, which rearranges numbers into the lexicographically next greater permutation of numbers.

If such arrangement is not possible, it must rearrange it as the lowest possible order (ie, sorted in ascending order).

The replacement must be **[in-place](http://en.wikipedia.org/wiki/In-place_algorithm)** and use only constant extra memory.

Here are some examples. Inputs are in the left-hand column and its corresponding outputs are in the right-hand column.

**Example:**

```
1,2,3 → 1,3,2
3,2,1 → 1,2,3
1,1,5 → 1,5,1
```

**Note:**

理解了这个题目的意思其实就很简单了（也不算简单吧，就看能不能想得到了）

这个题其实是说，给你一个排列 例如 123，这三个数字能构成3\*2\*1种排列，找到这那么多种排列中，比123大的下一个排列。

看到这个题目之后，应该这样想，既然要找下一个更大的，那肯定是要从后往前找，例如123，那肯定是把3和2换一下位置，而不是动1的位置，确定了之后，自己写一个例子 例如

1 3 8 5 2， 如果是这个例子的话，从后往前看 第一个是2,5比2大，如果把2 5交换没啥用，继续往前看，直到看到了3，发现3比8小，这个地方是个突破口，如果把8和3换一下，就是一个比现在大的序列。但是是不是next呢？ 明显不是，例如把5和3换，明显比把8和3换更小更接近next， 但是也不能用2和3换，因为换了之后变小了，所以我们把5 3 换位置，变成1 5 8 3 2 ，这就是next了吗，我们发现， 本来的3后面是个降序排列，我们给这个降序排列中恰好比3大的数字跟3换了位置后之后，还是个降序排列，所以现在5的后面还是个降序排列，如果是升序排列的话，就是next了！

我这个例子找的比较恰好，第一次找的例子是12854，当时就想着把4和2换了就行了，再把后面的逆序一下，发现就正好，没有想过为啥是4，所以得出的错误结论就是换掉最后那个。所以要多找几个例子，感受一下规律。

特殊情况：

长度为1，可以直接返回

如果数组本来是个降序数组，已经最大了，就直接逆序就好了。

## 知识储备

先遍历一遍这个链表，计算出来链表的长度，判断出应该正向删除第几个指针，将head移动到要删除的指针的前一个位置，删除要删除的指针即可。

#### 1、C++ reverse函数

  \#include<algorithm> 

 reverse(beg,end)  会将区间[beg,end)内的元素全部逆序 ，

 例如 reverse(nums.begin(), nums.end()); reverse(nums.begin()+count, nums.end());

#### 2、C++ swap函数

 \#include<std> 

例如 swap(nums[count],nums[i]);

#### 3、C++  [C++ lower_bound 和upper_bound](https://www.cnblogs.com/cunyusup/p/8438749.html) 

 \#include  <algorithm> 

 二分查找的函数有 3 个  二分查找的首要条件是数列有序！ ： 

##### 		1.lower_bound(起始地址，结束地址，要查找的数值) 

​		函数lower_bound()在first和last中的前闭后开区间进行二分查找，返回**大于或等于val的第一个元素位置**。如果所有元素都小于val，则返回last的位置.

​		注意：如果所有元素都小于val，则返回last的位置，且last的位置是**越界**的！！

##### 		2.upper_bound(起始地址，结束地址，要查找的数值) 

功能：函数upper_bound()返回的在前闭后开区间查找的关键字的上界，返回**大于val**的第一个元素位置

注意：返回查找元素的最后一个可安插位置，也就是“元素值>查找值”的第一个元素的位置。同样，如果val大于数组中全部元素，返回的是last。(注意：数组下标越界)

##### 		3.binary_search(起始地址，结束地址，要查找的数值)  返回的是是否存在这么一个数，是一个bool值。

例如auto itr = upper_bound(begin(num)+i+1, end(num), num[i]);

再例如

```
int a[6] = {0,1,2,4,5,7};
int position1 = lower_bound(a+1,a+n,3) - a; //position1 = 3
int position2 = upper_bound(a+1,a+n,3) - a; //position2 = 5 
```

 需要注意的是：如果数组中没有找到所求元素，函数就会返回一个假想的插入位置。 

```
int a[6] = {0,1,2,4,5,7};
int position1 = lower_bound(a+1,a+6,3) - a;
int position2 = upper_bound(a+1,a+6,3) - a;
cout << position1 << endl;
cout << position2 << endl;
// position1 = position2 = 3
```

## 代码

```c++
class Solution {
public:
    void nextPermutation(vector<int>& nums) {
        int len = nums.size();
        if (len<2){
            return;
        }
        int count=len-1;
        for (count = len - 2; count >= 0; count--) {
            if (nums[count] < nums[count + 1]) {
                break;
            }
        }
        if (count<0){
            reverse(nums.begin(), nums.end());
        }
        else{
            // put nums[len-1] to the count'th position
            int i;
            for (i=len-1;i>count-1;i--){
                if (nums[i]>nums[count])
                    break;
            }
            swap(nums[count],nums[i]);
            cout<<count<<i<<endl;
            reverse(nums.begin()+count+1, nums.end());
            return;
        }
        
    }
};
```

