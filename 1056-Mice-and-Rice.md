---
title: <Python><PAT> 1056 Mice and Rice
date: 2019-08-28 11:20:59
tags: 
- PAT
- 题解
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 非常恶心的题，题目意思属实抽象嗷。
---

# 题目

**Mice and Rice** is the name of a programming contest in which each programmer must write a piece of code to control the movements of a mouse in a given map. The goal of each mouse is to eat as much rice as possible in order to become a FatMouse.

First the playing order is randomly decided for $N_{P}$ programmers. Then every $N_{G}$ programmers are grouped in a match. The fattest mouse in a group wins and enters the next turn. All the losers in this turn are ranked the same. Every  $N_{G}$ winners are then grouped in the next match until a final winner is determined.

For the sake of simplicity, assume that the weight of each mouse is fixed once the programmer submits his/her code. Given the weights of all the mice and the initial playing order, you are supposed to output the ranks for the programmers.

### Input Specification:

Each input file contains one test case. For each case, the first line contains 2 positive integers: $N_{P}$ and $N_{G}( \leq 1000)$, the number of programmers and the maximum number of mice in a group, respectively. If there are less than $N_{G}$ mice at the end of the player's list, then all the mice left will be put into the last group. The second line contains $N_{P}$ distinct non-negative numbers $W_{i}\left(i=0, \cdots, N_{P}-1\right)$ where each $W_{i}$ is the weight of the *i*-th mouse respectively. The third line gives the initial playing order which is a permutation of $0, \cdots, N_{P}-1$(assume that the programmers are numbered from 0 to $N_{P}-1$). All the numbers in a line are separated by a space.

### Output Specification:

For each test case, print the final ranks in a line. The *i*-th number is the rank of the *i*-th programmer, and all the numbers must be separated by a space, with no extra space at the end of the line.

### Sample Input:

```in
11 3
25 18 0 46 37 3 19 22 57 56 10
6 0 8 7 10 5 9 1 4 2 3
```

### Sample Output:

```out
5 5 5 2 5 5 5 3 1 3 5
```

# 题解

## 思路

+ 题目意思真的太抽象了

+ 我来慢慢给你捋一捋

+ 题目先给你老鼠总数，和每个小组有几只老鼠

+ 接着呢，给你一串数，是每个老鼠的体重

+ 自然，这些数的出现顺序是依照老鼠的id下标的

+ 也就是说，给出来的一串数，下标是id，值是这个id的老鼠的体重，id从0开始计

+ 接着呢，给出一长串数，这串数是出场顺序

+ 注意咯！这给出的数，下标是id，值是出场顺序。

+ 拿题目的例子来说的话就是，给定下面这些数

+ ```
  11 3
  25 18 0 46 37 3 19 22 57 56 10
  6 0 8 7 10 5 9 1 4 2 3
  ```

+ 第0个老鼠体重是25,它在第6个出场。

+ 第1个老鼠体重是18,它在第0个出场。

+ 题目的意思是，按出场顺序，按分组个数对它们分组。

+ 像这题的分组个数是3

+ 那么第1号老鼠，第7号老鼠，第九号老鼠的出场顺序是`[0,1,2]`，它们就分为一组，以此类推。

+ 到最后剩下两只老鼠不足3个，那么把它们分到一组（题目里说和最后一组合并，是大骗子）。

+ 所以我们现在拥有四组老鼠。

+ 在每一组老鼠里面，选出一个最大的。

+ 即按照最后一行，每三个每三个分割，将值映射到体重，取最大的。

+ 对这题来说，我们选出来的老鼠id是`[8,7,9,3]`。它们的出场顺序是`[2,3,6,10]`。它们的体重是`[57,22,56,46]`。

+ 我们选出来了四个，那么剩下的七个老鼠的排名就是`1+4 = 5`，被淘汰，这四个老鼠进入下一轮。

+ 现在`[8,7,9,3]`四只老鼠的出场顺序是`[0,1,2,3]`

+ 那么`3`号老鼠直接晋级，而`[8,7,9]`这三只老鼠里面最重的是`8`。

+ 所以`[7,9]`的排名等于`1+2 = 3`

+ 继续对比`[8,3]`两位选手。

+ 就这样重复进行并更新排名，直到只剩最后一人。

## 数据结构

+ num_pro,max_mice 是老鼠总数和分组的每组老鼠数
+ weight是一个列表，表示老鼠们的体重
  + 下标是id
  + 值是体重
+ init_order 是给出的最初的出场顺序列表
  + **下标是出场顺序**
  + **值是id**
+ rank 是排名列表
  + 下标是id
  + 值是排名
+ weight_last 记录上一轮的各个选手的出场顺序以及它们的体重
  + **下标是出场顺序**
  + **值是选手体重**
+ weight_next 记录下一轮的各个选手的出场顺序以及它们的体重
  - **下标是出场顺序**
  - **值是选手体重**

## 算法

+ 先初始化weight,init_order,rank
+ 然后初始化weight_last为当前的比赛顺序，也就是将第0个登场的选手的体重放在第0个这样排列。
+ 初始化weight_next，它表示下一轮的擂台上的老鼠数，所以循环条件就是这个列表长度大于1。
+ 初始化的时候只要是一个不为空的长度列表就行（因为在循环里面都要清空）
+ 开始循环
  + 先设置weight_next为空
  + 对于上一轮也就是在weight_last的选手
  + 按照它们的出场顺序遍历
  + 找到遍历选手的所在组
    + 可以根据选手的下标，整除以一组人数，再乘以一组人数，得到这一组的起始下标（也就是选手下标 - （选手下标%一组人数））
    + 而这一组的结尾下标就是起始下标再加上一组人数
    + 对于最后人数不足的组也按照这一标准
    + 用weight_last和上面的起始结尾下标切片，就是所在组的信息
  + 当它不是所在组内的最大值的时候
    + 它需要更新排名，增加的人数就是这轮比赛会晋级的人数。
    + 也就是这场擂台的人数除以一组人数，但是有余数的话要加一。即场上的所剩组数。
  + 当这个选手的体重是所在组的最大值的时候
    + 放到weight_next里面
  + 最后把weight_last更新为weight_next，进入下一场比拼
+ 直到weight_next只剩下一人，即冠军，输出即可。

## 代码

+ 由于使用Python能够AC，因此只放了Python的代码。

```python
# 输入与数据结构初始化
from math import ceil
num_pro, max_mice = list(map(int, input().split()))
weight = list(map(int, input().split()))
init_order = list(map(int, input().split()))
rank = [1 for _ in range(num_pro)]
weight_last = [weight[init_order[i]] for i in range(num_pro)]

# 模拟擂台
weight_next = weight_last.copy()
while len(weight_next) > 1:
    weight_next.clear()
    for i in range(len(weight_last)):
        if weight_last[i] != max(weight_last[(i // max_mice) * max_mice:(i // max_mice) * max_mice + max_mice]):
            rank[weight.index(weight_last[i])] += (ceil(len(weight_last) / max_mice))
        else:
            weight_next.append(weight_last[i])
    weight_last = weight_next.copy()

# 输出
print(" ".join(map(str,rank)))
```

