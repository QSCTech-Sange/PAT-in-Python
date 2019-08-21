---
title: <Python><PAT> 1011 World Cup Betting
date: 2019-08-20 12:41:33
tags: 
- PAT
- 题解
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 憨憨题，题目得吧得吧半天，其实水得不行。
---

# 题目

With the 2010 FIFA World Cup running, football fans the world over were becoming increasingly excited as the best players from the best teams doing battles for the World Cup trophy in South Africa. Similarly, football betting fans were putting their money where their mouths were, by laying all manner of World Cup bets.

Chinese Football Lottery provided a "Triple Winning" game. The rule of winning was simple: first select any three of the games. Then for each selected game, bet on one of the three possible results -- namely `W` for win, `T` for tie, and `L` for lose. There was an odd assigned to each result. The winner's odd would be the product of the three odds times 65%.

For example, 3 games' odds are given as the following:

```
 W    T    L
1.1  2.5  1.7
1.2  3.1  1.6
4.1  1.2  1.1
```

To obtain the maximum profit, one must buy `W` for the 3rd game, `T` for the 2nd game, and `T` for the 1st game. If each bet takes 2 yuans, then the maximum profit would be (4.1×3.1×2.5×65%−1)×2=39.31 yuans (accurate up to 2 decimal places).

### Input Specification:

Each input file contains one test case. Each case contains the betting information of 3 games. Each game occupies a line with three distinct odds corresponding to `W`, `T` and `L`.

### Output Specification:

For each test case, print in one line the best bet of each game, and the maximum profit accurate up to 2 decimal places. The characters and the number must be separated by one space.

### Sample Input:

```in
1.1 2.5 1.7
1.2 3.1 1.6
4.1 1.2 1.1
```

### Sample Output:

```out
T T W 39.31
```

# 题解

## 思路

+ 其实只要看例子就行了，描述不用看，又臭又长
+ 意思是，每行给三个数，找到每行的最大值和下标
+ 下标映射到WTL，然后这三个数乘起来，再乘以0.65，再减1,再乘2

## 数据结构

+ 没有

## 算法

+ 用了一个比较炫技的方法

+ 这是python特有的
+ 读入数，分割成列表，转换成浮点数，排序，找到最大值和下标，一气呵成，全在一行
+ 实际考试别这么写啊



## 代码

使用Python能AC，因此只放了Python的代码。

```python
def num_to_word(num):
    if num == 0: return 'W'
    if num == 1: return 'T'
    if num == 2: return 'L'


a = sorted([(num_to_word(i), j) for i, j in enumerate(list(map(float, input().split())))], key=lambda x: x[1])[-1]
b = sorted([(num_to_word(i), j) for i, j in enumerate(list(map(float, input().split())))], key=lambda x: x[1])[-1]
c = sorted([(num_to_word(i), j) for i, j in enumerate(list(map(float, input().split())))], key=lambda x: x[1])[-1]

print(a[0], b[0], c[0], end='')
print(" %.2f"%((a[1] * b[1] * c[1] * 0.65 -1) * 2))

```

