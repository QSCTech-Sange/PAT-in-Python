---
title: <Python><PAT> 1079 Total Sales of Supply Chain
date: 2019-08-14 13:39:19
tags: 
- PAT
- 题解
- BFS
- DFS
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 较为简单的基础题，需要 BFS 或者 DFS 知识即可，无坑点
---

A supply chain is a network of retailers（零售商）, distributors（经销商）, and suppliers（供应商）-- everyone involved in moving a product from supplier to customer.

Starting from one root supplier, everyone on the chain buys products from one's supplier in a price P and sell or distribute them in a price that is r% higher than P. Only the retailers will face the customers. It is assumed that each member in the supply chain has exactly one supplier except the root supplier, and there is no supply cycle.

Now given a supply chain, you are supposed to tell the total sales from all the retailers.

## Input Specification:

Each input file contains one test case. For each case, the first line contains three positive numbers: $ N (≤10^5)$, the total number of the members in the supply chain (and hence their ID's are numbered from $0$ to $N−1$, and the root supplier's ID is $0$); $P$, the unit price given by the root supplier; and $r$, the percentage rate of price increment for each distributor or retailer. Then $N$ lines follow, each describes a distributor or retailer in the following format:
$$
K_i ID[1] ID[2]...ID[K_i]
$$
where in the $i$-th line, $K_i$  is the total number of distributors or retailers who receive products from supplier $i$, and is then followed by the ID's of these distributors or retailers. $K_j$ ​  being $0 $ means that the $j$-th member is a retailer, then instead the total amount of the product will be given after $K_j$. All the numbers in a line are separated by a space.

## Output Specification:

For each test case, print in one line the total sales we can expect from all the retailers, accurate up to 1 decimal place. It is guaranteed that the number will not exceed $10^{10}.$

## Sample Input:

```10 1.80 1.00
3 2 3 5
1 9
1 4
1 7
0 7
2 6 1
1 8
0 9
0 4
0 3
```

## Sample Output:

```
42.4
```



# 思路

这道题算比较简单，就是计算传销的最下级的销售额。简单的说，对图的一个遍历，更新每个节点的数据（传销层次）。然后算出最终结果。使用`Python3`的话会有三个点超时过不了，使用 `C++` 就OK了。使用`BFS`和`DFS`均可，注意层级的更新顺序就行了。



## 代码

### Python3

非常省事儿，就20行左右就能完成。

#### 数据结构的定义

- companies是一个列表，包含所有公司，下标即公司id
- 一个公司以一个字典的形式表示，包含层数
- 对retailer，会添加nums键，代表卖了多少份
- 对非retailer，会添加sons键，值是一个列表，代表子公司数量

```python
# 第一行元数据的输入
a = input().split()
companies_num = int(a[0])
init_price = float(a[1])
rate = 1 + float(a[2]) / 100

companies = [{'level':0} for i in companies_num]

# 更新每个公司的sons键或nums键
for father in range(companies_num):
    info = input().split()
    if info[0] != '0':
        companies[father]['sons'] = []
        for son in info[1:]:
            companies[father]['sons'].append(int(son))
    else:
        companies[father]['nums'] = int(info[1])

# 更新每个公司的层级，可以使用BFS或DFS，这里使用了DFS。BFS需要把队列改为栈
# 遇到了retailer就直接计算总和了
sums = 0
queue = [0]
while stack:
    father = queue.pop()
    if 'sons' in companies[father].keys():
        for son in companies[father]['sons']:
            companies[son]['level'] = companies[father]['level'] + 1
            queue.insert(0,son)
    else:
        sums += companies[father]['nums'] * init_price * rate ** companies[father]['level']

print(round(sums, 1))
```

### C ++

`c++` 其实也就不到60行，也挺简单的，此题建议直接使用`c++`来写。流程和 `Python3` 的一样，就不放注释了。

```c++
#include <iostream>
#include <stack>
#include <cmath>

using namespace std;


int main() {

    int nodes_num = 0;
    double init_price, rate;
    cin >> nodes_num >> init_price >> rate;
    rate = 1 + rate / 100;

    int level[nodes_num];
    int num[nodes_num];
    int *sons[nodes_num];
    for (int i = 0; i < nodes_num; i++) {
        level[i] = 0;num[i] = 0;
    }


    for (int father = 0; father < nodes_num; father++) {
        int sons_nums = 0;
        cin >> sons_nums;
        if (sons_nums != 0) {
            num[father] = sons_nums;
            sons[father] = (int *) malloc(sons_nums * sizeof(int));
            for (int i = 0; i < sons_nums; i++) {
                int son;cin >> son;
                sons[father][i] = son;
            }
        } else {
            int nm;cin >> nm;
            num[father] = nm;
            sons[father] = nullptr;
        }
    }

    double sums = 0;
    stack<int> sk;
    sk.push(0);
    while (!sk.empty()) {
        int father = sk.top();sk.pop();
        if (sons[father] != nullptr) {
            for (int son_index = 0; son_index < num[father]; son_index++) {
                level[sons[father][son_index]] = level[father] + 1;
                sk.push(sons[father][son_index]);
            }
        }
        else{
            sums += num[father] * init_price * pow(rate, level[father]);
        }

    }

    printf("%.1f\n", sums);

}

```

