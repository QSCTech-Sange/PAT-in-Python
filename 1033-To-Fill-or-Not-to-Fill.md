---
title: <Python><PAT> 1033 To Fill or Not to Fill
date: 2019-08-24 16:13:18
tags: 
- PAT
- 题解
- 动态规划
- 贪心算法
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 有一些难度的动态规划。使用循环或递归皆可。核心问题是找到最优子结构和状态转移公式。
---

# 题目

With highways available, driving a car from Hangzhou to any other city is easy. But since the tank capacity of a car is limited, we have to find gas stations on the way from time to time. Different gas station may give different price. You are asked to carefully design the cheapest route to go.

### Input Specification:

Each input file contains one test case. For each case, the first line contains 4 positive numbers: Cmax (≤ 100), the maximum capacity of the tank; *D* (≤30000), the distance between Hangzhou and the destination city; Davg (≤20), the average distance per unit gas that the car can run; and *N* (≤ 500), the total number of gas stations. Then *N* lines follow, each contains a pair of non-negative numbers: Pi, the unit gas price, and *Di* (≤*D*), the distance between this station and Hangzhou, for *i*=1,⋯,*N*. All the numbers in a line are separated by a space.

### Output Specification:

For each test case, print the cheapest price in a line, accurate up to 2 decimal places. It is assumed that the tank is empty at the beginning. If it is impossible to reach the destination, print `The maximum travel distance = X` where `X` is the maximum possible distance the car can run, accurate up to 2 decimal places.

### Sample Input 1:

```in
50 1300 12 8
6.00 1250
7.00 600
7.00 150
7.10 0
7.20 200
7.50 400
7.30 1000
6.85 300
```

### Sample Output 1:

```out
749.17
```

### Sample Input 2:

```
50 1300 12 2
7.10 0
7.00 600
```

### Sample Output 2:

```
The maximum travel distance = 1200.00
```

# 题解

## 思路

+ 题目意思是，一条路上，挑选合适的加油站和加油量来使得总花费最少（每个加油站定价不同）。
+ 我们先把这个问题化小，找一个最优子结构出来。
+ 比方说，如果我在车站Ａ，我应该在这里加多少油？下一个车站应该去哪？
+ 我首先需要看看，在我的车程范围内，有哪些加油站。
+ 如果车程范围没加油站了就输出到车站Ａ的距离 + 车程
+ 否则，要找到下一个要去的加油站，
+ 在车程范围内，如果出现了第一个油价比这个车站低的。
+ 我就加油加到正好能开到这个车站的量。
+ 如果车程范围内所有的车站的油价都比现在的贵，
+ 那我现在就得把车子油箱拉满，然后到车程范围内相对便宜的车站看情况补充。
+ 所以当我们到那个车站的时候，可能是油箱里还有油的。
+ 那么这么一来，整个结构就清晰了
+ 一个状态包含
  + 车站
  + 现在油箱里的油（或者能开的最远距离，反正可以转换）
  + 总价（这个也可以写在全局变量里，为了更好理解状态这个概念放在这里）
+ 而状态转移公式，就是从一个车站转移到另一个车站。转移的原则有两条
  + 车程范围内出现的第一个油价比这个车站低的。现在油箱里的油转移为０，总价相应转换。
  + 车程范围内油价都比这个车站高的情况下，选一个油价最低的，现在油箱里的油转移为满油减去到下个车站的耗费油，同时更新总价。
+ 这样就形成了从一个车站到下一个车站的状态转移
+ 直到转移到终点。

## 数据结构

+ stations　是一个列表，表示所有加油站
  + 下标是车站id
  + 值是一个列表
    + 第一项表示油单价
    + 第二项表示到起点的距离
+ station或i表示现在的车站id
+ next_sta　表示下一个车站id
+ price　表示总价
+ now_tank_can_run　表示目前的油箱能开多远

## 算法

+ 读入数据，并给车站添加一个“终点站”，其单价为０
+ 车站按照离起点的距离排序
+ 判断最近的车站是否距离为０，不是的话直接抛出答案（特判）
+ 然后以[0,0,0]的初始状态开始进入状态转移公式（递归），三个参数分别表示，所在的站点，油箱里剩的油，和总价格
  + 状态转移公式中
  + 先判断车站是否是终点站，是的话直接跳出
  + 我们需要找到下一个要去的车站
    + 先令下一个车站为这个车站+1，判断最近的下一个车站在不在车程范围内，不在就直接跳出
    + 遍历里程范围内第一个油价比车站低的车站
    + 如果找到的话
      + 设置下一个状态
      + 状态的车站就是这个更低油价的车站
      + 到这个车站之后，油量一定为０
      + 价格要加上(到下一个车站的距离　－　现在油量能开的距离)，再乘以原来车站的单价，除以油箱数
      + 将新状态放到状态转移公式
    + 如果找不到这样的车站，就找里程范围内油价最低的车站
      + 设置下一个状态
      + 这时候要把油量拉满，所以到下一个车站以后的油量为，车程 - 到下一个车站的距离
      + 价格加上满油箱的油量－现在已经有的油量，乘以原来车站的单价，除以油箱数
      + 将新状态放到状态转移公式
+ 输出答案

## 代码

+ 由于使用Python能够AC，所以放了Python的代码。
+ 递归和迭代的方法都有写，一般来说递归可能更好理解最优子结构的概念，不过会造成更大的空间复杂度。在递归解法里，注释写得比较详细。

### 迭代

```python
tank, distance, per_unit, num_sta = list(map(float, input().split()))
max_dis = tank * per_unit
stations = []
for _ in range(int(num_sta)):
    info = input().split()
    stations.append([float(info[0]), float(info[1])])
stations.append([0, distance])
stations.sort(key=lambda x: x[1])
if stations[0][1] != 0:
    print("The maximum travel distance = 0.00")
else:
    i = 0
    price = 0
    now_tank_can_run = 0
    while True:
        if i == num_sta:
            print("%0.2f" % price)
            break
        next_sta = i + 1
        if stations[next_sta][1] - stations[i][1] > max_dis:
            print("The maximum travel distance = %0.2f" % (stations[i][1] + max_dis))
            break
        while stations[next_sta][0] > stations[i][0] and stations[next_sta + 1][1] - stations[i][1] <= max_dis:
            next_sta += 1
        if stations[next_sta][0] > stations[i][0]:
            next_sta = i + 1
            temp = next_sta
            while stations[temp + 1][1] - stations[i][1] <= max_dis:
                temp += 1
                if stations[temp][0] < stations[next_sta][0]:
                    next_sta = temp
            price += ((max_dis - now_tank_can_run) / per_unit * stations[i][0])
            now_tank_can_run = max_dis - stations[next_sta][1] + stations[i][1]
        else:
            price += ((stations[next_sta][1] - stations[i][1] - now_tank_can_run) / per_unit * stations[i][0])
            now_tank_can_run = 0
        i = next_sta

```

### 递归

```python
def next_station(station, now_tank_can_run, price):
    global num_sta, stations, tank, max_dis, per_unit, distance
    if station == num_sta:
        return True, price
    # next_sta 先是下一个车站
    next_sta = station + 1
    # 如果下一个车站到这个车站的距离超过了最大能开的距离，报错
    if stations[next_sta][1] - stations[station][1] > max_dis:
        return False, stations[station][1] + max_dis
    # 否则，next_sta 找到下一个要前去的车站，应当是油价比现在低的第一个车站。先遍历里程范围内的第一个油价比此车站低的车站
    while stations[next_sta][0] > stations[station][0] and stations[next_sta + 1][1] - stations[station][1] <= max_dis:
        next_sta += 1
    # 如果next_sta还是比现在的价格高，说明里程范围内没有比现在更低的油价，那么就把现在的邮箱拉满，同时找到下一个里程范围内价格相对更低的车站
    if stations[next_sta][0] > stations[station][0]:
        next_sta = station + 1
        temp = next_sta
        while stations[temp + 1][1] - stations[station][1] <= max_dis:
            temp += 1
            if stations[temp][0] < stations[next_sta][0]:
                next_sta = temp
        return next_station(next_sta, max_dis - stations[next_sta][1] + stations[station][1],
                            price + (max_dis - now_tank_can_run) / per_unit * stations[station][0])
    # 否则，正好移动到next_station为止
    else:
        return next_station(next_sta, 0,
                            price + (stations[next_sta][1] - stations[station][1] - now_tank_can_run) / per_unit *
                            stations[station][0])


tank, distance, per_unit, num_sta = list(map(float, input().split()))
max_dis = tank * per_unit
stations = []
for _ in range(int(num_sta)):
    info = input().split()
    stations.append([float(info[0]), float(info[1])])
stations.append([0, distance])
stations.sort(key=lambda x: x[1])
if stations[0][1] != 0:
    print("The maximum travel distance = 0.00")
else:
    status, price = next_station(0, 0, 0)
    if status:
        print("%0.2f" % price)
    else:
        print("The maximum travel distance = %0.2f" % price)

```