---
title: 力扣 96. 不同的二叉搜索树
date: 2022/8/5 上午11:00
tags: [leetcode, 动态规划]
categories: [leetcode]
---

# 动态规划

![动态规划](/images/leetcode/leetcode96.jpeg)

```python
def numTrees(n):
    if n <= 1:
        return 1
    dp = [0] * (n + 1)
    dp[0], dp[1] = 1, 1
    for i in range(2, n + 1):   # 长度
        for j in range(1, i + 1):   # 根位置
            dp[i] += dp[j - 1] * dp[i - j]
    return dp[-1]
```