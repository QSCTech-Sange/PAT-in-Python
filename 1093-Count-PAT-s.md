---
title:  <Python><PAT> 1093 Count PAT's
date: 2019-09-03 09:28:50
tags: 
- PAT
- 题解
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 要想做对此题，你必须成为逻辑带师
---

# 题目

The string `APPAPT` contains two `PAT`'s as substrings. The first one is formed by the 2nd, the 4th, and the 6th characters, and the second one is formed by the 3rd, the 4th, and the 6th characters.

Now given any string, you are supposed to tell the number of `PAT`'s contained in the string.

### Input Specification:

Each input file contains one test case. For each case, there is only one line giving a string of no more than 105 characters containing only `P`, `A`, or `T`.

### Output Specification:

For each test case, print in one line the number of `PAT`'s contained in the string. Since the result may be a huge number, you only have to output the result moded by 1000000007.

### Sample Input:

```in
APPAPT
```

### Sample Output:

```out
2
```

# 题解

## 思路

+ 题目的意思是找出一个字符串中有多少个PAT。
+ 其中，PAT可以不连续，但是顺序必须是先P最后T。
+ 如果三者下标不同，就当作不同的字符串。
+ 这题的核心就是一个脑筋急转弯，转过来了就很简单。
+ 我们可以这么想。
+ 对于每一个A，能用它构成的字符串的总数就是它左边的P的个数乘以右边的T的个数。
+ 那么我们只需要先计算出总共有多少个T
+ 然后遍历字符串
  + 每遍历一个P，那么P的数量加一，代表下一个出现的A左边的P的数量会减1
  + 每遍历一个T，那么T的数量减一，代表下一个出现的A右边的T的数量会减1
  + 每遍历到一个A，将左边的T乘以右边的P添加进答案
+ 最后答案取模即可。
+ 注意，如果不使用Python的话，每次加的时候都要先取模，否则容易溢出。Python就不用考虑精度长度限制的问题。

## 数据结构

+ message 是原来的字符串
+ p统计左边的P的个数
+ t统计右边的T的个数
+ ans记录最终的答案
+ i是遍历的变量

## 算法

+ 参照思路里的算法来走一遍即可。

## 代码

+ 由于使用Python能够AC，因此只放了Python的解。

```python
message = input()
p, t, ans = 0, message.count("T"), 0
for i in message:
    if i == "P":
        p += 1
    elif i == "T":
        t -= 1
    elif i == "A":
        ans = ans + p * t
print(ans % 1000000007)
```

