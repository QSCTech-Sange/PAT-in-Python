---
title: <Python><PAT> 1124 Raffle for Weibo Followers
date: 2019-09-05 08:23:22
tags:
- PAT
- 题解
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 微博抽奖，女士优先。
---

# 题目

ohn got a full mark on PAT. He was so happy that he decided to hold a raffle（抽奖） for his followers on Weibo -- that is, he would select winners from every N followers who forwarded his post, and give away gifts. Now you are supposed to help him generate the list of winners.

### Input Specification:

Each input file contains one test case. For each case, the first line gives three positive integers M (≤ 1000), N and S, being the total number of forwards, the skip number of winners, and the index of the first winner (the indices start from 1). Then M lines follow, each gives the nickname (a nonempty string of no more than 20 characters, with no white space or return) of a follower who has forwarded John's post.

Note: it is possible that someone would forward more than once, but no one can win more than once. Hence if the current candidate of a winner has won before, we must skip him/her and consider the next one.

### Output Specification:

For each case, print the list of winners in the same order as in the input, each nickname occupies a line. If there is no winner yet, print `Keep going...` instead.

### Sample Input 1:

```in
9 3 2
Imgonnawin!
PickMe
PickMeMeMeee
LookHere
Imgonnawin!
TryAgainAgain
TryAgainAgain
Imgonnawin!
TryAgainAgain
```

### Sample Output 1:

```out
PickMe
Imgonnawin!
TryAgainAgain
```

### Sample Input 2:

```in
2 3 5
Imgonnawin!
PickMe
```

### Sample Output 2:

```out
Keep going...
```

# 题解

## 思路

+ 模拟抽奖，主要是理解它的规则。
+ 首先，第一个抽奖的人的下标是第一行的第三个数
+ 其次，每隔（第一行第二个数）的人，抽到奖品
+ 如果遇到的人正好是已经抽到过奖品的，顺次后移

## 数据结构

+ appeared 是一个集合，储存已经中奖的人
+ ans 存放答案，即中奖的人
+ index 存放中奖索引
+ handled 存放总的处理过的人数

## 算法

+ 遍历所有人
+ 读到普通人的时候，index += 1已经handled += 1
+ 正好遇到index == win即第一个中奖的人 或 index - win == skip （后面的中奖的人）的时候。
  + 如果名字没有抽奖过
    + 那么这个人中奖
    + 更新index = win
    + 将他添加进appeared里
  + 如果这个人中过奖了
    + 那么就继续读取
    + 同时handled + 1
    + 但是index 不变
    + 直到读到了没有中奖的

## 代码 

+ 由于使用Python能AC，因此只放了Python的解。

```python
forwards, skip, win = list(map(int, input().split()))
appeared = set()
ans = []
index = 0
handled = 0
while handled < forwards:
    name = input()
    handled += 1
    index += 1
    if index == win or index - win ==skip:
        while name in appeared:
            name = input()
            handled += 1
        ans.append(name)
        index = win
        appeared.add(name)
if len(ans) == 0:
    print("Keep going...")
else:
    for i in ans:
        print(i)

```