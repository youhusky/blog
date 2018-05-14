---
title: Tree总结
comments: true
date: 2017-09-24 14:50:36
updated: 2018-04-24 14:50:36
categories: Leetcode
tags: [Tree, DFS, BFS]
---
# Tree的性质

## Divide and Conquer模版
```python
def traversal(root):
	# none or leaf
	if not root:
		# do sth
	
	# divide
	left = traversal(root.left)
	right = traversal(root.right)
	
	# Conquer
	res = # merge 
	return res

```
## 小技巧
T O(n) 一般是遍历所有点
S O(h) 用堆栈来做的话是遍历所有点
  O(n) 用队列实现遍历所有点
  
<!--more-->
### 104. Maximum Depth of Binary Tree
深度等于子树高度+1
T O(n)
S O(h)

```python
class Solution(object):
    def maxDepth(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """
        if not root:
            return 0
        left = self.maxDepth(root.left)
        right = self.maxDepth(root.right)
        return max(left,right) + 1
```
### 111. Minimum Depth of Binary Tree
和上一道题相比，需要判断树的情况，如果一个node的左儿子为空 右儿子不空 从root 到左儿子的路径不算是minimum depth
因为左儿子不算这个node的leaf node。  
T O(n)  
S O(h)

```python
class Solution(object):
    def minDepth(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """
        if not root:
            return 0
        left = self.minDepth(root.left)
        right = self.minDepth(root.right)
        if not left:
            return right+1
        if not right:
            return left+1
        return min(left,right) + 1
```

### 110. Balanced Binary Tree
这道题和上面的类似，都是找深度，由于要返回一个boolean值，所以多用一个helper function

T O(n)  
S O(h)

```python
class Solution(object):
    def isBalanced(self, root):
        """
        :type root: TreeNode
        :rtype: bool
        """
        return self.check(root) == -1
    def check(self, root):
        if not root:
            return -1
        left = self.isBalanced(root.left)
        right = self.isBalanced(root.right)
        if left == -1 or right == -1 or abs(left-right) > 1:
            return -1
        else:
            return max(left, right) + 1
```

### 100. Same Tree
我们考虑一下结束条件，如果两个结点都是null，也就是到头了，那么返回true。如果其中一个是null，说明在一棵树上结点到头，另一棵树结点还没结束，即树不相同，或者两个结点都非空，并且结点值不相同，返回false。最后递归处理两个结点的左右子树，返回左右子树递归的与结果即可。  
T O(n)  
S O(h)

```python
class Solution(object):
    def isSameTree(self, p, q):
        """
        :type p: TreeNode
        :type q: TreeNode
        :rtype: bool
        """
        if not p and not q:
            return True
        if not p or not q:
            return False
        if p.val != q.val:
            return False
        return self.isSameTree(p.left, q.left) and self.isSameTree(p.right, q.right)
```

### 101. Symmetric Tree
本质上和上一题一样，区别就是从两棵树到左右孩子。   
T O(n)  
S O(h)

```python
class Solution(object):
    def isSymmetric(self, root):
        """
        :type root: TreeNode
        :rtype: bool
        """
        if not root:
            return True
        return self.check(root.left, root.right)
    def check(self, node1, node2):
        if not node1 and not node2:
            return True
        if not node1 or not node2:
            return False
        if node1.val != node2.val:
            return False
        return self.check(node1.left, node2.right) and self.check(node1.right, node2.left)
```

## 三种遍历
### recursion
recursion的方法很简单 Time O(n) Space O(1)  

```python
class Solution(object):
    def orderTraversal(self, root):
        """
        :type root: TreeNode
        :rtype: List[int]
        """
        res = []
         
        self.traverse(root, res)
        return res
    def traverse(self, root, res):
        if not root:
            return;
        res.append(root.val) # preorder
        self.traverse(root.left, res)
        ## res.append(root.val) # inorder
        self.traverse(root.right, res)
        ## res.append(root.val) # postorder
```

### iterative
因为不能使用recursion，所以我们要模拟构建栈。  
#### 前序遍历(pre-order): 
根->左->右1. 对root异常处理 2.cur 指向root, 循环条件为node!=null || !stack.isEmpty() 3.当cur不为空，就压入stack,并将元素加入结果，cur继续往左边找 4.当cur为空，就cur就为pop出的栈顶元素,.cur继续往右边找. 5.返回最终结果集合.  
T O(n)   
S O(h)  

```python
class Solution(object):
    def preorderTraversal(self, root):
        """
        :type root: TreeNode
        :rtype: List[int]
        """
        if not root:
            return []
        stack = []
        res = []
        node = root
        while node or stack:
            while node:
                stack.append(node)
                res.append(node.val)
                node = node.left
            node = stack.pop()
            node = node.right
            
        return res
```

#### 中序遍历
本质上是一样的，先访问左孩子所以就一路到底

```python
class Solution(object):
    def inorderTraversal(self, root):
        """
        :type root: TreeNode
        :rtype: List[int]
        """
        if not root:
            return []
        stack = []
        res = []
        node = root
        while node or stack:
            while node:
                stack.append(node)
                
                node = node.left
            node = stack.pop()
            res.append(node.val)
            node = node.right
            
        return res
```

#### 后序遍历
这个需要一点技巧以及练习，因为根节点需要访问两次，所以就需要判断是否已经访问过右节点了

```python
class Solution(object):
    def postorderTraversal(self, root):
        """
        :type root: TreeNode
        :rtype: List[int]
        """
        res = []
        if not root:
            return res
        stack = []
        pre = None
        while root or stack:
            while root:
                stack.append(root)
                root = root.left
            
            peak = stack[-1]
            if peak.right and peak.right != pre: # 如果当前栈顶元素的右结点存在并且还没访问过（也就是右结点不等于上一个访问结点）就访问右结点 
                root = peak.right
            else: # 如果栈顶元素右结点是空或者已经访问过，那么说明栈顶元素的左右子树都访问完毕 需要把栈顶元素加入结果并且回溯上一层

                stack.pop()
                res.append(peak.val)
                pre = peak
        return res
```

不过还有一种更加巧妙的办法，前序遍历和后续遍历能否直接颠倒呢？答案是否定的，我们来看看前序遍历：根-左子树-右子树
后序遍历：左子树-右子树-根 把前序遍历倒过来：右子树-左子树-根 ！左右子树相反，不能直接倒！
但是这题，哼哼哼，先左子树入栈，在右子树入栈！最后输出颠倒一下即可！

```python
class Solution:
    # @param root, a tree node
    # @return a list of integers
    def postorderTraversal(self, root):
        if not root: return []
        ans,q=[],[]
        q.append(root)
        while q:
            cur=q.pop()          
            if cur.left: q.append(cur.left)
            if cur.right: q.append(cur.right)
            ans.append(cur.val)
        return ans[::-1]
```
### 116. Populating Next Right Pointers in Each Node
前序遍历的性质的小变种题目
```python
class Solution:
    # @param root, a tree link node
    # @return nothing
    def connect(self, root):
        if not root:
            return
        if root.left:
            root.left.next = root.right
        if root.right:
            if root.next:
                root.right.next = root.next.left
            else:
                root.right.next = None
        self.connect(root.left)
        self.connect(root.right)
```
## 层序遍历
基本思想便是套用BFS模版，用queue实现，在Python中可以通过引入Deque

### 102. Binary Tree Level Order Traversal
这是基本题型，外层queue记录第几层，内层size记录当前层所存储的节点   
  T O(V+E)  
  S O(n)
  
```python
class Solution(object):
    def levelOrder(self, root):
        """
        :type root: TreeNode
        :rtype: List[List[int]]
        """
        if not root:
            return []
        res = []
        
        queue = []
        queue.append(root)
        while queue:
            size = len(queue)
            temp = []
            while size:
                node = queue.pop(0)
                temp.append(node.val)
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
                size -= 1
            res.append(list(temp))
        return res
```

### 107. Binary Tree Level Order Traversal II
本质上和上一道题一样，只是在外层倒叙输出

```python
class Solution(object):
    def levelOrderBottom(self, root):
        """
        :type root: TreeNode
        :rtype: List[List[int]]
        """
        if not root:
            return []
        res = []
        queue = []
        queue.append(root)
        while queue:
            size = len(queue)
            temp = []
            while size:
                node = queue.pop(0)
                temp.append(node.val)
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
                size -= 1
            **res.insert(0,list(temp))**
        return res
```

### 103. Binary Tree Zigzag Level Order Traversal
类似的题，在内层倒叙输出，设置flag记录奇偶

```python
class Solution(object):
    def zigzagLevelOrder(self, root):
        """
        :type root: TreeNode
        :rtype: List[List[int]]
        """
        if not root:
            return []
        res = []
        **count = 0**
        queue = []
        queue.append(root)
        while queue:
            size = len(queue)
            temp = []
            while size:
                node = queue.pop(0)
**                if count % 2 == 0:
                    temp.append(node.val)
                if count % 2 == 1:
                    temp.insert(0,node.val)**
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
                size -= 1
            count += 1
            res.append(list(temp))
        return res
```
### 637.Average of Levels in Binary Tree
层序遍历的基础上，每层的average

```python
class Solution(object):
    def averageOfLevels(self, root):
        """
        :type root: TreeNode
        :rtype: List[float]
        """
        if not root:
            return [0]
        queue = [root]
        res = []
       
        while queue:
            size = len(queue)
            temp = []
            while size:
                node = queue.pop(0)
                temp.append(node.val)
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
                size -= 1
            res.append(sum(temp)/float(len(temp)))
        return res  
```
### 314. Binary Tree Vertical Order Traversal
也是BFS traverse题，中间存贮index的值

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def verticalOrder(self, root):
        """
        :type root: TreeNode
        :rtype: List[List[int]]
        """
        if not root:
            return []
        queue = [(root,0)]
        dic = dict()
        while queue:
            node, index = queue.pop(0)
            if node.left:
                queue.append((node.left, index-1))
            if node.right:
                queue.append((node.right, index + 1))
            if index not in dic:
                dic[index] = []
            dic[index].append(node.val)
        minx,maxx = float('inf'), float('-inf')
        for key in dic.keys():
            minx = min(minx, key)
            maxx = max(maxx, key)
            
        res = [0] * (maxx - minx + 1)
        
        for key in dic:
            res[key-minx] = dic[key]
        
        return res
        
```
# Path系列问题
## 技巧
基本上是可以用递归和分治的方法来进行解决，存在解和所有解都是一样的操作
### 257. Binary Tree Paths
输出所有路径，DFS递归，然后到叶子结点的时候返回

```python
class Solution(object):
    def binaryTreePaths(self, root):
        """
        :type root: TreeNode
        :rtype: List[str]
        """
        res = []
        if not root:
            return res
        self.helper(root, res, str(root.val))
        return res
    def helper(self, root, res, temp):
        if not root:
            return 
        if not root.left and not root.right:
            res.append(temp)
            return
        if root.left:
            self.helper(root.left, res, temp + '->'+str(root.left.val))
        if root.right:
            self.helper(root.right, res , temp + '->'+str(root.right.val))
```
### 112. Path Sum
```python
T O(n)
S O(h)
class Solution(object):
    def hasPathSum(self, root, sum):
        """
        :type root: TreeNode
        :type sum: int
        :rtype: bool
        """
        if not root:
            return False
        if not root.left and not root.right and root.val == sum:
            return True
        return self.hasPathSum(root.left, sum - root.val) or self.hasPathSum(root.right, sum - root.val)
```

### 113. Path Sum II
发现解的时候需要list(temp)

```python
class Solution(object):
    def pathSum(self, root, sum):
        """
        :type root: TreeNode
        :type sum: int
        :rtype: List[List[int]]
        """
        res = []
        self.helper(root, sum, res, [])
        return res
    def helper(self, root, target, res, temp):
        if not root:
            return
        if not root.left and not root.right and root.val == target:
            res.append(list(temp+[root.val]))
        return self.helper(root.left, target - root.val, res, temp+[root.val]) or self.helper(root.right, target - root.val, res, temp+[root.val])
```

### 129. Sum Root to Leaf Numbers
每一条往下传的时候，根据题目要求`prev * 10 + root.val`,然后分治相加

```python
class Solution(object):
    def sumNumbers(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """
        return self.helper(root, 0)
    def helper(self, root, total):
        if not root:
            return 0
        if not root.left and not root.right:
            return total * 10 + root.val
        left = self.helper(root.left, total*10 + root.val)
        right = self.helper(root.right, total*10 + root.val)
        return left + right
```

### 124. Binary Tree Maximum Path Sum
这道题相比上一道题区别在于每个结点的local max 不一样，这道题是不需经过根节点的，所以可以变成无向图，然后分成四种情况: `root.val, root.val+root.left.val, root.val+root.right.val` 这三种是可以继续向上传的，`root.val+root.left.val+root.right.val`这种是不可以往上传的，所以这些情况可以进行local比较，最终返回global max

```python
class Solution(object):
    def maxPathSum(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """
        maxSum = [float('-inf')]
        self.helper(root, maxSum)
        return maxSum[0]
    def helper(self, root, maxSum):
        if not root:
            return 0
        left = self.helper(root.left, maxSum)
        right = self.helper(root.right, maxSum)
        temp = max(root.val + left, root.val+right, root.val)
        maxSum[0] = max(maxSum[0], temp, root.val+left+right)
        return temp
```

### 563. Binary Tree Tilt
这道题根据题设，每次结点的返回值是其左右孩子和本身的和，然后每次更新abs()

```python
class Solution(object):
    def findTilt(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """
        total = [0]
        if not root:
            return 0
        self.helper(root,total)
        return total[0]
    def helper(self, root,total):
        if not root:
            return 0
        left = self.helper(root.left,total)
        right = self.helper(root.right,total)
        total[0] += abs(left - right)
        return left+right+root.val # sum
```

## Binary Search Tree性质
这种题目是根据其性质，左孩子永远比根节点小，右孩子永远比根节点大

### 235. Lowest Common Ancestor of a Binary Search Tree
```python
class Solution(object):
    def lowestCommonAncestor(self, root, p, q):
        """
        :type root: TreeNode
        :type p: TreeNode
        :type q: TreeNode
        :rtype: TreeNode
        """
        if p.val > q.val:
            p,q = q,p
        if p.val <= root.val and q.val >= root.val:
            return root
        if q.val < root.val:
            return self.lowestCommonAncestor(root.left, p, q)
        if p.val > root.val:
            return self.lowestCommonAncestor(root.right, p, q)
```

### 270. Closest Binary Search Tree Value
```python
class Solution(object):
    def closestValue(self, root, target):
        """
        :type root: TreeNode
        :type target: float
        :rtype: int
        """
        r = root.val
        while root:
            if abs(root.val - target) < abs(r - target):
                r = root.val # generate new node
            root = root.left if target < root.val else root.right
        return r
```

### 272. Closest Binary Search Tree Value II
这道题要维持一个k长度的list，所以可以中序遍历，然后不断更新最后添加元素和队列首元素与target的差值

```python
class Solution(object):
    def closestKValues(self, root, target, k):
        """
        :type root: TreeNode
        :type target: float
        :type k: int
        :rtype: List[int]
        """
        res = []
        self.helper(root, target, k ,res)
        return res
    def helper(self, root, target, k, res):
        if not root:
            return
        self.helper(root.left, target, k, res)
        if len(res) < k:
            res.append(root.val)
        else:
            if abs(target - root.val) < abs(target - res[0]):
                res.pop(0)
                res.append(root.val)
                
        self.helper(root.right, target , k , res)
```