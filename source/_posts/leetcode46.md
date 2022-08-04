---
title: 力扣 46. 全排列
date: 2022/7/30 上午11:51
tags: [leetcode, 递归]
categories: [leetcode]
---

# 递归

```python
def permute(nums):
    def add(ret, num):
        tmp = []
        for r in ret:
            for i in range(len(r) + 1):
                tmp.append(r[:i] + [num] + r[i:])
        return tmp

    if not nums:
        return []
    ret = [[nums[0]]]
    for i in range(1, len(nums)):
        ret = add(ret, nums[i])
    
    return ret
```