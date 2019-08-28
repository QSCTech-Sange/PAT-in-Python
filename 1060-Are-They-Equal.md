---
title: <Python><PAT> 1060 Are They Equal
date: 2019-08-28 18:18:40
tags: 
- PAT
- 题解
- 字符串
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 令人痛快的边界条件。令人愉悦的Python
---

# 题目

If a machine can save only 3 significant digits, the float numbers 12300 and 12358.9 are considered equal since they are both saved as $0.123 \times 10^{5}$ with simple chopping. Now given the number of significant digits on a machine and two float numbers, you are supposed to tell if they are treated equal in that machine.

### Input Specification:

Each input file contains one test case which gives three numbers *N*, *A* and *B*, where *N* (<100) is the number of significant digits, and *A* and *B* are the two float numbers to be compared. Each float number is non-negative, no greater than 10100, and that its total digit number is less than 100.

### Output Specification:

For each test case, print in a line `YES` if the two numbers are treated equal, and then the number in the standard form `0.d[1]...d[N]*10^k` (`d[1]`>0 unless the number is 0); or `NO` if they are not treated equal, and then the two numbers in their standard form. All the terms must be separated by a space, with no extra space at the end of a line.

Note: Simple chopping is assumed without rounding.

### Sample Input 1:

```in
3 12300 12358.9
```

### Sample Output 1:

```out
YES 0.123*10^5
```

### Sample Input 2:

```in
3 120 128
```

### Sample Output 2:

```out
NO 0.120*10^3 0.128*10^3
```

# 题解

## 思路

+ 它看上去不难

+ 就是比较两个数在一个给定精度范围内是否相等，并输出

+ 但实际上边界条件很复杂而且题目也没有提示

+ 好在有Python，处理字符串会比较简单一点

+ 对于一个字符串，我们感兴趣的是它的指数(即10的几次方)和它的0.xxx这里的xxx，我把前者称之为`expo `，后者称之为`binary`（尽管叫这个名字不太确切）

+ 我们可以写一个函数，对一个给定的字符串，返回它的`expo`和`binary`，之后就可以按照格式输出了。比较依据就是两个字符串的`expo`和`binary`是不是都分别相同。

+ 我们先考虑指数expo好了。例如12345,对应的expo就是5,也就是0.12345 * 10^5。非常自然。那么是不是说expo 就等于字符串的长度了呢？非也。

+ 我们忽略了有小数点的情况。如果有小数点的话，例如1234.5，我们想要的是0.12345 * 10^4。

+ 所以总结一下

+ expo 在没有小数点的情况下，就是整个字符串的长度，否则，是小数点前的长度。

+ 那么再看binary。再结合前面12345和1234.5的例子，不难得出

+ binary就是去掉小数点后的字符串。

+ 然而这样没有考虑到，精度的问题。

+ 所以准确地说，binary 是去掉小数点后的字符串长度和精度之间的比较，如果小于精度，补0,大于精度，则去前精度长度的数。用一行Python表示的话就是（N是精度）

+ `string.replace(".", "")[:N] + "0" * (N - len(string.replace(".", "")))`

+ 一般人想到这里就差不多了，会写出这样的代码，返回`binary`和`expo`。你愿意的话甚至可以把它们写在一行，就是看起来会比较长

+ ```python
  def getBinary(string, N):
      expo = len(string) if string.find(".") == -1 else string.find(".")
      binary = string.replace(".", "")[:N] + "0" * (N - len(string.replace(".", "")))
      return binary, expo
  ```

+ 但是！

+ 这样忽略了小数的情况。什么意思呢？

+ 我举个例子。如果0.0123这个数，我们希望表示成0.123 * 10^-1这样子。然而用我们的函数，算出来的expo是1（因为小数点前是1个数），而binary是0123。

+ 这不是我们想要的

+ 所以我们应该增加一项考虑，即，在string去掉小数点后，binary前面的零，通通不算数。同时对与expo，也要剪掉前面的0的数量。比如说这里原来算出来expo是1,我们有两个0,那么expo = 1 -  2 = -1。

+ 利用`lstrip()`函数，我们的代码会长这样

+ ```python
  def getBinary(string, N):
      expo = len(string) if string.find(".") == -1 else string.find(".")
      string = string.replace(".", "")
      expo -= (len(string) - len(string.lstrip("0")))
      string = string.lstrip("0")
      return string[:N] + "0" * (N - len(string)), expo
  ```

+ 然而还是有一个点报错了。我想了很久，才反应过来。

+ 题目里是可能出现诸如0.000000这样的数的

+ 对于这样的数，我们想要输出的expo是0,即 0.000 * 10 ^0 这样的类型。而我们的算法会将expo变成一个负数。

+ 所以要多层判断。当数是0的时候，expo就是0。

+ 能自己想到这点很厉害，我承认我是上牛客提交才意识到的。（

## 数据结构

+ N 是精度
+ s1, s2 是原字符串
+ b1,b2 是两个字符串的0.xxx这里的xxx
+ e1,e2 是两个字符串的指数

## 算法

+ 如思路所述

## 代码

+ 因为使用Python可以AC，因此只放了Python的代码。

```python
def getBinary(string, N):
    expo = len(string) if string.find(".") == -1 else string.find(".")
    string = string.replace(".", "")
    expo -= (len(string) - len(string.lstrip("0")))
    string = string.lstrip("0")
    if not string:
        expo = 0
    return string[:N] + "0" * (N - len(string)), expo


N, s1, s2 = input().split()
N = int(N)
b1, e1 = getBinary(s1, N)
b2, e2 = getBinary(s2, N)
if b1 == b2 and e1 == e2:
    print("YES 0." + str(b1) + "*10^" + str(e1))
else:
    print("NO 0." + str(b1) + "*10^" + str(e1) + " 0." + str(b2) + "*10^" + str(e2))

```

