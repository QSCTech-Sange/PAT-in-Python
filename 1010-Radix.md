---
title: <Python><PAT> 1010 Radix
date: 2019-08-20 11:40:07
tags: 
- PAT
- 题解
- 字符串
- 二分查找
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 中档题，比较综合，如果没想到二分查找的话，会有一个点超时。
---

# 题目

Given a pair of positive integers, for example, 6 and 110, can this equation 6 = 110 be true? The answer is `yes`, if 6 is a decimal number and 110 is a binary number.

Now for any pair of positive integers *N*1 and *N*2, your task is to find the radix of one number while that of the other is given.

### Input Specification:

Each input file contains one test case. Each case occupies a line which contains 4 positive integers:

```
N1 N2 tag radix
```

Here `N1` and `N2` each has no more than 10 digits. A digit is less than its radix and is chosen from the set { 0-9, `a`-`z` } where 0-9 represent the decimal numbers 0-9, and `a`-`z` represent the decimal numbers 10-35. The last number `radix` is the radix of `N1` if `tag` is 1, or of `N2` if `tag` is 2.

### Output Specification:

For each test case, print in one line the radix of the other number so that the equation `N1` = `N2` is true. If the equation is impossible, print `Impossible`. If the solution is not unique, output the smallest possible radix.

### Sample Input 1:

```in
6 110 1 10
```

### Sample Output 1:

```out
2
```

### Sample Input 2:

```in
1 ab 1 2
```

### Sample Output 2:

```out
Impossible
```



# 题解

## 思路

+ 题目意思是
  + 输入有四个数
  + 第三个数为1或2，第四个数是进制
  + 如果第三个数是1,那么对应第一个数的进制就是第四个
  + 否则，对应第二个数的进制就是第四个。
  + 让你求另一个未知进制的数能不能有一个进制转换成已知的数。
  + 能就输出进制，不能就打印impossible

+ 步骤

  + 首先需要一个函数，能把指定字符串和进制转换成十进制的数
  + 将已知进制的数转换成十进制
  + 找到未知进制数的潜在进制范围
  + 通过二分查找缩小范围
  + 输出

  详细细节在算法里，要注意如果没有使用二分查找的话会有一个点超时。

## 数据结构

+ ori 是已知进制的字符串,ori_num是它的十进制整型
+ target 是未知进制的字符串，t 是它的十进制整型
+ low和high是二分查找的左右边界
+ min_radix 是计算初始low的工具变量，也就是target的最小潜在进制

## 算法

+ 先来谈谈将给定进制的字符串转换成十进制
  + 首先初始化一个ans = 0
  + 遍历字符串的每一个字符
  + 每次遍历，将ans乘以进制再加上这个字符
  + 返回ans就好
+ 再来谈谈二分查找的左右边界
  + 左边界是最小进制，是target所有字符里面的最大字符+1，小于这个进制的话，target就没法用这个进制来表示了。
  + 右边界是最小进制和ori_num的最大值。首先它不能比最小进制小，其次，如果它大于ori_num的话，那么一进位就超了，没有这种可能性
+ 最后谈谈二分查找
  + 这是一个很好用的二分查找模板，特点如下
  + 判断条件用 low < high 不用等于
  + mid 取左边界（在low和high之间是偶数的时候）
  + 对**排除左边界的所有情况**里，low =mid + 1
  + 其余情况 high = mid
  + 最后跳出循环后再比较一次low或high是否符合目标情况

> 记住以下几点
>
> 1. 循环判断条件不设等于号，这样跳出循环后拿low或high比较均可
> 2. mid取左边界的话，那么每次更新左边界都得加一，反之亦然
> 3. 条件里要放排除左边界的所有内容，也就是相等的情况下放右边界，反之依然
> 4. 不用比较相等的情况

## 代码

因为使用Python能AC，因此只留了Python的代码。

```python
# 函数：给定一个字符串和进制，返回它的十进制表示
def convert(ori: str, radix: int):
    ans = 0
    for i in ori:
        if i.isdigit():
            ans = ans * radix + int(i)
        else:
            ans = ans * radix + ord(i) - 87  # ord('a')为97
    return ans

# 数据读入
a = input().split()
if a[2] == '1':
    ori = a[0]
    target = a[1]
else:
    ori = a[1]
    target = a[0]
radix = int(a[3])

# 将已知进制数转为十进制
ori_num = convert(ori, radix)

# 找到未知进制数的最小进制
min_radix = chr(max([ord(i) for i in target]))
if min_radix.isdigit():
    min_radix = int(min_radix) + 1
else:
    min_radix = ord(min_radix) - 87

# 二分查找 其中high是low和已知进制数的较大值
low = min_radix
high = max(ori_num, low)
while low < high:
    mid = (low + high) // 2
    t = convert(target, mid)
    if t < ori_num:
        low = mid + 1
    else:
        high = mid

# 输出
if convert(target, low) == ori_num:
    print(low)
else:
    print("Impossible")

```

