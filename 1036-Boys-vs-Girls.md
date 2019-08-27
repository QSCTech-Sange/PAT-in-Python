---
title: <Python><PAT> 1036 Boys vs Girls
date: 2019-08-24 21:41:33
tags: 
- PAT
- 题解
- 字符串
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 大水题
---

# 题目

This time you are asked to tell the difference between the lowest grade of all the male students and the highest grade of all the female students.

### Input Specification:

Each input file contains one test case. Each case contains a positive integer *N*, followed by *N* lines of student information. Each line contains a student's `name`, `gender`, `ID` and `grade`, separated by a space, where `name` and `ID` are strings of no more than 10 characters with no space, `gender` is either `F` (female) or `M` (male), and `grade` is an integer between 0 and 100. It is guaranteed that all the grades are distinct.

### Output Specification:

For each test case, output in 3 lines. The first line gives the name and ID of the female student with the highest grade, and the second line gives that of the male student with the lowest grade. The third line gives the difference $gradeF−gradeM$. If one such kind of student is missing, output `Absent` in the corresponding line, and output `NA` in the third line instead.

### Sample Input 1:

```in
3
Joe M Math990112 89
Mike M CS991301 100
Mary F EE990830 95
```

### Sample Output 1:

```out
Mary EE990830
Joe Math990112
6
```

### Sample Input 2:

```in
1
Jean M AA980920 60
```

### Sample Output 2:

```out
Absent
Jean AA980920
NA
```

# 题解

## 思路

+ 照着题目意思来就行了，真没啥好讲的

## 数据结构

+ high_girl 存储高分妹子的信息
+ low_boy 存储低分汉子的信息

## 算法

+ 一边读一边更新
+ 按情况输出
+ 没了

## 代码

因为使用Python能AC，所以只放了Python的解。

```python
num_stu = int(input())
high_girl = None
low_boy = None
for _ in range(num_stu):
    name, gender, id, score = input().split()
    if gender == 'F':
        if high_girl is None or int(score) > high_girl[2]:
            high_girl = [name, id, int(score)]
    else:
        if low_boy is None or int(score) < low_boy[2]:
            low_boy = [name, id, int(score)]
if high_girl is None:
    print("Absent")
else:
    print(high_girl[0], high_girl[1])
if low_boy is None:
    print("Absent")
else:
    print(low_boy[0], low_boy[1])
if high_girl is not None and low_boy is not None:
    print(high_girl[2] - low_boy[2])
else:
    print("NA")

```

