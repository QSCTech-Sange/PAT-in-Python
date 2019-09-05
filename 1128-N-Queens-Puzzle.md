---
title: <Python><PAT> 1128 N Queens Puzzle
date: 2019-09-05 13:18:46
tags:
- PAT
- 题解
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 八皇后问题
---

# 题目

The "eight queens puzzle" is the problem of placing eight chess queens on an 8×8 chessboard so that no two queens threaten each other. Thus, a solution requires that no two queens share the same row, column, or diagonal. The eight queens puzzle is an example of the more general *N* queens problem of placing *N* non-attacking queens on an *N*×*N* chessboard. (From Wikipedia - "Eight queens puzzle".)

Here you are NOT asked to solve the puzzles. Instead, you are supposed to judge whether or not a given configuration of the chessboard is a solution. To simplify the representation of a chessboard, let us assume that no two queens will be placed in the same column. Then a configuration can be represented by a simple integer sequence $(Q_1,Q_2,\dots Q_N)$, where  $Q_i$ is the row number of the queen in the *i*-th column. For example, Figure 1 can be represented by (4, 6, 8, 2, 7, 1, 3, 5) and it is indeed a solution to the 8 queens puzzle; while Figure 2 can be represented by (4, 6, 7, 2, 8, 1, 9, 5, 3) and is NOT a 9 queens' solution.

| ![8q.jpg](https://images.ptausercontent.com/7d0443cf-5c19-4494-98a6-0f0f54894eaa.jpg) |      | ![9q.jpg](https://images.ptausercontent.com/d187e37a-4eb8-4215-8e2c-040a73c5c8d8.jpg) |
| :----------------------------------------------------------: | ---- | :----------------------------------------------------------: |
|                           Figure 1                           |      |                           Figure 2                           |

### Input Specification:

Each input file contains several test cases. The first line gives an integer *K* (1<*K*≤200). Then *K* lines follow, each gives a configuration in the format "$N \;Q_1 \;Q_2\; ...\;Q_N$", where 4≤*N*≤1000 and it is guaranteed that $1\leq Q_i \leq N$ for all *i*=1,⋯,*N*. The numbers are separated by spaces.

### Output Specification:

For each configuration, if it is a solution to the *N* queens problem, print `YES` in a line; or `NO` if not.

### Sample Input:

```in
4
8 4 6 8 2 7 1 3 5
9 4 6 7 2 8 1 9 5 3
6 1 5 2 6 4 3
5 1 3 5 2 4
```

### Sample Output:

```out
YES
NO
NO
YES
```

# 题解

+ 题目其实很憨憨，虽然说了很多
+ 给你一个棋盘，每行都有一个皇后，给出了每行的皇后位置`[1,2,4,6,8,3,5,7]`这样的形式。
+ 让你判断这些皇后会不会有出现在相同列或对角线上的
+ 如果没有出现在相同列上的皇后，只要看给定的皇后没有重复的就行
+ 难点在于处理对角线的
+ 其实，同一个对角线有一个特征，就是行和列索引加起来是一样的
+ 所以，如果我们给定的序列中有两项的下标和值的和是相同的，那么它们就是在同一对角线上的。

## 数据结构

+ queens 是读取的测试集

## 算法

+ 读取测试集数量
+ 读取一行测试
  + 调用`judge()`函数来判断
    + 测试集取集合后的长度和原测试集的长度不同，说明有重复的
    + 遍历queens的每一项，如果这一项下标与值的和在集合里，那么说明有在同一对角线上的
    + 否则把它添加到集合里。

## 代码

+ 因为使用Python可以AC，因此只放了Python的题解。

```python
def judge(queens):
    if len(set(queens))!= len(queens):
        return "NO"
    else:
        diag = set()
        for i,j in enumerate(queens):
            if i + j in diag:
                return "NO"
            diag.add(i+j)
        return "YES"


n = int(input())
for _ in range(n):
    queens = list(map(int,input().split()[1:]))
    print(judge(queens))

```

