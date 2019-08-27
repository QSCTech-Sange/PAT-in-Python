---
title: <Python><PAT> 1042 Shuffling Machine
date: 2019-08-25 20:18:18
tags: 
- PAT
- 题解
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 我是一个没有感情的洗牌机。
---

# 题目

Shuffling is a procedure used to randomize a deck of playing cards. Because standard shuffling techniques are seen as weak, and in order to avoid "inside jobs" where employees collaborate with gamblers by performing inadequate shuffles, many casinos employ **automatic shuffling machines**. Your task is to simulate a shuffling machine.

The machine shuffles a deck of 54 cards according to a given random order and repeats for a given number of times. It is assumed that the initial status of a card deck is in the following order:

```
S1, S2, ..., S13, 
H1, H2, ..., H13, 
C1, C2, ..., C13, 
D1, D2, ..., D13, 
J1, J2
```

where "S" stands for "Spade", "H" for "Heart", "C" for "Club", "D" for "Diamond", and "J" for "Joker". A given order is a permutation of distinct integers in [1, 54]. If the number at the *i*-th position is *j*, it means to move the card from position *i* to position *j*. For example, suppose we only have 5 cards: S3, H5, C1, D13 and J2. Given a shuffling order {4, 2, 5, 3, 1}, the result will be: J2, H5, D13, S3, C1. If we are to repeat the shuffling again, the result will be: C1, H5, S3, J2, D13.

### Input Specification:

Each input file contains one test case. For each case, the first line contains a positive integer *K* (≤20) which is the number of repeat times. Then the next line contains the given order. All the numbers in a line are separated by a space.

### Output Specification:

For each test case, print the shuffling results in one line. All the cards are separated by a space, and there must be no extra space at the end of the line.

### Sample Input:

```in
2
36 52 37 38 3 39 40 53 54 41 11 12 13 42 43 44 2 4 23 24 25 26 27 6 7 8 48 49 50 51 9 10 14 15 16 5 17 18 19 1 20 21 22 28 29 30 31 32 33 34 35 45 46 47
```

### Sample Output:

```out
S7 C11 C10 C12 S1 H7 H8 H9 D8 D9 S11 S12 S13 D10 D11 D12 S3 S4 S6 S10 H1 H2 C13 D2 D3 D4 H6 H3 D13 J1 J2 C1 C2 C3 C4 D1 S5 H5 H11 H12 C6 C7 C8 C9 S2 S8 S9 H10 D5 D6 D7 H4 H13 C5
```

# 题解

## 思路

+ 首先得理解题意
+ 意思是给你一副牌，按照S0 S1 … S13 … H1 … J1 J2 这样的顺序排列。J是大王小王，其他字母表示花色。
+ 然后你需要洗牌
+ 会给你一个序列，按照34 52 … 31 这样给你54个数。含义是，原来第一张要移动到34位置，第二张要移动到第52位置，以此类推。
+ 会给你一个数，是重复次数。即仿照上一步进行的次数
+ 那么我们直接模拟就行了

## 数据结构

+ prev_cards 为原牌序
+ new_cards 为新牌序

## 算法

+ 先用Python的列表生成式构造原来的一副牌序
+ 读入重复次数和排列规则。注意排列规则要每项减1,因为题目里的位置是从1开始的，而我们要从0开始
+ 迭代 重复次数
  + 每一次，生成新卡序列。按照列表生成式的方式生成。举例来说，对new_cards的第一项，它应该是，先找到order出现1的位置，然后找旧卡这个位置上的值。这个值就是new_cards的第一项。抓住了这个规律就好写了。
  + 将老卡改成新卡序列
+ 输出老卡序列

## 代码

+ 因为使用Python能AC，所以只放了Python的代码。只有7行，你问我爽不爽，我肯定是爽的。因为我康C++的选手一般都写了五六十行以上。

```python
prev_cards = [i + str(j) for i in ["S", "H", "C", "D"] for j in range(1, 14)] + ["J1", "J2"]
repeat = int(input())
order = list(map(lambda x: int(x) - 1, input().split()))
for _ in range(repeat):
    new_cards = [prev_cards[order.index(i)] for i in range(54)]
    prev_cards = new_cards
print(" ".join(prev_cards))
```

