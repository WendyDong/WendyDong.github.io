---
layout:     post
title:      " 面经记录 "
subtitle:   " 面经记录 "
date:       2020-12-04 17:09:00
author:     "DHH"
header-img: "img/bg/leetcodebg.jpg"
catalog: true
tags:
    - 面经
    - 计算机视觉
typora-root-url: ..\..
---

> “Yeah It's leetcode problem. ”

## 看过的

1. BN的好处 

   加快训练速度，这样我们就可以使用较大的学习率来训练网络。
   
   提高网络的泛化能力。（没回答出来）
   
2. resnet为什么有效[参考](https://www.cnblogs.com/shine-lee/p/12363488.html#resnet%E8%A6%81%E8%A7%A3%E5%86%B3%E7%9A%84%E6%98%AF%E4%BB%80%E4%B9%88%E9%97%AE%E9%A2%98)

   ResNets要解决的是深度神经网络的“退化”问题。ResNet的作者从后者入手，探求更好的模型结构。将堆叠的几层layer称之为一个block，对于某个block，其可以拟合的函数为F(x)，如果期望的潜在映射为H(x)，与其让F(x) 直接学习潜在的映射，不如去学习残差H(x)−x，即F(x):=H(x)−x，这样原本的前向路径上就变成了F(x)+x，用F(x)+x来拟合H(x)H(x)。作者认为这样可能更易于优化，因为相比于让F(x)学习成恒等映射，让F(x)学习成0要更加容易——后者通过L2正则就可以轻松实现。这样，对于冗余的block，只需F(x)→0F(x)→0就可以得到恒等映射，性能不减。

3. inception之间的区别 v2? 为什么不用v3

4.  map recall precision [参考](https://blog.csdn.net/mdjxy63/article/details/79822555)

5. Focal loss 有几个参数可以调节?（手写）

   两个，一个用于控制样本的困难程度的权重，一个用于控制样本的数量对应的权重

6. NMS复杂度，多个类别的时候是先算单类别，还是混合在一起？

   混合在一起

7. Sigmoid（多标签分类）和softmax(多分类）有什么不同

8. 快排的优化

   **随机选取基准**；**三数取中**

   **当待排序序列的长度分割到一定大小后，使用插入排序**

   **在一次分割结束后，可以把与Key相等的元素聚在一起**

## 还需要看的

CDVS shuffle 

最长无重复子串 大数加法 开方 nms

矩阵旋转任意角度

第一步可以走一格，第二部2,走到k格最少多少步

200个球 100个白，100个黑 拿球可以拿两个 最后剩下两个黑的概率怎么算

疯狗[answer](https://www.jisilu.cn/question/269059)，[毒水](https://blog.csdn.net/jxlincong/article/details/45014227)，海盗分金币[超过半数](https://www.jianshu.com/p/ab2f71802733) [等于半数](https://blog.csdn.net/u013487601/article/details/102614263)，两根蜡烛，三门问题等。

经验风险、期望风险、结构风险

扔鸡蛋问题（了解到O（kn）的做法即可）

分层采样和蓄水池采样，O（N）的洗牌算法

手动实现堆排序

64匹马，八个赛道，找出最快的四匹，最坏情况下最少要比多少次（更常见的是25匹马，5个赛道找出最快的3匹）。

为什么LR权重可以全部初始化为0，NN不行

抛均匀硬币，到两面都出现过停下来。求次数的期望

  答：3次（[参考](https://zhidao.baidu.com/question/1447009134549964940.html)）

超大数据中寻找中位数

写交叉熵和KL散度的公式

1. linux的一些操作：如何配置环境变量，查看文件夹里文件数目，计算文件夹的大小
2. pytorch的多卡怎么做，混合精度运算

袋子里一共有M个白球，每次取1个后刷成红色放回，如果为红色则直接放回，问M个全红的期望次数？（M*(1+1/2+1/3+...+1/M)）

  答案： M*(ln(M)+O(1)) 分析： E(1)=1 //拿到第一个白球并将它涂红的期望时间 E(2)=M/M-1 //拿到第2个白球并将它涂红的期望时间 E(3)=M/M-2 //拿到第3个白球并将它涂红的期望时间 ... E(M)=M/1 //拿到第M个白球并将它涂红的期望时间 E(total)=E(1)+E(2)+...+E(M)=M(1+1/2+1/3+...+1/M)=M*(ln(M)+O(1))

开关灯问题[答案](https://www.cnblogs.com/lightwindy/p/9650687.html)

快速排序（三路）

[旋转数组](https://leetcode-cn.com/problems/rotate-array/)，尽可能多的方法，更少的时间、空间复杂度

[无重复字符的最长子串](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

求一维数组中，子序列和最大 O(n) [链接](https://www.cnblogs.com/allzy/p/5162815.html)

求二维数组中，子矩阵和最大 O(n^3) [链接](https://blog.csdn.net/qq_18343569/article/details/52066380)

求一维数组中，数字为复数的情况下，子序列最大 （时间不够，没让我做）

