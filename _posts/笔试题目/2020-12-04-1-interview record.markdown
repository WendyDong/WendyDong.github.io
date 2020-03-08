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

1. BN的好处 

   加快训练速度，这样我们就可以使用较大的学习率来训练网络。
   
   提高网络的泛化能力。（没回答出来）
   
2. resnet为什么有效[参考](https://www.cnblogs.com/shine-lee/p/12363488.html#resnet%E8%A6%81%E8%A7%A3%E5%86%B3%E7%9A%84%E6%98%AF%E4%BB%80%E4%B9%88%E9%97%AE%E9%A2%98)

   ResNets要解决的是深度神经网络的“退化”问题。ResNet的作者从后者入手，探求更好的模型结构。将堆叠的几层layer称之为一个block，对于某个block，其可以拟合的函数为F(x)，如果期望的潜在映射为H(x)，与其让F(x) 直接学习潜在的映射，不如去学习残差H(x)−x，即F(x):=H(x)−x，这样原本的前向路径上就变成了F(x)+x，用F(x)+x来拟合H(x)H(x)。作者认为这样可能更易于优化，因为相比于让F(x)学习成恒等映射，让F(x)学习成0要更加容易——后者通过L2正则就可以轻松实现。这样，对于冗余的block，只需F(x)→0F(x)→0就可以得到恒等映射，性能不减。

3. inception之间的区别 v2? 为什么不用v3

4.  map recall precision [参考](https://blog.csdn.net/mdjxy63/article/details/79822555)

5. Focal loss 有几个参数可以调节?

   两个，一个用于控制样本的困难程度的权重，一个用于控制样本的数量对应的权重

6. NMS复杂度，多个类别的时候是先算单类别，还是混合在一起？

   混合在一起

7. Sigmoid（多标签分类）和softmax(多分类）有什么不同