---
title: <Python><PAT> 1108 Finding Average
date: 2019-09-03 20:42:31
tags:
- PAT
- 题解
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 好无聊啊这道题
---

# 题目

The basic task is simple: given *N* real numbers, you are supposed to calculate their average. But what makes it complicated is that some of the input numbers might not be legal. A **legal** input is a real number in [−1000,1000] and is accurate up to no more than 2 decimal places. When you calculate the average, those illegal numbers must not be counted in.

### Input Specification:

Each input file contains one test case. For each case, the first line gives a positive integer *N* (≤100). Then *N* numbers are given in the next line, separated by one space.

### Output Specification:

For each illegal input number, print in a line `ERROR: X is not a legal number` where `X` is the input. Then finally print in a line the result: `The average of K numbers is Y` where `K` is the number of legal inputs and `Y` is their average, accurate to 2 decimal places. In case the average cannot be calculated, output `Undefined` instead of `Y`. In case `K` is only 1, output `The average of 1 number is Y` instead.

### Sample Input 1:

```in
7
5 -3.2 aaa 9999 2.3.4 7.123 2.35
```

### Sample Output 1:

```out
ERROR: aaa is not a legal number
ERROR: 9999 is not a legal number
ERROR: 2.3.4 is not a legal number
ERROR: 7.123 is not a legal number
The average of 3 numbers is 1.38
```

### Sample Input 2:

```in
2
aaa -9999
```

### Sample Output 2:

```out
ERROR: aaa is not a legal number
ERROR: -9999 is not a legal number
The average of 0 numbers is Undefined
```

# 题解

## 思路

+ 难度没什么
+ 就是复杂，就是绕
+ 不过好在样例里已经把所有的异常情况告诉你了
+ 照着情况排除就可以了

## 数据结构

+ nums 存放所有的数（以字符串的形式）
+ ans 存放所有合法的数

## 算法

+ 写了一个check() 函数辅助判断一个字符串是否合法。
+ 可以不写，我只是为了结构清晰一点。因为一次return后面的就不执行了。
+ 根据ans的长度来判断合适的输出
+ 异常情况处理就是
  + 当字符串不含有小数点 或者 （字符串含有一个小数点且小数点后字符小于等于2位）
  + 且大小在正负1000以内
  + 返回True
+ 其他情况返回False

## 代码

+ 因为使用Python可以AC，因此只放了Python的解。

```python
def check(string):
    if all([i in "-1234567890." for i in string]):
        if ((string.count(".") == 0) or (
                string.count(".") == 1 and len(string) - string.index(".") <= 3)) and -1000 <= float(string) <= 1000:
            return True
    return False


n = int(input())
nums = input().split()
ans = []
for i in nums:
    if check(i):
        ans.append(float(i))
    else:
        print("ERROR: %s is not a legal number" % i)
if len(ans) == 0:
    print("The average of 0 numbers is Undefined")
elif len(ans) == 1:
    print("The average of 1 number is %.2f" % float(ans[0]))
else:
    print("The average of %d numbers is %.2f" % (len(ans), sum(ans) / len(ans)))

```

​	