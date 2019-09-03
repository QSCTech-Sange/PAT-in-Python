---
title: <Python><PAT> 1103 Integer Factorization
date: 2019-09-03 17:10:55
tags:
- PAT
- 题解
- DFS
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 比较抽象的DFS，难度较大
---

# 题目

The *K*−*P* factorization of a positive integer *N* is to write *N* as the sum of the *P*-th power of *K* positive integers. You are supposed to write a program to find the *K*−*P* factorization of *N* for any positive integers *N*, *K* and *P*.

### Input Specification:

Each input file contains one test case which gives in a line the three positive integers *N* (≤400), *K* (≤*N*) and *P* (1<*P*≤7). The numbers in a line are separated by a space.

### Output Specification:

For each case, if the solution exists, output in the format:

```
N = n[1]^P + ... n[K]^P
```

where `n[i]` (`i` = 1, ..., `K`) is the `i`-th factor. All the factors must be printed in non-increasing order.

Note: the solution may not be unique. For example, the 5-2 factorization of 169 has 9 solutions, such as $12^2+4^2+2^2+2^2+1^2$, or $11^2+6^2+2^2+2^2+2^2$, or more. You must output the one with the maximum sum of the factors. If there is a tie, the largest factor sequence must be chosen -- sequence $\{a_1,a_2,\cdots ,a_K \}$ is said to be **larger** than $\{b_1,b_2,\cdots ,b_K \}$ if there exists 1≤*L*≤*K* such that $a_i = b_i$ for *i*<*L* and $a_L > b_L$.

If there is no solution, simple output `Impossible`.

### Sample Input 1:

```in
169 5 2
```

### Sample Output 1:

```out
169 = 6^2 + 6^2 + 6^2 + 6^2 + 5^2
```

### Sample Input 2:

```in
169 167 3
```

### Sample Output 2:

```out
Impossible
```

# 题解

## 思路

+ 这道题题目意思有点抽象，但是借助例子应该能看懂。
+ n 就是等号左边的数，K 就是右边加的那些项的个数，P就是指数
+ 我们最大的底数是`pow(n,1/P)`，从1到这个数之间的所有数都是潜在的底数，且可以重复选取。
+ 一开始我想到用DFS，以数值的大小为转移
+ 但是这样的话，每次要对所有潜在的底数DFS，复杂度过高
+ 更好的方法是以底数为转移，每次dfs更新底数

## 数据结构

+ n,k,p 对应题目里的原数，项数和指数
+ ans是最佳的底数们，是一个列表
+ maxFacsum 是目前最佳的底数们的底数和 
+ sums 是目前计算中的指数和
+ facsums 是目前计算中的底数和
+ now 是目前计算中的底数们

## 算法

+ 从最大的潜在底数开始DFS
  + 更新sums和facsums和now
  + 继续DFS这个底数——因为底数可以重复选取
  + 回溯sums和facsums和now
  + DFS上一个底数
+ 当DFS到sums > n 或 now里面多于k了或者index 为0 的时候
  + 及时剪枝
+ 当DFS到sums = n 且 now的个数等于k的时候
  + 比较最佳解和目前解的底数和，判断是否要更新最佳解。
+ 输出即可

## 代码

+ 这道题使用Python会有一个点超时。所以放上了Python和C++的两种解法。

### Python

```python
n, k, p = list(map(int, input().split()))
ans, maxFacSum = [], 0
sums, facSums, now = 0, 0, []


def DFS(index):
    global sums, facSums, now, ans, maxFacSum, n, k, p
    if sums > n or len(now) > k or index == 0:
        return
    if sums == n and len(now) == k and facSums > maxFacSum:
        maxFacSum = facSums
        ans = now.copy()
        return

    now.append(index)
    sums += pow(index, p)
    facSums += index
    DFS(index)
    now.pop()  				# 回溯
    sums -= pow(index, p)
    facSums -= index
    DFS(index - 1)


DFS(int(pow(n, 1 / p)))
if ans:
    ans = list(map(lambda x: str(x) + "^" + str(p), ans))
    ans = " + ".join(ans)
    print(n, "=", ans)
else:
    print("Impossible")

```

### C++

```c++
#include <cmath>
#include <iostream>
#include <vector>

using namespace std;

int n, k, p;
int maxFacSum, facSums, sums;
vector<int> ans, now;

int DFS(int index) {
    if (sums > n or now.size() > k or index == 0)
        return 0;
    if (sums == n and now.size() == k and facSums > maxFacSum) {
        maxFacSum = facSums;
        ans = now;
        return 0;
    }
    now.push_back(index);
    sums += pow(index, p);
    facSums += index;
    DFS(index);
    now.pop_back();             // 回溯
    sums -= pow(index, p);
    facSums -= index;
    DFS(index - 1);
}


int main() {
    cin >> n >> k >> p;
    DFS(int(pow(n, 1.0 / p)));
    if (!ans.empty()) {
        printf("%d = ", n);
        for (int i = 0;i<ans.size();i++) {
            if (i!= ans.size()-1)
                printf("%d^%d + ", ans[i], p);
            else
                printf("%d^%d\n", ans[i], p);
        }
    } else
        printf("Impossible");
}

```

