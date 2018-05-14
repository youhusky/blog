---
title: BFS-Board类型总结
comments: true
date: 2017-09-10 15:13:49
updated: 2017-09-10 15:13:49
categories: Leetcode
tags: [BFS, DFS]
---

# Maze
这一系列题目的要求是小球滚动直到遇到障碍才停止，最后找到出口，求出valid，shorest，shortest的变种；所以用BFS可以比较简洁的解决这一系列的问题。

```python
queue = [start]

direction = [(1,0),(-1,0),(0,1),(0,-1)]

while queue:
	i, j = queue.pop(0) # python 用List模仿实现Queue
	maze[i][j] = -1 # visited
	# 终止条件
	if (i,j) == destination:
		xxx
	
	# 遍历四个方向
	for x, y in direction: # local dir
		row = i
		col = j
		
		while xxx and xxx : # condition
			row += x
			col += y
			
	if visited and not in the queue:
		queue.append()
```
<!--more-->
## I
`490. The Maze  
基本款，套用模版就好了  
O(mn),O(mn)

```python
class Solution(object):
    def hasPath(self, maze, start, destination):
        """
        :type maze: List[List[int]]
        :type start: List[int]
        :type destination: List[int]
        :rtype: bool
        """
        queue = [start]
        m = len(maze)
        n = len(maze[0])
        dir = [(1,0),(0,1),(-1,0),(0,-1)]
        while queue:
            i, j = queue.pop(0)
            # record
            maze[i][j] = -1
            if i == destination[0] and j == destination[1]:
                return True
            for x, y in dir:
                row = x + i
                col = y + j
                # move until the wall
                while 0<= row < m and 0<= col < n and maze[row][col] != 1:
                    row += x
                    col += y
                # move back a step
                row -= x
                col -= y
                if maze[row][col] == 0 and [row,col] not in queue:
                    queue.append([row, col])
        return False
        
```

## II - 求最短路径
`505. The Maze II   
可以考虑Dijkstra's algorithm，在Python使用heapq最小堆，因为每一次都要记录当前路径，所以需要记录local_count

```python
count,i,j = heapq.heappop(pq)
...

for x,y in dir:
    row = i+x
    col = j+y
    local = 1
...
heapq.heappush(pq, (count+local, row, col))
```
时间：O(mn*log(mn)) -- 用heapq方法每次heapify的时候是Log级别的复杂度
空间：O（mn）

```python
import heapq
class Solution(object):
    def shortestDistance(self, maze, start, destination):
        """
        :type maze: List[List[int]]
        :type start: List[int]
        :type destination: List[int]
        :rtype: int
        """
        pq = [(0,start[0],start[1])]
        m = len(maze)
        n = len(maze[0])
        dir = [(0,1),(1,0),(-1,0),(0,-1)]
        while pq:
            count,i,j = heapq.heappop(pq)
            if maze[i][j] == -1:
                continue # cut
            maze[i][j] = -1 # record
            if i == destination[0] and j == destination[1]:
                return count
            for x,y in dir:
                row = i+x
                col = j+y
                local = 1
                # record local variable
                while 0 <= row < m and 0 <= col < n and maze[row][col] != 1:
                    row += x
                    col += y
                    local += 1
                row -= x
                col -= y
                local -= 1
                #print pq
                heapq.heappush(pq, (count+local, row, col))
        return -1
```

## III - 遇到洞
`499. The Maze III  
同样道理，只需要添加条件在while循环里，然后在dir里面加入dir的字符，heap的时候


```python
if [row,col] == destination:
    break
    ...
heapq.heappush(pq, (count+local, move+path, row, col))
```

时间：O(mn*log(mn)) -- 用heapq方法每次heapify的时候是Log级别的复杂度
空间：O（mn）

```python
class Solution(object):
    def findShortestWay(self, maze, start, destination):
        """
        :type maze: List[List[int]]
        :type ball: List[int]
        :type hole: List[int]
        :rtype: str
        """
        pq = [(0,"", start[0],start[1])]
        m = len(maze)
        n = len(maze[0])
        dir = [(0,1,'r'),(1,0,'d'),(-1,0,'u'),(0,-1,'l')]
        while pq:
            #print pq
            count,move,i,j = heapq.heappop(pq)
            if maze[i][j] == -1:
                continue # cut
            maze[i][j] = -1 # record
            if i == destination[0] and j == destination[1]:
                return move
            for x,y,path in dir:
                row = i
                col = j
                local = 0
                # record local variable
                while 0 <= row+x < m and 0 <= col+y < n and maze[row+x][col+y] != 1 :
                    row += x
                    col += y
                    local += 1
                    if [row,col] == destination:
                        break
                heapq.heappush(pq, (count+local, move+path, row, col))
                
        return "impossible"
```

# Matrix
## 01 Matrix
`542. 01 Matrix  
Given a matrix consists of 0 and 1, find the distance of the nearest 0 for each cell.
The distance between two adjacent cells is 1
用类似的思维，在Queue中只加入0的点，然后预设其他的点到0的距离为无穷大，这样的话，遍历的时候四个方向每次加1；遇到重复的时候取最小值就好
T O(mn)
S O(mn)

```python
    def updateMatrix(self, rooms):
        """
        :type matrix: List[List[int]]
        :rtype: List[List[int]]
        """
        queue = []
        for row in range(len(rooms)):
            for col in range(len(rooms[0])):
                if rooms[row][col] == 0:
                    queue.append((row,col))
                else:
                    rooms[row][col] = float('inf')
        direction = [(1,0),(0,1),(-1,0),(0,-1)]
        while queue:
            i,j = queue.pop(0)
            for x,y in direction:
                row = i + x
                col = j + y
                if 0 <= row < len(rooms) and 0 <= col < len(rooms[0]) and rooms[row][col] > 1 + rooms[i][j]:
                    rooms[row][col] = rooms[i][j] + 1
                    queue.append((row, col))
            
        return rooms
```

# 286. Walls and Gates
几乎一摸一样的题，除了题目中的1改为inf

```python
    # init
    queue = []
    for row in range(len(rooms)):
        for col in range(len(rooms[0])):
            if rooms[row][col] == 0:
                queue.append((row,col))
    direction = [(1,0),(0,1),(-1,0),(0,-1)]
    while queue:
        i,j = queue.pop(0)
        for x,y in direction:
            row = i + x
            col = j + y
            if 0 <= row < len(rooms) and 0 <= col < len(rooms[0]) and rooms[row][col] == 2147483647:
                rooms[row][col] = rooms[i][j] + 1
                queue.append((row, col))
```