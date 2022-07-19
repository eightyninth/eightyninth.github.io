---
title: 力扣 15. 三数之和
date: 2022-07-02 21:22:45
tags: [leetcode, 算法]
categories: [leetcode]
---

# 快排 + 双指针

1. 快排

```python
def quick_sort(left, right):
    if left >= right: return
    i, j = left, right

    while i < j:
        while nums[j] > nums[left] and i < j:
            j -= 1
        while nums[i] <= nums[left] and i < j:
            i += 1
        
        nums[i], nums[j] = nums[j], nums[i]
    nums[i], nums[left] = nums[left], nums[i]

    quick_sort(left, i - 1)
    quick_sort(i + 1, right)
```

2. 双指针(三指针)

```python
res = []
for k in range(0, num_len - 2):
    if nums[k] > 0:
        break
    if k > 0 and nums[k] == nums[k - 1]: continue
    i, j = k + 1, num_len - 1
    while i < j:
        cur = nums[k] + nums[i] + nums[j]
        if cur > 0:
            j -= 1
            while i < j and nums[j] == nums[j + 1]: j -= 1
        elif cur < 0:
            i += 1
            while i < j and nums[i] == nums[i - 1]: i += 1
        else:
            res.append([nums[k], nums[i], nums[j]])
            i += 1
            j -= 1
            while i < j and nums[i] == nums[i - 1]: i += 1
            while i < j and nums[j] == nums[j + 1]: j -= 1

```


