---
title: <Python><PAT> 1061 Dating
date: 2019-08-28 20:29:56
tags: 
- PAT
- 题解
- 字符串
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 又是一道题目意思错乱的题。
---

# 题目

Sherlock Holmes received a note with some strange strings: `Let's date! 3485djDkxh4hhGE 2984akDfkkkkggEdsb s&hgsfdk d&Hyscvnm`. It took him only a minute to figure out that those strange strings are actually referring to the coded time `Thursday 14:04` -- since the first common capital English letter (case sensitive) shared by the first two strings is the 4th capital letter `D`, representing the 4th day in a week; the second common character is the 5th capital letter `E`, representing the 14th hour (hence the hours from 0 to 23 in a day are represented by the numbers from 0 to 9 and the capital letters from `A` to `N`, respectively); and the English letter shared by the last two strings is `s` at the 4th position, representing the 4th minute. Now given two pairs of strings, you are supposed to help Sherlock decode the dating time.

### Input Specification:

Each input file contains one test case. Each case gives 4 non-empty strings of no more than 60 characters without white space in 4 lines.

### Output Specification:

For each test case, print the decoded time in one line, in the format `DAY HH:MM`, where `DAY` is a 3-character abbreviation for the days in a week -- that is, `MON` for Monday, `TUE` for Tuesday, `WED` for Wednesday, `THU` for Thursday, `FRI`for Friday, `SAT` for Saturday, and `SUN` for Sunday. It is guaranteed that the result is unique for each case.

### Sample Input:

```in
3485djDkxh4hhGE 
2984akDfkkkkggEdsb 
s&hgsfdk 
d&Hyscvnm
```

### Sample Output:

```out
THU 14:04
```

# 题解

## 思路

+ 有一点我一定要提一下
+ 这道题说的重复字符
+ 它们的**下标**也要是相同的
+ 举个例子，题目里的意思是，第二个字符串出现的第一个在第一个字符串中的“A-G”字符是“D”。然而这样的意思是**错的**。
+ 但实际上，题目里的真实含义是，第二个和第一个字符串下标相同且字符相同并且字符在“A-G”的第一个字符是“D”。
+ 这样就简化了很多，原来拿哈希集合怼半天不对，还不知道问题错在哪。
+ 这道题属实弟弟哦。

## 数据结构

+ day_of_week 是一个字典，将字母映射到礼拜几
+ a,b,c,d分别是原始的四个字符串
+ day,hour,minute 是礼拜几，小时数和分钟数
+ 在遍历中，i是下标，j是值
+ start 记录一下前两个字符串第一次出现重复字符且字符在“A-G”中的下标

## 算法

+ 照着题目意思来就行了

## 代码

+ 由于使用Python3 能AC，因此只放了Python的代码。

```python
day_of_week = {
    'A': "MON",
    'B': "TUE",
    'C': "WED",
    "D": "THU",
    "E": "FRI",
    "F": "SAT",
    "G": "SUN"
}

a = input()
b = input()
for i, j in enumerate(b):
    if a[i] == b[i] and j in "ABCDEFG":
        day = day_of_week[j]
        start = i
        break
for i, j in enumerate(b[start+1:]):
    if a[i+start+1] == j and (j.isdigit() or ord("A") <= ord(j) <= ord("N")):
        hour = int(j) if j.isdigit() else ord(j) - ord("A") + 10
        break

c = input()
d = input()
for i, _ in enumerate(d):
    if d[i] == c[i] and d[i].isalpha():
        minute = i
        break
print(day, end=" ")
print("%02d:%02d" % (hour, minute))

```

