---
title: <Python><PAT> 1084 Broken Keyboard
date: 2019-09-02 12:31:05
tags: 
- PAT
- 题解
- 字符串
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 用Python的集合操作，又爽爆了。
---

# 题目

On a broken keyboard, some of the keys are worn out. So when you type some sentences, the characters corresponding to those keys will not appear on screen.

Now given a string that you are supposed to type, and the string that you actually type out, please list those keys which are for sure worn out.

### Input Specification:

Each input file contains one test case. For each case, the 1st line contains the original string, and the 2nd line contains the typed-out string. Each string contains no more than 80 characters which are either English letters [A-Z] (case insensitive), digital numbers [0-9], or `_` (representing the space). It is guaranteed that both strings are non-empty.

### Output Specification:

For each test case, print in one line the keys that are worn out, in the order of being detected. The English letters must be capitalized. Each worn out key must be printed once only. It is guaranteed that there is at least one worn out key.

### Sample Input:

```in
7_This_is_a_test
_hs_s_a_es
```

### Sample Output:

```out
7TI
```

# 题解

## 思路

+ 题目给一行正确的字符串
+ 再给一行键盘坏掉了的字符串
+ 找到坏掉的按键，以大写形式给出，按照找到崩坏的顺序
+ 用集合作差即可

## 数据结构

+ a 好的字符串
+ b 坏的字符串
+ c 坏掉的键盘字符

## 算法

+ 读a和b
+ 将它们转为大写
+ 使c为集合a和集合b的差，即坏掉的字符们
+ 因为集合是无序的，所以我们输出的时候要遍历a
+ 当a在c中的时候，输出它，同时c里面移除它。

## 代码

+ 因为使用Python能AC，因此只放了Python的解。

```python
a = input().upper()
b = input().upper()
c = set(a) - set(b)
for i in a:
    if i in c:
        print(i, end='')
        c.remove(i)
```

