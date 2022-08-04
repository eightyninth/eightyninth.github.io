---
title: 力扣 62. 不同路径
date: 2022/7/31 下午3:01
tags: [leetcode, 动态规划]
categories: [leetcode]
---

# 动态规划 - 二维数组

```python
def uniquePaths(m, n):
    dp = [[0] * n for _ in range(m)]
    for i in range(m):
        for j in range(n):
            if i == 0 and j == 0:
                dp[i][j] = 1
            elif i == 0: 
                dp[i][j] = dp[i][j - 1]
            elif j == 0:
                dp[i][j] = dp[i - 1][j]
            else:
                dp[i][j] = dp[i - 1][j] + dp[i][j - 1]
    # print(dp)
    return dp[-1][-1]
```

# 动态规划 - 一维数组

```python
def uniquePaths(m, n):
    dp = [0] * n
    for i in range(m):
        for j in range(n):
            if i == 0 and j == 0:
                dp[j] = 1
            elif i == 0: 
                dp[j] = dp[j - 1]
            elif j == 0:
                dp[j] = dp[j]
            else:
                dp[j] = dp[j] + dp[j - 1]
        # print(dp)
    return dp[-1]
```