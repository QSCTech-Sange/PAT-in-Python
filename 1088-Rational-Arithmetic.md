---
title: <Python><PAT> 1088 Rational Arithmetic
date: 2019-09-02 17:36:15
tags: 
- PAT
- 题解
- 字符串
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 在20分的题里面算非常难了。主要是要处理的细节，非常非常多。我已经使用了Fraction类来降低复杂性，可是依然非常复杂。
---

# 题目

For two rational numbers, your task is to implement the basic arithmetics, that is, to calculate their sum, difference, product and quotient.

### Input Specification:

Each input file contains one test case, which gives in one line the two rational numbers in the format `a1/b1 a2/b2`. The numerators and the denominators are all in the range of long int. If there is a negative sign, it must appear only in front of the numerator. The denominators are guaranteed to be non-zero numbers.

### Output Specification:

For each test case, print in 4 lines the sum, difference, product and quotient of the two rational numbers, respectively. The format of each line is `number1 operator number2 = result`. Notice that all the rational numbers must be in their simplest form `k a/b`, where `k` is the integer part, and `a/b` is the simplest fraction part. If the number is negative, it must be included in a pair of parentheses. If the denominator in the division is zero, output `Inf` as the result. It is guaranteed that all the output integers are in the range of **long int**.

### Sample Input 1:

```in
2/3 -4/2
```

### Sample Output 1:

```out
2/3 + (-2) = (-1 1/3)
2/3 - (-2) = 2 2/3
2/3 * (-2) = (-1 1/3)
2/3 / (-2) = (-1/3)
```

### Sample Input 2:

```in
5/3 0/6
```

### Sample Output 2:

```out
1 2/3 + 0 = 1 2/3
1 2/3 - 0 = 1 2/3
1 2/3 * 0 = 0
1 2/3 / 0 = Inf
```

# 题解

## 思路

+ 为了方便我使用了Fraction类，该类可以直接读取“-1/4”的字符串并当作数来存储以及加减乘除，它可以自动转换为最简形式，但是不能转为分离整数的形式。
+ 如果不记得这个类的话，就硬怼，最后用最大公因数化简分子分母。
+ 读入就按照Fraction类来读入
+ 简单使用加减乘除便可得到结果
+ 要注意除法的话，要区别除数是否为0,为0的话除法就是`Inf`
+ 复杂的是把这个结果转为我们想要的格式
+ 我这里写了一个函数方便转换。
+ 首先是如果是负数的话，要将其反转
+ 并且记录一下输出的时候要再外面包裹`(-`和`)`。
+ 我们把分数分为整数部分和小数部分，两者０与非０共组成４种组合。需要对这四种组合分别讨论。
+ 看整数部分，由`ans.numerator // ans.denominator `决定。即分子除以分母
  + 当它不为０的时候，要将返回的字符串添加上它。
    + 这时候再看小数部分，即分子与分母求余
    + 当小数部分不为０的时候，字符串添加“**空格**　＋　分子　＋　除号　＋　分母”
    + 为０的时候什么都不添加
  + 当它为０的时候，什么都不添加
    + 这时候再看小数部分，即分子与分母求余
    + 看小数部分，如果不为０，添加“分子　＋　除号　＋　分母”
    + 当它也是０的时候，添加一个“０”
+ 如果是负数的话，别忘了输出的时候要再外面包裹`(-`和`)`
+ 返回这个新的字符串即可

## 数据结构

+　a,b是原来的两个数
+　 _sum, diff, prod, quo　是和，差，积，商
+　在fraction函数中
  +　ans 是原Fraction
  +　string 是要返回的新字符串
  +　negative 记录是否是负数

## 算法

+ 照着思路里的算就可以了

## 代码

+ 因为使用Python 可以AC，因此只放了Python的解

```python
from fractions import Fraction


def fraction(ans):
    string = ""
    negative = False
    if ans < 0:
        ans = -ans
        negative = True
    if ans.numerator // ans.denominator != 0:
        string += str(ans.numerator // ans.denominator)
        if ans.numerator % ans.denominator != 0:
            string += (" " + str(ans.numerator % ans.denominator) + "/" + str(ans.denominator))
    else:
        if ans.numerator % ans.denominator != 0:
            string += (str(ans.numerator % ans.denominator) + "/" + str(ans.denominator))
        else:
            string += "0"
    if negative:
        string = "(-" + string + ")"
    return string


a, b = list(map(Fraction, input().split()))
if b != 0:
    _sum, diff, prod, quo = fraction(a + b), fraction(a - b), fraction(a * b), fraction(a / b)
else:
    _sum, diff, prod, quo = fraction(a + b), fraction(a - b), fraction(a * b), "Inf"
a, b = fraction(a), fraction(b)
# 输出
print(a, "+", b, "=", _sum)
print(a, "-", b, "=", diff)
print(a, "*", b, "=", prod)
print(a, "/", b, "=", quo)

```

