---
title: 力扣 75. 颜色分类
date: 2022/8/2 上午9:27
tags: [leetcode, 双指针, 快排]
categories: [leetcode]
---

# 双指针 + 快排

```python
def sortColors(nums):
    """
    Do not return anything, modify nums in-place instead.
    """
    # 双指针 + 快排
    def quick_sort(left, right):
        if left >= right: return
        i, j = left, right
        while i < j:
            while i < j and nums[left] < nums[j]: j -= 1
            while i < j and nums[left] >= nums[i]: i += 1
            nums[i], nums[j] = nums[j], nums[i]
        nums[left], nums[i] = nums[i], nums[left]
        quick_sort(left, i - 1)
        quick_sort(i + 1, right)
    
    quick_sort(0, len(nums) - 1)
```

# 单指针

```python
def sortColors(nums):
    """
    Do not return anything, modify nums in-place instead.
    """
    def recover(n_len, p, target):
        for i in range(n_len):
            if nums[i] == target:
                nums[i], nums[p] = nums[p], nums[i]
                p += 1
        return p
    # 单指针遍历
    n_len, point = len(nums), 0
    # 复位0
    point = recover(n_len, point, 0)
    # 复位1
    point = recover(n_len, point, 1)
```

# 双指针1

```python
def sortColors(nums):
    """
    Do not return anything, modify nums in-place instead.
    """
    # 双指针遍历
    p0, p2, i = 0, len(nums) - 1, 0
    while i <= p2:
        if nums[i] == 0:
            nums[i], nums[p0] = nums[p0], nums[i]
            p0 += 1
        elif nums[i] == 2:
            nums[i], nums[p2] = nums[p2], nums[i]
            p2 -= 1
            if nums[i] != 1:
                i -= 1
        i += 1
```

# 双指针2

```python
def sortColors(nums):
    """
    Do not return anything, modify nums in-place instead.
    """
    # 双指针遍历
    p0, p1 = 0, 0
    for i in range(len(nums)):
        if nums[i] == 1:
            nums[i], nums[p1] = nums[p1], nums[i]
            p1 += 1
        elif nums[i] == 0:
            nums[i], nums[p0] = nums[p0], nums[i]
            if p0 < p1:
                nums[i], nums[p1] = nums[p1], nums[i]
            p0 += 1
            p1 += 1
```