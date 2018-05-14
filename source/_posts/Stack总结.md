---
title: Stack总结
comments: true
date: 2017-10-24 21:51:46
updated: 2017-10-27 21:56:46
categories: Leetcode
tags: stack
---
# Stack性质
## 定义
Stack的定义便是先进后出，在python中用list实现

```python
class Stack(object):
	def __init__(self):
		self.stack = []
	def push(self, i):
		self.stack.append(i)
	def pop(self):
		if self.stack：
			return self.stack.pop()
		else:
			raise("Error")
	def peek(self):
		return self.stack[-1]
	
```
## Basic 题目
### 225. Implement Stack using Queues
只用一个queue，每次append的时候，都要把前面的给pop出来再append进去

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

<!--more-->
### 232. Implement Queue using Stacks
用两个stack来存，输出的时候再全部放入到output的stack中

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
### 20. Valid Parentheses
左边的Parentheses作为key进入,右面的来判断是不是跟顶部的一样

```python
class Solution(object):
    def isValid(self, s):
        """
        :type s: str
        :rtype: bool
        """
        stack = []
        for char in s:
            if char == '(':
                stack.append(')')
            elif char == '[':
                stack.append(']')
            elif char == '{':
                stack.append('}')
            elif not stack or char != stack.pop():
                return False
        return not stack
```
<!--more-->
### 42. Trapping Rain Water
这道题感觉不是十分好想，需要维持一个stack来进行操作，当遇到新加的元素比栈顶元素大的时候，我们就要比较之前的元素，如果栈里面是有一个，则不能形成坑，continue；不然就比较之前的元素和当前的最小值，减去高度。哎呀，还是需要画图用例子来说比较好
> 我们的做法是，遍历高度，如果此时栈为空，或者当前高度小于等于栈顶高度，则把当前高度的坐标压入栈，注意我们不直接把高度压入栈，而是把坐标压入栈，这样方便我们在后来算水平距离。当我们遇到比栈顶高度大的时候，就说明有可能会有坑存在，可以装雨水。此时我们栈里至少有一个高度，如果只有一个的话，那么不能形成坑，我们直接跳过，如果多余一个的话，那么此时把栈顶元素取出来当作坑，新的栈顶元素就是左边界，当前高度是右边界，只要取二者较小的，减去坑的高度，长度就是右边界坐标减去左边界坐标再减1，二者相乘就是盛水量啦
> http://www.cnblogs.com/grandyang/p/4402392.html

```python
class Solution(object):
    def trap(self, height):
        """
        :type height: List[int]
        :rtype: int
        """
        stack = []
        i = 0
        size = len(height)
        res = 0
        while i < size:
            if not stack or height[i] <= height[stack[-1]]:
                stack.append(i)
                i += 1
            else:
                top = stack.pop()
                if not stack:
                    continue
                else:
                    res += (min(height[i], height[stack[-1]]) - height[top]) * (i - stack[-1] -1) # height * width
        return res
```


## Decreasing stack
递减stack主要是记录数组中第一个比它大的数

```python
while stack and nums[i] > stack[-1]:
	dic[stack.pop()] = nums[i]
stack.append(nums[i])
```

### 496. Next Greater Element I

```python
class Solution(object):
    def nextGreaterElement(self, findNums, nums):
        """
        :type findNums: List[int]
        :type nums: List[int]
        :rtype: List[int]
        """
        res = []
        stack = []
        # to store
        dic = dict()
        for i in range(len(nums)):
            while stack and nums[i] > stack[-1]:
                dic[stack.pop()] = nums[i]
            stack.append(nums[i])
        # deal with last   
        while stack:
            dic[stack.pop()] = -1
            
        for i in range(len(findNums)):
            res.append(dic[findNums[i]])
        return res
```
### 739. Daily Temperatures

```python
class Solution(object):
    def dailyTemperatures(self, temperatures):
        """
        :type temperatures: List[int]
        :rtype: List[int]
        """
        res = [0] * len(temperatures)
        stack = []
        left = 0
        for i in range(len(temperatures)):
            while stack and temperatures[i] > temperatures[stack[-1]]:
                index = stack.pop()
                res[index] = i - index
            stack.append(i)
        return res
```
### 503. Next Greater Element II

```python
class Solution(object):
    # stack
    def nextGreaterElements(self, nums):
        stack, res = [], [-1] * len(nums)
        for i in range(len(nums)) * 2:
            while stack and (nums[stack[-1]] < nums[i]):
                res[stack.pop()] = nums[i]
            stack.append(i)
            
        return res
```
### 316. Remove Duplicate Letters

```python
class Solution(object):
    def removeDuplicateLetters(self, s):
        """
        :type s: str
        :rtype: str
        """
        stack = []
        dic = dict()
        for char in s:
            dic[char] = dic.get(char, 0) + 1
            
        for char in s:
            dic[char] -= 1
            if char in stack:
                continue
            else:
                while stack and ord(char) < ord(stack[-1]) and dic[stack[-1]] > 0:
                    stack.pop()
            stack.append(char)
        return "".join(stack)
```

## 稍难题
### 84. Largest Rectangle in Histogram
维持一个递增stack，碰到一个比栈顶元素小的数，不断比较，更新最大面积

```python
class Solution(object):
    def largestRectangleArea(self, heights):
        """
        :type heights: List[int]
        :rtype: int
        """
        res = 0
        stack = []
        for i in range(len(heights)+1):
            height = heights[i] if i!= len(heights) else 0
            while stack and height <= heights[stack[-1]]:
                h = heights[stack.pop()]
                w = i - stack[-1] -1 if stack else i
                res = max(res, h*w)
            stack.append(i)
        return res
```
### 85. Maximal Rectangle
同样的操作，只是这次是把上一道题的高度，变成矩阵中连续长度

```python
class Solution(object):
    def maximalRectangle(self, matrix):
    	# O(m^2)
        if not matrix or not matrix[0]:
            return 0
        n = len(matrix[0])
        # init heights array 
        height = [0] * (n + 1)
        ans = 0
        # calculate each row
        for row in matrix:
            for i in range(n):
                # count next level '1'
                height[i] = height[i] + 1 if row[i] == '1' else 0
                
            stack = []
            
            for i in range(n + 1):
                while stack and height[i] <= height[stack[-1]]:
                    h = height[stack.pop()]
                    # if not stack means left boundary is zero then width is i else is the stack[-1] index
                    w = i - 1 - stack[-1] if stack else i
                    ans = max(ans, h * w)
                stack.append(i)
        return ans
```