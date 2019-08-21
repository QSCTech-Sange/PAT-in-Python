---
title: 可能是全网唯一的 PAT 甲级题解 in Python3
date: 2019-08-13 21:48:47
tags: 
- PAT 
- 序言
categories: PAT
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 我会用 C++ 和 Python3 两种语言的方式来趣味性地讲解 PAT 甲级的题目。
---

最新内容会更新到[我的博客](https://qsctech-sange.github.io/PAT-Preface.html)，里面还有好多好玩的东西，一起来康康吧！

## PAT

[PAT]([https://www.patest.cn](https://www.patest.cn/)) 是什么呢？

> 浙江大学计算机程序设计能力考试（Programming Ability Test，简称PAT）是由浙江大学计算机科学与技术学院组织的统一考试。旨在培养和展现学生分析问题、解决问题和计算机程序设计的能力，科学评价计算机程序设计人才，并为企业选拔人才提供参考标准。

尽管 PAT 考试自称“IT届的托福”略有夸张，但是 PAT 的含金量还是有目共睹的。

在接下来的时间里，我会主攻 PAT 甲级的题目，并尽自己最大所能，来写出通俗易懂且内容翔实的题解。



## 题解内容

我会分析题目，给出思路，然后使用 `Python3` 和 `C++` 两种语言来给出AC代码。

为什么使用两种语言呢？

+ `Python3` 越来越通用，被接受，正如算法届对 `Java` 的鄙夷随着`Java`的流行而销声匿迹，使用`Python3` 来写算法题也即将是大势所趋。 
+ `Python3` 语法更简单，可以更好地理解算法本身，而不是关注语法的细致末节。
+ PAT 虽然支持 `Python3`，但是对于时间和内存限制并没有对 `Python3` 作优化。同样的题目，同样的算法， `C++ `能通过的，`Python3`  往往好几个点内存超限或者时间超时。（LeetCode就不会这样，点名批评PAT）

**所以**，我建议使用 `Python3` 来理解算法，用 `C++` 来做题。文体两开花。

**同时建议**，尽量上 PAT 官网上刷而不要在牛客上刷，因为牛客的测试点往往比 PAT 的要简陋很多，有时候 PAT AC的代码，在 PAT 上只有几个点能过。



## 创新

+ 大多数目前题解都是 `C++` 的。我给出了 `Python3` 的思路和做法。
+ 应该是全网**唯一**且最新的`Python3`版本的PTA题解
+ 我的题解写得更风趣幽默（如果你信的话）
+ 我的代码会尽可能多注释和更优雅



## 考试做题建议

+ 有暴力解思路的先用C++暴力解先过一遍，有时候使用C的暴力解能AC
+ 使用Python先写一遍代码，有时候Python能ac
+ 再者，就把刚刚的Python代码翻译成c++
+ 字符串的问题就全部用Python

> 为什么先Python后C++？因为Python写起来更顺应思路，不用考虑指针之类的细致末节，所以写出一个解的时间要大大小于c++。而有了思路以后，将python代码翻译成c++其实并不需要多少时间，最多就五到十分钟。对于总共就四个大题的pat来说，显然是值得的。



> 立即戳 [这里](https://qsctech-sange.github.io/categories/PAT/) 开始你的PAT大冒险吧！

