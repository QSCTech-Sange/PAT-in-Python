---
title: <Python><PAT> 1092 To Buy or Not to Buy
date: 2019-09-02 19:50:49
tags: 
- PAT
- 题解
- 哈希表
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 使用Counter的爽爆Python应用。
---

# 题目

Eva would like to make a string of beads with her favorite colors so she went to a small shop to buy some beads. There were many colorful strings of beads. However the owner of the shop would only sell the strings in whole pieces. Hence Eva must check whether a string in the shop contains all the beads she needs. She now comes to you for help: if the answer is `Yes`, please tell her the number of extra beads she has to buy; or if the answer is `No`, please tell her the number of beads missing from the string.

For the sake of simplicity, let's use the characters in the ranges [0-9], [a-z], and [A-Z] to represent the colors. For example, the 3rd string in Figure 1 is the one that Eva would like to make. Then the 1st string is okay since it contains all the necessary beads with 8 extra ones; yet the 2nd one is not since there is no black bead and one less red bead.

![Figure 1](https://images.ptausercontent.com/b7e2ffa6-8819-436d-ad79-a41263abe914.jpg)



### Input Specification:

Each input file contains one test case. Each case gives in two lines the strings of no more than 1000 beads which belong to the shop owner and Eva, respectively.

### Output Specification:

For each test case, print your answer in one line. If the answer is `Yes`, then also output the number of extra beads Eva has to buy; or if the answer is `No`, then also output the number of beads missing from the string. There must be exactly 1 space between the answer and the number.

### Sample Input 1:

```in
ppRYYGrrYBR2258
YrR8RrY
```

### Sample Output 1:

```out
Yes 8
```

### Sample Input 2:

```in
ppRYYGrrYB225
YrR8RrY
```

### Sample Output 2:

```out
No 2
```

# 题解

## 思路

+ 给你一串珠子和你想要的珠子
+ 判断那一串珠子是否包含所有你想要的
+ 并且输出多的珠子数量或不够的珠子数量
+ 最方便的当然是使用Counter啦！
+ Counter统计字符串每个字符的出现次数，以一个字典的形式表示
+ 键是字符(也就是珠子)
+ 值是出现次数
+ 遍历想要的珠子Counter里的每一个珠子
+ 如果在提供的珠子里面这个珠子的数量多于想要的，那么more加上差值，否则，less加上差值。
+ 遍历结束后，当less <0 的时候，说明珠子不够，那么要输出no和珠子数量
+ 否则，我们需要遍历offer的Counter
+ 因为有些珠子颜色不在want里，之前的遍历不会遍历到。
+ 当一个珠子不再want里，我们添加offer字典里对应珠子的数量到more里
+ 输出Yes 和多余的珠子数量

## 数据结构

+ offer 是提供的串的Counter
+ want 是想要的串的Counter
+ more 是多出来的珠子数量
+ less 是不够的珠子数量

## 算法

+ 都写在思路里了。

## 代码

+ 因为使用Python3 能AC，因此只放了Python的题解。

```python
from collections import Counter

offer = Counter(input())
want = Counter(input())
more, less = 0, 0
for i in want.keys():
    less += max(want[i] - offer.get(i, 0), 0)
    more += max(offer.get(i, 0) - want[i], 0)
if less > 0:
    print("No", less)
else:
    for i in offer.keys():
        if i not in want:
            more += offer[i]
    print("Yes", more)

```

