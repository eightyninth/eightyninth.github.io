---
title: 力扣 56. 合并区间
date: 2022/7/31 下午1:15
tags: [leetcode]
categories: [leetcode]
---

# 排序

```python
def merge(intervals):
    ret = []
    intervals = sorted(intervals, key=lambda x: x[0])

    for inter in intervals:
        if not ret or ret[-1][1] < inter[0]:
            ret.append(inter)
        else:
            ret[-1][1] = max(ret[-1][1], inter[1])
    
    return ret
```