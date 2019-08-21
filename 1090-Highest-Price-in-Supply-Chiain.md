---
title: <Python><PAT> 1090 Highest Price in Supply Chiain
date: 2019-08-15 16:27:11
tags: 
- PAT
- 题解
- DFS
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: BFS或DFS遍历，原理不难，但是有坑点。
---
## 1090 Highest Price in Supply Chiain
A supply chain is a network of retailers（零售商）, distributors（经销商）, and suppliers（供应商）-- everyone involved in moving a product from supplier to customer.

Starting from one root supplier, everyone on the chain buys products from one's supplier in a price *P* and sell or distribute them in a price that is *r*% higher than *P*. It is assumed that each member in the supply chain has exactly one supplier except the root supplier, and there is no supply cycle.

Now given a supply chain, you are supposed to tell the highest price we can expect from some retailers.

### Input Specification:

Each input file contains one test case. For each case, The first line contains three positive numbers: $$N (≤10^5)$$, the total number of the members in the supply chain (and hence they are numbered from $0$ to $N−1$); $P$, the price given by the root supplier; and $r$, the percentage rate of price increment for each distributor or retailer. Then the next line contains $N$ numbers, each number $S_i$ is the index of the supplier for the $i$-th member. $S_{root}$ for the root supplier is defined to be $$-1$$. All the numbers in a line are separated by a space.

### Output Specification:

For each test case, print in one line the highest price we can expect from some retailers, accurate up to $2$ decimal places, and the number of retailers that sell at the highest price. There must be one space between the two numbers. It is guaranteed that the price will not exceed $10^{10}$.

### Sample Input:

```in
9 1.80 1.00
1 5 4 4 -1 4 5 3 6
```

### Sample Output:

```out
1.85 2
```



## 思路

输入的第一行是公司数量，初始价格，和加钱比例

第二行的第0个数1代表公司[0]的上级是公司[1]，以此类推。我们不难想出这样的思路——

+ 记录level数组，表示传销层级
+ 记录last数组，记录每个公司的上级
+ 对last数组深度优先遍历，更新level

因为BFS还要重新记录所有的下级公司，所以推荐直接使用DFS完事儿嗷。

听起来不难对吧？我们看第一遍简单的 Python3 代码。



## 第一遍代码（缺一个点）

```python
from math import pow

# 第一行元数据的输入
a = input().split()
companies_num, init_price, rate = int(a[0]), float(a[1]), 1 + float(a[2]) / 100

# last列表记录上家
# level列表记录层级，-1表示还没计算，0表示根，传销最头头
last = []
level = [-1 for i in range(companies_num)]
for i in input().split():
    last.append(int(i))
    if int(i) == -1:
        level[len(last) - 1] = 0
 
# dfs 深度优先遍历
def count_level(index):
    last_company = last[index]
    if level[last_company] != -1:
        return level[last_company] + 1
    else:
        return count_level(last_company) + 1

# 记录最大传销层和数量
max_index = 0
max_count = 0
for i in range(companies_num):
    if level[i] == -1:
        level[i] = count_level(i)
    if level[max_index] < level[i]:
        max_count = 1
        max_index = i
    elif level[max_index] == level[i]:
        max_count += 1

        # 输出
print("%.2f %d" % (init_price * (pow(rate, level[max_index])), max_count))

```

看起来没毛病，放到牛客上直接AC，然而在 PAT 当中的第二个点，显示“非零返回”。这也不是超时啊，问题出在哪儿呢？为什么会非零返回呢？我转到C++上一探究竟。



## 第二遍代码（缺一个点）

```c++
#include<cstdio>
#include<iostream>
#include<cmath>

using namespace std;


int count_level(int index, const int level[], const int last[]) {
    int up = last[index];
    if (level[up] != -1)
        return level[up] + 1;
    else
        return count_level(up,level,last) + 1;
}

int main() {
    int companies_num;
    double init_price, rate;
    cin>>companies_num>>init_price>>rate;
    int last[companies_num];
    int level[companies_num];
    rate = 1 + rate / 100;
    for (int i = 0; i < companies_num; i++) {
        cin>>last[i];
        level[i] = last[i] == -1?0:-1;
    }
    int max_index = 0;
    int max_count = 0;
    for (int i = 0; i < companies_num; i++) {
        if (level[i] == -1)
            level[i] = count_level(i,level,last);
        if (level[max_index] < level[i]){
            max_count = 1;
            max_index = i;
        }
        else if(level[max_index] == level[i]){
            max_count += 1;
        }
    }
    double ans = init_price * pow(rate, level[max_index]);
    printf("%.2lf %d\n", ans, max_count);
}
```

这就是和刚刚的 Python3 一模一样的代码，除了转为了 c++。可是第二个点又错误了，这次是超时。想了想，应该使用因为level数组没有及时保存中间的计算值，浪费了很多计算。于是我将c++代码修改为如下



## 第三遍代码 （AC代码）

将第二遍代码中的`count_level()`函数修改为

```c++
int count_level(int index, int level[], int last[]) {
    int up = last[index];
    if (level[up] != -1)
        level[index] = level[up] + 1;
    else
        level[index] = count_level(up,level,last) + 1;
    return level[index];
}
```

同时调用的时候直接调用，不用赋值给`level[i]`。

其他内容和第二遍代码一样。

这次的代码AC了，看来我第二遍代码没错，只是没有剪枝超时了？那为什么第一遍Python的会显示“非零返回”？如果将Python修改为这样的代码会怎么样？



## 第四遍代码（错一个点）

将第一遍代码中的`count_level()`函数修改为

```python
def count_level(index):
    last_company = last[index]
    if level[last_company] != -1:
        level[index] = level[last_company] + 1
    else:
        level[index] = count_level(last_company) + 1
    return level[index]
```

同时调用的时候直接调用，不用赋值给`level[i]`。

其他内容和第一遍代码一样。

这次的代码应该AC了吧？结果还是第二个点显示“非零返回”。实在是让人迷惑。



## 思考

+ 建议使用第二遍代码，即使用剪枝的深度优先遍历，使用C++来写。
+ 这道题目给Python的时间其实比C++要充裕，通过第一二四遍的代码对比可以得出。
+ Python始终不对不是因为时间原因。
+ 使用错误异常机制定位到，是`count_level()` 函数里的内容报错。
+ 可是函数里的内容，这两种语言写法几乎是一模一样，除了最基础的语法部分。
+ 所以我陷入了迷惑。
+ 希望读者评论能够点醒我。