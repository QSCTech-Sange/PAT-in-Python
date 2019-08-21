---
title: <Python><PAT> 1096 Consecutive Factors
date: 2019-08-18 08:56:19
tags: 
- PAT
- 题解
- 双指针
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: Easy题，无坑点，双指针一把梭
---

# 题目

Among all the factors of a positive integer N, there may exist several consecutive numbers. For example, 630 can be factored as 3×5×6×7, where 5, 6, and 7 are the three consecutive numbers. Now given any positive N, you are supposed to find the maximum number of consecutive factors, and list the smallest sequence of the consecutive factors.

### Input Specification:

Each input file contains one test case, which gives the integer N (1<N<231).

### Output Specification:

For each test case, print in the first line the maximum number of consecutive factors. Then in the second line, print the smallest sequence of the consecutive factors in the format `factor[1]*factor[2]*...*factor[k]`, where the factors are listed in increasing order, and 1 is NOT included.

### Sample Input:

```in
630
```

### Sample Output:

```out
3
5*6*7
```



# 思路

+ 设置slow和fast两个变量，称之为双指针
+ 分别指向目前计算中的最佳解的连乘的那几个数的头和尾
+ 当从slow到fast连乘起来的结果正好能被原数整除的时候，记录现在从slow到fast的数量并和最优解作对比
+ 当slow到fast的连乘小于原数的时候，增加fast，否则增加slow，同时把fast置为slow加一
+ 循环结束的条件是$fast >= sqrt(num)$ 即如果快指针已经超过了开根号的原数，再算下去已经没意义了，只剩下原数自己一个因数这种情况了。

# 代码

## 数据结构

+ num 是原数
+ slow,fast 是慢快指针
+ temp_sum 是这次运算中从slow到fast的连乘
+ max_num和max_sum 是已有最佳结果的起始因数和因数数量

## 算法

+ 读入数据，并初始化几个变量，先将slow和fast和temp_sum都置2
+ 将max_num, max_begin置为0,0
+ 开始循环，结束条件为$fast >= sqrt(num)$
+ 当temp_sum是num的因数的时候，将slow到fast的数量与最优解比较并更新最优解
+ fast再加一,temp_sum相应乘以新fast
+ 当temp_sum大于num的时候，slow加一，将fast置为slow，temp_sum置为slow
+ 最后若max_num是0，说明就num它自己和自己一个解。
+ 输出，8说了，开冲

## Python3

采用Python3直接能AC，就不放C++的了。

```python
from math import sqrt

num = int(input())
slow, fast = 2, 2
temp_sum = 2
max_num, max_begin = 0, 0

while fast < sqrt(num):
    
    if num % temp_sum == 0:
        if fast - slow + 1 > max_num:
            max_num = fast - slow + 1
            max_begin = slow
    
    fast += 1
    temp_sum *= fast
    
    if temp_sum > num:
        slow += 1
        fast = slow
        temp_sum = slow

if max_num == 0:
    max_num = 1
    max_begin = num

print(max_num)
print("*".join([str(i) for i in range(max_begin,max_begin+max_num)]))

```

