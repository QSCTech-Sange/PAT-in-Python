---
title: <Python><PAT> 1068 Find More Coins
date: 2019-08-30 12:22:14
tags: 
- PAT
- 题解
- 动态规划
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 有技巧的动态规划
---

# 题目

Eva loves to collect coins from all over the universe, including some other planets like Mars. One day she visited a universal shopping mall which could accept all kinds of coins as payments. However, there was a special requirement of the payment: for each bill, she must pay the exact amount. Since she has as many as $10^4$ coins with her, she definitely needs your help. You are supposed to tell her, for any given amount of money, whether or not she can find some coins to pay for it.

### Input Specification:

Each input file contains one test case. For each case, the first line contains 2 positive numbers: `N` (≤$10^4$ , the total number of coins) and `M` (≤$10^2$ , the amount of money Eva has to pay). The second line contains `N` face values of the coins, which are all positive numbers. All the numbers in a line are separated by a space.

### Output Specification:

For each test case, print in one line the face values $V_1\leq V_2 \leq \cdots \leq V_k$ such that $V_1+ V_2 + \cdots + V_k = $`M`. All the numbers must be separated by a space, and there must be no extra space at the end of the line. If such a solution is not unique, output the smallest sequence. If there is no solution, output "No Solution" instead.

Note: sequence {A[1], A[2], ...} is said to be "smaller" than sequence {B[1], B[2], ...} if there exists *k*≥1 such that A[*i*]=B[*i*] for all *i*<*k*, and A[*k*] < B[*k*].

### Sample Input 1:

```in
8 9
5 9 8 7 2 3 4 1
```

### Sample Output 1:

```out
1 3 5
```

### Sample Input 2:

```
4 8
7 2 4 3
```

### Sample Output 2:

```
No Solution
```

# 题解

## 思路

+ 容易想到动态规划和DFS，更合理的方法是动态规划，DFS容易超时
+ 这道题是经典的动态规划的变形
+ 我们可以开一个dp数组，下标是要凑到的数，值是凑到的方法
+ 例如，我们凑到5元的方法是`[1,4]`，那么`dp=[1,4]`
+ 假如我们有三块硬币，1元，4元，3元，而我们需要凑到8元
+ 实际上就是求`dp[8]`，那我们就可以先从`dp[8-1]`，`dp[8-4]`，`dp[8-3]`里面找，即想要凑到8元，我先看看凑到7元，凑到4元，凑到3元的方法。如果要凑到8元，那么必定是在这三种里面之一。
+ 那么是三种里面的哪一种呢？经典的动态规划，就让你求取到硬币数量最少的方法。
+ 而这道题想让你求下标最小的方法，也就是字符串排序最小的方法。
+ 例如我们有一枚8元硬币，一枚7元硬币，一枚1元硬币，要凑到8元，更合理的方法是`1+7`而不是`8`，因为在字符串排序中，`“17"<"8”`，第一位1小于8。
+ 这样也导致了我们的状态转移公式失效了。例如我们要凑到9元，最优的是1和8,但是用上面的例子来看，`dp[8]` 并不是一个8,而是`1 + 7`
+ 这怎么办呢？
+ 我想了很久，终于幡然领悟。
+ 根本思路在于**要从金币从大到小遍历**，而不是从dp的从小到大遍历。
+ 我们往往会陷入，dp下标小的确定了，dp下标大的就确定了。
+ 而这题并不是这么回事。
+ 按照金币从大到小遍历，再按照dp从大到小遍历的结果是
+ 越大的数额，越先被较大的数占领
+ 随着金币慢慢变小，越大的数额慢慢被较小的数占领
+ 这就是题目想要的最优解。
+ 举例来说，我们先考虑`6,7,8`三个数，它们能组成`13~15`这三个数。目前`dp[13:16]`都是由`6,7,8`把持的。
+ 然后金币数额遍历到了`5`，再从15到13遍历一遍
+ 先看`15`,那要检查`10`是不是空的，如果是空的，那么跳过
+ 到了`13`，要检查`8`是不是空的，发现有一个只有`8`的组合凑成了`8`。那么更新`13`为`5+8`。
+ 因为是从金币从大到小遍历的，所以金币8的组合一定是被考虑过的，所以可以这么算。
+ 确实比较抽象，想到这里不容易。
+ 还有一个细节就是如果遍历的dp数和金币数是相等的话，也要直接更新。因为这时候会检查`dp[0]`是不是空的，而它显然是空的。
+ 还不懂的话，可以看看代码理一理。
+ 主要还是转换状态的思想
+ 一般认为dp是一个状态，但是这里更应该把金币当作一个状态
+ 即，数额大的金币整完以后，固定所有的dp状态，再加入一个数量小的金币进行更新，所以的dp状态又是最优的状态。
+ 状态转移是以硬币递减为迁移的，不是传统以dp递增为迁移的。

## 数据结构

+ dp是一个数组
  + 下标是要凑到的数
  + 值是一个列表，存放凑到这个数的最优解。
+ coins是所有的硬币

## 算法

+ 动态规划，详解在思路里。

## 代码

+ 因为使用Python能够AC，因此只放了Python的代码。

```python
num_coins, target = list(map(int, input().split()))
coins = sorted(list(map(int, input().split())), reverse=True)
dp = [[] for _ in range(target + 1)]
# 状态转移
for coin in coins:
    for money in range(target, coin - 1, -1):
        if dp[money - coin] or money - coin == 0:
            dp[money] = dp[money - coin] + [coin]
# 输出
if not dp[target]:
    print("No Solution")
else:
    print(" ".join(map(str,sorted(dp[target]))))

```

