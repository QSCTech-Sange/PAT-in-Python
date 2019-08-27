---
title: <Python><PAT> 1048 Find Coins
date: 2019-08-27 10:44:11
tags: 
- PAT
- 题解
- 哈希集合
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 暴力法，两遍哈希与一遍哈希。
---

# 题目


Eva loves to collect coins from all over the universe, including some other planets like Mars. One day she visited a universal shopping mall which could accept all kinds of coins as payments. However, there was a special requirement of the payment: for each bill, she could only use exactly two coins to pay the exact amount. Since she has as many as 10​5​​ coins with her, she definitely needs your help. You are supposed to tell her, for any given amount of money, whether or not she can find two coins to pay for it.

### Input Specification:

Each input file contains one test case. For each case, the first line contains 2 positive numbers: *N* (≤105, the total number of coins) and *M* (≤103, the amount of money Eva has to pay). The second line contains *N* face values of the coins, which are all positive numbers no more than 500. All the numbers in a line are separated by a space.

### Output Specification:

For each test case, print in one line the two face values *V*1 and *V*2 (separated by a space) such that *V*1+*V*2=*M* and *V*1≤*V*2. If such a solution is not unique, output the one with the smallest *V*1. If there is no solution, output `No Solution` instead.

### Sample Input 1:

```in
8 15
1 2 8 7 2 4 11 15
```

### Sample Output 1:

```out
4 11
```

### Sample Input 2:

```in
7 14
1 8 7 2 4 11 15
```

### Sample Output 2:

```out
No Solution
```

# 题解

## 思路

+ 建议康康LeetCode上的经典题目两数之和
+ 一般来说有三种方法，暴力法，两遍哈希和一遍哈希
+ 暴力法就是对每个数查找一遍其他的数，看看能不能组合成相应的和，太不优雅，不写了
+ 两遍哈希就是先把整个数组读一遍，把每个数存放在哈希集和里。
+ 然后再过一遍数组，如果`目标和 -当前数` 在哈希集合里，那么答案等于（`当前答案`，`目标和 - 当前数`，`当前数`）的最小值。最后输出答案以及目标和减答案即可。
+ 一遍哈希对两遍哈希做了一个简化，即边读，边判断`目标和-当前数`在不在哈希集和，并最后把它放在哈希集合里。
+ 因为是配套匹配的，所以一对数，第一个数找不到匹配被放进哈希集和，第二个数就一定能把它找到。
+ 这里提供一遍哈希的解法。因为最优雅。

## 数据结构

+ _set 是一个哈希集合，存放目前已经出现过的数
+ ans 是目前的匹配成功的最小值

## 算法

+ 读入数据
+ 遍历数据
  + 如果`目标和 -当前数` 在哈希集合里，那么答案等于（`当前答案`，`目标和 - 当前数`，`当前数`）的最小值。
  + 哈希集和添加`当前数`
+ 如果ans没被更新过
  + 输出无解
+ 否则
  + 输出答案以及目标和减答案即可。

## 代码

+ Python代码可以AC，因此只放了Python的代码。

```python
num_coins, sums = list(map(int, input().split()))
_set = set()
ans = 99999
num = list(map(int, input().split()))
for i in num:
    if sums - i in _set:
        ans = min(ans, sums - i, i)
    _set.add(i)
if ans != 99999:
    print(ans, sums - ans)
else:
    print("No Solution")

```

