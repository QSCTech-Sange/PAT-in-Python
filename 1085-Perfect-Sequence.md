---
title: <Python><PAT> 1085 Perfect Sequence
date: 2019-09-02 12:59:49
tags: 
- PAT
- 题解
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 关键点在于理解题意。涉及到数学的表述总是有些抽象，单这题其实很简单。
---

# 题目

Given a sequence of positive integers and another positive integer *p*. The sequence is said to be a **perfect sequence** if *M*≤*m*×*p* where *M* and *m* are the maximum and minimum numbers in the sequence, respectively.

Now given a sequence and a parameter *p*, you are supposed to find from the sequence as many numbers as possible to form a perfect subsequence.

### Input Specification:

Each input file contains one test case. For each case, the first line contains two positive integers *N* and *p*, where $N(\leq 10 ^ 5)$ is the number of integers in the sequence, and $p(\leq 10 ^ 9)$ is the parameter. In the second line there are *N* positive integers, each is no greater than $10^9$.

### Output Specification:

For each test case, print in one line the maximum number of integers that can be chosen to form a perfect subsequence.

### Sample Input:

```in
10 8
2 3 20 4 5 1 6 7 8 9
```

### Sample Output:

```out
8
```

# 题解

## 思路

+ 题目的意思是
+ 给定一个序列和一个参数
+ 最多从序列中拿多少项
+ 能使得最大值除以最小值小于等于参数
+ 我们将序列从小到大排列
+ 遍历这个序列
+ 不断更新符合要求的最大距离即可
+ 注意C++要使用long int，int范围无法表示那么大的数量级。

## 数据结构

+ n 为列表长度
+ para 为给定参数
+ seq 为序列
+ ans 为答案

## 算法

+ 读入列表
+ 按从小到大排序
+ 遍历每一个数
  + 当这个数到最后一个数的距离大于ans的时候，说明还存在更新ans的可能
  + 当这个数和距离ans位置以后的数符合题目的最大最小值限定的时候
  + ans + 1
+ 输出ans即可

## 代码

+ 因为Python会有一个点过不了，因此也放了C++的解

### Python

```python
n, para = list(map(int, input().split()))
seq = sorted(list(map(int, input().split())))
ans = 0
for i, j in enumerate(seq):
    while i + ans < n and seq[i + ans] <= j * para:
        ans += 1
print(ans)

```

### C++

```c++
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

int main() {
    int n;
    long para;
    scanf("%d %ld",&n,&para);
    vector<long int> seq(n);
    for (int i = 0; i < n; i++)
        scanf("%ld",&seq[i]);
    sort(seq.begin(), seq.end());
    int ans = 0;
    for (int i = 0; i < n;i++){
        while (i  + ans < n && seq[i+ans]<=seq[i] * para)
            ans += 1;
    }
    cout << ans << endl;
}
```

