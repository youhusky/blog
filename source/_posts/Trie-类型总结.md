---
title: Trie 类型总结
comments: true
date: 2018-04-22 16:32:32
updated: 2018-04-22 16:32:32
categories: Leetcode
tags: Trie
---
# Introduce to Trie
## What is Trie
A Trie is a special form of a Nary tree. Typically, a trie is used to store strings. Each Trie node represents a string (a prefix). Each node might have several children nodes while the paths to different children nodes represent different characters. And the strings the child nodes represent will be the origin string represented by the node itself plus the character on the path.

## How to represent
### Dict
In Python we can use Dictionary to represent it, key is the char and value is the dict. It can save some space but slower because we need to calculate the hashcode every time.  

```python
class Trie(object):
    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.root = dict()
        
    def insert(self, word):
        """
        Inserts a word into the trie.
        :type word: str
        :rtype: void
        """
        curr = self.root
        for char in word:
            if char not in curr:
                curr[char] = dict()
            curr = curr[char]
        curr['#'] = '#'
```
用defalutdict会更加方便

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
```
<!--more-->
### Array
We use array can save time but need to create length at least 26 to 256. Key is everytime we need to calculate index `ord(char)-97`

```python
from collections import defaultdict
class TrieNode(object):
    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.nodes = [0 for _ in range(26)]   # Easy to insert new node.
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
            index = ord(char) - 97
            if curr.nodes[index] == 0:
                temp = TrieNode()
                curr.nodes[index] = temp
                curr = temp
            else:
                curr = curr.nodes[index]
        curr.isword = True
```

# Basic operation
## Insert
pseudo-code

```
1. Initialize: cur = root
2. for each char c in target string S:
3.      if cur does not have a child c:
4.          cur.children[c] = new Trie node
5.      cur = cur.children[c]
6. cur is the node which represents the string S
```

## Search
pseudo-code

```
1. Initialize: cur = root
2. for each char c in target string S:
3.      if cur does not have a child c:
4.          search fails
5.      cur = cur.children[c]
6. search successes
```

## 208 Implement a Trie

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

# Question
## 677. Map Sum Pairs
Using Trie to Srote each char and the count of that

```python
class TrieNode():
    def __init__(self, count = 0):
        self.count = count
        self.children = {}
                
class MapSum(object):

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.root = TrieNode()
        self.keys = {}

    def insert(self, key, val):
        """
        :type key: str
        :type val: int
        :rtype: void
        """
        # Time: O(k)
        curr = self.root
        delta = val - self.keys.get(key, 0)
        self.keys[key] = val
        
        curr = self.root
        curr.count += delta
        for char in key:
            if char not in curr.children:
                curr.children[char] = TrieNode()
            curr = curr.children[char]
            curr.count += delta
        
    def sum(self, prefix):
        """
        :type prefix: str
        :rtype: int
        """
        # Time: O(k)
        curr = self.root
        for char in prefix:
            if char not in curr.children:
                return 0
            curr = curr.children[char]
        return curr.count
```
## 648. Replace Words
本质思想是构建一个Trie，然后查询的时候如果对应char没有发现，直接返回word，如果查到了prefix 直接返回prefix，都不满足返回word本身

```python
class TrieNode:
    
    def __init__(self):
        self.root = dict()
    
    def insert(self, root):
        node = self.root
        
        for char in root:
            if char not in node:
                node[char] = dict()
            node = node[char]
        node['#'] = root
            
    def replace(self, word):
        node = self.root
        for char in word:
            if char not in node:
                return word
            node = node[char]
            if node.get('#'):
                return node['#']
        return word


class Solution(object):
    def replaceWords(self, dict, sentence):
        """
        :type dict: List[str]
        :type sentence: str
        :rtype: str
        """

        trie = TrieNode()
        for root in dict:
            trie.insert(root)
            
        strs = sentence.split()
        for i,v in enumerate(strs):
            strs[i] = trie.replace(v)
        return " ".join(strs)
        
```

## 211. Add and Search Word - Data structure design
在基础的Trie上，查询的时候利用DFS，如果当前的char == ‘.' 那么继续dfs查询之后的一个char，只要有一个满足即可；else则是判断当前char是否在prefix树中

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
## 212. Word Search II
对于words中的每一个词建立Trie，然后DFS查询在board中能否找到
访问前存储该字母，之后再还原

```python
tmp = board[i][j]
board[i][j] ="@"
board[i][j] = tmp
```

```python
class Solution(object):
    def findWords(self, board, words):
        trie = {}
        for w in words:
            t = trie
            for c in w:
                if c not in t:
                    t[c] = {}
                t = t[c]
            t['#'] = '#'
        
        res = []
        for i in range(len(board)):
            for j in range(len(board[0])):
                self.find(board, i, j, trie, '', res)
        return list(set(res))

    def find(self, board, i, j, trie, path, res):
        # we find
        if '#' in trie:
            res.append(path)
        # not legal
        if i<0 or i>=len(board) or j<0 or j>=len(board[0]) or board[i][j] not in trie:
            return
        tmp = board[i][j]
        board[i][j] ="@"
        self.find(board, i+1, j, trie[tmp], path+tmp, res)
        self.find(board, i, j+1, trie[tmp], path+tmp, res)
        self.find(board, i-1, j, trie[tmp], path+tmp, res)
        self.find(board, i, j-1, trie[tmp], path+tmp, res)
        board[i][j] = tmp
```
## 425. Word Squares
Hard题目，抄了答案，但本质上还是Trie

```python
'''
Input:
["area","lead","wall","lady","ball"]

Output:
[
  [ "wall",
    "area",
    "lead",
    "lady"
  ],
  [ "ball",
    "area",
    "lead",
    "lady"
  ]
]

Explanation:
The output consists of two word squares. The order of output does not matter (just the order of words in each word square matters).
O(n), O(n) Trie
'''
from collections import defaultdict


class TrieNode(object):
	def __init__(self):
		# index
		self.indices = []
		self.children = defaultdict(TrieNode)

	def insert(self, words, i):
		cur = self
		for c in words[i]:
			if not cur.children[c]:
				cur.children[c] = TrieNode()
			cur = cur.children[c]
			cur.indices.append(i)


class Solution(object):
	def wordSquares(self, words):
		"""
		:type words: List[str]
		:rtype: List[List[str]]
		"""
		result = []

		trie = TrieNode()
		#init

		for index, word in enumerate(words):
			trie.insert(words,index)

		curr = []
		#dfs
		for s in words:
			curr.append(s)
			self.wordSquaresHelper(words, trie, curr, result)
			curr.pop()

		return result

	def wordSquaresHelper(self, words, trie, curr, result):
		if len(curr) >= len(words[0]):
			#print (curr)
			return result.append(list(curr))

		node = trie
		# check
		for s in curr:
			node = node.children[s[len(curr)]]
			if not node:
				return

		for i in node.indices:
			curr.append(words[i])
			self.wordSquaresHelper(words, trie, curr, result)
			curr.pop()
```