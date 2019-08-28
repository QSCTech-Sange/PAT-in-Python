---
title: <Python><PAT> 1054 The Dominant Color
date: 2019-08-27 16:58:53
tags: 
- PAT
- 题解
- 哈希表
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 水水的。顺带介绍一下Counter类。
---

# 题目

Behind the scenes in the computer's memory, color is always talked about as a series of 24 bits of information for each pixel. In an image, the color with the largest proportional area is called the dominant color. A **strictly** dominant color takes more than half of the total area. Now given an image of resolution *M* by *N* (for example, 800×600), you are supposed to point out the strictly dominant color.

### Input Specification:

Each input file contains one test case. For each case, the first line contains 2 positive numbers: *M* (≤800) and *N* (≤600) which are the resolutions of the image. Then *N* lines follow, each contains *M* digital colors in the range [0,224). It is guaranteed that the strictly dominant color exists for each input image. All the numbers in a line are separated by a space.

### Output Specification:

For each test case, simply print the dominant color in a line.

### Sample Input:

```in
5 3
0 0 255 16777215 24
24 24 0 0 24
24 0 24 24 24
```

### Sample Output:

```out
24
```

# 题解

## 思路

+ 题目意思就是给你一堆数据，让你找到出现次数大于总数一半的
+ 很显然用哈希表来存储每个点的出现频次
+ 这是传统的方法
+ 在Python里统计频次有专门的Counter，也一并附上代码，使用它非常方便。

## 数据结构

+ _dict 是一个哈希表
  + 键是题目里的颜色
  + 值是出现次数

## 算法

+ 就哈希表呗

## 代码

+ 因为使用Python能AC，所以只放了Python的代码。

### Python3 defaultdict

```python
from collections import defaultdict

M, N = list(map(int, input().split()))
half = M * N // 2
_dict = defaultdict(int)
for _ in range(N):
    for i in input().split():
        _dict[i] += 1
        if _dict[i] > half:
            print(i)
            break

```
### Python3 counter

```python
from collections import Counter

M, N = list(map(int, input().split()))
ans = Counter()
for _ in range(N):
    ans.update(input().split())
print(ans.most_common(1)[0][0])

```

