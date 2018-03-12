---
title: DFS-BFS 总结
comments: true
date: 2018-03-10 14:49:35
updated: 2018-03-11 14:49:35
categories: Leetcode
tags: [DFS, BFS]
---
# DFS 
## Matrix
### 介绍
对于不是Tree下面的DFS题来说，一直是我的一个弱点（不知道为什么），所以现在特意开贴来总结常见的题型

大致的模版就是明确dfs函数中不合法的状态要直接return和继续dfs的情况；同时也要做好visited的标记，从而避免无限循环的错误

```python
def dfs(i,j,matrix):
	if i < 0 or j < 0 or i >= len(matrix) or j >= len(matrix) or matrix[i][j] == X:
		return
		
	# set visited
	matrix[i][j] = X
	dfs(i+1, j, matrix)
	dfs(i-1, j, matrix)
	...
	
```
<!--more-->
### 733. Flood Fill
最简单的DFS版本

```python
class Solution(object):
    def floodFill(self, image, sr, sc, newColor):
        """
        :type image: List[List[int]]
        :type sr: int
        :type sc: int
        :type newColor: int
        :rtype: List[List[int]]
        """
        m = len(image)
        n = len(image[0])
        color = image[sr][sc]
        if color != newColor:
            self.dfs(sr, sc,image,color,newColor)
        return image
    
    def dfs(self, i, j,image,color,newColor):
        if i < 0 or j < 0 or i >= len(image) or j >= len(image[0]) or image[i][j] != color:
            return
        image[i][j] = newColor
        self.dfs(i+1, j, image, color, newColor)
        self.dfs(i-1, j, image, color, newColor)
        self.dfs(i, j+1, image, color, newColor)
        self.dfs(i, j-1, image, color, newColor)
```

### 130. Surrounded Regions
如果1的周围有边界的话，设置为M作为标准，之后再改回来。之后的same as Island

```python
class Solution(object):
    def solve(self, board):
        """
        :type board: List[List[str]]
        :rtype: void Do not return anything, modify board in-place instead.
        """
        if not board or not board[0]:
            return
        m = len(board)
        n = len(board[0])
        for i in range(m):
            for j in range(n):
                # border case
                if (i == 0 or j == 0 or i == m-1 or j == n -1) and board[i][j] == 'O':
                    board[i][j] == 'M'
                    self.dfs(i,j,board)
        
        for i in range(m):
            for j in range(n):
                if board[i][j] == 'O':
                    board[i][j] = 'X'
                elif board[i][j] == 'M':
                    board[i][j] = 'O'
    
    def dfs(self, r, c, board):
        if r < 0 or c < 0 or r >= len(board) or c >= len(board[0]) or board[r][c] == 'X' or board[r][c] == 'M':
            return
        board[r][c] = 'M'
        self.dfs(r+1, c, board)
        self.dfs(r-1, c, board)
        self.dfs(r, c+1, board)
        self.dfs(r, c-1, board)
```

### 200. Number of Islands
如果岛屿周围为1，置为0

```python
class Solution(object):
    def numIslands(self, grid):
        """
        :type grid: List[List[str]]
        :rtype: int
        """
        if not grid or not grid[0]:
            return 0
        m = len(grid)
        n = len(grid[0])
        count = 0
        for i in range(m):
            for j in range(n):
                if grid[i][j] == '1':
                    count += 1
                    self.dfs(i,j,grid)
        return count
    
    def dfs(self, r, c, grid):
        if r < 0 or c < 0 or r >= len(grid) or c >= len(grid[0]) or grid[r][c] == '0':
            return
        grid[r][c] = '0'
        
        self.dfs(r+1,c,  grid)
        self.dfs(r-1, c, grid)
        self.dfs(r, c-1, grid)
        self.dfs(r, c+1, grid)
```
### 542	01 Matrix
Same as Wall and Gate

```python
class Solution(object):
    def updateMatrix(self, matrix):
        """
        :type matrix: List[List[int]]
        :rtype: List[List[int]]
        """
        if not matrix or not matrix[0]:
            return []
        m = len(matrix)
        n = len(matrix[0])
        for i in range(m):
            for j in range(n):
                if matrix[i][j] == 1:
                    matrix[i][j] = float('inf')
        for i in range(m):
            for j in range(n):
                if matrix[i][j] == 0:
                    self.dfs(i,j,matrix,0)
        return matrix
    
    def dfs(self, i, j, matrix, d):
        if i < 0 or j < 0 or i >= len(matrix) or j >= len(matrix[0]) or matrix[i][j] < d:
            return
        matrix[i][j] = d
        self.dfs(i+1, j, matrix, d+1)
        self.dfs(i-1, j, matrix, d+1)
        self.dfs(i, j-1, matrix, d+1)
        self.dfs(i, j+1, matrix, d+1)
```
### 286	Walls and Gates
找出0的位置，然后-1为墙

```python
class Solution(object):
    def wallsAndGates(self, rooms):
        """
        :type rooms: List[List[int]]
        :rtype: void Do not return anything, modify rooms in-place instead.
        """
        if not rooms or not rooms[0]:
            return
        m = len(rooms)
        n = len(rooms[0])
        for i in range(m):
            for j in range(n):
                # start
                if rooms[i][j] == 0:
                    self.dfs(i, j, rooms, 0)
        
    def dfs(self, r, c, rooms, d):
        if r < 0 or c < 0 or r >= len(rooms) or c >= len(rooms[0]) or rooms[r][c] < d:
            return
        # mark as 0 and then other path + 1
        rooms[r][c] = d
        self.dfs(r+1, c, rooms, d+1)
        self.dfs(r-1, c, rooms, d+1)
        self.dfs(r, c+1, rooms, d+1)
        self.dfs(r, c-1, rooms, d+1)
```
## Matrix 2
有时候在Matrix的时候需要判断每一次DFS的情况，这时候也可以通过比较每一次更新的值来抉择
### 329. Longest Increasing Path in a Matrix
每一次找到递增的时候，继续DFS，然后用cache来记录每一个（i，j）最大可能的递增长度

```python
class Solution(object):
    def longestIncreasingPath(self, matrix):
        """
        :type matrix: List[List[int]]
        :rtype: int
        """
        if not matrix or not matrix[0]:
            return 0
        
        m = len(matrix)
        n = len(matrix[0])
        
        cache = [[0 for _ in range(n)] for _ in range(m)]
        
        ans = 0
        for i in range(m):
            for j in range(n):
                ans = max(ans, self.dfs(i,j,cache, matrix))
        return ans
    
    def dfs(self, i, j, cache, matrix):
        # find cache
        if cache[i][j] != 0:
            return cache[i][j]
        direction = [(0,1),(0,-1),(1,0),(-1,0)]
        for dire in direction:
            x, y = i + dire[0], j + dire[1]
            if x < 0 or y < 0 or x >= len(matrix) or y >= len(matrix[0]) or matrix[i][j] >= matrix[x][y]:
                continue
            cache[i][j] = max(cache[i][j], self.dfs(x,y,cache, matrix))
        # self (i,j) + 1
        cache[i][j] += 1
        return cache[i][j]
        
```
### 695. Max Area of Island
另外一种思路

```python
class Solution:
    def maxAreaOfIsland(self, grid):
        """
        :type grid: List[List[int]]
        :rtype: int
        """
        seen = set()  # function scope var 
        res = 0
        for row in range(len(grid)):
            for col in range(len(grid[0])):
                   if grid[row][col] == 1:
                    	res = max(res,self.dfs(row, col, grid,seen))        return res

    def dfs(self, row, col,grid, seen):
            """if a point is valid, it should meet all requirements using "and" """
            if 0 <= row < len(grid) and 0 <= col < len(grid[0]) and (grid[row][col] == 1) and (row, col) not in seen:
                seen.add((row, col))             # list this point to seen set so we won't count it again
                return (self.dfs(row - 1, col,grid,seen) + self.dfs(row + 1, col,grid,seen)
                        + self.dfs(row, col - 1,grid,seen) + self.dfs(row, col + 1,grid, seen)
                        + 1)                     # add 1 to the area and DFS(4-Conn Neighbors)

            else:
                return 0                         # not valid point return 0

```
        
### 417. Pacific Atlantic Water Flow
这道题要从两方面来判断，太平洋和大西洋

```python
class Solution(object):
    def pacificAtlantic(self, matrix):
        """
        :type matrix: List[List[int]]
        :rtype: List[List[int]]
        """
        # O(MN) 
        # need to write more
        if not matrix or not matrix[0]:
            return []
        
        m = len(matrix)
        n = len(matrix[0])
        
        p_visited = [[False for _ in range(n)] for _ in range(m)]
        a_visited = [[False for _ in range(n)] for _ in range(m)]
        
        res = []
        for i in range(m):
            # left and right
            
            self.dfs(i, 0, p_visited, m, n, matrix)
            self.dfs(i, n-1, a_visited, m, n, matrix)
            
        for j in range(n):
            # up and down
            self.dfs(0, j, p_visited, m, n, matrix)
            self.dfs(m-1, j, a_visited, m, n, matrix)
        #print p_visited, a_visited
        for i in range(m):
            for j in range(n):
                if p_visited[i][j] and a_visited[i][j]:
                    res.append([i,j])
        return res
    
    def dfs(self, i, j, visited, m, n, matrix):
        visited[i][j] = True
        direction = [(0,1),(0,-1),(1,0),(-1,0)]
        for dire in direction:
            x, y = i + dire[0], j + dire[1]
            if x < 0 or y < 0 or x >= m or y >= n or visited[x][y] or matrix[i][j] > matrix[x][y]:
                continue
            self.dfs(x, y, visited, m, n, matrix)
```

# BFS
## 普通的
参见之前写过的BFS-border总结  
[BFS-Maze总结](http://joshuablog.herokuapp.com/2017/09/10/BFS-Maze%E7%B1%BB%E5%9E%8B%E6%80%BB%E7%BB%93/)
### 490
### 499
### 505
### 542
### 286

## 狄杰斯特拉算法
求有缘路径的最短距离  
算法导论的经典例子
### 743. Network Delay Time
使用heap操作，每次添加最短的路径cost

```python
import heapq
class Solution(object):
    def networkDelayTime(self, times, N, K):
        """
        :type times: List[List[int]]
        :type N: int
        :type K: int
        :rtype: int
        """
        # O(ElgV) E edges, V vertices
        # Dijkstra's
        pq = [[0,K]]
        graph = collections.defaultdict(list)
        for u,v,w in times:
            graph[u].append([v,w])
            
        dic = dict()  
        # len(pq) ~ V
        while pq:
            distance, node = heapq.heappop(pq)
            
            #visited
            if node in dic:
                continue
            dic[node] = distance
            # O(E) ~ edges
            for desination, path in graph[node]:
                if desination not in dic:
                    heapq.heappush(pq,(distance + path, desination))
        return max(dic.values()) if len(dic) == N else -1
```
### 787. Cheapest Flights Within K Stops
几乎和上题一样

```python
class Solution(object):
    def findCheapestPrice(self, n, flights, src, dst, k):
        """
        :type n: int
        :type flights: List[List[int]]
        :type src: int
        :type dst: int
        :type K: int
        :rtype: int
        """
        # O(ElgV) E flight ways ,V cities
        f = collections.defaultdict(dict)
        for a, b, p in flights:
            f[a][b] = p
        
        heap = [(0, src, k + 1)]
        while heap:
            p, i, k = heapq.heappop(heap)
            if i == dst:
                return p
            if k > 0:
                for j in f[i]:
                    heapq.heappush(heap, (p + f[i][j], j, k - 1))
        return -1
```