---
title: DP总结
comments: true
date: 2017-10-19 15:56:32
updated: 2017-10-19 15:56:32
categories: Leetcode	
tags: DP
---
# 概念
动态规划是通过拆分问题，定义问题状态和状态之间的关系，使得问题能够以递推（或者说分治）的方式去解决。  
DP问题是Leetcode中的经典问题，也是面试中经常考到的类别之一，没有通用的模版，有些DP题思考的过程也比较繁琐.所以这篇总结可能会不断更新，以便达到更好的效果

## 适用场景
1. 找Max， Min的问题
2. 发现可能性的问题
3. 输出所有解的个数问题

不适用场景  
1. 列出所有具体方案（起码是指数级别的复杂度，通常是递归，backtracking）  
2. 集合问题

## 考虑
1. 状态
2. 转移方程
3. 初始化条件
4. 返回结果
<!--more-->
# 单序列问题
通常是数组，字符串的前N个为...

## Warm Up
### 爬梯子
作为DP的入门题来说，思考过程还是很重要的。 一次可以爬1级或者两级的台阶，问有多少种爬法。  
符合输出所有解个数的问题。  
因为只能爬一级或者两级所以到N级的话，你只能从n-1爬到n或者n-2爬到n；这样说来，如果`dp[n]`代表到n级台阶有多少种可能性的话，转移方程为`dp[n] = dp[n-1]+dp[n-2]`  
所以代码很容易写出  

```python
def climb(n):
	if n == 1:
		return 1
	dp = [1 for _ in range(n+1)]
	dp[1] = 1
	dp[2] = 2
	for i in range(3,n+1):
		dp[i] = dp[i-1]+dp[i-2]
	return dp[-1]
```

当然这道题有优化条件，否则就没有必要花大篇幅写了。通过分析状态转移方程可以发现`dp[i]`只与`dp[i-1],dp[i-2]`有关，说明再之前的状态是不会影响到当前状态的，所以我们可以通过只保留两个状态来不断滚动从而求出最后的结果。

```java
public class Solution {
    public int climbStairs(int n) {
        if (n == 1) {
            return 1;
        }
        int first = 1;
        int second = 2;
        for (int i = 3; i <= n; i++) {
            int third = first + second;
            first = second;
            second = third;
        }
        return second;
    }
}
```
进而我们能看出，如果当前状态只与前面的相关的话，我们都可以通过滚动数组，变量来简化空间复杂度--这种尤其适合不太复杂的动态规划问题，简单的二维DP

### 53. Maximum Subarray
找Max问题  
很容易得出当前局部最大+当前值，和当前值的对比，而从决定是继续加还是从新来过

```python
class Solution(object):
    def maxSubArray(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        if not nums:
            return 0
        if len(nums) == 1:
            return nums[0]
        dp = [float('-inf') for _ in range(len(nums))]
        
        dp[0] = nums[0]
        res = nums[0]
        for each in range(1,len(nums)):
            dp[each] = max(dp[each-1]+nums[each], nums[each])
            res = max(res, dp[each])
            #print dp
        return res
```
从上一段分析可以看出，dp状态只与上一个状态有关，从而可以简化成变量来储存dp[]

```python
class Solution(object):
    def maxSubArray(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        if not nums:
            return 0
        if len(nums) == 1:
            return nums[0]
        prevMax = nums[0]
        res = nums[0]
        for each in range(1,len(nums)):
            prevMax = max(prevMax+nums[each], nums[each])
            res = max(res, prevMax)
            #print dp
        return res
```
### 300. Longest Increasing Subsequence
找出Max  
这道题有点不一样的地方是最后的结果有可能是任意一个位置，所以不是简单的`return dp[-1]`而是`max(dp)`  
`dp[i] = 1 + max(dp[j]) j < i and A[i] > A[j]`

```python
class Solution(object):
    def lengthOfLIS(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        if not nums:
            return 0
        dp = [1] * (len(nums) + 1)
        for i in range(len(nums)):
            for j in range(i):
                if nums[j] < nums[i]:
                    dp[i] = max(dp[i], dp[j]+1)
        return max(dp)
```

### 139.Word Break
寻求解的存在性
和上一题有点像，dp[i] 为当前字符满足之前的字符在字典里

```python
class Solution(object):
    def wordBreak(self, s, wordDict):
        """
        :type s: str
        :type wordDict: List[str]
        :rtype: bool
        """
        if not s:
            return False
        # dp保存dp【i】i之前的最少字符串
        dp = [False] * (len(s) + 1)
        dp[0] = True
        # 本身的循环，对字符串，在内层循环中需要使用i
        for i in range(1, len(s)+1):
            for j in range(i):
                if dp[j] and s[j:i] in wordDict:
                    dp[i] = True # 因为j～i是一个回文字符串
        return dp[-1]
```

### 198. House Robber
`dp[i] = max(dp[i-2]+A[i], dp[i-1])`
当然一共就3个状态，我们也可以通过类似爬梯子的方式，把空间复杂度降为O（1）

```python
class Solution(object):
    def rob(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        if len(nums) == 0:
            return 0
        if len(nums) < 2:
            return nums[0] 
        dp = [0] * len(nums)
        dp[0] = nums[0]
        dp[1] = max(dp[0], nums[1])
        for i in range(2, len(nums)):
            dp[i] = max(dp[i-2] + nums[i], dp[i-1])
        return dp[-1]
        
```

### 303. Range Sum Query - Immutable
简单的累加求和做为DP，则转移方程为`res(x,y) = dp[y] - dp[x-1]`

```python
class NumArray(object):

    def __init__(self, nums):
        """
        :type nums: List[int]
        """
        total = 0
        self.dp = []    
        for i in nums:
            total += i
            self.dp.append(total)
        

    def sumRange(self, i, j):
        """
        :type i: int
        :type j: int
        :rtype: int
        """
        if i == 0:
            return self.dp[j]
        return self.dp[j] - self.dp[i-1]
        


# Your NumArray object will be instantiated and called as such:
# obj = NumArray(nums)
# param_1 = obj.sumRange(i,j)
```
## Medium
### 368. Largest Divisible Subset
这道题需要一点思考，除了创建一个DP数组来记录到i位置最长的长度之外，我们还要知道其对应能整除的数字，所以还需要一个array来记录上一个数字的位置。dp的默认值为1， pre的初始值为None就是没有对应的数字；当且仅当dp需要更新的时候，更新其上一个能整除数字的index。最后找出max（dp）所对应的数字，根据其pre的index逐个找数字，输出。

```python
class Solution(object):
    def largestDivisibleSubset(self, nums):
        """
        :type nums: List[int]
        :rtype: List[int]
        """
        if not nums:
            return []
        nums = sorted(nums)
        size = len(nums)
        dp = [1] * size
        pre = [None] * size
        for x in range(size):
            for y in range(x):
                if nums[x] % nums[y] == 0 and dp[y] + 1 > dp[x]:
                    dp[x] = dp[y] + 1
                    pre[x] = y
        idx = dp.index(max(dp))
        ans = []
        while idx is not None:
            ans += nums[idx],
            idx = pre[idx]
        return ans
```
### 338. Counting Bits
需要熟悉Bit运算和概念，要能发现countbit(n) = countbit(n/2) + n % 2这么一个方程，就是说一个数乘2意味着bit位左移一位

```python
class Solution(object):
    def countBits(self, num):
        """
        :type num: int
        :rtype: List[int]
        """
        dp = [0 for _ in range(num+1)]
        for i in range(num+1):
            dp[i] = dp[i>>1] + i%2
        return dp
```

### 264. Ugly Number II
用三个dp存2，3，5出现作为乘子的个数

```python
class Solution(object):
    def nthUglyNumber(self, n):
        """
        :type n: int
        :rtype: int
        """
        res = [1]
        i2, i3, i5 = 0,0,0
        while n:
            u2, u3, u5 = 2 *res[i2], 3*res[i3], 5*res[i5]
            temp = min(u2,u3,u5)
            if temp == u2:
                i2 += 1
            if temp == u3:
                i3 += 1
            if temp == u5:
                i5 += 1
            res.append(temp)
            n -= 1
        return res[-2]
```


### 673. Number of Longest Increasing Subsequence
相关题目，需要额外数组来记录已经出现最长的次数，也就是说如果前面有多个长度相等的连续子串的话，cnt要一直+1

```python
class Solution(object):
    def findNumberOfLIS(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        if not nums:
            return 0
        dp = [1 for _ in range(len(nums))]
        dc = [1 for _ in range(len(nums))]
        for i in range(len(nums)):
            for j in range(i):
                
                if nums[i] > nums[j]:
                    if dp[i] < dp[j] + 1:
                        dp[i] = dp[j] + 1
                        dc[i] = dc[j]
                    elif dp[i] == dp[j] + 1:
                        dc[i] += dc[j] # we have multiple same length LIS, we need to add them
        res = 0
        
        for index, value in enumerate(dp):
            if value == max(dp):
                res += dc[index]
        return res
                    
```

### 309. Best Time to Buy and Sell Stock with Cooldown
这道题一开始确实想不出来，感觉情况太多了；我承认有些DP题就是想不出来怎么做.....后来看到Discuss上有一个解法很不错，就是我们就考虑buy和sell的情况，buy的情况是最大利益只和前一个状态有关或者前两个状态的时候卖，然后这时候买。所以DP的定义就是buy[i] 在i的时候和在i之前买的最大值   
`buy[i] = Math.max(buy[i - 1], sell[i - 2] - prices[i]); `  
同理，sell的时候也和之前状态有关  
`sell[i] = Math.max(sell[i - 1], buy[i - 1] + prices[i]);`

```python
class Solution(object):
    def maxProfit(self, prices):
        """
        :type prices: List[int]
        :rtype: int
        """
        if not prices or len(prices) <= 1:
            return 0
        # buy[i] = max(buy[i-1], sell[i-2] - prices[i])
        # sell[i] = max(sell[i-1], buy[i-1] + prices[i])
        
        b0 = -prices[0]
        b1 = b0
        s0 = 0
        s1 = 0
        s2 = 0
        
        for i in range(1,len(prices)):
            b1 = max(b0, s0 - prices[i])
            s2 = max(s1, b0 + prices[i])
            
            b0 = b1
            s0 = s1
            s1 = s2
        return s2
```


# 矩阵DP
这种问题需要初始化DP数组，第0行和第0列，这样会方便之后的操作，通常这种问题是只能向右或者向下操作，否则则需要用BFS--求最短路径；DFS来解决
## Warm Up
### 62. Unique Paths
符合求解的个数问题  
`dp[i][j] = dp[i-1][j] + dp[i][j-1]`

```python
class Solution(object):
    def uniquePaths(self, m, n):
        """
        :type m: int
        :type n: int
        :rtype: int
        """
        if not m or not n:
            return 0
        dp = [[1 for _ in range(n)] for _ in range(m)]
        for i in range(1,m):
            for j in range(1,n):
                dp[i][j] = dp[i-1][j] + dp[i][j-1]
        return dp[-1][-1]
        
```
当然我们知道每一个dp状态只与上方或者左方的状态相关，所以可以考虑通过某种方式来保存状态；一种方法是建立一个额外数组来存储列状态，dp变成一维存行状态；不过更进一步的话，每一个dp[i]状态就意味着当前从i行过来的状态总数，这里还是贴图吧，更好理解![img_6600](https://user-images.githubusercontent.com/10191895/31799214-a436210a-b4ed-11e7-8f79-5c0c1cf489d9.JPG)

```python
class Solution(object):
    def uniquePaths(self, m, n):
        """
        :type m: int
        :type n: int
        :rtype: int
        """
        dp = [1 for _ in range(m)]
        for i in range(1,n):
            for j in range(1,m):
                dp[j] += dp[j-1] 
        return dp[-1]
        
```

### 63.Unique Paths II
初始化首行首列的时候如果有障碍的话，就都变成0了

```python
class Solution(object):
    def uniquePathsWithObstacles(self, obstacleGrid):
        """
        :type obstacleGrid: List[List[int]]
        :rtype: int
        """
        m = len(obstacleGrid)
        n = len(obstacleGrid[0])
        dp = [[0] * n] * m
        for i in range(m):
            for j in range(n):
                if obstacleGrid[i][j] == 1:
                    dp[i][j] = 0
                elif i == 0 and j == 0:
                    dp[i][j] = 1
                elif i == 0:
                    dp[i][j] = dp[i][j-1]
                elif j == 0:
                    dp[i][j] = dp[i-1][j]
                else:
                    dp[i][j] = dp[i-1][j] + dp[i][j-1]
        return dp[-1][-1]
                
```

### 64. Minimum Path Sum

```python
class Solution(object):
    def minPathSum(self, grid):
        """
        :type grid: List[List[int]]
        :rtype: int
        """
        m = len(grid)
        n = len(grid[0])
        dp = [[0 for _ in range(n)] for _ in range(m)]
        dp[0][0] = grid[0][0]
        for i in range(1,m):
            dp[i][0] = dp[i-1][0] + grid[i][0]
        for j in range(1,n):
            dp[0][j] = dp[0][j-1] + grid[0][j]
        for i in range(1,m):
            for j in range(1,n):
                dp[i][j] = min(dp[i-1][j], dp[i][j-1]) + grid[i][j]
        #print dp
        return dp[-1][-1]
        
```
题目变成了正方形，逐层扩展的时候要记录，每一列计算之前要加上第0行第j列的值，所以沿用上一题的图，dp[i] = dp[i]--上方的值+ dp[i-1] -- 左方的值



```python
class Solution(object):
    def minPathSum(self, grid):
        """
        :type grid: List[List[int]]
        :rtype: int
        """
        m = len(grid)
        n = len(grid[0])
        dp = [0 for _ in range(m)]
        dp[0] = grid[0][0]
        for i in range(1,m):
            dp[i] = dp[i-1] + grid[i][0]
        
        for j in range(1,n):
            dp[0] = dp[0] + grid[0][j]
            for i in range(1,m):
                dp[i] = min(dp[i], dp[i-1]) + grid[i][j]
        #print dp
        return dp[-1]
```
### 256. Paint House
由于每次喷涂的房子颜色不能与之前的相同，所以转移方程为`dp[i][2] += min(dp[i-1][0], dp[i-1][1])` dp为costs

```python
class Solution(object):
    def minCost(self, costs):
        """
        :type costs: List[List[int]]
        :rtype: int
        """
        if not costs:
            return 0
        length = len(costs)
        for i in range(1, length):
            costs[i][0] += min(costs[i-1][1],costs[i-1][2])
            costs[i][1] += min(costs[i-1][0],costs[i-1][2])
            costs[i][2] += min(costs[i-1][0],costs[i-1][1])
        return min(costs[length-1][0], costs[length-1][1], costs[length-1][2])
```

### 276. Paint Fence
这道题其实跟上一道题很像，但是每一次涂得颜色可以和上一个一样（连续的颜色最多出现两次），所以当前状态与前两个有关，可以和前两个状态其中任何一个颜色一样 `dp[2] = (k-1) * (dp[0] + dp[1]), dp = [k, k*k, 0]`

```python
class Solution(object):
    def numWays(self, n, k):
        """
        :type n: int
        :type k: int
        :rtype: int
        """
        if n == 0: return 0
        if n == 1: return k

        # for the first 2 posts
        dp = [k, k*k, 0]
        

        for i in range(2, n):
            dp[2] = (k-1) * (dp[0] + dp[1])
            dp[0] = dp[1]
            dp[1] = dp[2]
        return dp[1]
```
### 174. Dungeon Game
最主要的区别是，要从后往前找，由于生命最少为1，所以DP的条件也要相应变一下

```python
class Solution(object):
    def calculateMinimumHP(self, dungeon):
        """
        :type dungeon: List[List[int]]
        :rtype: int
        """
        m = len(dungeon)
        n = len(dungeon[0])
        dp = [[0 for _ in range(n)] for _ in range(m)]
       
        for i in range(m-1,-1,-1):
            for j in range(n-1,-1,-1):
                if i == m-1 and j == n-1:
                    dp[i][j] = max(1, 1 - dungeon[i][j])
                elif i == m-1:
                    dp[i][j] = max(1, dp[i][j+1] - dungeon[i][j])
                elif j == n-1:
                    dp[i][j] = max(1, dp[i+1][j] - dungeon[i][j])
                else:
                    dp[i][j] = max(1, min(dp[i+1][j], dp[i][j+1]) - dungeon[i][j])
        return dp[0][0]
        
```
## Medium
### 651. 4 Keys Keyboard
这道题想了半天，发现最后还是举例子最好；在6个操作内，只有A的操作是最大的，之后的话dp[i] = dp[j] * (i-j-1), 比如i==7， j==1的时候，最后是A * 7-1-1 = 5A 

```python
class Solution(object):
    def maxA(self, N):
        """
        :type N: int
        :rtype: int
        """
        if N <= 6:
            return N
        dp = [i for i in range(N+1)]
        for i in range(7,N+1):
            for j in range(1,i-2):
                dp[i] = max(dp[i], dp[j] * (i-j-1))
        return dp[-1]
        
```

### 304. Range Sum Query 2D - Immutable
这道题最关键的是处理corner case

```python
class NumMatrix(object):

    def __init__(self, matrix):
        """
        :type matrix: List[List[int]]
        """
        if not matrix:
            return
        m = len(matrix)
        n = len(matrix[0])
        # deal with zero row and column
        self.dp = [[0 for _ in range(n+1)] for _ in range(m+1)]
        for i in range(1, m+1):
            for j in range(1, n+1):
                self.dp[i][j] = matrix[i-1][j-1] + self.dp[i-1][j] + self.dp[i][j-1] - self.dp[i-1][j-1]
        

    def sumRegion(self, row1, col1, row2, col2):
        """
        :type row1: int
        :type col1: int
        :type row2: int
        :type col2: int
        :rtype: int
        """
        return self.dp[row2+1][col2+1] - self.dp[row2+1][col1]- self.dp[row1][col2+1] + self.dp[row1][col1]
        


# Your NumMatrix object will be instantiated and called as such:
# obj = NumMatrix(matrix)
# param_1 = obj.sumRegion(row1,col1,row2,col2)
```

### 221. Maximal Square
可行的解法是很巧妙的：以这个square的最右下角的位置作为存储点f(i, j),当matrix(i, j)是1的时候，f(i, j) = min{f(i - 1, j - 1), f(i - 1, j), f(i, j -1)} + 1. 这是因为如果这是一个square，那么构成这个square的最基本条件就是跟它相邻的边的最小所在square.所以一个square的f值如下：

```python
class Solution(object):
    def maximalSquare(self, matrix):
        """
        :type matrix: List[List[str]]
        :rtype: int
        """
        if not matrix:
            return 0
        res = 0
        m = len(matrix)
        n = len(matrix[0])
        dp = [[0 for _ in range(n+1)] for _ in range(m+1)]
        for i in range(1,m+1):
            for j in range(1, n+1):
                if matrix[i-1][j-1] == '1':
                   
                    dp[i][j] = min(dp[i-1][j-1], dp[i-1][j], dp[i][j-1]) + 1
                    res = max(res, dp[i][j])
        
        return res * res
```


## 二维DP
### 72. Edit Distance
这道题是一道经典问题，分为之下三个操作，我觉得以下的解释是最好的   
a) 插入一个字符：word1[0:i] -> word2[0:j-1]，然后在word1[0:i]后插入word2[j]
DP[i+1][j+1] = DP[i+1][j]+1

b) 删除一个字符：word1[0:i-1] -> word2[0:j]，然后删除word1[i]
DP[i+1][j+1] = DP[i][j+1]+1

c) 替换一个字符：word1[0:i-1] -> word2[0:j-1]
word1[i] != word2[j]时，word1[i] -> word2[j]：DP[i+1][j+1] = DP[i][j] + 1
word1[i] == word2[j]时：DP[i+1][j+1] = DP[i][j] 

```python
class Solution(object):
    def minDistance(self, word1, word2):
        """
        :type word1: str
        :type word2: str
        :rtype: int
        """
        l1 = len(word1)+1
        l2 = len(word2) + 1
        dp = [[0 for _ in range(l2)] for _ in range(l1)]
        
        for i in range(l1):
            dp[i][0] = i
        for j in range(l2):
            dp[0][j] = j
        for i in range(1,l1):
            for j in range(1,l2):
                if word1[i-1] == word2[j-1]:
                    dp[i][j] = dp[i-1][j-1]
                else:
                    dp[i][j] = min(dp[i][j-1],dp[i-1][j], dp[i-1][j-1]) + 1
        #print dp
        return dp[-1][-1]
```