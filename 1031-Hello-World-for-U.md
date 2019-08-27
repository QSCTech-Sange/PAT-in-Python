---
title: <Python><PAT> 1031 Hello World for U
date: 2019-08-24 10:48:53
tags: 
- PAT
- 题解
- 字符串
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 字符串的定位与输出。
---

# 题目

Given any string of *N* (≥5) characters, you are asked to form the characters into the shape of `U`. For example, `helloworld` can be printed as:

```
h  d
e  l
l  r
lowo
```

That is, the characters must be printed in the original order, starting top-down from the left vertical line with $n_1$ characters, then left to right along the bottom line with $n_2$ characters, and finally bottom-up along the vertical line with $n_3$ characters. And more, we would like `U` to be as squared as possible -- that is, it must be satisfied that $n1=n3=max \{k | k≤n2$ for all $3≤n2≤N \}$ with $n1+n2+n3−2=N$.

### Input Specification:

Each input file contains one test case. Each case contains one string with no less than 5 and no more than 80 characters in a line. The string contains no white space.

### Output Specification:

For each test case, print the input string in the shape of U as specified in the description.

### Sample Input:

```in
helloworld!
```

### Sample Output:

```out
h   !
e   d
l   l
lowor
```

# 题目

## 思路

+ 就是按要求输出一个U型
+ 主要是抓height和width
+ 这里把height定义为总高度减1,就是不算最后一行。middle为width减2,也就是不算左右两列，就中间两列。
+ 先理想情况下，围成一个正方形。
+ 那么不算左下右下两个角落，总共字符串长度为n-2，它正好能被3整除，商就是middle和height。
+ 如果(n-2)//3余1呢？那么需要插到中间，也就是middle加一
+ 余2的话，就插在两边，height加一。

## 数据结构

+ 这还能有个啥

## 算法

+ 也不需要吧

## 代码

+ 因为使用Python能AC，因此只放了Python的代码。

```python
a = input()
# 找到合适的高和宽
middle = (len(a) - 2) // 3
height = middle
if (len(a) - 2) % 3 == 1:
    middle += 1
elif (len(a) - 2) % 3 == 2:
    height += 1
# 输出
for i in range(height):
    print(a[i], end='')
    print(middle * " ", end='')
    print(a[-i - 1])
print(a[height:-height])
```

