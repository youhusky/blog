---
title: Two Pointer问题总结
comments: true
date: 2017-10-24 21:56:46
updated: 2017-10-24 21:56:46
categories:
tags:
---
### 239. Sliding Window Maximum
这道题是Two Pointer的升级版，我们不仅需要维护一个window size，还要判断最大值出现的位置，以便能节省空间；所以这道题我们用deque来进行解决

```python
from collections import deque
class Solution(object):
    def maxSlidingWindow(self, nums, k):
        """
        :type nums: List[int]
        :type k: int
        :rtype: List[int]
        """
        dq = deque()
        res = []
        for i in range(len(nums)):
            # limit range
            if dq and dq[0] == i-k:
                dq.popleft()
            while dq and nums[i] >= nums[dq[-1]]:
                # no meaning
                dq.pop()
            dq.append(i)
            if i - k + 1 >= 0:
                res.append(nums[dq[0]])
        return res
        
```