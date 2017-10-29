---
title: Indeed面试总结
comments: true
date: 2017-10-19 13:11:15
updated: 2017-10-19 13:11:15
categories: Interview
tags: Indeed
---
# 流程
HR联系-> 确认电话面试时间-> 电话面试 -> 后续跟进（move on fail）
Data Engineer

# 参考
1. 易梦前尘前辈的地里帖子 [链接](http://www.1point3acres.com/bbs/thread-223228-1-1.html)
2. Github总结 [链接] (https://github.com/shi-edward/Company-Algorithm-Solution/tree/master/src/indeed)
<!--more-->
# 题目
当时根据地里的总结了一下题目，但是电话面试的时候还是出了一道我似乎见过的但是没有复习到题目，直接给出了brute force解，优化的时候有点紧张就没有答上来....就最后跪了
## UnrolledLinked List

```python
class Node(object):
	def __init__(self, array):
		self.array = array
		self.len = len(self.array)
		while len(self.array) < 5:
			self.array.append(' ')
		

class LinkedList(object):
	def __init__(self, head):
		self.head = head
		self.next = None
		self.len = head.len


def get(head, index):
	if index < 0:
		return ''
	cur = head
	while cur and index:
		if index >= 5: # cur.len
			index -= 5
		else:
			return cur.head.array[index]
		cur = cur.next
	return ''

def insert(head, index, char):
	if index < 0:
		return 
	cur = head
	while cur and index:
		if index >= 5:
			index -= 5
		else:
			if cur.len == 5:
				newNode = Node([cur.head.array[-1]])
				newlist = LinkedList(newNode)
				cur.head.array[4] = char
			else:
				length = cur.len
				for i in range(length+1,index,-1):
					cur.head.array[i] = cur.head.array[i-1]
				cur.head.array[index] = char 
				return cur.head.array
			#break
		prev = cur
		cur = cur.next
	if not cur:
		node = Node([char])
		
		newlist = LinkedList(node)
		prev.next = newlist
		return newlist.head.array
	#return cur.head.array
n1 = Node(['a','t','y'])
n2 = Node(['r','i','p'])
l1 = LinkedList(n1)
l2 = LinkedList(n2)
l1.next = l2


print get(l1, 2)
print insert(l1,1,'c')
```

## Dice Sum

```python
import math
# O(6*m)
# O(mn)
def helper(dice, target, memo):
	res = 0
	# base case
	if dice == 0:
		if target == 0:
			return 1

	if target > 6 * dice or target < dice:
		return 0

	if memo[dice][target]:
		return memo[dice][target]

	for i in range(1,7):
		res += helper(dice-1, target-i, memo)

	memo[dice][target] = res
	return res

def dicesum(dice, target):
	# dice --- number of dice
	# target --- int

	# base case
	if dice < 1 or target < dice or target > 6* dice:
		return 0.0
	total = int(math.pow(6,dice))

	memo = [[0 for _ in range(target+1)] for _ in range(dice+1)]

	count = helper(dice, target, memo)

	return float(count) / total

print dicesum(2,4)
```

## Expire Time

```python
from collections import OrderedDict
import time

class expireDict(object):
	def __init__(self):
		self.key = ""
		self.value = ""
		self.regular_dict = dict()
		self.ordered_dict = OrderedDict()

	def put(self, key, value,duration):
		self.regular_dict[key] = value
		ticks = time.time()
		self.ordered_dict[str(ticks+duration)] = key
		self.check()

	def get(self, key):
		self.check()
		if key not in self.regular_dict:
			return None
		else:
			return self.regular_dict[key]

	def check(self):
		ticks = time.time()
		for timestamp in self.ordered_dict:
			key = self.ordered_dict[timestamp]
			if ticks - float(timestamp) > 0:
				del(self.ordered_dict[timestamp])
				del(self.regular_dict[key])
			
		

# T = expireDict()
# T.put('A',100,1)
# time.sleep(0.6)
# print T.ordered_dict
# T.put('B',200,1)
# time.sleep(0.6)
# print T.ordered_dict
# T.put('C',300,1)
# time.sleep(0.1)
# print T.ordered_dict
# T.put('D',400,1)
# time.sleep(0.1)
# print T.ordered_dict
# print T.get('D')
# print T.ordered_dict

# print T.get('A')
```

## Find Peak Element

```python
def findPeakElement(nums): 
	# O (lgN)
	# O(1)
	l = 0
	r = len(nums) - 1
	while l < r :
		mid = l + (r - l) / 2
		if nums[mid] > nums[mid+1]:
			r = mid
		else:
			l = mid + 1
	return l
print findPeakElement([1,2,3,4,1])
```

## Git relate question

```python
class GitNode(object):
	"""docstring for ClassName"""
	def __init__(self,id, parent):
		self.id = id
		self.parent = parent

def findAllCommits(node):
	res = []
	queue = [node]
	visited = set()
	visited.add(node.id)

	while queue:
		cur = queue.pop(0)
		res.append(cur.id)
		for par in cur.parent:

			if par.id not in visited:
				queue.append(par)
				visited.add(par.id)
	return res

g1 = GitNode(1,[])
g2 = GitNode(2,[g1])
g3 = GitNode(3,[g2])
g4 = GitNode(4,[g1])
print findAllCommits(g3)

def findLCA(node1, node2):
	# O(V+E)
	# visit every node and for each node needs to visit each edge
	# O(N)
	# create queue, each node enter 
	if not node1 or not node2:
		return None

	queue1 = [node1]
	queue2 = [node2]

	set1 = set()
	set1.add(node1.id)
	set2 = set()
	set2.add(node2.id)

	while queue1 and queue2:
		size1 = len(queue1)
		size2 = len(queue2)
		while size1:
			cur = queue1.pop(0)
			if cur.id in set2:
				return cur.id
			set1.add(cur.id)
			for par in cur.parent:
				queue1.append(par)
				set1.add(par.id)
			size1 -= 1
		while size2:
			cur = queue2.pop(0)
			if cur.id in set1:
				return cur.id
			set2.add(cur.id)
			for par in cur.parent:
				queue2.append(par)
				set2.add(par.id)
			size2 -= 1
	return None

print findLCA(g2, g4)
```

## Merge List

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def mergeTwoLists(self, l1, l2):
        """
        :type l1: ListNode
        :type l2: ListNode
        :rtype: ListNode
        """
        # O(N)
        # O(1)
        dummy = ListNode(-1)
        curr = dummy
        while l1 and l2:
            if l1.val < l2.val:
                curr.next = l1
                l1 = l1.next
            else:
                curr.next = l2
                l2 = l2.next
            curr = curr.next
        if l1:
            curr.next = l1
        else:
            curr.next = l2
        return dummy.next

import heapq
class Solution(object):
    def mergeKLists(self, lists):
        """
        :type lists: List[ListNode]
        :rtype: ListNode
        """
        # O(Nlogk)
        # O(N)
        dummy = ListNode(0)
        curr = dummy
        pq = []
        for node in lists:
            if node: ## empty
                heapq.heappush(pq,(node.val, node))
        while pq:
            curr.next = heapq.heappop(pq)[1]
            curr = curr.next
            if curr.next:
                heapq.heappush(pq,(curr.next.val, curr.next))
        return dummy.next
```

## Range Summary

```python
def rangePrint(nums):
	# O(n)
	# O(1)
	res = []
	if not nums:
		return res
	ori = 0
	preVal = nums[0]

	for i in range(1, len(nums)):
		if nums[i] == preVal+1:
			preVal = nums[i]
		if nums[i] == preVal:
			continue
		else:
			if i - ori == 1:
				res.append(str(nums[ori]))
			else:
				if nums[ori] == preVal:
					res.append(str(nums[ori]))
				else:
					temp = str(nums[ori]) + "->" + str(preVal)
					res.append(temp)
			ori = i
			preVal = nums[i]
	if ori +1 != len(nums):
		res.append(str(ori)+"->"+str(nums[-1]))	
	else:
		res.append(str(nums[-1]))
	return res

print rangePrint([1,2,3,4,5,5,7,7,8,9,11])

def rangesummary2(nums):
	# O(nlgn)
	def helper(nums, l, r):
		while l +1 < r:
			m = (l+r)/2
			if nums[m] - nums[l] == m - l:
				l = m
			else:
				r = m
		return l
	res = []
	i = 0

	while i < len(nums):
		k = helper(nums, i, len(nums))

		if i != k:
			res.append(str(nums[i])+"->"+str(nums[k]))
		else:
			res.append(str(nums[i]))
		i = k+1
	return res
print rangesummary2([1,2,3,4,4,5,6,8])
```

## Rearrange Lists

```python 
# ["a", "b", "c", "a", "a", "b"] -> [ ["a", "b", "c"], ["a", "b"], ["a"] ]
# This is not a unique solution; [ ["a", "b"], ["a", "b"], ["a", "c"] ] would also be a valid output. You only need to return one valid output.
# Order does not matter in any way for either the input or the output.
# Your solution should minimize the number of inner lists. 
# For example: 
# [ ["a"], ["a"], ["b"] ] would not be a correct solution for ["a", "a", "b"].
# The correct solution would be [ ["a", "b"], ["a"] ]



# rearrange the strings into a list of lists of strings where each inner list contains no duplicates
# ["a", "b", "c", "a", "a", "b"] -> [ ["a", "b", "c"], ["a", "b"], ["a"] ]
from collections import defaultdict
def func(s):
	res = [[]]
	index = [0 for _ in range(26)]
	for char in s:
		if len(res) <=index[ord(char) - 97]:
			temp = []
			res.append(temp)
		res[index[ord(char) - 97]].append(char)
		index[ord(char) - 97] += 1

	
	return res
print func(["a", "a", "b"])
```

## Reverse String

```python
def reverse_a_string_more_slowly(a_string):
    new_strings = []
    index = len(a_string)
    while index:
        index -= 1                       
        new_strings.append(a_string[index])
    return ''.join(new_strings)

def reverse_a_string_slowly(a_string):
    new_string = ''
    index = len(a_string)
    while index:
        index -= 1                    # index = index - 1
        new_string += a_string[index] # new_string = new_string + character
    return new_string

def reverseWords(s):
    """
    :type s: str
    :rtype: str
    """
    if not s:
        return ''
    li = s.strip().split()
    print li
    res = []
    for each in range(len(li)-1,-1,-1):
        res.append(li[each])
    return ' '.join(res)
print reverseWords('  a  b ')
```

## shortest word distance

```python
class Solution(object):
    

    def shortestDistance1(self, words, word1, word2):
        """
        :type words: List[str]
        :type word1: str
        :type word2: str
        :rtype: int
        """
        # O(n)
        # O(1)
        p1, p2 = -1, -1
        res = len(words)
        for i in range(len(words)):
            if words[i] == word1:
                p1 = i
            if words[i] == word2:
                p2 = i
            if p1!=-1 and p2 != -1:
                temp = abs(p1-p2)
                res = min(res, temp)
        return res
        
    def shortestWordDistance3(self, words, word1, word2):
        """
        :type words: List[str]
        :type word1: str
        :type word2: str
        :rtype: int
        """
        # O(n)
        # O(1)
        p1, p2 = -len(words), -1
        res = len(words)
        for i in range(len(words)):
            if word1 != word2:
                if words[i] == word1:
                    p1 = i
                if words[i] == word2:
                    p2 = i
                if p1!=-1 and p2 != -1:
                    temp = abs(p1-p2)
                    res = min(res, temp)
            else:
                if words[i] == word1:
                    res = min(res, abs(p1 - i))
                    p1 = i
        return res
        
 
from collections import defaultdict
class WordDistance(object):

    def __init__(self, words):
        """
        :type words: List[str]
        """
        self.map = defaultdict(list)
        for i in range(len(words)):
            self.map[words[i]] += [i]

    def shortest(self, word1, word2):
        """
        :type word1: str
        :type word2: str
        :rtype: int
        """
        # O(n)
        # O(n)
        l1 = self.map[word1]
        l2 = self.map[word2]
        if l1[0]>l2[0]:
            l1,l2 = l2, l1
        p1 = 0
        p2 = 0
        res = float('inf')
        while p1 < len(l1) and p2 < len(l2):
            temp = abs(l1[p1]- l2[p2])
            res = min(temp, res)
            if l1[p1] < l2[p2]:
                p1 +=1
            else:
                p2 += 1
        return res
        


# Your WordDistance object will be instantiated and called as such:
# obj = WordDistance(words)
# param_1 = obj.shortest(word1,word2)
```