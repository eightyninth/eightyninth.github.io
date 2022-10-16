---
title: 力扣 78. 子集
date: 2022/8/3 上午10:13
tags: [leetcode, 回溯, dfs]
categories: [leetcode]
---

# 回溯思想 + dfs实现

```python
def subsets(nums):
    def dfs(i, tmp):
        if i >= n_len:
            return

        tmp.append(nums[i])
        res.append(tmp)

        for j in range(i + 1, n_len):
            dfs(j, tmp.copy())

        
    n_len = len(nums)
    res = [[]]
    for i in range(n_len):
        dfs(i, [])
    
    return res
```