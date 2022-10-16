---
title: 力扣 169. 多数元素
date: 2022/8/24 下午4:49
tags: [leetcode, hash表]
categories: [leetcode]
---

# 暴力hash解法

```python
def majorityElement(nums):
    # 假设只有两个数
    save = {}
    for num in nums:
        if num in save.keys():
            save[num] += 1
        else:
            save[num] = 1
    
    save = {v: k for k, v in save.items()}
    return save[max(save.keys())]
```

# 摩尔投票法

```python
def majorityElement(nums):
    # 假设只有两个数
    cand, count = nums[0], 0
    for num in nums:
        if cand == num:
            count += 1
        else:
            count -= 1
            if count == 0:
                cand, count = num, 1
    
    return cand
```