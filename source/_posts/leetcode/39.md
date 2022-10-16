---
title: 力扣 39. 组合总和
date: 2022/7/28 下午7:51
tags: [leetcode, 回溯, dfs]
categories: [leetcode]
---


# 回溯思想, dfs实现


```python
def combinationSum(candidates, target):
    if not candidates: return []
    res, cand_len = [], len(candidates)
    def dfs(target, combins, idx, begin):
        target -= candidates[idx]
        combins.append(candidates[idx])
        if target <= 0:
            if target == 0:
                res.append(combins)
            return
        for i in range(begin, cand_len):
            dfs(target, combins.copy(), i, i)
    
    for i in range(cand_len):
        dfs(target, [], i, i)

    # print(res)
    return res
```