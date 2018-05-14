---
title: Heap总结
comments: true
date: 2017-09-20 21:07:39
updated: 2018-03-20 21:07:39
categories: Leetcode
tags: Heap
---
# heapq--堆数据结构

heapq模块是python的一个标准库，它实现了一个堆数据结构，堆数据结构是一种二叉树。

## 什么是堆数据结构？

官网给出的定义是：
`
This implementation uses arrays for which 
heap[k] <= heap[2*k+1] and heap[k] <= heap[2*k+2]
for all k, counting elements from zero.`
我们可以这样理解：
堆是完全二叉树或者近似二叉树，它的各个节点的键值都有固定对应的的数字，根节点（即root,最上面起始位置）是0，若父节点为heap[k]，则子节点为heap[2*k+1]和heap[2*k+2]。父节点对应的值总是小于或者等于子节点，称为最小堆。对应的，父节点的值总是大于或者等于子节点，称为最大堆。**在heapq中，使用的是最小堆。**

正因为堆的这种特殊结构，使得通过heapq模块，可以快速获取一个列表的前N个最大(小)值，即Top N。

<!--more-->

## 特点 
这里，可能我们不禁要问，python不是内置了sort方法用来排序么？

现在我们假设一种情景，我们在维护一个列表，并且这个列表在变化，不断有新元素加入，而在任何时候我们可能需要获取里面的Top N，因此我们要求列表始终可以处于已排序状态。

这时候sort方法就显得不那么好用了，因为每次新加入一个元素，我们都要调用一次sort。数据量小时还是可以的，当数据量较大时，效率就会降低，并且python内置的sort方法本身在实现上也不是那么的高效，复杂度是O(NlgN)。

**特别强调，当初我思考了半天，构建heap需要O(N)的复杂度--见算法导论，而heapsort的话，每次操作heappop()是需要lgN的复杂度，而list中有N个元素，所以整体复杂度是O(NlgN)**

python维护了一个堆，使用的存储结构是列表，通过heapq模块来管理、操作这个堆。heapq提供了插入、删除元素的方法，并且保证在插入或删除元素时，所有节点自动调整，保证堆的结构，同时尽量高效，复杂度为O(log n)，在大数据时，效率高于sort排序。

## 常用方法使用
heapq.heappush(heap, item) 把item添加到heap这个list中  
heapq.heappop(heap)把堆顶元素弹出
两种操作的复杂度均为O（logN）

# 题目
## 215. Kth Largest Element in an Array
基本操作就好

```python
T O(Nlgk)
S O(N)
import heapq
class Solution(object):
    def findKthLargest(self, nums, k):
        """
        :type nums: List[int]
        :type k: int
        :rtype: int
        """
        heap = []
        for num in nums:
            heapq.heappush(heap,num)
            if len(heap) > k:
                heapq.heappop(heap)
        return heapq.heappop(heap)
        
```
## 347. Top K Frequent Elements
最基本的题目，只要注意Python中是最小堆就好，由于我们只取前K个元素，所以是klgN

```python
T O(N + klgN)
S O(N)
import heapq
class Solution(object):
    def topKFrequent(self, nums, k):
        """
        :type nums: List[int]
        :type k: int
        :rtype: List[int]
        """
        
        cntDict = collections.defaultdict(int)
        for i in nums:
            cntDict[i] += 1
        cnt_list = []
        res = []
        for key in cntDict.keys():
            heapq.heappush(cnt_list,(-cntDict[key],key))
        while k:
            res.append(heapq.heappop(cnt_list)[1])
            k -= 1
        return res
```

## 378. Kth Smallest Element in a Sorted Matrix
Example:

```
matrix = [
   [ 1,  5,  9],
   [10, 11, 13],
   [12, 13, 15]
],
k = 8,

return 13.
```
这道题目当然可以暴力解，就是n^2lgK，不过显然不是题目要求的,通过观察可以发现每一行每一列都是增序排列，所以可以每次只加横行或者纵列。但如何避免重复呢？可以维持一个数组记录已经访问过的点，要是为了节约空间的话，借鉴网上大神的想法，只有当处于第一列时才往下遍历，否则只横向遍历。

```python
# T O(klgN)
# S O(N)
import heapq
class Solution(object):
    def kthSmallest(self, matrix, k):
        """
        :type matrix: List[List[int]]
        :type k: int
        :rtype: int
        """
        m, n = len(matrix), len(matrix[0])
        q = [(matrix[0][0], 0, 0)]
        # ans = None
        while k:
            ans, i, j = heapq.heappop(q)
            if j == 0 and i + 1 < m:
                heapq.heappush(q, (matrix[i + 1][j], i + 1, j))
            if j + 1 < n:
                heapq.heappush(q, (matrix[i][j + 1], i, j + 1))
            k -= 1
        return ans
```