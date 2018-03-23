---
title: Backtracking总结
comments: true
date: 2017-09-06 23:31:24
updated: 2018-03-22 14:31:24
categories: Leetcode
tags: [Backtracking, DFS]
---

# BackingTracking系列
4／35  
[x] Easy  
[x] Medium  
[] Hard  

# Tips
凡是含有duplicate的都需要之前sorted，才能保证没有结果中没有重复
## 经典7道题
### 46. Permutations  

```python
1-2-3
 -3-2
def backtracking(self, nums, temp, ans):
    if len(nums) == len(temp): # quit loop
        ans.append(list(temp))
    for i in range(len(nums)):
        if nums[i] in temp: # cut duplicate
            continue
        temp.append(nums[i])
        self.backtracking(nums, temp, ans)
        temp.pop()
```

### 47. Permutations II  
由于输入可能包含重复数字，所以就要保证去重。先排序然后创建Array记录访问过的数字，然后前面的一个数是否和自己相等，相等的时候则前面的数必须使用了，自己才能使用，这样就不会产生重复的排列了

```python
def backtracking(self, nums, temp, ans, used):
    if len(temp) == len(nums):
        ans.append(list(temp))
    for i in range(len(nums)):
        if used[i] or i>0 and nums[i]==nums[i-1] and not used[i-1]: # 判断条件
            continue
        temp.append(nums[i])
        used[i] = True # 记录访问
        self.backtracking(nums, temp, ans, used)
        used[i] = False
        temp.pop()
```

### 78. Subsets  
终止条件不同，因为要返回每一个set，所以每次backtracking的时候都要返回tempList；然后保证唯一性就是backtrack的时候index+1

```python
def backtracking(self, nums, temp, res, start):
    res.append(list(temp))
    for i in range(start,len(nums)):
        temp.append(nums[i])
        self.backtracking(nums, temp, res, i+1)
        temp.pop()
```

### 90. Subsets II  
因为input含有duplicate，所以在进入backtracking之前需要检查

```python
def backtracking(self, nums, temp, res, start):
    res.append(list(temp))
    for i in range(start,len(nums)):
        if i > start and nums[i] == nums[i-1]:
            continue
        temp.append(nums[i])
        self.backtracking(nums, temp, res, i+1)
        temp.pop()
```

### 39 Combination Sum  
本质上是一样的，每次传的时候target-candidates[i], 然后因为每个数字可以重复使用，所以index可以保持不变

```python
def backtracking(self, candidates, target, res, temp, start):
    if target<0:
        return
    elif target == 0:
        res.append(list(temp))
    else:
        for i in range(start, len(candidates)):
            temp.append(candidates[i])
            self.backtracking(candidates, target - candidates[i], res, temp, i)
            temp.pop()
```

### 40 Combination Sum II  
变化就是不可以重复利用数字，index+1

```python
def backtracking(self, candidates, target, res, temp, start):
    if target<0:
        return
    elif target == 0:
        res.append(list(temp))
    else:
        for i in range(start, len(candidates)):
            if i > start and candidates[i-1] == candidates[i]:
                continue
            temp.append(candidates[i])
            self.backtracking(candidates, target - candidates[i], res, temp, i+1)
            temp.pop()
```

### 216 Combination Sum III
与上一道题的区别就是，输入为[1...9] 

```python
class Solution(object):
    def combinationSum3(self, k, n):
        """
        :type k: int
        :type n: int
        :rtype: List[List[int]]
        """
        res = []
        self.backtracking(k,n,[],res,1)
        return res
    
    def backtracking(self, k, target, temp, res, start):
        if len(temp) > k:
            return
        elif len(temp) == k and target == 0:
            res.append(list(temp))
        else:
            for i in range(start, 10):
                temp.append(i)
                self.backtracking(k, target - i, temp, res, i+1)
                temp.pop()
        
        
```

## Medium
### 22 Generate Parentheses  
recursion rule 就是判断left，right的count啦，直到满足right == n  

```python
def dfs(self,temp, left, right, res,n):
    if left < n:
        self.dfs(temp+'(',left+1,right,res,n)
    if right < left:
        self.dfs(temp+')',left, right+1, res,n)
    if right==n:
        res.append(temp)
```

### 320. Generalized Abbreviation  
这道题debug了好久，困惑于如何使得数字和字母不会在base case的时候重复导出

```
4
3d
2r1 2rd
...
```

```python
def backtrack(res, word, pos, string, count):
    if pos == len(word):
        if count>0:
            string += str(count)
        res.append(string)
    else:
        backtrack(res,word,pos+1,string,count+1)
        ## 这个track保证了index每次不断加1 从而在base的时候输出，然后每一次count同时加1，为了记录count
        backtrack(res, word, pos+1, string + (str(count) if count>0 else "")+ word[pos], 0)
        ## 这个是为了退一步，先保存当前的count数字，然后因为数字不能连续，所以+word【index】，同时把count清0
```

### 二维backtracking