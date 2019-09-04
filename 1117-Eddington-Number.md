---
title: <Python><PAT> 1117 Eddington Number
date: 2019-09-04 18:11:34
tags:
- PAT
- 题解
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 脑筋急转弯，小学奥数题
---

# 题目 

British astronomer Eddington liked to ride a bike. It is said that in order to show off his skill, he has even defined an "Eddington number", *E* -- that is, the maximum integer *E* such that it is for *E* days that one rides more than *E* miles. Eddington's own *E* was 87.

Now given everyday's distances that one rides for *N* days, you are supposed to find the corresponding *E* (≤*N*).

### Input Specification:

Each input file contains one test case. For each case, the first line gives a positive integer *N* (≤105), the days of continuous riding. Then *N* non-negative integers are given in the next line, being the riding distances of everyday.

### Output Specification:

For each case, print in a line the Eddington number for these *N* days.

### Sample Input:

```in
10
6 7 6 9 3 10 8 2 7 8
```

### Sample Output:

```out
6
```

# 题解

## 思路

+ 首先理解题意
+ 要使得N天骑车距离大于（注意没有等于）N，求这个最大的N
+ 暴力算肯定超时
+ 稍微变通一下，容易想到
+ 可以将所有的数排序，比如逆序
+ 这样的话，第N个数说明前N天都是距离大于第N天的
+ 为了使下标和天数对应，我们可以给列表前面加一项，比如0
+ 举例来说的话，比如`32145`，逆序加0后，变为`054321`
+ 此时下标1的5表示有1天的距离是大于等于5的。
+ 下标2的4表示有2天的距离是大于等于4的。
+ 我们从下标1开始遍历整个数列
+ 当我们检测到一个数满足`nums[i]<=i`了，即总共有`i`天的骑车距离大于等于`nums[i]`。
+ 比如`nums[i] =5`而`i =5`的时候，说明有5天的骑车距离大于等于5,那么显然只有4天的距离是大于5的。
+ 再比如`num[i]= 4`而`i=5`的时候，说明有5天的距离是大于等于4的，那么显然E只能取4。
+ 所以答案就是`i-1`。
+ 想到这个方法并不难，难的是边界条件的判断，不瞒你说我试了N久。

## 数据结构

+ nums 是逆序排列的原数，前面加一项0
+ E 是爱丁顿数

## 算法

+ 算法写在思路里了
+ 这里用了一个技巧把E初始化为n，这样当循环没有被break的话，说明整个数列都符合，那么自然E=n。

## 代码

+ 由于使用Python可以AC，因此只放了Python的解。

```python
n = int(input())
nums = [0] + sorted(list(map(int, input().split())), reverse=True)
E = n
for i in range(1,n+1):
    if nums[i] <= i:
        E = i - 1
        break
print(E)

```

