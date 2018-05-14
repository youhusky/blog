---
title: Two Pointer问题总结
comments: true
date: 2017-10-24 21:56:46
updated: 2018-04-24 21:56:46
categories: Leetcode
tags: TwoPointer
---
### 209. Minimum Size Subarray Sum
这道题没有那么多复杂的计算size方法，只是和大于k后，左移一位

```python
class Solution(object):
    def minSubArrayLen(self, s, nums):
        """
        :type s: int
        :type nums: List[int]
        :rtype: int
        """
        # sliding widows
        if not nums:
            return 0
        l, r = 0, 0
        minLength = len(nums)+1
        res = 0
        while r < len(nums):
            res += nums[r]
            
            r += 1
            
            while res >= s:
                minLength = min(minLength, r - l)
                res -= nums[l]
                l += 1
        return 0 if minLength == len(nums)+1 else minLength
```
### 713. Subarray Product Less Than K
这道题甚至是上一道题的简略版本，要求出所有符合条件的。

```python
class Solution(object):
    def numSubarrayProductLessThanK(self, nums, k):
        if k <= 1: return 0
        prod = 1
        ans = left = 0
        for right, val in enumerate(nums):
            prod *= val
            while prod >= k:
                prod /= nums[left]
                left += 1
            ans += right - left + 1
        return ans
```

### 763. Partition Labels
简化版本的windows题

```python
class Solution(object):
    def partitionLabels(self, S):
        last = {c: i for i, c in enumerate(S)}
        j = anchor = 0
        ans = []
        for i, c in enumerate(S):
            # update j like a sliding window
            j = max(j, last[c])
           
            if i == j:
                ans.append(i - anchor + 1)
                anchor = i + 1
            
        return ans
```
<!--more-->
### 424. Longest Repeating Character Replacement
这道题的关键是最多可以替换k个字母，所以维护窗口的size是max出现字母的次数，剩下的都要替换

```python
#from collections import defaultdict
class Solution(object):
    def characterReplacement(self, s, k):
        """
        :type s: str
        :type k: int
        :rtype: int
        """

        count = {}
        max_count = start = result = 0
        for end in range(len(s)):
            count[s[end]] = count.get(s[end], 0) + 1
            max_count = max(max_count, count[s[end]])
            if end - start + 1 - max_count > k:
                count[s[start]] -= 1
                start += 1
            result = max(result, end - start + 1)
        return result
        
```
### 567. Permutation in String
这道题是找Permutation in String，所以窗口size永远是end-start + 1,只要比较两个dict是否相同就可以了

```python
class Solution:
    def checkInclusion(self, s1, s2):
        """
        :type s1: str
        :type s2: str
        :rtype: bool
        """
        # O(n)
        dic1, dic2 = dict(), dict()
        for s in s1:
            dic1[s] = dic1.get(s,0) + 1
        start, end = 0, 0
        
        while end < len(s2):
            dic2[s2[end]] = dic2.get(s2[end],0) + 1
            if dic1 == dic2:
                return True
            
            end += 1
                
            # compare
            if end -start + 1 > len(s1):
                dic2[s2[start]] -= 1
                if dic2[s2[start]] == 0:
                    del(dic2[s2[start]])
                start += 1
        return False
```
### 438. Find All Anagrams in a String

```python
class Solution(object):
    def findAnagrams(self, s, p):
        """
        :type s: str
        :type p: str
        :rtype: List[int]
        """
        dic1, dic2 = dict(), dict()
        for each in p:
            dic1[each] = dic1.get(each,0) + 1
        start, end = 0, 0
        res = []
        
        while end < len(s):
            dic2[s[end]] = dic2.get(s[end],0) + 1
            if dic1 == dic2:
                res.append(start)
            
            end += 1
                
            # compare
            if end -start + 1 > len(p):
                dic2[s[start]] -= 1
                if dic2[s[start]] == 0:
                    del(dic2[s[start]])
                start += 1
        return res

```
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
### 76. Minimum Window Substring
这道题记录出现字母次数，然后知道windows里满足substring的时候再移动`duplicate-- dic value maybe < 0`

```python
class Solution(object):
    def minWindow(self, s, t):
        """
        :type s: str
        :type t: str
        :rtype: str
        """
        # sliding windows
        if not s or not t:
            return ""
        res = ""
        dic = dict()
        # init
        for char in t:
            dic[char] = dic.get(char, 0) + 1
        l,r = 0, 0
        minLength = len(s)
        # windows
        size = len(t)
        
        while r < len(s):
            if s[r] in dic:
                # duplicate-- dic value maybe < 0
                if dic[s[r]] > 0:
                    size -= 1
                    
                dic[s[r]] -= 1
            
            # windos
            r += 1
            
            while size == 0:
                if minLength >= r-l:
                    minLength = r-l
                    res = s[l:r]
                    #t = [l :r]
                    
                # left bound
                if s[l] in dic:
                    dic[s[l]] += 1
                    if dic[s[l]] > 0:
                        size += 1
                l += 1
        return res      
```
