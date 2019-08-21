---
title: <Python><PAT> 1001 A+B Format
date: 2019-08-15 17:33:14
tags: 
- PAT
- 题解
- 字符串
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 大水题，字符串。答应我，遇到字符串的题就用Python，好吗？
---

## 1001 A+B Format

Calculate $a + b$ and output the sum in standard format -- that is, the digits must be separated into groups of three by commas (unless there are less than four digits).

### Input Specification:

Each input file contains one test case. Each case contains a pair of integers $a$ and $b$ where $−10^6≤a,b≤10^6$. The numbers are separated by a space.

### Output Specification:

For each test case, you should output the sum of $a$ and $b$  in one line. The sum must be written in the standard format.

### Sample Input:

```in
-1000000 9
```

### Sample Output:

```out
-999,991
```

## 思路

+ 使用Python处理字符串，一下子就简单了很多，当然这题用C++的`string`类拼接也可以，并不麻烦。不过我还是建议使用Python，因为所以的点都能过。
+ 读取两个数，求它们的和，保存成字符串。
+ 然后从后向前遍历，每隔三个位置，加一个逗号。
+ 当再向前一个是负号的时候不用加逗号了。

## 代码

```python
a = str(sum(list(map(int,input().split()))))
count = 0
for i in range(len(a)-1,0,-1):
    count += 1
    if count % 3 == 0 and a[i-1]!='-':
        a = a[:i] + ',' + a[i:]
print(a)
```

>  太水了这题。