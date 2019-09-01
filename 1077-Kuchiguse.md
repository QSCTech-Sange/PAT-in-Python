---
title: <Python><PAT> 1077 Kuchiguse
date: 2019-08-31 23:01:06
tags: 
- PAT
- 题解
- 字符串
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 题目得吧得吧半天，其实就是找公共后缀
---

# 题目

The Japanese language is notorious for its sentence ending particles. Personal preference of such particles can be considered as a reflection of the speaker's personality. Such a preference is called "Kuchiguse" and is often exaggerated artistically in Anime and Manga. For example, the artificial sentence ending particle "nyan~" is often used as a stereotype for characters with a cat-like personality:

- Itai nyan~ (It hurts, nyan~)
- Ninjin wa iyada nyan~ (I hate carrots, nyan~)

Now given a few lines spoken by the same character, can you find her Kuchiguse?

### Input Specification:

Each input file contains one test case. For each case, the first line is an integer *N* (2≤*N*≤100). Following are *N* file lines of 0~256 (inclusive) characters in length, each representing a character's spoken line. The spoken lines are case sensitive.

### Output Specification:

For each test case, print in one line the kuchiguse of the character, i.e., the longest common suffix of all *N* lines. If there is no such suffix, write `nai`.

### Sample Input 1:

```in
3
Itai nyan~
Ninjin wa iyadanyan~
uhhh nyan~
```

### Sample Output 1:

```out
nyan~
```

### Sample Input 2:

```in
3
Itai!
Ninjinnwaiyada T_T
T_T
```

### Sample Output 2:

```out
nai
```

# 题解

## 思路

+ 题目的意思是让我们找字符串的公共后缀
+ 我们可以先把它们逆序，求前缀，方便一点。
+ 直接遍历每一个字符串的第0个字符，看看是不是都相等
+ 相等了，添加进答案里，然后遍历第1个字符，以此类推
+ 当发现了不符合的或者字符下标超过了字符串长度的时候，退出
+ 输出答案即可

## 数据结构

+ lines 是一个列表，存放所有逆序的字符串们
+ i指示字符串的下标

## 算法

+ 将逆序后的字符串们放到lines当中
+ 从0开始遍历下标，遍历每一个字符串
+ 如果有一个字符串的下标位置的字符和第0个字符串的下标位置的值不一样，或者下标超过了一个字符串的长度
+ 那么直接退出。
+ 否则把这个字符添加进答案。
+ 这里使用了`judge()`作为一个函数的目的是跳出两层循用一个return即可，很方便。直接用break的话只能跳出一层。
+ 将答案逆序输出。

## 代码

+ 因为使用Python3能AC，因此只放了Python的代码。

```python
num = int(input())
lines = []
ans = []
for _ in range(num):
    lines.append(input()[::-1])


def judge():
    for i in range(0, 999):
        for line in lines:
            if len(line) < i or line[i] != lines[0][i]:
                return
        else:
            ans.append(lines[0][i])
    return


judge()
if not ans:
    print("nai")
else:
    print("".join(ans[::-1]))

```

