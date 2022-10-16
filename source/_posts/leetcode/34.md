---
title: 力扣 34. 在排序数组中查找元素的第一个和最后一个位置
date: 2022/7/27 上午10:27
tags: [leetcode, 二分查找]
categories: [leetcode]
---

# 左右二分查找

```python
def searchRange(nums, target):
    # 左右二分
    def search_left(l, r, t):
        while l < r:
            m = l + r >> 1
            if nums[m] < t:
                l = m + 1
            else:
                r = m
        return l

    def search_right(l, r, t): 
        while l < r:
            m = l + r + 1 >> 1
            if nums[m] > t:
                r = m - 1
            else:
                l = m
        return l
    if not nums: return [-1, -1]
    nums_len = len(nums) - 1
    left = search_left(0, nums_len, target)
    right = search_right(0, nums_len, target)

    return [left, right] if nums[left] == target and nums[right] == target else [-1, -1]
```