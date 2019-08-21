---
title: <Python><PAT> 1016 Phone Bills
date: 2019-08-21 16:12:09
tags: 
- PAT
- 题解
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 有点繁琐，耗时间
---

# 题目

A long-distance telephone company charges its customers by the following rules:

Making a long-distance call costs a certain amount per minute, depending on the time of day when the call is made. When a customer starts connecting a long-distance call, the time will be recorded, and so will be the time when the customer hangs up the phone. Every calendar month, a bill is sent to the customer for each minute called (at a rate determined by the time of day). Your job is to prepare the bills for each month, given a set of phone call records.

### Input Specification:

Each input file contains one test case. Each case has two parts: the rate structure, and the phone call records.

The rate structure consists of a line with 24 non-negative integers denoting the toll (cents/minute) from 00:00 - 01:00, the toll from 01:00 - 02:00, and so on for each hour in the day.

The next line contains a positive number *N* (≤1000), followed by *N* lines of records. Each phone call record consists of the name of the customer (string of up to 20 characters without space), the time and date (`mm:dd:hh:mm`), and the word `on-line` or `off-line`.

For each test case, all dates will be within a single month. Each `on-line` record is paired with the chronologically next record for the same customer provided it is an `off-line` record. Any `on-line` records that are not paired with an `off-line` record are ignored, as are `off-line` records not paired with an `on-line` record. It is guaranteed that at least one call is well paired in the input. You may assume that no two records for the same customer have the same time. Times are recorded using a 24-hour clock.

### Output Specification:

For each test case, you must print a phone bill for each customer.

Bills must be printed in alphabetical order of customers' names. For each customer, first print in a line the name of the customer and the month of the bill in the format shown by the sample. Then for each time period of a call, print in one line the beginning and ending time and date (`dd:hh:mm`), the lasting time (in minute) and the charge of the call. The calls must be listed in chronological order. Finally, print the total charge for the month in the format shown by the sample.

### Sample Input:

```in
10 10 10 10 10 10 20 20 20 15 15 15 15 15 15 15 20 30 20 15 15 10 10 10
10
CYLL 01:01:06:01 on-line
CYLL 01:28:16:05 off-line
CYJJ 01:01:07:00 off-line
CYLL 01:01:08:03 off-line
CYJJ 01:01:05:59 on-line
aaa 01:01:01:03 on-line
aaa 01:02:00:01 on-line
CYLL 01:28:15:41 on-line
aaa 01:05:02:24 on-line
aaa 01:04:23:59 off-line
```

### Sample Output:

```out
CYJJ 01
01:05:59 01:07:00 61 $12.10
Total amount: $12.10
CYLL 01
01:06:01 01:08:03 122 $24.40
28:15:41 28:16:05 24 $3.85
Total amount: $28.25
aaa 01
02:00:01 04:23:59 4318 $638.80
Total amount: $638.80
```

# 题解

## 思路

+ 题目意思画得很庞大但看例子还是蛮清楚的
+ 第一行是24个小时每个小时不同的收费
+ 第二行告诉你有几个记录
+ 然后是每条记录
+ **注意**不是每条记录都是有效的，对于同一个人来说，所有的账单里按时间排序，只有一前一后正好是on-line和off-line的这样一对订单才算数
+ **注意**当一个人的订单全部都是错误的时候，不是输出这个人的月度账单总额为0,而是直接不输出它。



## 数据结构

+ toll是一个数组
  + 下标是小时
  + 值是这一小时的每分钟收费
+ records是一个字典
  + 键是用户名
  + 值是元祖列表，代表这个用户的所有账单
    + 一开始录入数据，元祖由(时间，状态)组成
    + 清理过后，元祖由(on-line时间，off-line时间)这样来表示一对有效账单，时间不算上月

## 算法

+ 读入数据
  + 记得记录月份
+ 清理数据
  + 对字典的每一项清理
  + 对一个用户的所有账单按照时间排序
  + 只有一前一后正好on-line和off-line匹配的才算数
  + 更新这个用户的账单，用成对的元祖来表示
+ 计算时间差和价格
  + 采用终点减起点的方式计算
  + 从这个月初开始，计算到起点的时间，计算到终点的时间，之间的差就是时间差
  + 价格同理，假设从月初一直打电话，然后计算价格差
  + 价格就是先计算不算今天，以前的所有天的总价，再计算不算分钟，这天零点到这个小时的总价，再计算分钟的价格，加起来

## 代码

因为使用Python3能AC，因此只写了Python3的代码。

```python
from collections import defaultdict

# 给定dd:hh::mm的字符串，返回从这个月头到这个时间点的总分钟数和总价格（假设每一分都在打电话）
def time_to_money(a: str):
    day, hour, minute = int(a[:2]), int(a[3:5]), int(a[6:])
    minutes = day * 24 * 60 + hour * 60 + minute
    money = sum(toll) * 60 * day
    for i in range(hour):
        money += toll[i] * 60
    money += toll[hour] * minute
    return minutes, money

# 数据结构初始化与录入数据
toll = list(map(int, input().split()))
records = defaultdict(list)
num_records = int(input())
for i in range(num_records):
    info = input().split()
    records[info[0]].append((info[1], info[2]))
    if i == 0: month = info[1][:2]

# 清理数据
for i in records.keys():
    records[i].sort(key=lambda x: x[0])
    temp_list = []
    for j, k in enumerate(records[i]):
        if j + 1 < len(records[i]) and k[1] == "on-line" and records[i][j + 1][1] == "off-line":
            temp_list.append((k[0][3:], records[i][j + 1][0][3:]))
    records[i] = temp_list

# 计算并输出
name_list = sorted([i for i in records.keys() if records[i]])
for user in name_list:
    print(user, month)
    sums = 0
    for begin, end in records[user]:
        minute_begin, money_begin = time_to_money(begin)
        minute_end, money_end = time_to_money(end)
        print("%s %s %d %c%.2f" % (begin, end, minute_end - minute_begin, "$", (money_end - money_begin) / 100))
        sums += (money_end - money_begin)
    print("Total amount: $%.2f" % (sums / 100))

```

 