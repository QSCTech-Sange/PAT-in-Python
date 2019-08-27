---
title: <Python><PAT> 1041 Be Unique
date: 2019-08-25 18:39:38
tags: 
- PAT
- 题解
- 哈希集合
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 虽然很想用一行解极速AC，但是会超时。比较优雅的方法是使用哈希集合。
---

# 题目

Being unique is so important to people on Mars that even their lottery is designed in a unique way. The rule of winning is simple: one bets on a number chosen from [1,104]. The first one who bets on a unique number wins. For example, if there are 7 people betting on { 5 31 5 88 67 88 17 }, then the second one who bets on 31 wins.

### Input Specification:

Each input file contains one test case. Each case contains a line which begins with a positive integer *N* (≤105) and then followed by *N* bets. The numbers are separated by a space.

### Output Specification:

For each test case, print the winning number in a line. If there is no winner, print `None` instead.

### Sample Input 1:

```in
7 5 31 5 88 67 88 17
```

### Sample Output 1:

```out
31
```

### Sample Input 2:

```in
5 888 666 666 888 888
```

### Sample Output 2:

```out
None
```

# 题解

## 思路

+ 很想一行暴力做
+ 但是会有两个点超时
+ 所以考虑用哈希集合
+ 因为它的查找是O(1)的

## 数据结构

+ appeared 是一个哈希集合，记录已经出现过的数
+ ans 是一个哈希集合，记录只出现一次的数
+ a 是原数组

## 算法

+ 遍历字符串a
  + 如果这个字符没有出现过（即不在appeared里面）
    + 那么appered添加它，同时ans里也添加它
  + 如果这个字符出现过（即在appeared里面）
    + 那么ans删除它
+ 如果ans长度为0
  + 输出None
+ 否则
  + 再遍历字符串a
    + 找到第一个在出现在ans里面的字符
    + 输出它
+ 注意，必须要有两个哈希集合，如果只用一个ans的话，如果字符出现了三次，理应是不能放在ans里的，但是如果出现一次就添加，再出现一次就抹去的话，那么出现第三次还是会被添加进去
+ 注意，处理完ans后必须再来一遍遍历，不能直接输出ans的第一项，因为集合是无序的。

## 代码

```python
a = input().split()[1:]
appeared, ans = set(), set()
for i in a:
    if i not in appeared:
        appeared.add(i)
        ans.add(i)
    elif i in ans:
        ans.remove(i)
if len(ans) == 0:
    print("None")
else:
    for i in a:
        if i in ans:
            print(i)
			break

```

