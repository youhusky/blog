---
title: Design 问题
comments: true
date: 2018-03-22 21:21:36
updated: 2018-03-22 21:21:36
categories: Leetcode
tags: Design
---

# Design系列问题
## 高频题
这类题基本上都是高频题
### 146. LRU Cache
这道题是很高频的题目，主要hint就是用双向链表来实现

```python
class Node:
	def __init__(self, k, v):
		self.key = k
		self.val = v
		self.prev = None
		self.next = None
class LRUCache(object):

    def __init__(self, capacity):
        """
        :type capacity: int
        """
        self.capacity = capacity
        self.dic = {}
        self.head = Node(0,0)
        self.tail = Node(0,0)
        self.head.next = self.tail
        self.tail.prev = self.head
        

    def get(self, key):
        """
        :type key: int
        :rtype: int
        """
        if key in self.dic:
            n = self.dic[key]
            self._remove(n)
            self._add(n)
            return n.val
        return -1

    def put(self, key, value):
        """
        :type key: int
        :type value: int
        :rtype: void
        """
        if key in self.dic:
            self._remove(self.dic[key])
        n = Node(key, value)
        self._add(n)
        # imp value - node
        self.dic[key] = n
        if len(self.dic) > self.capacity:
            n = self.head.next
            self._remove(n)
            del(self.dic[n.key])
            
    def _add(self, node):
        p = self.tail.prev
        p.next = node
        self.tail.prev = node
        node.prev = p
        node.next = self.tail
        
        
    def _remove(self, node):
        p = node.prev
        n = node.next
        p.next = n
        n.prev = p

# Your LRUCache object will be instantiated and called as such:
# obj = LRUCache(capacity)
# param_1 = obj.get(key)
# obj.put(key,value)
```
<!--more-->
### 155. Min Stack
这道题就是用stack来存sofar的最小值

```python
class MinStack(object):

    def __init__(self):
        """
        initialize your data structure here.
        """
        self.stack = []
        

    def push(self, x):
        """
        :type x: int
        :rtype: void
        """
        if len(self.stack) == 0:
            self.stack.append((x,x))
        else:
            min_sofar = min(x, self.stack[-1][1])
            self.stack.append((x, min_sofar))

    def pop(self):
        """
        :rtype: void
        """
        self.stack.pop()  

    def top(self):
        """
        :rtype: int
        """
        return self.stack[-1][0]

    def getMin(self):
        """
        :rtype: int
        """
        return self.stack[-1][1]

# Your MinStack object will be instantiated and called as such:
# obj = MinStack()
# obj.push(x)
# obj.pop()
# param_3 = obj.top()
# param_4 = obj.getMin()
```

### 716. Max Stack
与上一道题相类似，区别就是在popMax的时候用临时stack来记录

```python
class MaxStack(object):

    def __init__(self):
        """
        initialize your data structure here.
        """
        self.stack = []
        
        

    def push(self, x):
        """
        :type x: int
        :rtype: void
        """
        maxNumber = max(x, self.stack[-1][1]) if self.stack else x
        self.stack.append((x,maxNumber))

    def pop(self):
        """
        :rtype: int
        """
        return self.stack.pop()[0]
        

    def top(self):
        """
        :rtype: int
        """
        return self.stack[-1][0]
        

    def peekMax(self):
        """
        :rtype: int
        """
        return self.stack[-1][1]
        

    def popMax(self):
        """
        :rtype: int
        """
        temp = self.stack[-1][1]
        tempStack = []
        
        while self.stack[-1][0] != temp:
            tempStack.append(self.stack.pop())
        self.stack.pop()
        while tempStack:
            self.push(tempStack.pop()[0])
      
        return temp
        


# Your MaxStack object will be instantiated and called as such:
# obj = MaxStack()
# obj.push(x)
# param_2 = obj.pop()
# param_3 = obj.top()
# param_4 = obj.peekMax()
# param_5 = obj.popMax()
```
### 225. Implement Stack using Queues
在init的时候，每当新元素进来的时候，不断让queue的元素pop出来在加到queue尾，从而index为0 的元素就是最后加进来的

```python
class MyStack(object):

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.queue = []
        

    def push(self, x):
        """
        Push element x onto stack.
        :type x: int
        :rtype: void
        """
        self.queue.append(x)
        size = len(self.queue)
        while size > 1:
            self.queue.append(self.queue.pop(0))
            size -= 1
        

    def pop(self):
        """
        Removes the element on top of the stack and returns that element.
        :rtype: int
        """
        return self.queue.pop(0)
        

    def top(self):
        """
        Get the top element.
        :rtype: int
        """
        return self.queue[0]
        

    def empty(self):
        """
        Returns whether the stack is empty.
        :rtype: bool
        """
        return len(self.queue) == 0

# Your MyStack object will be instantiated and called as such:
# obj = MyStack()
# obj.push(x)
# param_2 = obj.pop()
# param_3 = obj.top()
# param_4 = obj.empty()
```
### 232. Implement Queue using Stacks
用两个stack，在pop的时候，也是同样的像上一题的操作，加到另外一个stack中

```python
class MyQueue(object):
    def __init__(self):
        self.input = []
        self.output = []
        
    def push(self, x):
        self.input.append(x)

    def pop(self):
        self.peek()
        return self.output.pop()
        
    def peek(self):
        if(self.output == []):
            while(self.input != []):
                self.output.append(self.input.pop())
        return self.output[-1]
        
    def empty(self):
        return self.input == [] and self.output == []
```
### 297. Serialize and Deserialize Binary Tree
这道题就是BFS遍历树，然后BFS解析树，注意index的值

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Codec:

    def serialize(self, root):
        """Encodes a tree to a single string.
        
        :type root: TreeNode
        :rtype: str
        """
        if not root:
            return ""
        queue = [root]
        res = []
        while queue:
            node = queue.pop(0)
            if node:
                queue.append(node.left)
                queue.append(node.right)
            res.append(str(node.val) if node else "#")
        # strip left ','
        return ",".join(res).strip(',')
        

    def deserialize(self, data):
        """Decodes your encoded data to tree.
        
        :type data: str
        :rtype: TreeNode
        """
        if not data:
            return None
        nodes = []
        for i in data.split(","):
            if i != '#':
                nodes.append(TreeNode(i))
            else:
                nodes.append(None)
        
        queue = [nodes[0]]
        index = 1
        while queue:
            node = queue.pop(0)
            if index < len(nodes) and nodes[index]:
                node.left = nodes[index]
                queue.append(nodes[index])
            if index + 1 < len(nodes) and nodes[index+1]:
                node.right = nodes[index+1]
                queue.append(nodes[index+1])
            index += 2
            
        return nodes[0]
        

# Your Codec object will be instantiated and called as such:
# codec = Codec()
# codec.deserialize(codec.serialize(root))
```
### 173. Binary Search Tree Iterator
这道题也是高频题，FB面过.注意pushAll的时候是判断root！

```python
# Definition for a  binary tree node
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class BSTIterator(object):
    def __init__(self, root):
        """
        :type root: TreeNode
        """
        self.stack = []
        self.pushAll(root)
        
    def pushAll(self,root):
        while root:
            self.stack.append(root)
            root = root.left
            
    def hasNext(self):
        """
        :rtype: bool
        """
        return self.stack
        

    def next(self):
        """
        :rtype: int
        """
        node = self.stack.pop()
        
        self.pushAll(node.right)
        return node.val
        

# Your BSTIterator will be called like this:
# i, v = BSTIterator(root), []
# while i.hasNext(): v.append(i.next())
```
### 380. Insert Delete GetRandom O(1)
用dic来记录value和对应的index，从而能保证O（1）时间内删除

```python
import random
class RandomizedSet(object):

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.array = []
        self.dic = dict()

    def insert(self, val):
        """
        Inserts a value to the set. Returns true if the set did not already contain the specified element.
        :type val: int
        :rtype: bool
        """
        if val in self.dic:
            return False
        self.dic[val] = len(self.array)
        self.array.append(val)
        return True

    def remove(self, val):
        """
        Removes a value from the set. Returns true if the set contained the specified element.
        :type val: int
        :rtype: bool
        """
        if val not in self.dic:
            return False
        # val.index in self.array
        index = self.dic[val]
        
        # check not the last
        if index < len(self.array) - 1:
            last = self.array[-1]
            self.dic[last] = index
            self.array[index] = last
            
        self.array.pop()
        del(self.dic[val])
        return True
        

    def getRandom(self):
        """
        Get a random element from the set.
        :rtype: int
        """
        return self.array[random.randint(0, len(self.array)-1)]


# Your RandomizedSet object will be instantiated and called as such:
# obj = RandomizedSet()
# param_1 = obj.insert(val)
# param_2 = obj.remove(val)
# param_3 = obj.getRandom()
```
### 381. Insert Delete GetRandom O(1) - Duplicates allowed
dic里面的值用set来记录

```python
import random
class RandomizedCollection(object):

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.array = []
        self.dic = dict()

    def insert(self, val):
        """
        Inserts a value to the collection. Returns true if the collection did not already contain the specified element.
        :type val: int
        :rtype: bool
        """
        
        if val not in self.dic:
            self.dic[val] = set()
            self.dic[val].add(len(self.array))
            self.array.append(val)
            return True
        else:
            self.dic[val].add(len(self.array))
            self.array.append(val)
            
            return False

    def remove(self, val):
        """
        Removes a value from the set. Returns true if the set contained the specified element.
        :type val: int
        :rtype: bool
        """
        if val not in self.dic:
            return False
        
        # val.index in self.array
        index = self.dic[val].pop()
        
        if index < len(self.array) - 1:
            
            last = self.array[-1]
            self.array[index] = last
            # last item delete
            # remove old insert new
            
            self.dic[last].remove(len(self.array)-1)
            self.dic[last].add(index)
            
        self.array.pop()
        if not self.dic[val]:
            del(self.dic[val])
        return True
        

    def getRandom(self):
        """
        Get a random element from the set.
        :rtype: int
        """
        return self.array[random.randint(0, len(self.array)-1)]


# Your RandomizedCollection object will be instantiated and called as such:
# obj = RandomizedCollection()
# param_1 = obj.insert(val)
# param_2 = obj.remove(val)
# param_3 = obj.getRandom()
```

## Trie类型
### 简单构建
在程序中需要简单构建一个Trie

```python
trie = {}
for w in words:
    t = trie
    for c in w:
        if c not in t:
            t[c] = {}
        t = t[c]
    t['#'] = '#'
```
### 208. Implement Trie (Prefix Tree)
基础类型

```python
from collections import defaultdict
class TrieNode(object):
    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.nodes = defaultdict(TrieNode)  # Easy to insert new node.
        self.isword = False  # True for the end of the trie.
class Trie(object):

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.root = TrieNode()
        

    def insert(self, word):
        """
        Inserts a word into the trie.
        :type word: str
        :rtype: void
        """
        curr = self.root
        for char in word:
            curr = curr.nodes[char]
        curr.isword = True
        

    def search(self, word):
        """
        Returns if the word is in the trie.
        :type word: str
        :rtype: bool
        """
        curr = self.root
        for char in word:
            if char not in curr.nodes:
                return False
            else:
                curr = curr.nodes[char]
        return curr.isword
        

    def startsWith(self, prefix):
        """
        Returns if there is any word in the trie that starts with the given prefix.
        :type prefix: str
        :rtype: bool
        """
        curr = self.root
        for char in prefix:
            if char not in curr.nodes:
                return False
            else:
                curr = curr.nodes[char]
        return True
        
# Your Trie object will be instantiated and called as such:
# obj = Trie()
# obj.insert(word)
# param_2 = obj.search(word)
# param_3 = obj.startsWith(prefix)
```
### 211. Add and Search Word - Data structure design
如何处理. 的问题，用string的slice来搞，find的递归操作

```python
from collections import defaultdict
class TrieNode(object):
    def __init__(self):
        self.node = defaultdict(TrieNode)
        self.isWord = False

    def __repr__(self):
        return repr(self.node)
        
class WordDictionary(object):

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.root = TrieNode()
        

    def addWord(self, word):
        """
        Adds a word into the data structure.
        :type word: str
        :rtype: void
        """
        curr = self.root
        for char in word:
            curr = curr.node[char]
        curr.isWord = True
        

    def search(self, word):
        """
        Returns if the word is in the data structure. A word could contain the dot character '.' to represent any one letter.
        :type word: str
        :rtype: bool
        """
        return self.find(self.root, word)
    
    def find(self, trie, word):
        if word == '':
            return trie.isWord
        
        if word[0] == '.':
            for i in trie.node:
                if self.find(trie.node[i], word[1:]):
                    return True
        else:
            child = trie.node.get(word[0])
            if child:
                return self.find(child, word[1:])
        return False
```
### 642. Design Search Autocomplete System
进阶版本 取前三个。很复杂的题

```python
class TrieNode(object):
    def __init__(self):
        self.children = dict()
        self.isEnd = False
        self.rank = 0
        self.data  = None
        
class AutocompleteSystem(object):

    def __init__(self, sentences, times):
        """
        :type sentences: List[str]
        :type times: List[int]
        """
        self.root = TrieNode()
        self.keyword = ""
        for i, sentence in enumerate(sentences):
            self.addRecord(sentence, times[i])
            
    def addRecord(self, word, rank):
        p = self.root
        for char in word:
            if char not in p.children:
                p.children[char] = TrieNode()
            p = p.children[char]
        p.isEnd = True   
        p.data = word
        # compare
        p.rank -= rank
        

    def search(self, word):
        p = self.root
        for char in word:
            if char not in p.children:
                return []
            p = p.children[char]
        return self.dfs(p)
    
    def dfs(self, root):
        res = []
        if root:
            # find the end
            if root.isEnd:
                res.append((root.rank, root.data))
            for child in root.children:
                res.extend(self.dfs(root.children[child]))
        return res
    
    def input(self, c):
        """
        :type c: str
        :rtype: List[str]
        """
        res = []
        if c != "#":
            self.keyword += c
            res = self.search(self.keyword)
        else:
            self.addWord(self.keyword,1)
            self.keyword = ""
        return [item[1] for item in sorted(res)[:3]]
        


# Your AutocompleteSystem object will be instantiated and called as such:
# obj = AutocompleteSystem(sentences, times)
# param_1 = obj.input(c)
```

## 一般的题
### 281. Zigzag Iterator
就是正常的来

```python
class ZigzagIterator(object):

    def __init__(self, v1, v2):
        """
        Initialize your data structure here.
        :type v1: List[int]
        :type v2: List[int]
        """
        self.res = []
        pos = 0
        while pos < max(len(v1), len(v2)):
            if pos < len(v1):
                self.res.append(v1[pos])
            if pos < len(v2):
                self.res.append(v2[pos])
            pos += 1
        self.index = 0
        

    def next(self):
        """
        :rtype: int
        """
        res = self.res[self.index]
        self.index += 1
        return res
        

    def hasNext(self):
        """
        :rtype: bool
        """
        return self.index < len(self.res)
        

# Your ZigzagIterator object will be instantiated and called as such:
# i, v = ZigzagIterator(v1, v2), []
# while i.hasNext(): v.append(i.next())
```
### 284. Peeking Iterator
先预存next的值

```python
# Below is the interface for Iterator, which is already defined for you.
#
# class Iterator(object):
#     def __init__(self, nums):
#         """
#         Initializes an iterator object to the beginning of a list.
#         :type nums: List[int]
#         """
#
#     def hasNext(self):
#         """
#         Returns true if the iteration has more elements.
#         :rtype: bool
#         """
#
#     def next(self):
#         """
#         Returns the next element in the iteration.
#         :rtype: int
#         """

class PeekingIterator(object):
    def __init__(self, iterator):
        self.iter = iterator
        self.temp = self.iter.next() if self.iter.hasNext() else None

    def peek(self):
        return self.temp

    def next(self):
        ret = self.temp
        self.temp = self.iter.next() if self.iter.hasNext() else None
        return ret

    def hasNext(self):
        return self.temp is not None
        

# Your PeekingIterator object will be instantiated and called as such:
# iter = PeekingIterator(Iterator(nums))
# while iter.hasNext():
#     val = iter.peek()   # Get the next element but not advance the iterator.
#     iter.next()         # Should return the same value as [val].
```
### 359. Logger Rate Limiter
window的size为10

```python
class Logger(object):

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.dic = dict()
        

    def shouldPrintMessage(self, timestamp, message):
        """
        Returns true if the message should be printed in the given timestamp, otherwise returns false.
        If this method returns false, the message will not be printed.
        The timestamp is in seconds granularity.
        :type timestamp: int
        :type message: str
        :rtype: bool
        """
        # condition one 
        if message in self.dic and timestamp - self.dic[message] < 10:
            return False
        else:
            self.dic[message] = timestamp
            return True


# Your Logger object will be instantiated and called as such:
# obj = Logger()
# param_1 = obj.shouldPrintMessage(timestamp,message)
```
### 362. Design Hit Counter
因为是统计当时的hit值，所以可以维护一个全局变量hit

```python
class HitCounter(object):

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.count = 0
        self.queue = []

    def hit(self, timestamp):
        """
        Record a hit.
        @param timestamp - The current timestamp (in seconds granularity).
        :type timestamp: int
        :rtype: void
        """
        if not self.queue or self.queue[-1][0] != timestamp:
            self.queue.append([timestamp,1])
        else:
            self.queue[-1][1] += 1
        self.count += 1
        
    def getHits(self, timestamp):
        """
        Return the number of hits in the past 5 minutes.
        @param timestamp - The current timestamp (in seconds granularity).
        :type timestamp: int
        :rtype: int
        """
        # move forward
        while self.queue and timestamp - self.queue[0][0] >= 300:
            self.count -= self.queue.pop(0)[1]
        return self.count


# Your HitCounter object will be instantiated and called as such:
# obj = HitCounter()
# obj.hit(timestamp)
# param_2 = obj.getHits(timestamp)
```