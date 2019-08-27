---
title: <Python><PAT> 1045 Favorite Color Stripe
date: 2019-08-26 17:24:56
tags: 
- PAT
- 题解
- 动态规划
- DFS
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 有点难的动态规划
---

# 题目

Eva is trying to make her own color stripe out of a given one. She would like to keep only her favorite colors in her favorite order by cutting off those unwanted pieces and sewing the remaining parts together to form her favorite color stripe.

It is said that a normal human eye can distinguish about less than 200 different colors, so Eva's favorite colors are limited. However the original stripe could be very long, and Eva would like to have the remaining favorite stripe with the maximum length. So she needs your help to find her the best result.

Note that the solution might not be unique, but you only have to tell her the maximum length. For example, given a stripe of colors {2 2 4 1 5 5 6 3 1 1 5 6}. If Eva's favorite colors are given in her favorite order as {2 3 1 5 6}, then she has 4 possible best solutions {2 2 1 1 1 5 6}, {2 2 1 5 5 5 6}, {2 2 1 5 5 6 6}, and {2 2 3 1 1 5 6}.

### Input Specification:

Each input file contains one test case. For each case, the first line contains a positive integer *N* (≤200) which is the total number of colors involved (and hence the colors are numbered from 1 to *N*). Then the next line starts with a positive integer *M* (≤200) followed by *M* Eva's favorite color numbers given in her favorite order. Finally the third line starts with a positive integer *L* (≤104) which is the length of the given stripe, followed by *L* colors on the stripe. All the numbers in a line a separated by a space.

### Output Specification:

For each test case, simply print in a line the maximum length of Eva's favorite stripe.

### Sample Input:

```in
6
5 2 3 1 5 6
12 2 2 4 1 5 5 6 3 1 1 5 6
```

### Sample Output:

```out
7
```

# 题解

## 思路

+ 一开始想到用DFS做，然而超时超得头皮发麻
+ 更好的方式是动态规划
+ 我们开一个dp数组，比如`dp[i][j]` ，前面一项是喜欢的彩带的颜色下标，后面表示的是总彩带的颜色下标。整个dp数组的含义是，在以`i`颜色结尾，在总彩带`[:j]`范围内最长的符合要求的切割字符串的长度。
+ 比如说，我喜欢的彩带是1,2,3，给出来的彩带是1,3,2,3,5。那么`dp[0][2]`的意思就是，在`132`组成的所有子串中，以颜色1结尾的最长字串有多长。
+ 先初始化整个dp数组的值都是0
+ 按照喜欢的颜色下标的顺序开始遍历dp的外层——题目中要求的喜欢的颜色下标也是有序的，所以可以这么遍历。
  + 按照总彩带的长度开始遍历
    + 考虑`dp[i1][j]`，即从头到这个字符中的所有子串中，以喜欢这个彩带颜色结尾的最大符合要求的字串长度。
    + 它应该是以这个字符结尾，以喜欢彩带下一位结尾和以这个彩带结尾，以字符下一位结尾的最大值。即`max(dp[i][j-1], dp[i-1][j])`，如果在喜欢的彩带里的`i`和`j`所表示的颜色相同，那么再加一
+ 输出dp的最后一项
+ 完事儿。

## 数据结构

+ dp 是一个动态规划数组
+ 含义如思路所说

## 算法

+ 如思路所说

## 代码

+ 因为使用Python即便用了动态规划还是会超时，所以建议使用Python来AC这题。

### Python

```python
_ = int(input())
like = list(map(int, input().split()[1:]))
strip = list(map(int, input().split()[1:]))
dp = [[0 for _ in range(len(strip))] for _ in range(len(like))]
for i in range(len(like)):
    for j in range(len(strip)):
        dp[i][j] = 0 if i - 1 < 0 and j - 1 < 0 else max(dp[i - 1][j], dp[i][j - 1])
        dp[i][j] += 1 if like[i] == strip[j] else 0
print(dp[-1][-1])

```

### C++

```c++
#include <memory>
#include <cstring>
using namespace std;

int main() {
    int num_color;
    scanf("%d", &num_color);
    int num_like;
    scanf("%d", &num_like);
    int like[num_like];
    for (int i = 0; i < num_like; i++)
        scanf("%d", &like[i]);
    int num_strip;
    scanf("%d", &num_strip);
    int strip[num_strip];
    for (int i = 0; i < num_strip; i++)
        scanf("%d", &strip[i]);
    int dp[num_like][num_strip];
    for (int i = 0; i < num_like; i++) {
        for (int j = 0; j < num_strip; j++) {
            if (i-1<0 and j - 1<0)
                dp[i][j] = 0;
            else if (i-1<0)
                dp[i][j] = dp[i][j - 1];
            else if (j-1<0)
                dp[i][j] = dp[i - 1][j];
            else
                dp[i][j] = max(dp[i][j - 1],dp[i - 1][j]);
            dp[i][j] = like[i] == strip[j] ? dp[i][j] + 1 : dp[i][j];
        }
    }
    printf("%d", dp[num_like - 1][num_strip - 1]);
}
```

