---
title: 力扣 53. 最大子数组和
date: 2022/7/30 下午10:03
tags: [leetcode, 动态规划]
categories: [leetcode]
---

# 动态规划

```python
def maxSubArray(nums):
    cur, ret = float("-inf"), float("-inf")
    for n in nums:
        cur = max(n, cur + n)
        ret = max(cur, ret)
    return ret
```