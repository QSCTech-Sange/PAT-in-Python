---
title: <Python><PAT> 1040 Longest Symmetric String
date: 2019-08-25 17:37:19
tags: 
- PAT
- 题解
- 哈希表
- 动态规划
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 做法有很多，我推荐使用动态规划
---

# 题目

Given a string, you are supposed to output the length of the longest symmetric sub-string. For example, given `Is PAT&TAP symmetric?`, the longest symmetric sub-string is `s PAT&TAP s`, hence you must output `11`.

### Input Specification:

Each input file contains one test case which gives a non-empty string of length no more than 1000.

### Output Specification:

For each test case, simply print the maximum length in a line.

### Sample Input:

```in
Is PAT&TAP symmetric?
```

### Sample Output:

```out
11
```

# 题解

## 思路

+ 这道题方法有很多
+ 我推荐使用动态规划 + 剪枝，比较省空间，同时使用Python也能AC
+ 思路是这样
+ 首先对每一个字符，建立一个dp数组，比如说“apple”，dp\[0\]\[0\] 表示第一个a与它距离0个单位（也就是自己）组成的字符串是否是回文，是一个布尔变量。dp\[0\]\[1\] 表示a到与它距离一个单位组成的字符串（也就是ap）是否是回文。
+ 首先自己和自己一定是回文，也就是所有的 dp\[i\]\[i\] 都是`True`。这是距离为0的情况。
+ 然后我们遍历距离从1到len(string)
  + 对每个距离，遍历所有的字符
  + 如果这个字符到这个距离组成的字符串是回文，那么设为`True`。方法是比较这个字符下面一个字符到这个距离-2的字符是否是回文以及这个字符与这个距离后的字符是不是回文。即，如果这个字符串不算端点，中间都是回文，且端点也是相同的，那么这个字符串就是回文。
  + 因为每次都是从距离往外扩大的，所以判断的时候里面的字符串总是有效的。
  + 如果为True了，那么需要更新最大距离为这个距离
  + 注意要剪枝，即如果这个距离已经全军覆没，大家都是`False`，那么再下两个距离也是不可能滴，此时直接`break`就行了。为什么是下两个距离？比如说，`kik`这个字符串在1距离下就全军覆没但是在2距离下依然有`True`的。如果不剪枝，有一个点会超时。
+ 最后输出最大距离+1，因为距离是不算首的，所以要把首个字符加上去。

## 数据结构

+ dp 是一个二维数组，一个列表，含义在思路里。
  + 下标表示字符串的下标
  + 值是一个列表
    + 下标表示距离原字符相隔多少距离
    + 值表示这个距离后的字符和原字符之间组成的字符串是否为回文的布尔变量

## 算法

+ 就照着思路里的来就行了。

## 代码

因为使用Python3代码能AC，因此使用Python来做这道题。

```python
string = input()
dp = [[True for j in range(i, len(string))] for i in range(len(string))]
max_length = 0
for distance in range(1, len(string)):
    for i in range(0, len(string) - distance):
        dp[i][distance] = dp[i + 1][distance - 2] and string[i] == string[i + distance]
        if dp[i][distance] is True:max_length = distance
    if max_length == distance - 2:break
print(max_length + 1)
```

