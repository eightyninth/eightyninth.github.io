---
title: 力扣 70. 爬楼梯
date: 2022/8/1 上午10:20
tags: [leetcode, 动态规划]
categories: [leetcode]
---

# 动态规划 + 一维数组

```python
def climbStairs(n):
    if n == 1:
        return 1
    elif n == 2:
        return 2
    dp = [0] * n
    dp[0], dp[1] = 1, 2
    for i in range(2, n):
        dp[i] = dp[i - 1] + dp[i - 2]
    
    return dp[-1]
```

# 动态规划 + 两个数

```python
def climbStairs(n):
    if n == 1:
        return 1
    elif n == 2:
        return 2
    a, b = 1, 2
    for i in range(2, n):
        a, b = b, a + b
    
    return b
```