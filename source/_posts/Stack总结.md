---
title: Stack总结
comments: true
date: 2017-10-24 21:51:46
updated: 2017-10-27 21:56:46
categories: Leetcode
tags: stack
---
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