---
title: 力扣 31. 下一个排列
date: 2022/7/21 上午9:46
tags: [leetcode]
categories: [leetcode]
---

# 类似双指针

```python
"""
核心是保证变大的幅度尽可能小
"""
def nextPermutation(nums):
    """
    Do not return anything, modify nums in-place instead.
    """
    if len(nums) <= 1: return
        i = len(nums) - 2
        # 求最靠右的较小数
        while i >= 0 and nums[i] >= nums[i + 1]: i-= 1
        # 求最远离较小数的较大数
        if i>= 0:
            j = len(nums) - 1
            while j > i and nums[j] <= nums[i]: j -= 1
            
            # 交换较大较小数
            nums[i], nums[j] = nums[j], nums[i]
        
        # 保证变大幅度不大的降序反序
        l, r = i + 1, len(nums) - 1
        while l < r:
            nums[l], nums[r] = nums[r], nums[l]
            l += 1
            r -= 1
                
```