---
title: <Python><PAT> 1026 Table Tennis
date: 2019-08-23 17:32:09
tags: 
- PAT
- 题解
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: 模拟排队，有VIP的情况
---

A table tennis club has N tables available to the public. The tables are numbered from 1 to N. For any pair of players, if there are some tables open when they arrive, they will be assigned to the available table with the smallest number. If all the tables are occupied, they will have to wait in a queue. It is assumed that every pair of players can play for at most 2 hours.

Your job is to count for everyone in queue their waiting time, and for each table the number of players it has served for the day.

One thing that makes this procedure a bit complicated is that the club reserves some tables for their VIP members. When a VIP table is open, the first VIP pair in the queue will have the priviledge to take it. However, if there is no VIP in the queue, the next pair of players can take it. On the other hand, if when it is the turn of a VIP pair, yet no VIP table is available, they can be assigned as any ordinary players.

### Input Specification:

Each input file contains one test case. For each case, the first line contains an integer `N` (≤10000) - the total number of pairs of players. Then `N` lines follow, each contains 2 times and a VIP tag: `HH:MM:SS` - the arriving time, `P` - the playing time in minutes of a pair of players, and `tag` - which is 1 if they hold a VIP card, or 0 if not. It is guaranteed that the arriving time is between 08:00:00 and 21:00:00 while the club is open. It is assumed that no two customers arrives at the same time. Following the players' info, there are 2 positive integers: `K` (≤100) - the number of tables, and `M` (< K) - the number of VIP tables. The last line contains `M` table numbers.

### Output Specification:

For each test case, first print the arriving time, serving time and the waiting time for each pair of players in the format shown by the sample. Then print in a line the number of players served by each table. Notice that the output must be listed in chronological order of the serving time. The waiting time must be rounded up to an integer minute(s). If one cannot get a table before the closing time, their information must NOT be printed.

### Sample Input:

```in
9
20:52:00 10 0
08:00:00 20 0
08:02:00 30 0
20:51:00 10 0
08:10:00 5 0
08:12:00 10 1
20:50:00 10 0
08:01:30 15 1
20:53:00 10 1
3 1
2
```

### Sample Output:

```out
08:00:00 08:00:00 0
08:01:30 08:01:30 0
08:02:00 08:02:00 0
08:12:00 08:16:30 5
08:10:00 08:20:00 10
20:50:00 20:50:00 0
20:51:00 20:51:00 0
20:52:00 20:52:00 0
3 3 2
```

# 题解

## 思路

+ 核心是，要记录每个桌子将要空闲的时间点
+ 所有时间用秒的形式表示
+ 按照人遍历
+ 计算出vip桌子中和普通桌子中最近的空闲时间点
+ 判断有没有VIP可以插队
+ 分配进桌子，更新人的被服务时间和桌子的空闲时间点

## 数据结构

+ normal 指各个普通桌的下一次空闲时间，注意下标并不是题目的桌子编号。
+ vip 指各个VIP桌子的下一次空闲时间，注意下标并不是题目的桌子编号。
+ vip_table_ids 将vip的下标映射到真正的桌子编号 - 1
+ normal_table_ids 将vip的下标映射到真正的桌子编号 - 1
+ served存放各个桌子服务的总人数（下标为真正的桌子编号 - 1）
+ tables 包含normal和vip，这是为了写函数的方便。
+ tables_ids 包含normal_table_ids和vip_table_ids，这是为了写函数的方便。
+         fast_normal 是离空闲时间最近的普通桌
+ fast_normal_id 是离空闲时间最近的普通桌的下标（在normal中的）
+          fast_vip 是离空闲时间最近的VIP桌
+           fast_vip_id 是离空闲时间最近的VIP桌的下标（在normal中的）
+ players 列表存放所有的玩家，一个玩家由一个五元素的列表组成，分别是
  +           到来时间
  +           需要服务时间
  +           是否是VIP
  +           有没有被遍历过
  +           开始被服务的时间

## 算法

+ 读入数据与数据结构初始化，注意服务时间大于2小时的按2小时算
+ 所有玩家按照来的时间排序
+ 遍历所有的未被服务的顾客
+ 找到最近的普通桌和VIP桌
+ 如果顾客是VIP
  + 找VIP桌和普通桌哪一个更快，相同时间里选择VIP桌
+ 如果顾客是普通顾客
  + 先找下一个VIP到来的时间
  + 根据桌子空闲的时间和VIP来的时间判断有没有VIP在等着
  + 如果有VIP等着并且恰好下一个空桌是VIP
  + 那么先分配VIP
  + 再分配普通顾客
+ 注意，VIP在面临两种桌时间相等的时候会选择VIP桌，而普通顾客会选择下标更小的。
+ 排序并输出
+ 其中，分配的过程包括
  + 标记这个顾客已经处理过
  + 更新这个顾客的被服务时间
  + 更新这张桌子的下一次空闲时间
  + 如果被服务时间在营业时间内，增加相应桌子服务顾客数量

## 代码

这道题使用Python会有一个点超时，因此还有了一个C++的版本。C++的是参考[柳神](https://www.liuchuo.net/archives/2955)的代码。原理其实差不多。

### Python

```python
# 把人分配进桌子的函数
def allocate(cus_index, serve_time, vip_table, table_index):
    global players, tables, tables_ids, served
    players[cus_index][4] = max(players[cus_index][0], serve_time)
    tables[vip_table][table_index] = max(players[cus_index][0], serve_time) + players[cus_index][1]
    players[cus_index][3] = True
    if players[cus_index][4] < 21 * 3600:
        served[tables_ids[vip_table][table_index]] += 1

# 数据结构初始化与信息录入
num_player = int(input())
players = []
for i in range(num_player):
    arrive_time, play_time, vip = input().split()
    arrive_time = int(arrive_time[0:2]) * 3600 + int(arrive_time[3:5]) * 60 + int(arrive_time[6:])
    players.append([arrive_time, min(120 * 60, int(play_time) * 60), int(vip), False, 0])

players.sort(key=lambda x: x[0])
table_num, vip_table_num = list(map(int, input().split()))
vip_table_ids = list(map(lambda x:int(x) - 1, input().split()))
normal_table_ids = [i for i in range(table_num) if i not in vip_table_ids]
normal = [8 * 3600 for _ in range(table_num - vip_table_num)]
vip = [8 * 3600 for _ in range(vip_table_num)]
served = [0 for _ in range(table_num)]
tables = [normal, vip]
tables_ids = [normal_table_ids, vip_table_ids]

# 遍历每一个还未被服务的顾客
cus_index = 0
while cus_index < num_player:
    if not players[cus_index][3]:
        # 找到普通桌和特殊桌的最快腾出时间
        fast_normal = min(normal)
        fast_normal_id = normal.index(fast_normal)
        fast_vip = min(vip)
        fast_vip_id = vip.index(fast_vip)
        # 如果是普通人的话
        if players[cus_index][2] == 0:
            # 找到下一个VIP到来的时间
            next_vip_arrive = 21 * 3600
            for i in range(cus_index, num_player):
                if players[i][2] == 1 and not players[i][3]:
                    next_vip_arrive = players[i][0]
                    next_vip_id = i
                    break
            # 如果VIP桌更快腾出来而恰好有VIP在排队中，那就得先安排VIP的
            if fast_normal >= fast_vip >= next_vip_arrive:
                allocate(next_vip_id, fast_vip, 1, fast_vip_id)
                fast_vip = min(vip)
                fast_vip_id = vip.index(fast_vip)
            # 再安排普通人
            if fast_vip < fast_normal or (fast_vip == fast_normal and vip_table_ids[fast_vip_id] < normal_table_ids[fast_normal_id]):
                allocate(cus_index, fast_vip, 1, fast_vip_id)
            elif fast_vip > fast_normal or (fast_vip == fast_normal and vip_table_ids[fast_vip_id] > normal_table_ids[fast_normal_id]):
                allocate(cus_index, fast_normal, 0, fast_normal_id)
        else:  # 对于VIP
            if fast_vip <= fast_normal:
                allocate(cus_index, fast_vip, 1, fast_vip_id)
            else:
                allocate(cus_index, fast_normal, 0, fast_normal_id)
    cus_index += 1

# 排序并输出
players.sort(key=lambda x: x[4])
for i in players:
    if i[4] < 21 * 3600:
        print("%02d:%02d:%02d %02d:%02d:%02d %d" % (
            i[0] // 3600, i[0] % 3600 // 60, i[0] % 60, i[4] // 3600, i[4] % 3600 // 60, i[4] % 60,
            (i[4] - i[0] + 30) / 60))

print(" ".join(list(map(str,served))))
```

### C++

```c++
#include <iostream>
#include <vector>
#include <algorithm>
#include <cmath>
using namespace std;
struct person {
    int arrive, start, time;
    bool vip;
}tempperson;
struct tablenode {
    int end = 8 * 3600, num;
    bool vip;
};
bool cmp1(person a, person b) {
    return a.arrive < b.arrive;
}
bool cmp2(person a, person b) {
    return a.start < b.start;
}
vector<person> player;
vector<tablenode> table;
void alloctable(int personid, int tableid) {
    if(player[personid].arrive <= table[tableid].end)
        player[personid].start = table[tableid].end;
    else
        player[personid].start = player[personid].arrive;
    table[tableid].end = player[personid].start + player[personid].time;
    table[tableid].num++;
}
int findnextvip(int vipid) {
    vipid++;
    while(vipid < player.size() && !player[vipid].vip) vipid++;
    return vipid;
}
int main() {
    int n, k, m, viptable;
    scanf("%d", &n);
    for(int i = 0; i < n; i++) {
        int h, m, s, temptime, flag;
        scanf("%d:%d:%d %d %d", &h, &m, &s, &temptime, &flag);
        tempperson.arrive = h * 3600 + m * 60 + s;
        tempperson.start = 21 * 3600;
        if(tempperson.arrive >= 21 * 3600) continue;
        tempperson.time = temptime <= 120 ? temptime * 60 : 7200;
        tempperson.vip = flag == 1;
        player.push_back(tempperson);
    }
    scanf("%d%d", &k, &m);
    table.resize(k + 1);
    for(int i = 0; i < m; i++) {
        scanf("%d", &viptable);
        table[viptable].vip = true;
    }
    sort(player.begin(), player.end(), cmp1);
    int i = 0, vipid = -1;
    vipid = findnextvip(vipid);
    while(i < player.size()) {
        int index = -1, minendtime = 999999999;
        for(int j = 1; j <= k; j++) {
            if(table[j].end < minendtime) {
                minendtime = table[j].end;
                index = j;
            }
        }
        if(table[index].end >= 21 * 3600) break;
        if(player[i].vip && i < vipid) {
            i++;
            continue;
        }
        if(table[index].vip) {
            if(player[i].vip) {
                alloctable(i, index);
                if(vipid == i) vipid = findnextvip(vipid);
                i++;
            } else {
                if(vipid < player.size() && player[vipid].arrive <= table[index].end) {
                    alloctable(vipid, index);
                    vipid = findnextvip(vipid);
                } else {
                    alloctable(i, index);
                    i++;
                }
            }
        } else {
            if(!player[i].vip) {
                alloctable(i, index);
                i++;
            } else {
                int vipindex = -1, minvipendtime = 999999999;
                for(int j = 1; j <= k; j++) {
                    if(table[j].vip && table[j].end < minvipendtime) {
                        minvipendtime = table[j].end;
                        vipindex = j;
                    }
                }
                if(vipindex != -1 && player[i].arrive >= table[vipindex].end) {
                    alloctable(i, vipindex);
                    if(vipid == i) vipid = findnextvip(vipid);
                    i++;
                } else {
                    alloctable(i, index);
                    if(vipid == i) vipid = findnextvip(vipid);
                    i++;
                }
            }
        }
    }
    sort(player.begin(), player.end(), cmp2);
    for(i = 0; i < player.size() && player[i].start < 21 * 3600; i++) {
        printf("%02d:%02d:%02d ", player[i].arrive / 3600, player[i].arrive % 3600 / 60, player[i].arrive % 60);
        printf("%02d:%02d:%02d ", player[i].start / 3600, player[i].start % 3600 / 60, player[i].start % 60);
        printf("%.0f\n", round((player[i].start - player[i].arrive) / 60.0));
    }
    for(int i = 1; i <= k; i++) {
        if(i != 1) printf(" ");
        printf("%d", table[i].num);
    }
    return 0;
}
```





