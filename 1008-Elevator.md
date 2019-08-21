---
title: <Python><PAT> 1008 Elevator
date: 2019-08-19 18:40:25
tags: 
- PAT
- 题解
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 终极大水题！
---

# 题目

The highest building in our city has only one elevator. A request list is made up with *N* positive numbers. The numbers denote at which floors the elevator will stop, in specified order. It costs 6 seconds to move the elevator up one floor, and 4 seconds to move down one floor. The elevator will stay for 5 seconds at each stop.

For a given request list, you are to compute the total time spent to fulfill the requests on the list. The elevator is on the 0th floor at the beginning and does not have to return to the ground floor when the requests are fulfilled.

### Input Specification:

Each input file contains one test case. Each case contains a positive integer *N*, followed by *N* positive numbers. All the numbers in the input are less than 100.

### Output Specification:

For each test case, print the total time on a single line.

### Sample Input:

```in
3 2 3 1
```

### Sample Output:

```out
41
```

# 题解

## 思路

+ 遍历request，添加进总时间就好了

## 数据结构

+ now代表电梯现在停在哪，time代表总时间

## 算法

+ 遍历并更新time和now
+ 没了，真没了

## 代码

+ 因为使用Python3能AC，因此只使用了Python3代码。

```python
request = list(map(int, input().split()[1:]))
now, time = 0, 0
for i in request:
    if i > now:time += (i - now) * 6 + 5
    elif i < now:time += (now - i) * 4 + 5
    else:time += 5
    now = i
print(time)
```

