---
title: Google面经不断更新
comments: true
date: 2017-07-08 16:18:45
updated: 2017-07-08 16:18:45
categories: Leetcode
tags: [String, BFS, DFS]
---

## 把之前准备的Google面经总结一下
### 持续更新
.281 Zigzag Iterator  
用全局变量index记录call next位置，pos记录append位置

```python
while pos < max(len(v1), len(v2)):
    if pos < len(v1):
        self.res.append(v1[pos])
    if pos < len(v2):
        self.res.append(v2[pos])
    pos += 1
```

.298 Binary Tree Longest Consecutive Sequence  
用BFS（stack添加的时候记录node 和 value）， DFS记录`dfs(self,root, cur, target)`

.163 Missing Range  
注意最后一个需要curr = upper + 1， 然后其他的情况curr = nums[i] ，getRange（pre+1， cur-1）

·66 Plus one  
需要注意当进位最后仍为1的时候，digits.insert(0,1)

`346 Moving Average from Data Stream  
用queue来做

`425 Valid WordSquare  
每行每列的长度需要保持一致，对应的字符一样words[i][j] == words[j][i]