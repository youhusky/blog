---
title: Longest Substring系列
comments: true
date: 2017-07-07 15:40:57
updated: 2018-04-23 15:40:57
categories: Leetcode	
tags: [String, SlidingWindow]
---
## 先从最基础的开始
### 3. Longest Substring Without Repeating Characters  
这道题就是使用一个dict来维护字符出现的位置，一旦发现新字符出现在字典里并且start的位置<= 记录位置（就是连续同样字符保留最后一个） start更新为上个出现该字符的index+1，类似滑动窗口，一旦发现重复元素就去把上一次的元素位置+1

```python
class Solution(object):
    def lengthOfLongestSubstring(self, s):
        """
        :type s: str
        :rtype: int
        """
        dic = {}
        res = 0
        start = 0
        for i in range(len(s)):
            if s[i] in dic and start <= dic[s[i]]:
                start = dic[s[i]] + 1
            else:
                res = max(res, i - start + 1)
            dic[s[i]] = i
        return res
        
```

### 159. Longest Substring with At Most Two Distinct Characters.  
### 340. Longest Substring with At Most K Distinct Characters.  
类似的思路，用字典来保存出现次数，用字典的长度维护K值

```python
class Solution(object):
    def lengthOfLongestSubstringTwoDistinct(self, s):
        """
        :type s: str
        :rtype: int
        """
        char_dict = {}
        start = 0
        res = 0
        
        for i in range(len(s)):
            if s[i] not in char_dict:
                char_dict[s[i]] = 1
            else:
                char_dict[s[i]] += 1

            while len(char_dict)>2:
                temp = s[start]
                if char_dict[temp] > 1:
                    char_dict[temp] -= 1
                else:
                    del(char_dict[temp])
                start += 1
            res = max(res, i -start + 1)
        return res
```
<!--more-->
## 另外的形式
### 395. Longest Substring with At Least K Repeating Characters  
At least就表明至少有那么多，用字典就不太好使了，因为要不断考虑到之前的情况，倒不如退而求其次，divide and conquer 找到最不可能的字符，然后知道里面的字符至少出现过k次

```python
if len(s) < k:
    return 0
c = min(set(s), key=s.count) ## 按照count排序
if s.count(c) >= k:
    return len(s) ## 都满足
return max(self.longestSubstring(t, k) for t in s.split(c))
```

### 424. Longest Repeating Character Replacement
比较类似 340那道题，同样用字典记录字符出现次数，然后用子序列中出现频率最大的次数加上能被修改的次数K 和窗口长度相比（也就是说窗口中都能统一）

```python
char_dict[value] += 1
res = max(res, char_dict[value])
if res + k <= index - start :
    char_dict[s[start]] -= 1
    start += 1
```