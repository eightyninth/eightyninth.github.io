---
title: 力扣 2090. 半径为k的子数组平均值
date: 2022-07-02 21:22:45
tags: [leetcode, 算法]
categories: [leetcode]
---

# 使用前缀和 + 双指针实现

1. 前缀和

```python
curs = [0]
for n in nums:
    curs.append(curs[-1] + n)
```

2. 双指针

```python
left, right, res, nums_len = -k - 1, k - 1, [], len(nums)
        
for _ in range(nums_len):
    left += 1
    right += 1
    if left >= 0 and right < nums_len:
        cur = curs[right + 1] - curs[left]
        res.append(cur // (2 * k + 1))
    else:
        res.append(-1)
```
