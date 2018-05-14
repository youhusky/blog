---
title: Union-Find总结
comments: true
date: 2017-09-18 14:53:14
updated: 2017-09-18 14:53:14
categories: Leetcode
tags: [Union-Find, DFS]
---
# Union Find 总结
## 介绍
### 概念
并查集是一种树型的数据结构，其保持着用于处理一些不相交集合（Disjoint Sets）的合并及查询问题。有一个联合-查找算法（union-find algorithm）定义了两个操作用于此数据结构：  
Find：确定元素属于哪一个子集。它可以被用来确定两个元素是否属于同一子集。  
Union：将两个子集合并成同一个集合。
### 适用场景

适合于判断，给出一组结点，判断他们是否联通。从判断是否为图（一个节点的两个边都会指向同一节点--构成三角形从而不再是树）到岛屿问题（如果节点不与其它节点联通，则会孤立成一个岛屿）

### 实现思路
建立n个分组，每个分组代表一堆可以互相联通的结点  
遍历每对结点，找到他们各自所属的分组A, B  
如果A != B，则将A, B分组union起来，表示A, B分组联通了  
如果A == B，则跳过  （说明A，B已经在一个组里）
<!--more-->
初始化，每一个点都设置为单独的一个组，标记为index

```python
groupTag = [i for i in range(n)]
```
Find操作是为每一个节点找到它的最远祖先，如果节点对应的数值为初始的index的话，证明其仍然为isolate，如果不是的话，存储的index为其祖先的index，这样就能find其最远祖先

```python
def find(self, e, groupTag):
    # isolate
    if groupTag[e] == e:
        return e
    # group
    else:
        return self.find(groupTag[e], groupTag)
```
Union操作,进来的这两个节点是联通的，union函数是将这两个节点合并为一个组。通过调用Find操作，对两个节点找到其祖先，然后如果祖先相同的话，证明这两个节点已经在一个组里，跳过；如果不是的话，将第二个节点的祖先设置为第一个节点。

```python
def union(self, i, j, x, y, groupTag, n):
    index1 = i*n+j
    index2 = x*n+y
    root1 = self.find(index1, groupTag)
    root2 = self.find(index2, groupTag)
    # already unioned
    if root1 == root2:
        return
    else:
        groupTag[root2] = root1
```

### 复杂度
Nearly to O（1）  
参考 [Union-Find算法 动态连通性概念介绍](https://neo1218.github.io/unionfind/)

## 题目
200	Number of Islands	34.8%	Medium	
128	Longest Consecutive Sequence	37.0%	Hard
130	Surrounded Regions	18.6%	Medium	
547	Friend Circles	49.1%	Medium	
305	Number of Islands II 	39.0%	Hard	
261	Graph Valid Tree 	37.9%	Medium	
323	Number of Connected Components in an Undirected Graph 

### 261. Graph Valid Tree
判断一张图是否是一颗树的两个关键点：  
不存在环路(对于有向图，不存在环路也就意味着不存在强连通子图)  
满足边数加一等于顶点数的规律(不考虑重边和指向自身的边)

所以可以套用最简单的Union-Find模版来解这道题，判断条件为一个顶点的两个祖先为同一个组的话，False，因为这两个祖先顶点既然在一个组里，肯定已经联成一条线，这样这三个点就会成为环

```python
T O(n) union-find nearly o(1)
S O(n)
class Solution(object):
    def validTree(self, n, edges):
        """
        :type n: int
        :type edges: List[List[int]]
        :rtype: bool
        """
        
        group = [i for i in range(n)]
        for e1, e2 in edges:
            root1 = self.find(e1, group)
            root2 = self.find(e2, group)
            if root1 == root2:
                return False
            else:
                group[root2] = root1
        return len(edges) == n - 1
    
    def find(self, e, group):
        if e == group[e]:
            return e
        else:
            return self.find(group[e], group)
        
```

### 323. Number of Connected Components in an Undirected Graph
在上一题的基础上，算出有几个unconnected components，还是利用性质 不存在环路(对于有向图，不存在环路也就意味着不存在强连通子图)  
满足边数加一等于顶点数的规律(不考虑重边和指向自身的边)，每一次成功union操作后，孤立顶点数减1

```python
T O(n) union-find nearly o(1)
S O(n)
class Solution(object):
    def countComponents(self, n, edges):
        """
        :type n: int
        :type edges: List[List[int]]
        :rtype: int
        """
        count = n
        group = [i for i in range(n)]
        for e1, e2 in edges:
            root1 = self.find(e1, group)
            root2 = self.find(e2, group)
            if root1 == root2:
                pass
            else:
                count -= 1
                group[root2] = root1
        return count
    
    def find(self, e, group):
        if e == group[e]:
            return e
        else:
            return self.find(group[e], group)
```

### 547. Friend Circles
```
Input: 
[[1,1,0],
 [1,1,1],
 [0,1,1]]
Output: 1
Explanation:The 0th and 1st students are direct friends, the 1st and 2nd students are direct friends, 
so the 0th and 2nd students are indirect friends. All of them are in the same friend circle, so return 1.
```
这道题和上一道题类似，算出孤立的朋友就好... trick的地方就是我们只用算一半的矩阵就好，因为If M[i][j] = 1, then M[j][i] = 1. 而且对角线一定为1

```python
T O(n^2) union-find nearly o(1)
S O(n)
class Solution(object):
    def findCircleNum(self, M):
        """
        :type M: List[List[int]]
        :rtype: int
        """
        if not M or not M[0]:
            return 0
        m = len(M)
        group = [i for i in range(m)]
        count = m
        # init
        
        for i in range(m):
            for j in range(i+1,m):
                if M[i][j] == 1:
                    p1 = self.find(i, group)
                    p2 = self.find(j, group)
                    if p1 != p2:
                        count -= 1
                        group[p2] = p1
        return count
                      
    def find(self, e, group):
        if e == group[e]:
            return e
        else:
            return self.find(group[e], group)      
```

### 200. Number of Islands
本质上和上面的策略相同，不过可以把二维的矩阵变为一维，联通条件为上下左右的方向需要为1

```python
class Solution(object):
    def numIslands(self, grid):
        """
        :type grid: List[List[str]]
        :rtype: int
        """
        # TIME O(MN)
        # SPACE O(MN)
    
        if not grid or not len(grid) or not grid[0]:
            return 0
        m = len(grid)
        n = len(grid[0])
        # two dimension to one
        groupTag = [0 for i in range(m*n)]

        for i in range(m):
            for j in range(n):
                if grid[i][j] == '1':
                    groupTag[i*n+j] = i*n + j
                else:
                    groupTag[i*n+j] = -1
        
        for i in range(m):
            for j in range(n):
                if grid[i][j] == '0':
                    continue
                if j+1 < n and grid[i][j+1] == '1':
                    self.union(i,j,i,j+1,groupTag,n)
                if i+1 < m and grid[i+1][j] == '1':
                    self.union(i,j,i+1,j,groupTag,n)
        
        count = 0
        for i in range(len(groupTag)):
            if groupTag[i] == i:
                count += 1
        return count
        
    def find(self, e, groupTag):
        # isolate
        if groupTag[e] == e:
            return e
        # group
        else:
            return self.find(groupTag[e], groupTag)
    
    def union(self, i, j, x, y, groupTag, n):
        index1 = i*n+j
        index2 = x*n+y
        root1 = self.find(index1, groupTag)
        root2 = self.find(index2, groupTag)
        # already unioned
        if root1 == root2:
            return
        else:
            groupTag[root2] = root1
```

### 305. Number of Islands II
在上一题的基础上，需要满足操作add，然后得出isolate的岛屿。  
这道题需要建一个Union的类，这样每次调用类的操作能更好的减少时间空间复杂度，这次因为是需要在每次Add操作（isolate岛屿数量预先 count+1）的时候算出isolate的数量，所以可以对于每个新加入的点，向四周move一步，判断是否和已知岛屿联通，从而count-1

```python
class Solution(object):
    def numIslands2(self, m, n, positions):
        ans = []
        islands = Union()
        for p in map(tuple, positions):
            islands.add(p)
            for dp in (0, 1), (0, -1), (1, 0), (-1, 0):
                q = (p[0] + dp[0], p[1] + dp[1])
                if q in islands.group:
                    islands.union(p, q)
            ans += [islands.count]
        return ans

class Union(object):
    def __init__(self):
        self.group = {}
        self.island = {}
        self.count = 0

    def add(self, p):
        self.group[p] = p
        self.island[p] = 1
        self.count += 1

    def find(self, i):
        if i == self.group[i]:
            return i
        else:
            return self.find(self.group[i])

    def union(self, p, q):
        root1, root2 = self.find(p), self.find(q)
        if root1 == root2:
            return
        if self.island[root1] > self.island[root2]:
            root1, root2 = root2, root1
        self.group[root1] = root2
        self.island[root2] += self.island[root1]
        self.count -= 1
```