---
title: 力扣 2089. 找出数组排序后的目标下标
date: 2022-07-02 21:22:45
tags: [leetcode, 算法]
categories: [leetcode]
---

# 排序 + 查找

1. 排序: 快速排序

```python
def sort_b2s(nums, left, right):
    if left >= right: return
    i, j= left, right
    while i < j:
        while nums[j] > nums[left] and i < j: j -= 1
        while nums[i] <= nums[left] and i < j: i += 1
        nums[i], nums[j] = nums[j], nums[i]
    nums[i], nums[left] = nums[left], nums[i]
    sort_b2s(nums, left, i - 1)
    sort_b2s(nums, i + 1, right)
```

2. 查找: 二分查找

```python
def search_left(nums, left, right, x):
    while left < right:
        mid = left + right >> 1
        if nums[mid] >= x:
            right = mid
        else:
            left = mid + 1
    return left


def search_right(nums, left, right, x):
    while left < right:
        mid = left + right + 1 >> 1
        if nums[mid] > x:
            right = mid - 1
        else:
            left = mid
    return left
```