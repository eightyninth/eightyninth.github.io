---
title: 力扣 11. 盛最多水的容器
date: 2022-07-02 21:22:45
tags: [leetcode, 双指针]
categories: [leetcode]
---

# 枚举: time N/A

```python
def maxArea(height):
    cur = 0
    for i in range(len(height)):
        for j in range(i, len(height)):
            cur = max(cur, (j - i) * min(height[j], height[i]))
    
    return cur
```

# 双指针收缩

```python
def maxArea(height):
    i, j = 0, len(height) - 1
    res = (j - i) * min(height[i], height[j])
    while i < j:
        if height[i] >= height[j]: j -= 1
        else: i += 1
        res = max(res, (j - i) * min(height[i], height[j]))
    
    return res
```

