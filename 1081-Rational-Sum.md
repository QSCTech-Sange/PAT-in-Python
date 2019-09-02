---
title: <Python><PAT> 1081 Rational Sum
date: 2019-09-02 10:04:12
tags: 
- PAT
- 题解
- 字符串
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 有点点复杂的字符串处理。使用Fraction类会很方便。想不起来的话就硬怼了。
---

# 题目

Given *N* rational numbers in the form `numerator/denominator`, you are supposed to calculate their sum.

### Input Specification:

Each input file contains one test case. Each case starts with a positive integer *N* (≤100), followed in the next line *N* rational numbers `a1/b1 a2/b2 ...` where all the numerators and denominators are in the range of **long int**. If there is a negative number, then the sign must appear in front of the numerator.

### Output Specification:

For each test case, output the sum in the simplest form `integer numerator/denominator` where `integer` is the integer part of the sum, `numerator` < `denominator`, and the numerator and the denominator have no common factor. You must output only the fractional part if the integer part is 0.

### Sample Input 1:

```in
5
2/5 4/15 1/30 -2/60 8/3
```

### Sample Output 1:

```out
3 1/3
```

### Sample Input 2:

```in
2
4/3 2/3
```

### Sample Output 2:

```out
2
```

### Sample Input 3:

```in
3
1/3 -1/6 1/8
```

### Sample Output 3:

```out
7/24
```

# 题解

## 思路

+ 题目的意思是想让我们对所有分数加和，并且表示成“几又几分之几”的表示方法。
+ Python有一个类，叫Fraction，这个类可以直接读取一个类似“-1/2”的字符串并且转换成分数。两个Fraction可以直接相加，并且自动化简为最简。
+ 这样只有结尾处理一下转为几又几分之几的形式即可。
+ 最后的处理要注意分成四种情况来处理。
+ 如果不知道Fraction这个类的话，可以直接全加上，就用分数的加法，相信你肯定会。然后最后用最大公约数化简，用辗转相除法。这个忘了的话就自己草稿纸上随便画一画。
+ 反正Python没有整数长度限制，这样直接怼也很爽。

## 数据结构

+ ans 是加总和，是一个Fraction类。可以通过ans.numerator和ans.denominator来获得分子和分母。

## 算法

+ 把每个数转为Fraction然后直接相加。
+ ans.numerator // ans.denominator 即前面的整数
+ ans.numerator % ans.denominator 即后面的分子
+ 对于它们俩是否为0形成四种组合。
+ 对四种组合分别作判断并写出相应的输出。
+ 主要有0和空格的问题，这两个要明辨。
+ 这道题的测试点输出不会有负数，不用考虑这一点。

## 代码

+ 因为使用Python能AC，因此只放了Python的代码。

```python
from fractions import Fraction

_, ans = input(), sum([Fraction(i) for i in input().split()])
if ans.numerator // ans.denominator != 0:
    print(ans.numerator // ans.denominator, end="")
    if ans.numerator % ans.denominator != 0:
        print(" " + str(ans.numerator % ans.denominator) + "/" + str(ans.denominator))
else:
    if ans.numerator % ans.denominator != 0:
        print(str(ans.numerator % ans.denominator) + "/" + str(ans.denominator))
    else:
        print("0")

```

