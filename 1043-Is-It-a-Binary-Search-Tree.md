---
title: <Python><PAT> 1043 Is It a Binary Search Tree
date: 2019-08-26 09:23:43
tags: 
- PAT
- 题解
- BST
cover: http://pwb80dtf4.bkt.clouddn.com/PAT-cover.webp
categories: PAT
top_img: http://pwb80dtf4.bkt.clouddn.com/PAT-top.webp
description: BST的前序转后序，难度略大
---

# 题目

A Binary Search Tree (BST) is recursively defined as a binary tree which has the following properties:

- The left subtree of a node contains only nodes with keys less than the node's key.
- The right subtree of a node contains only nodes with keys greater than or equal to the node's key.
- Both the left and right subtrees must also be binary search trees.

If we swap the left and right subtrees of every node, then the resulting tree is called the **Mirror Image** of a BST.

Now given a sequence of integer keys, you are supposed to tell if it is the preorder traversal sequence of a BST or the mirror image of a BST.

### Input Specification:

Each input file contains one test case. For each case, the first line contains a positive integer $N( \leq 1000)$. Then *N* integer keys are given in the next line. All the numbers in a line are separated by a space.

### Output Specification:

For each test case, first print in a line `YES` if the sequence is the preorder traversal sequence of a BST or the mirror image of a BST, or `NO` if not. Then if the answer is `YES`, print in the next line the postorder traversal sequence of that tree. All the numbers in a line must be separated by a space, and there must be no extra space at the end of the line.

### Sample Input 1:

```in
7
8 6 5 7 10 8 11
```

### Sample Output 1:

```out
YES
5 7 6 8 11 10 8
```

### Sample Input 2:

```in
7
8 10 11 8 6 7 5
```

### Sample Output 2:

```out
YES
11 8 10 7 5 6 8
```

### Sample Input 3:

```in
7
8 6 8 5 10 9 11
```

### Sample Output 3:

```out
NO
```

# 题解

## 思路

+ 题目意思是判断一个树是不是BST或者大小反转的BST。题目给出前序遍历，让你输出后序遍历

+ 如果树只有一项，那直接输出是BST

+ 如果大于一项，可以通过比较前两项来判断是BST还是反转的BST

+ 注意，这道题[柳神](https://www.liuchuo.net/archives/2153)的方法其实是有问题的，因为测试数据不够完善导致了能AC。因为，测试数据对于BST来说，与根相等的正好在左子树，而对于翻转的BST来说，与根相等的正好在右子树。

+ 理论上，不管是BST还是反转BST，判断[柳神](https://www.liuchuo.net/archives/2153)方法里i和j的标准的等号位置应该不变，变的是大于号和小于号。这样分开写就可以解决上面的问题，实际上就是钻了测试数据的漏洞。如果你尝试一棵与根相等的在右子树的BST或者一棵与根相等的在左子树的反转BST，就会发现柳神的解答是错误的。

+ 对于反转的BST，我们可以给每个数取负号，这样就变成了不反转的BST，可以运用BST的比较规则

+ 我们先考虑如何把前序转为后序。假定给定的序列是合法BST

+ 前序是根左右，后序是左右根。

+ 所以根一定在前序序列首，然后是左子树，然后是右子树。

+ 我们先遍历序列左子树，再遍历右子树，再把根添加到答案里，这样可以递归来写，即

+ ```python
  def getpost(root,tail):
  	getpost(left_root,left_tail)	# 先遍历左子树
  	getpost(right_root,right_tail)	# 再遍历右子树
  	post.append(pre[root])			# 添加自己
  # 调用
  getpost(0,num - 1)
  ```

+ 逻辑上，就可以把pre前序转换成post后序列表。当然要处理一下边界条件，就是当`root > tail` 的时候，直接`return`。

+ 左子树的根就是原root + 1，右子树的尾就是原来的尾，那么如何找到左子树的末尾和右子树的开始呢？

+ 根据BST的性质，左子树都是小于等于根部的，我们要找到root后第一个比root大的数，它就是右子树的根。相应的，它前面一位就是左子树的末尾。

+ 不难写出这样的代码。基本框架就搭好了，剩下的就是处理细节。即考虑到题目的特殊要求。

+ ```python
  def getpost(root, tail):
      if root > tail:
          return
      i = root	# i 找到左子树的末尾
      while i < tail and pre[i + 1] < pre[root]:
          i += 1
      getpost(root + 1, i)
      getpost(i + 1, tail)
      post.append(pre[root])
  ```

+ 要注意，题目里允许了等于根部的节点存在。

+ 如何处理等于根部的情况？

+ 如果找到了一个等于根部的值，那么它一定不能作为右子树的根部，不然右子树的根和原根相等了。所以遇到和根相等的，一定算在左子树里。

+ 所以很容易想到把while里的`<`改成`<=`。

+ 但实际上光这样是不对的

+ 因为我们找到了等于根部的值，就把它当作左子树的末尾。而下一个数，必须比根大，否则树就是假BST。

+ 比如说，根为8,遇到了8，应该把8设为左子树的末尾。

+ 而如果下一个数是5,那么5<8,不能作右子树的开头，应该报错。

+ 可是如果用`<=`的话，它还会略过5再寻找下面一个比8大的作为右子树的根。

+ 所以我们应该先找到大于等于根部的数。等于根部,把它作为左子树的末尾，大于根部,把它作为右子树的开头。

+ 同时，如果是等于的话，要再多层判断。如果这个数等于且下一个数比根部小，就报错。

+ 报错的方式是直接return，这样post就会少一项，就和pre的长度不一样。

+ 输出的时候就可以根据两个序列的长度来判断是不是`YES`

+ 所以代码可以这样写

+ ```python
  def getpost(root, tail):
      if root > tail:
          return
      i = root	# i 找到左子树的末尾
      while i < tail and pre[i + 1] < pre[root]:
          i += 1
      if i < tail and pre[i + 1] == pre[root]:
          i += 1
          if i < tail and pre[i + 1] < pre[root]:
              return
      getpost(root + 1, i)
      getpost(i + 1, tail)
      post.append(pre[root])
  ```

+ 接下来考虑BST的要求。

+ BST要求左子树都小于根部，右子树都大于根部。

+ 我们在寻找左子树的末尾的时候已经保证了左子树都小于根部

+ 怎么保证右子树都大于根部呢？

+ 我们在对一个根开始遍历的时候，先递归进入左子树，再递归进入右子树。

+ 再进入右子树的时候，要携带一下现在这个根的值。

+ 假如现在的根是A，左边是A-，右边是A+

+ 对A+进入函数的时候，要判断A+里面的左侧是不是都大于A。（右边都甚至大于A+，所以不用判断了）

+ 在进入A-的时候，不用判断，那么传递函数的时候就设现在的根为一个极小值-99999就行了。

+ 总的来说，递归函数的代码就这样。

+ ```python
  def getpost(root, tail, last_root):
      if root > tail:
          return
      i = root    # i 找到左子树的末尾
      while i < tail and pre[i + 1] < pre[root]:
          if pre[i + 1] < last_root:	# 如果在右子树，里面的值比原来的根小，就报错
              return
          i += 1
      if i < tail and pre[i + 1] == pre[root]:
          i += 1
          if i < tail and pre[i + 1] < pre[root]: # 如果右子树开头比根小，就报错
              return
      getpost(root + 1, i, -99999)
      getpost(i + 1, tail, pre[root])
      post.append(pre[root])
  ```

+ 递归函数就写好了，那么剩下的就迎刃而解了。

## 数据结构

+ pre 是原来的先序序列
+ post 是后序序列
+ 递归里i 找到左子树的末尾的下标

## 算法

+ 都写在思路里了，应该说是足够翔实了。

## 代码

+ 因为原数据格式有问题，所以Python3有一个点会非零返回。C++没问题
### Python3 

```python
num_nodes = int(input())
pre = list(map(int, input().split()))
if num_nodes == 1:
    print("YES")
    print(pre[0])
else:
    if pre[1] > pre[0]:
        pre = list(map(lambda x: -x, pre))
    post = []

    def getpost(root, tail, last_root):
        if root > tail:
            return
        i = root
        while i < tail and pre[i + 1] < pre[root]:
            if pre[i + 1] < last_root:
                return
            i += 1
        if i < tail and pre[i + 1] == pre[root]:
            i += 1
            if i < tail and pre[i + 1] < pre[root]:
                return
        getpost(root + 1, i, -99999)
        getpost(i + 1, tail, pre[root])
        post.append(pre[root])


    getpost(0, num_nodes - 1, -99999)
    if len(post) == num_nodes:
        print("YES")
        print(" ".join(list(map(lambda x: str(abs(x)), post))))
    else:
        print("NO")

```

### C++

```c++
#include <iostream>
#include <vector>

using namespace std;

int getpost(int root, int tail, int last_root, vector<int> &pre, vector<int> &post) {
    if (root > tail)
        return 0;
    int i = root;
    while (i < tail && pre[i + 1] < pre[root]) {
        if (pre[i + 1] < last_root)
            return 0;
        i += 1;
    }
    if (i < tail and pre[i + 1] == pre[root])
        i += 1;
    if (i < tail and pre[i + 1] < pre[root])
        return 0;
    getpost(root + 1, i, -99999, pre, post);
    getpost(i + 1, tail, pre[root], pre, post);
    post.emplace_back(pre[root]);
}

int main() {
    int num_nodes;
    cin >> num_nodes;
    vector<int> pre(num_nodes, num_nodes);
    vector<int> post;
    for (int i = 0; i < num_nodes; i++)
        cin >> pre[i];
    if (num_nodes == 1) {
        printf("YES\n");
        printf("%d\n", pre[0]);
        return 0;
    }
    if (pre[1] > pre[0]) {
        for (int i = 0; i < num_nodes; i++)
            pre[i] = -pre[i];
    }
    getpost(0, num_nodes - 1, -99999,pre,post);
    if (post.size() == num_nodes) {
        printf("YES\n");
        printf("%d", abs(post[0]));
        for (int i = 1; i < num_nodes; i++)
            printf(" %d", abs(post[i]));
    } else {
        printf("NO\n");
    }
}
```

