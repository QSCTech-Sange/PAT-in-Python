---
title: <Python><PAT> 1017 Queueing at Bank
date: 2019-08-21 17:22:59
tags: 
- PAT
- 题解
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 又是一道银行排队的问题
---

# 题目

Suppose a bank has *K* windows open for service. There is a yellow line in front of the windows which devides the waiting area into two parts. All the customers have to wait in line behind the yellow line, until it is his/her turn to be served and there is a window available. It is assumed that no window can be occupied by a single customer for more than 1 hour.

Now given the arriving time *T* and the processing time *P* of each customer, you are supposed to tell the average waiting time of all the customers.

### Input Specification:

Each input file contains one test case. For each case, the first line contains 2 numbers: *N* (≤104) - the total number of customers, and *K* (≤100) - the number of windows. Then *N* lines follow, each contains 2 times: `HH:MM:SS` - the arriving time, and *P* - the processing time in minutes of a customer. Here `HH` is in the range [00, 23], `MM` and `SS` are both in [00, 59]. It is assumed that no two customers arrives at the same time.

Notice that the bank opens from 08:00 to 17:00. Anyone arrives early will have to wait in line till 08:00, and anyone comes too late (at or after 17:00:01) will not be served nor counted into the average.

### Output Specification:

For each test case, print in one line the average waiting time of all the customers, in minutes and accurate up to 1 decimal place.

### Sample Input:

```in
7 3
07:55:00 16
17:00:01 2
07:59:59 15
08:01:00 60
08:00:00 30
08:00:02 2
08:03:00 10
```

### Sample Output:

```out
8.2
```

# 题解

## 思路

+ 一个银行有几个窗口，每个窗口前面只能站一个人
+ 剩下进来排队的统一在黄线后面，看哪个窗口好了去哪个窗口服务
+ 给定顾客来的时间和需要服务的时间，让你求平均等待时间
+ 注意，在八点以前到的只能干等
+ 注意，在下午五点以后到的不被统计进去，也不被服务
+ 注意，服务时间大于60分钟的当作60分钟来看
+ 注意，题目给定的顾客是无序的
+ 所以这道题的核心是开一个pop数组，记录每个窗口服务完成的时间
+ 同时，所有的时间被记录为从00:00:00到时间点经过了多少秒

## 数据结构

+ infomation 是一个列表，包含读取的顾客信息。每一项是一个长度为2的列表，包含
  + 第一项是这个顾客来的时间
  + 第二项是他需要的服务时间
  + 设定这个infomation列表是因为题目给的时间是无序的，便于排序
+ arrive是一个列表
  + 下标是顾客编号（按来的顺序排列）
  + 值是顾客到来的时间（按秒来计算）
+ process是一个列表
  + 下标是顾客编号（按来的顺序排列）
  + 值是这个顾客需要被服务的时间（按秒来计算）
+ total_wait 总等待时间
+ pop 是核心，代表每个窗口的空闲出来的时间点
  + 下标是窗口编号
  + 值是下一个空闲时间点，默认是8点钟，以秒计就是28800

## 算法

+ 开一个total_wait记录总等待时间
+ 读入所有顾客信息，按照时间排序，添加到数据结构里
  + 来晚的就减少cus_num然后直接continue
  + 来早的不用管，因为预设了pop是28800，就相当于前面的被占着。
  + 服务时间大于60的按60算
+ 模拟排队，设一个cus_id代表顾客下标
  + 顾客会找到一个pop时间最小的窗口
  + 如果他来的时候，没有空闲窗口
    + 总等待时间要加上（该窗口pop时间 - 该顾客来的时间）
    + 同时这个窗口的pop要加上这个顾客的process_time
  + 如果他来的时候正好有空闲窗口
    + 只要更新这个窗口的pop就行
  + 进入下一个顾客
+ 输出

## 代码

因为使用Python3能AC，因此只放了Python3的代码。

```python
# 读取数据并初始化
num_cus, num_windows = list(map(int, input().split()))
arrive, process, pop = [], [], [28800 for _ in range(num_windows)]
total_wait = 0

information = []
for _ in range(num_cus):
    information.append(input().split())
information.sort(key=lambda x: x[0])

for a, pro_time in information:
    time = int(a[:2]) * 3600 + int(a[3:5]) * 60 + int(a[6:])
    if time > 61200:
        num_cus -= 1
        continue
    else:
        arrive.append(time)
    if int(pro_time) > 60:
        process.append(3600)
    else:
        process.append(int(pro_time) * 60)

# 模拟排队的流程
cus_id = 0
while cus_id < num_cus:
    window = pop.index(min(pop))
    if pop[window] - arrive[cus_id] > 0:
        total_wait += (pop[window] - arrive[cus_id])
        pop[window] += process[cus_id]
    else:
        pop[window] = arrive[cus_id] + process[cus_id]
    cus_id += 1

print("%.1f" % (total_wait / num_cus / 60))

```

