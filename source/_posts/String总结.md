---
title: String总结
comments: true
date: 2017-10-24 21:53:46
updated: 2017-10-24 21:56:46
categories: Leetcode
tags: String
---
### 3. Longest Substring Without Repeating Characters
这道题就是用一个Dict来统计字符所出现的index，然后不断计算不重复字符串的长度

```python
class Solution(object):
    def lengthOfLongestSubstring(self, s):
        """
        :type s: str
        :rtype: int
        """
        useddict = {}
        maxnum,start = 0,0
        for i in range(len(s)):
            if s[i] in useddict and start <= useddict[s[i]]:
                start = useddict[s[i]] + 1
            else:
                maxnum = max(maxnum, i - start + 1)
                
            useddict[s[i]]= i
        return maxnum
```

### 76. Minimum Window Substring
这题和之前的非常像，就是需要处理的边界条件很多

```python
from collections import defaultdict
class Solution(object):
    def minWindow(self, s, t):
        """
        :type s: str
        :type t: str
        :rtype: str
        """
        if not s:
            return ""
        if not t:
            return ""
        useddict = defaultdict(int)
        for char in t:
            useddict[char] += 1
        length = len(t)
        minnum,start,end = len(s)+1,0,0
        head = 0
        for i in range(len(s)):
            if s[i] in useddict:
                useddict[s[i]] -= 1
                if useddict[s[i]]>=0: 
                    length -= 1
                while length==0:
                    if i - start+1 < minnum:
                        minnum = i-start+1
                        head = start
                    if s[start] in useddict:
                        useddict[s[start]] += 1
                        if useddict[s[start]] > 0: ## more same char was used
                            length += 1
                    start += 1
                
        
        return s[head:head+minnum] if minnum <= len(s) else ""
```