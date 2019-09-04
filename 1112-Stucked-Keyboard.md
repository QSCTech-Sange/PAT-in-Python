---
title: <Python><PAT> 1112 Stucked Keyboard
date: 2019-09-04 12:24:31
tags:
- PAT
- 题解
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 崩坏的键盘
---

# 题目

On a broken keyboard, some of the keys are always stucked. So when you type some sentences, the characters corresponding to those keys will appear repeatedly on screen for *k* times.

Now given a resulting string on screen, you are supposed to list all the possible stucked keys, and the original string.

Notice that there might be some characters that are typed repeatedly. The stucked key will always repeat output for a fixed *k* times whenever it is pressed. For example, when *k*=3, from the string `thiiis iiisss a teeeeeest` we know that the keys `i` and `e` might be stucked, but `s` is not even though it appears repeatedly sometimes. The original string could be `this isss a teest`.

### Input Specification:

Each input file contains one test case. For each case, the 1st line gives a positive integer *k* (1<*k*≤100) which is the output repeating times of a stucked key. The 2nd line contains the resulting string on screen, which consists of no more than 1000 characters from {a-z}, {0-9} and `_`. It is guaranteed that the string is non-empty.

### Output Specification:

For each test case, print in one line the possible stucked keys, in the order of being detected. Make sure that each key is printed once only. Then in the next line print the original string. It is guaranteed that there is at least one stucked key.

### Sample Input:

```in
3
caseee1__thiiis_iiisss_a_teeeeeest
```

### Sample Output:

```out
ei
case1__this_isss_a_teest
```

# 题解

## 思路

+ 题目给出了一个重复次数n和一个字符串
+ 如果字符串中有一个字符是坏的，那么每一次出现它它都会重复那个次数。
+ 我们遍历整个字符串
+ 如果这个字符到后面n个字符都是这个字符，那么把它添加进嫌疑坏键
+ 否则，把这个键从怀疑坏键中删掉
+ 遍历这个字符串，把所有重复的按键转为出现一次
+ 输出怀疑坏键和字符串

## 数据结构

- n 是重复次数
- string 是原字符串
- candidate 是一个集合，包含那些可能坏掉了的按键
- ans保存答案，即有顺序的candidate

## 算法

+ 参见思路。

## 代码

+ 由于使用Python3能AC，因此只放了Python的题解。

```python
n = int(input())
string = input()
candidate = set()

index = 0
while index < len(string):
    if index + n <= len(string) and string[index:index + n] == string[index] * n:
        candidate.add(string[index])
        index += n
    else:
        if string[index] in candidate:
            candidate.remove(string[index])
        index += 1
        
for i in candidate:
    string = string.replace(i * n, i)

ans = []
for i in string:
    if i in candidate:
        ans.append(i)
        candidate.remove(i)
print("".join(ans))
print(string)
```

