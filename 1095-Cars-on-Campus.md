---
title: <Python><PAT> 1095 Cars on Campus
date: 2019-08-17 17:30:31
tags: 
- PAT
- 题解
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 大坑题，高难，难的不是算法，是题目意思太费解了。同时也有些繁琐。不过我尽可能最优化了运行时间和代码量。
---

 # 题目

Zhejiang University has 8 campuses and a lot of gates. From each gate we can collect the in/out times and the plate numbers of the cars crossing the gate. Now with all the information available, you are supposed to tell, at any specific time point, the number of cars parking on campus, and at the end of the day find the cars that have parked for the longest time period.

### Input Specification:

Each input file contains one test case. Each case starts with two positive integers *N* (≤104), the number of records, and *K* (≤8×104) the number of queries. Then *N* lines follow, each gives a record in the format:

```
plate_number hh:mm:ss status
```

where `plate_number` is a string of 7 English capital letters or 1-digit numbers; `hh:mm:ss` represents the time point in a day by hour:minute:second, with the earliest time being `00:00:00` and the latest `23:59:59`; and `status` is either `in`or `out`.

Note that all times will be within a single day. Each `in` record is paired with the chronologically next record for the same car provided it is an `out` record. Any `in` records that are not paired with an `out` record are ignored, as are `out`records not paired with an `in` record. It is guaranteed that at least one car is well paired in the input, and no car is both `in` and `out` at the same moment. Times are recorded using a 24-hour clock.

Then *K* lines of queries follow, each gives a time point in the format `hh:mm:ss`. Note: the queries are given in **ascending** order of the times.

### Output Specification:

For each query, output in a line the total number of cars parking on campus. The last line of output is supposed to give the plate number of the car that has parked for the longest time period, and the corresponding time length. If such a car is not unique, then output all of their plate numbers in a line in alphabetical order, separated by a space.

### Sample Input:

```in
16 7
JH007BD 18:00:01 in
ZD00001 11:30:08 out
DB8888A 13:00:00 out
ZA3Q625 23:59:50 out
ZA133CH 10:23:00 in
ZD00001 04:09:59 in
JH007BD 05:09:59 in
ZA3Q625 11:42:01 out
JH007BD 05:10:33 in
ZA3Q625 06:30:50 in
JH007BD 12:23:42 out
ZA3Q625 23:55:00 in
JH007BD 12:24:23 out
ZA133CH 17:11:22 out
JH007BD 18:07:01 out
DB8888A 06:30:50 in
05:10:00
06:30:50
11:00:00
12:23:42
14:00:00
18:00:00
23:59:00
```

### Sample Output:

```out
1
4
5
2
1
0
1
JH007BD ZD00001 07:20:09
```

# 思路

首先来理解题意

+ 你是停车场管理员，手头有乱序的停车记录
+ 老板让你查询几个时间点停车场分别有多少辆车，以及停的总时间最长的车和这个最长时间。

输入是

+ 先给你记录数量，和老板查询次数
  + 每条记录，以
    + 车牌号
    + 时间点
    + 车进来还是车出去 
  + 三个部分组成
  + 查询就是给一个时间点

输出是

+ 每次查询，输出现在停车场的车数
+ 最后输出停的总时间最长的车（可能有多个）和这个最长时间

这么一剖析，变得清楚了很多。

# 难点

这道题烦就烦在，它给的记录 **不是** 每条都有效的。

一辆车可以多次进停车场，比如说，早上三点到五点，下午一点到两点，那么这辆车总共三个小时，三条记录，这是OK的。

但是呢，数据可能是，这样，同一辆车，早上三点进了一次，早上五点进了一次，早上六点离开了。这很明显有数据是冲突的。同一辆车没出去前怎么进的来？那么我们需要哪些数据？题目需要我们选取后面一条。也就是说，真实数据遵循这样一个原则——

**同一辆车的所有记录按照时间排序，如果一条入记录的下一条正好是出，那么这两条有效。**

这是一大，或者说最大的坑点，完全应该归咎于题目的表达不清晰。

还有一个就是时间上的处理。以及查询的时候怎么尽可能减少遍历次数。

+ 时间上，将它全转为秒
+ 查询的时候，先把记录排序，然后按照记录的下标作为时间点的卡点
+ 因为查询时间是顺序进行的，所以我们只需要沿着卡点记录车的数量就行了。

# 代码

### 数据结构

+ records 记录全部的记录，是一个列表，包含多个record
  + 每一个record第一项是id
  + 第二项是时间点（转为秒）
  + 第三项是进来还是出去的flag，进为1,出来为-1
+ clean_records是洗干净的记录，剔除了records无效的记录
+ max_time, max_id 记录目前已经计算出来的最大停车时间和停这些时间的id
+ pre,  park_time 记录上一次遍历的车的id和这个车的停车时间，便于和max_time做比较
+ time_index是时间点，和按照时间排序后的clean_records下标相对应。

### 算法

+ 先读入数据
+ 按照先id后时间的两个原则排序records
+ 建立一个clean_records
+ 遍历records，把符合条件的添加到clean_reocrds里，同时不断更新最大停车时间以及id
+ 把clean_records按照时间来排序
+ 遍历clean_records，遍历中更新count，也就是停车场车数，遍历和回答质询在一个循环里同时进行
  + 如果质询的时间大于目前遍历的时间，那么遍历时间继续增加
  + 否则，打印出来count

+ 最后打印最大停车时间和id

### Python3

这道题使用Python3会有一个点超时，建议使用`C++`来AC。但是Python写得是真的爽啊，尤其是sort的依据，太简单直接了。

```python
records_num, queries_num = list(map(int, input().split()))
records = []

# 每一个record是一个元祖，包含id,时间，进出状态标记
for _ in range(records_num):
    record_info = input().split()
    id = record_info[0]
    hour, minute, second = record_info[1].split(":")
    time = int(hour) * 3600 + int(minute) * 60 + int(second)
    status = 1 if record_info[2] == 'in' else -1
    records.append((id, time, status))

# 按照id优先，时间点第二的升序原则排序所有记录
records.sort(key=lambda x: (x[0], x[1]))

# 清理并整理出干净的records，同时计算出最大停车时间以及车牌号数组
# 题目意思是，同一辆车，按时间排序，必须前后两个record正好是一进一出，才统计进去。
max_time, max_id, pre, clean_records, park_time = 0, [], records[0][0], [], 0
for i in range(records_num - 1):
    if records[i][0] == records[i + 1][0] and records[i][2] == 1 and records[i + 1][2] == -1:
        clean_records.append(records[i])
        clean_records.append(records[i + 1])
        if records[i][0] != pre:
            if max_time < park_time:
                max_time = park_time
                max_id = [pre]
            elif max_time == park_time:
                max_id.append(pre)
            pre = records[i][0]
            park_time = 0
        park_time += (records[i + 1][1] - records[i][1])
        
# 刚刚的对比会忽略最晚的车，最晚的车也要比一比
if max_time < park_time:
    max_time = park_time
    max_id = [pre]
elif max_time == park_time:
    max_id.append(pre)
    
# 将所有有效记录按时间排列
clean_records.sort(key=lambda x: x[1])

# 执行查询任务,因为查询时间是递增的，所以每次查询从上次的时间下标之后就可以了
time_index, count = 0, 0
for _ in range(queries_num):
    hour, minute, second = list(map(int,input().split(":")))
    query_time = hour * 3600 + minute * 60 + second
    while time_index < len(clean_records) and clean_records[time_index][1] <= query_time:
        count += clean_records[time_index][2]
        time_index += 1
    print(count)

# 打印所有的最大停车时间的车牌号
for i in max_id:
    print(i, end=' ')
print("%02d:%02d:%02d" % (max_time // 3600, max_time % 3600 // 60, max_time % 60))
```

### C++

```c++
#include <vector>
#include <algorithm>
#include <string>
#include <iostream>

using namespace std;

// 每一条记录的数据结构，当记录是进来，flag=1,反之，flag=-1
struct Record {
    string id;
    int time, flag;

    // 重载小于号以便排序
    bool operator<(const Record &b) {
        if (id != b.id)
            return id < b.id;
        return time < b.time;
    }
};

bool cmp_by_time(Record *a, Record *b) {
    return a->time < b->time;
}

int main() {
    int n, k, maxtime = -1;
    cin >> n >> k;

    // 读取记录
    vector<Record> records(n);
    for (int i = 0; i < n; i++) {
        string status;
        cin >> records[i].id;
        int h, m, s;
        scanf("%d:%d:%d", &h, &m, &s);
        records[i].time = h * 3600 + m * 60 + s;
        cin >> status;
        records[i].flag = status == "in" ? 1 : -1;
    }
    // 按照id优先，时间点第二的升序原则排序所有记录
    sort(records.begin(), records.end());

    // 清理并整理出干净的records，同时计算出最大停车时间以及车牌号数组
    // 题目意思是，同一辆车，按时间排序，必须前后两个record正好是一进一出，才统计进去。
    vector<Record *> clean_records;
    int park_time = 0;
    string pre = records[0].id;
    vector<string> ans;
    for (int i = 0; i < n - 1; i++) {
        if (records[i].id == records[i + 1].id && records[i].flag == 1 && records[i + 1].flag == -1) {
            clean_records.push_back(&records[i]);
            clean_records.push_back(&records[i + 1]);
            if (records[i].id != pre){
                if (park_time > maxtime){
                    maxtime = park_time;
                    ans.clear();
                    ans.emplace_back(pre);
                }else if (park_time == maxtime)
                    ans.emplace_back(pre);
                park_time = records[i + 1].time - records[i].time;
                pre = records[i].id;
            }else
                park_time += (records[i + 1].time - records[i].time);
        }
    }

    // 刚刚的对比会忽略最晚的车，最晚的车也要比一比
    if (park_time > maxtime){
        maxtime = park_time;
        ans.clear();
        ans.emplace_back(pre);
    }else if (park_time == maxtime)
        ans.emplace_back(pre);

    //将所有有效记录按时间排列
    sort(clean_records.begin(), clean_records.end(), cmp_by_time);

    // 执行查询任务,因为查询时间是递增的，所以每次查询从上次的时间下标之后就可以了
    int count = 0;
    int time_index = 0;
    for (int i = 0; i < k; i++) {
        int h, m, s;
        scanf("%d:%d:%d", &h, &m, &s);
        while(time_index<clean_records.size() && clean_records[time_index]->time <= h * 3600 + m * 60 + s){
            count += clean_records[time_index]->flag;
            time_index ++;
        }
        printf("%d\n", count);
    }

    // 打印所有的最大停车时间的车牌号
    for (auto &it:ans)
        cout<<it<<" ";
    printf("%02d:%02d:%02d", maxtime / 3600, (maxtime % 3600) / 60, maxtime % 60);
    return 0;
}
```

