---
title:  <Python><PAT> 1070 Mooncake
date: 2019-08-29 20:44:14
tags: 
- PAT
- 题解
- Python
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 题目本身不难，使用lambda排序非常方便，有一个坑点是输入的库存也可能是小数。
---

# 题目

Mooncake is a Chinese bakery product traditionally eaten during the Mid-Autumn Festival. Many types of fillings and crusts can be found in traditional mooncakes according to the region's culture. Now given the inventory amounts and the prices of all kinds of the mooncakes, together with the maximum total demand of the market, you are supposed to tell the maximum profit that can be made.

Note: partial inventory storage can be taken. The sample shows the following situation: given three kinds of mooncakes with inventory amounts being 180, 150, and 100 thousand tons, and the prices being 7.5, 7.2, and 4.5 billion yuans. If the market demand can be at most 200 thousand tons, the best we can do is to sell 150 thousand tons of the second kind of mooncake, and 50 thousand tons of the third kind. Hence the total profit is 7.2 + 4.5/2 = 9.45 (billion yuans).

### Input Specification:

Each input file contains one test case. For each case, the first line contains 2 positive integers *N* (≤1000), the number of different kinds of mooncakes, and *D* (≤500 thousand tons), the maximum total demand of the market. Then the second line gives the positive inventory amounts (in thousand tons), and the third line gives the positive prices (in billion yuans) of *N* kinds of mooncakes. All the numbers in a line are separated by a space.

### Output Specification:

For each test case, print the maximum profit (in billion yuans) in one line, accurate up to 2 decimal places.

### Sample Input:

```in
3 200
180 150 100
7.5 7.2 4.5
```

### Sample Output:

```out
9.45
```

# 题解

## 思路

+ 题目的意思很简单
+ 给你几种蛋糕的库存和总价，以及市场上需要的总蛋糕量
+ 去计算能获得的最大收益
+ 唯一一个坑点是库存也可以是小数，不然有一个点过不了。这个点我debug了超级久，也是闲的。
+ 我们只要把蛋糕按照单位价格最高的排序
+ 然后遍历蛋糕
+ 将需求不断用前面的蛋糕填满
+ 计算价格就可以了

## 数据结构

+ kinds 是月饼种类数量
+ demands是总需求
+ storage 是各个月饼的库存
+ prices 是各个月饼的总价
+ cakes 是一个元祖列表，每一项是一个月饼元祖，包含
  + 库存
  + 总价
+ profits 总利润

## 算法

+ 读取信息
+ 将cakes用storage和prices组合起来
+ 按照**单价**从高到低排列月饼
+ 遍历月饼
+ 只要需求大于这个月饼的库存
+ 那么就当作这个月饼全被买了，更新demands和profits
+ 否则，将剩下的需求用库存填补，相应利润增加(demands/storage) * 总价
+ 输出利润即可。

## 代码

+ 由于使用Python能够AC，因此只放Python的代码。

```python
kinds, demands = list(map(int, input().split()))
storages = list(map(float, input().split()))
prices = list(map(float, input().split()))
cakes = sorted([(i, j) for i, j in zip(storages, prices)], key=lambda x: x[1]/x[0], reverse=True)
profits = 0
for storage, price in cakes:
    if demands > storage:
        demands -= storage
        profits += price
    elif demands <= storage:
        profits += (demands / storage) * price
        break
print("{:.2f}".format(profits))

```

