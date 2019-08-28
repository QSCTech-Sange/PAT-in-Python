---
title: <Python><PAT> 1059 Prime Factors
date: 2019-08-28 16:29:43
tags: 
- PAT
- 题解
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 带你复习一下Python的生成器（yield）。
---

# 题目

Given any positive integer *N*, you are supposed to find all of its prime factors, and write them in the format $N=p_{1}^{k_{1}} \times p_{2}^{k_{2}} \times \cdots \times p_{m}^{k_{m}}$.

### Input Specification:

Each input file contains one test case which gives a positive integer *N* in the range of **long int**.

### Output Specification:

Factor *N* in the format *N* `=` *p*1`^`*k*1`*`*p*2`^`*k*2`*`…`*`$p_m$`^`$k_m$, where $p_i$ 's are prime factors of *N* in increasing order, and the exponent  $k_i$ is the number of  $p_i$-- hence when there is only one $p_i$, $p_i$ is 1 and must **NOT** be printed out.

### Sample Input:

```in
97532468
```

### Sample Output:

```out
97532468=2^2*11*17*101*1291
```

# 题解

## 思路

+ 题目让我们找质因子
+ 思路很简单
+ 先从质数2开始，记录能被2整除几次
+ 然后看下一个质数，不断看到最后一个为止
+ 按要求输出即可

## 数据结构

+ ori_num 是原始给我们的数
+ num 是我们运算中的数，它会不断除以质因子而发生改变
+ ans列表存放答案，一组质因子按(质因子，出现次数)这样的元祖形式存放。
+ it 是迭代器，指向了我们的生成器函数
+ prime 是我们正在进行判断的素数

## 算法

+ 首先讲讲如何使用生成器不断生成下一个素数
+ 什么是生成器？
+ 生成器就是普通函数，只是return 改成了yield
+ 然后我们可以使用一个变量指向这个函数，例如这里的`it = nextPrime()`
+ 这样子当我们调用`next(it)`的时候，会返回yield的值，同时程序暂停到yield的时刻。当我们再一次调用`next(it)`的时候直接从上一次yield的时候继续执行
+ 简单吧！
+ 那我们就可以写一个不断生成下一个素数的函数
+ 首先令一个数，这里是j，等于1
+ 然后循环条件是While True，永真
+ 循环里面对j += 2，j是潜在的素数
+ 对j进行判断，判断它是不是素数。
  + 判断的方法是从3到sqrt(j)，每隔两个数迭代，判断j能否被这个数整除
  + 当从3到sqrt(i) 的所有奇数都不是j的因子的时候，j就是一个素数（记得我们的j本身就是奇数）
+ yield j，即返回j
+ 那么当我们调用`next(it)`的时候，就再一次回到while True那里，并将原来的j继续+2,这样就形成了我们想要的生成器
+ 当然这题不用生成器也行，只是正好这题也适合使用生成器，顺带讲一下。
+ 那么回到题目
+ 我们先令`num = ori_num`
+ 每次找到一个质因子，num去整除它，同时把它添加进答案
+ 直到prime > num就停止。
+ 按要求输出即可。

## 代码

+ 因为使用Python能AC，因此只放了Python的题解。

```python
# 生成器，不断生成下一个素数
def nextPrime():
    j = 1
    while True:
        j += 2
        for i in range(3, int(j ** 0.5) + 1, 2):
            if j % i == 0:
                break
        else:
            yield j


ori_num = int(input())
num = ori_num
ans = []
it = nextPrime()

# 开始找质因子
prime = 2
while prime <= num:
    count = 0
    while num % prime == 0:
        num = num // prime
        count += 1
    if count > 0:
        ans.append([prime, count])
    prime = next(it)

# 按照它想要的格式输出，注意 “1” 的特殊情况
print(ori_num, end='=')
if not ans:
    print(ori_num)
for i, j in ans:
    if i != ans[-1][0]:
        if j != 1:
            print("%d^%d" % (i, j), end='*')
        else:
            print(i, end="*")
    else:
        if j != 1:
            print("%d^%d" % (i, j))
        else:
            print(i)

```

