---
title: TopLogicalSort 总结
comments: true
date: 2018-03-11 13:39:51
updated: 2018-03-20 13:39:51
categories: Leetcode
tags: TopLogicalSort
---
# 拓扑排序
拓扑排序适合于求解相关联的依赖状态，比如0依赖于1，2; 2 依赖于3
这样就类似于BFS的模版，用一个queue来维持；然后把各个链接用图的形式连接起来，从入度为0的点开始，遍历每一个字节点也就是每一个出度；同时对应的入度-1， 如果对应的节点入度为0，证明该节点的依赖关系已经被计算过，从而加入Queue进行下一步操作
## 题目
模版

```python
outdegree = [[] for _ in range(numCourses)]
indegree = [0 for _ in range(numCourses)]

queue = []
# find zero indegree

for i in indegree:
	if not i:
	queue.append(i) 
	
count = 0
while queue:
	node = queue.pop(0)
	count += 1
	for succ in outdegree[course]:
		indegree[succ] -= 1
		# no indegree
		if indegree[succ] == 0:
			queue.append(succ)
```
<!--more-->
### 207. Course Schedule

```python
class Solution(object):
    def canFinish(self, numCourses, prerequisites):
        """
        :type numCourses: int
        :type prerequisites: List[List[int]]
        :rtype: bool
        """
        outdegree = [[] for _ in range(numCourses)]
        indegree = [0] * numCourses
        
        for succ, pre in prerequisites:
            outdegree[pre].append(succ)
            indegree[succ] += 1
           
        queue = []
        # find start from all course - indegree == 0
        for i in range(numCourses):
            if indegree[i] == 0:
                queue.append(i)
                
        count = 0
        
        while queue:
            course = queue.pop(0)
            count += 1
            for succ in outdegree[course]:
                indegree[succ] -= 1
                if indegree[succ] == 0:
                    queue.append(succ)
                    
        # if we find all course that are equal to the given course
        return count == numCourses
        
```
### 210. Course Schedule II
区别就是输出list

```python
class Solution(object):
    def findOrder(self, numCourses, prerequisites):
        """
        :type numCourses: int
        :type prerequisites: List[List[int]]
        :rtype: List[int]
        """
        outdegree = [[] for _ in range(numCourses)]
        indegree = [0] * numCourses
        
        for succ, pre in prerequisites:
            outdegree[pre].append(succ)
            indegree[succ] += 1
        
        queue = []
        for i in range(numCourses):
            if indegree[i] == 0:
                queue.append(i)
                
        res = []
        
        while queue:
            pre = queue.pop(0)
            res.append(pre)
            for succ in outdegree[pre]:
                indegree[succ] -= 1
                if indegree[succ] == 0:
                    queue.append(succ)
        return res if len(res) == numCourses else []
```
### 802. Find Eventual Safe States
也是一道可以用这种方法做的题，就是经过拓扑排序后出度为0的点输出出来就好。

```python
class Solution(object):
    def eventualSafeNodes(self, graph):
        """
        :type graph: List[List[int]]
        :rtype: List[int]
        """
        outdegree = [0] * len(graph)
        indegree = [[] for _ in range(len(graph))]

        for i in range(len(graph)):
            outdegree[i] = len(graph[i])
            for j in range(len(graph[i])):
                indegree[graph[i][j]].append(i)
                
        queue = []
        for i in range(len(outdegree)):
            if outdegree[i] == 0:
                queue.append(i)
        res = []   
        while queue:
            node = queue.pop(0)
            res.append(node)
            if indegree[node]:
                for rest in indegree[node]:
                    outdegree[rest] -= 1
                    if outdegree[rest] == 0:
                        queue.append(rest)
                    
        return sorted(res)
```
### 444. Sequence Reconstruction
这道题有两个点，一个是入度为0的只能有一个；二是如何控制只有一个数字的list-虽说对结果没啥影响，不过还要处理这么一个case[1],[[1],[1],[1]] 挺无聊的

```python
class Solution(object):
    def sequenceReconstruction(self, org, seqs):
        """
        :type org: List[int]
        :type seqs: List[List[int]]
        :rtype: bool
        """
        indegree = collections.defaultdict(int)
        outdegree = collections.defaultdict(list)
        
        st = set()
        
        for seq in seqs:
            # union set
            st |= set(seq)
            if len(seq) == 1:
                if seq[0] not in indegree:
                    indegree[seq[0]] = 0
                continue
            for i in range(len(seq)-1):
                if seq[i] not in indegree:
                    indegree[seq[i]] = 0
                if seq[i+1] not in outdegree[seq[i]]:
                    outdegree[seq[i]].append(seq[i+1])
                    indegree[seq[i+1]] += 1
        
        zero_degree = 0
        queue = []
        
        for each in indegree:
            if indegree[each] == 0:
                queue.append(each)
                zero_degree += 1
                # unique
                if zero_degree > 1:
                    return False
        
        res = []
        
        while queue:
            prev = queue.pop(0)
            res.append(prev)
            count = 0
            for succ in outdegree[prev]:
                indegree[succ] -= 1
                if indegree[succ] == 0:
                    count += 1
                    queue.append(succ)
                    # not unique
                    if count > 1:
                        return False
            # if left
            if outdegree[prev] and not count:
                return False
        return res == org and set(org) == set(st)
```
### 269. Alien Dictionary
Hard 难度，一方面是构建dictionary的时候很繁琐.  
每次判断完后要del掉outdegree所对应pop出来的元素，直到没有出度，也就是全部遍历完了才成功。因为order的长度没有给出，所以不能用len(order) == len(origin) 来判断

```python
class Solution(object):
    def alienOrder(self, words):
        """
        :type words: List[str]
        :rtype: str
        """
        res = []
        indegree, outdegree = collections.defaultdict(int), collections.defaultdict(list)
        
        queue = []
        
        for i in range(1, len(words)):
            # consider play and playing
            if len(words[i-1]) > len(words[i]) and words[i-1][:len(words[i])] == words[i]:
                continue
            self.buildToplogicalSort(words[i-1], words[i], indegree, outdegree)
            
        # build number of char
        nodes = set()
        for word in words:
            for char in word:
                nodes.add(char)
                
        for char in nodes:
            if indegree[char] == 0:
            #if char not in indegree:
                queue.append(char)
          
        while queue:
            prev = queue.pop(0)
            res.append(prev)
            # we need to check outdegree because we del outdegree if we find
        
            for succ in outdegree[prev]:
                indegree[succ] -= 1
                if indegree[succ] == 0:
                    queue.append(succ)
            # del outdegree for this char
            del(outdegree[prev])
         
        if outdegree:
            return ""
        return "".join(res)
        
    def buildToplogicalSort(self, word1, word2, indegree, outdegree):
        length = min(len(word1), len(word2))
        for i in range(length):
            if word1[i] != word2[i]:
                # init pre char
                # if word1[i] not in outdegree:
                #     outdegree[word1[i]] = set()
                if word2[i] not in outdegree[word1[i]]:
                    indegree[word2[i]] += 1
                    outdegree[word1[i]].append(word2[i])
                # only contain two char is not the same its order after that is irrelevent
                break

```