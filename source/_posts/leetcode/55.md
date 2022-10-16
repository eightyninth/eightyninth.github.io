---
title: 力扣 55. 跳跃游戏
date: 2022/7/31 上午10:26
tags: [leetcode, 动态规划, 贪心]
categories: [leetcode]
---


# 动态规划

```python
def canJump(nums):
    n_len = len(nums)
    if n_len == 1: return True
    dp = [0] * (n_len - 1) 
    dp[0] = nums[0]
    if dp[0] == 0: return False
    for i in range(1, n_len - 1):
        dp[i] = max(dp[i - 1], i + nums[i])
        if dp[i] == i:
            return False
    # print(dp)
    return dp[-1] >= n_len - 1
```

# 优化动态规划 == 贪心

```python
def canJump(nums):
    n_len = len(nums)
    if n_len == 1: return True
    max_hop = nums[0]
    if max_hop == 0: return False
    for i in range(1, n_len - 1):
        max_hop = max(max_hop, i + nums[i])
        if max_hop == i:
            return False
    # print(dp)
    return max_hop >= n_len - 1
```