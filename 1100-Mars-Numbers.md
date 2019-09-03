---
title: <Python><PAT> 1100 Mars Numbers
date: 2019-09-03 10:02:24
tags: 
- PAT
- 题解
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 进制转换问题，难点是处理所有条件分支
---

# 题目

People on Mars count their numbers with base 13:

- Zero on Earth is called "tret" on Mars.
- The numbers 1 to 12 on Earth is called "jan, feb, mar, apr, may, jun, jly, aug, sep, oct, nov, dec" on Mars, respectively.
- For the next higher digit, Mars people name the 12 numbers as "tam, hel, maa, huh, tou, kes, hei, elo, syy, lok, mer, jou", respectively.

For examples, the number 29 on Earth is called "hel mar" on Mars; and "elo nov" on Mars corresponds to 115 on Earth. In order to help communication between people from these two planets, you are supposed to write a program for mutual translation between Earth and Mars number systems.

### Input Specification:

Each input file contains one test case. For each case, the first line contains a positive integer *N* (<100). Then *N* lines follow, each contains a number in [0, 169), given either in the form of an Earth number, or that of Mars.

### Output Specification:

For each number, print in a line the corresponding number in the other language.

### Sample Input:

```in
4
29
5
elo nov
tam
```

### Sample Output:

```out
hel mar
may
115
13
```

# 题解

## 思路

+ 题目是一个基础的进制转换，但是条件有一些复杂， 细节要处理好
+ 思路就是整数映射到火星文，火星文映射到整数
+ 13进制转10进制就是对十位数转成十进制然后乘以十三加上个位数转10进制
+ 10进制转13进制就是对原数每次取13的模，添加到最开头，直到原数为0
+ 大体思路很简单，就是细节处理，详见算法部分

## 数据结构

+ to_mars_ones 是0-12的个位数转火星文的
+ to_mars_tens 是0-12的十位数转火星文（这里的0其实不会出现，是为了下标统一加的一项）
+ message 是原字符串

## 算法

+ 首先根据原字符串是不是数字来分火星文和地球文
+ 如果是数字，即地球文转火星文
  + 对原数转整型，同时开一个ans数组
  + 当原数不为0的时候，反复进行如下操作
    + 取原数和13的模，添加到ans的开头
    + 对原数整除以13
  + 结束操作后
  + 如果ans长度为2
    + 那么对第一项也就是十位数转成十位数火星文
    + 对第二项转为个位数火星文
    + 注意！如果第二项是0的话，是不用输出的，这点和地球文是不一样的。即13 0 是tam 而不是 tam tret，这点题目上没有说，很坑。
  + 如果长度是1 的话
    + 那么对第一项转为个位数输出
  + 注意！这样没有考虑到原数是0的情况，如果原数是0，上述算法会直接输出空，所以对0的情况要单独处理一下
+ 如果不是，则是火星文转地球文
  + 如果火星文长度为2
    + 那么对第一位用十位数转成地球文
    + 对第二位用个位数转地球文
    + 将第一位乘以十三加上第二位输出
  + 如果火星文长度为1
    + 如果它在十位数的火星文里
      + 输出它乘以13
    + 如果它在个位数的火星文里
      + 直接输出它。

## 代码

+ 由于使用Python能AC，因此只放了Python的解。

```python
to_mars_ones = ['tret', 'jan', 'feb', 'mar', 'apr', 'may', 'jun', 'jly', 'aug', 'sep', 'oct', 'nov', 'dec']
to_mars_tens = ['tret', 'tam', 'hel', 'maa', 'huh', 'tou', 'kes', 'hei', 'elo', 'syy', 'lok', 'mer', 'jou']

num_cases = int(input())
for _ in range(num_cases):
    message = input()
    if message.isdigit():
        num = int(message)
        ans = []
        if num != 0:
            while num != 0:
                ans = [num % 13] + ans
                num //= 13
            if len(ans) == 2:
                ans[0] = to_mars_tens[ans[0]]
                if ans[1] != 0:
                    ans[1] = to_mars_ones[ans[1]]
                else:
                    ans.pop()
            else:
                ans[0] = to_mars_ones[ans[0]]
        else:
            ans = ['tret']
        print(" ".join(ans))
    else:
        message = message.split()
        if len(message) == 2:
            message[0] = to_mars_tens.index(message[0])
            message[1] = to_mars_ones.index(message[1])
            print(message[0] * 13 + message[1])
        else:
            if message[0] in to_mars_tens:
                message[0] = to_mars_tens.index(message[0])
                print(message[0] * 13)
            else:
                message[0] = to_mars_ones.index(message[0])
                print(message[0])

```

