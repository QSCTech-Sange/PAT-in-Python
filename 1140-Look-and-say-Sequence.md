---
title: <Python><PAT> 1140 Look-and-say Sequence
date: 2019-09-06 10:45:05
tags:
- PAT
- 题解
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 报数问题
---

# 题目

ook-and-say sequence is a sequence of integers as the following:

```
D, D1, D111, D113, D11231, D112213111, ...
```

where `D` is in [0, 9] except 1. The (n+1)st number is a kind of description of the nth number. For example, the 2nd number means that there is one `D` in the 1st number, and hence it is `D1`; the 2nd number consists of one `D` (corresponding to `D1`) and one 1 (corresponding to 11), therefore the 3rd number is `D111`; or since the 4th number is `D113`, it consists of one `D`, two 1's, and one 3, so the next number must be `D11231`. This definition works for `D` = 1 as well. Now you are supposed to calculate the Nth number in a look-and-say sequence of a given digit `D`.

### Input Specification:

Each input file contains one test case, which gives `D` (in [0, 9]) and a positive integer N (≤ 40), separated by a space.

### Output Specification:

Print in a line the Nth number in a look-and-say sequence of `D`.

### Sample Input:

```in
1 8
```

### Sample Output:

```out
1123123111
```

# 题解

## 思路

+ 这道题是报数问题，和LeetCode的报数问题几乎一样，可以结合着看。顺带一提，我在leetcode上这道题的题解应该被顶到第一个了（
+ 题目的核心是理解题意，属实抽象
+ 第一个数先随便给你一个，就当作D吧
+ 然后第二个数根据前一个数来抉择
+ 第一个数里面有“1个D”，那么第二个数就是D1,含义就是1个D
+ 第二个数是“1个D   1个1”，那么第三个数就是D111
+ 同理，第四个数就是D113，含义是1个D 3个1
+ 然后就仿照这个做就完事了

## 数据结构

+ old 前一个人的数
+ new 后一个人的数
+ index 指遍历到了前一个人的哪一个下标
+ digit 是遍历前一个人的某个字符
+ count 指digit出现了几次

## 算法

+ 就是单纯模拟报数的流程，可以参阅代码
+ 注释得很详尽了

## 代码

+ 因为使用Python可以AC，因此只放了Python的解。

```python
old, N = input().split()
# 开始模拟报数
for i in range(int(N) - 1):
    # 预设下一个人为空字符串，已经读取到上一个人的下标是0,就是没开始读取
    new = ""
    index = 0
    # 遍历上一个人的字符串
    while index < len(old):
        count = 1
        digit = old[index]
        # 每找到一个数，看看后面有几个和它是相同的
        while index + 1 < len(old) and digit == old[index + 1]:
            count += 1
            index += 1
        # 添加进new里
        new += (str(digit)+str(count))
        # 看这个人的下一个字符
        index += 1
    # 后一个人成为前一个人
    old = new

print(old)
```

