---
title: <Python><PAT> 1014 Waiting in Line
date: 2019-08-21 10:19:42
tags:
- PAT
- 题解
- 动态规划
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 算法难度不大，但是数据结构还是很有挑战性的，需要使用动态规划的思想。
---

# 题目

Suppose a bank has *N* windows open for service. There is a yellow line in front of the windows which devides the waiting area into two parts. The rules for the customers to wait in line are:

- The space inside the yellow line in front of each window is enough to contain a line with *M* customers. Hence when all the *N* lines are full, all the customers after (and including) the (*NM*+1)st one will have to wait in a line behind the yellow line.
- Each customer will choose the shortest line to wait in when crossing the yellow line. If there are two or more lines with the same length, the customer will always choose the window with the smallest number.
- $Customer_i$ will take $T_i$ minutes to have his/her transaction processed.
- The first *N* customers are assumed to be served at 8:00AM.

Now given the processing time of each customer, you are supposed to tell the exact time at which a customer has his/her business done.

For example, suppose that a bank has 2 windows and each window may have 2 customers waiting inside the yellow line. There are 5 customers waiting with transactions taking 1, 2, 6, 4 and 3 minutes, respectively. At 08:00 in the morning, $Customer_1$ is served at $window_1$ while $Customer_2$ is served at $window_2$. $Customer_3$ will wait in front of $window_1$ and $Customer_4$ will wait in front of $window_2$. $Customer_5$ will wait behind the yellow line.

At 08:01, $Customer_1$  is done and $Customer_5$  enters the line in front of $window_1$ since that line seems shorter now. $Customer_2$  will leave at 08:02, $Customer_4$ at 08:06,  $Customer_3$ at 08:07, and finally $Customer_5$  at 08:10.

### Input Specification:

Each input file contains one test case. Each case starts with a line containing 4 positive integers: *N* (≤20, number of windows), *M* (≤10, the maximum capacity of each line inside the yellow line), *K* (≤1000, number of customers), and *Q* (≤1000, number of customer queries).

The next line contains *K* positive integers, which are the processing time of the *K* customers.

The last line contains *Q* positive integers, which represent the customers who are asking about the time they can have their transactions done. The customers are numbered from 1 to *K*.

### Output Specification:

For each of the *Q* customers, print in one line the time at which his/her transaction is finished, in the format `HH:MM` where `HH` is in [08, 17] and `MM` is in [00, 59]. Note that since the bank is closed everyday after 17:00, for those customers who cannot be served before 17:00, you must output `Sorry` instead.

### Sample Input:

```in
2 2 7 5
1 2 6 4 3 534 2
3 4 5 6 7
```

### Sample Output:

```out
08:07
08:06
08:10
17:00
Sorry
```

# 题解

## 思路

+ 首先理解题意
+ 银行办理业务要排队
+ 题目的意思是，所有顾客，在今天早上八点，**全部**都到了，但是按照序号分先后
+ 有几个窗口，有一根黄线。在黄线里面的，就是排一个特定的窗口。黄线外面的，就是看哪个窗口人少了就去哪个窗口。每个窗口对于黄线里面的人数都有统一的容量限制
+ **注意**，题目的意思是，如果你在下班前一分钟办理了一个小时的业务，那也是可以帮你办的，但是如果正好下班了，你要办理哪怕一分钟的业务，都不可以，这是本题的坑点。
+ 数据结构上，开一个窗口数组，给每一个窗口开一个长度固定的队列，存放顾客，这个很容易想到。
+ 但是怎么合理的计算时间呢？
+ 我们可以开一个pop数组和一个end数组
+ pop数组代表，这个窗口下一个人结束的时间点
+ end数组代表，这个窗口最终结束的时间
+ pop用于给顾客选择，该去哪一个窗口
+ end数组用于给顾客计算时间
+ 这样整个问题就迎刃而解了
+ 先把没填满黄线的顾客处理掉
+ 再处理黄线后的

## 数据结构

+ Windows 含所有窗口，一个窗口含一个队列，包含等待的人的序号
+ pop 含每个窗口第一个人业务办完的时间
+ end 含每个窗口最后一个人业务办完的时间
+ time 含每个人需要的服务时间
+ result 含每个人的最终答案，也就是服务完的时间
+ index 就是顾客下标


## 算法

+ 先看黄线内的
+ 先按照容量来遍历
  + 再按照窗口来遍历
    + 给每个人分配到相应窗口的相应队伍位置
    + 更新pop为窗口**第一人**的服务时间
    + 更新end为窗口加上这个遍历的人的服务时间
    + 更新每个人的result为对应窗口的end
+ 再看黄线外的
  + 遍历每一个人
  + 每次找到所有窗口里pop时间最短的
  + 这个窗口弹出第一个人，更新pop为弹出后的队伍的第一个人的服务时间
  + 更新end为加上这个遍历人的服务时间
  + 更新result为对应窗口的end
+ 输出结果，注意要判断sorry的情况



## 代码

因为Python能AC，因此就放了Python的代码。尽管代码不长，但是思考量着实不小。

```python
num_window, capacity, num_cus, num_que = list(map(int, input().split()))
queues = [[] for _ in range(num_window)]
pop = [0 for _ in range(num_window)]
end = [0 for _ in range(num_window)]
time = list(map(int, input().split()))
result = [0 for _ in range(num_cus)]

# 先把黄线内的填满
index = 0
for i in range(capacity):
    for j in range(num_window):
        if index < num_cus:
            if i == 0:
                pop[j] = time[index]
            queues[j].insert(0, index)
            end[j] += time[index]
            result[index] = end[j]
            index += 1
            
# 处理黄线外的每一个人
while index < num_cus:
    # window是最快能腾出位置的窗口下标
    window = pop.index(min(pop))
    queues[window].pop()
    pop[window] += time[queues[window][-1]]
    queues[window].insert(0, index)
    end[window] += time[index]
    result[index] = end[window]
    index += 1

# 输出结果
que = list(map(int, input().split()))
for i in que:
    minute = result[i-1]
    if minute - time[i-1] >= 540:
        print("Sorry")
    else:
        minute += 480
        print("%02d:%02d" % (minute // 60, minute % 60))

```



