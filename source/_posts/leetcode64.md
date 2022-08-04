---
title: 力扣 64. 最小路径和
date: 2022/8/1 上午10:02
tags: [leetcode, 动态规划]
categories: [leetcode]
---

# 动态规划 + 二维数组

```python
def minPathSum(grid):
    m, n = len(grid), len(grid[0])
    dp = [[0] * n for _ in range(m)]
    for i in range(m):
        for j in range(n):
            if i == 0 and j == 0:
                dp[i][j] = grid[i][j]
            elif i == 0:
                dp[i][j] = grid[i][j] + dp[i][j - 1]
            elif j == 0:
                dp[i][j] = grid[i][j] + dp[i - 1][j]
            else:
                dp[i][j] = grid[i][j] + min(dp[i][j - 1], dp[i - 1][j])
    
    return dp[-1][-1]
```


# 动态规划 + 一维数组

```python
def minPathSum(grid):
    m, n = len(grid), len(grid[0])
    dp = [0] * n 
    for i in range(m):
        for j in range(n):
            if i == 0 and j == 0:
                dp[j] = grid[i][j]
            elif i == 0:
                dp[j] = grid[i][j] + dp[j - 1]
            elif j == 0:
                dp[j] = grid[i][j] + dp[j]
            else:
                dp[j] = grid[i][j] + min(dp[j - 1], dp[j])
    
    return dp[-1]
```