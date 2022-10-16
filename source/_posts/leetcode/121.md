---
title: 力扣 121. 买卖股票的最佳时机
date: 2022/8/6 上午11:33
tags: [leetcode, 动态规划]
categories: [leetcode]
---

# 动态规划

```python
def maxProfit(prices):
    p_len = len(prices)
    if p_len <= 1: return 0
    profit, in_ = 0, prices[0]
    for i in range(1, p_len):
        in_ = min(prices[i], in_)
        profit = max(profit, prices[i] - in_)
    return profit
```