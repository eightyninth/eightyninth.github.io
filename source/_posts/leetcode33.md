---
title: 力扣 33. 搜索旋转排序树组
date: 2022/7/26 下午10:04
tags: [leetcode, 二分查找]
categories: [leetcode]
---

# 二分查找

```python
def search(nums, target):
    if not nums: return -1
    # 数组是局部有序的
    left, right = 0, len(nums) - 1
    while left <= right:
        mid = left + right >> 1
        if nums[mid] == target:
            return mid
        # [l, mid - 1] 有序
        if nums[0] <= nums[mid]:
            if nums[left] <= target < nums[mid]:
                right = mid - 1
            else:
                left = mid + 1
        # [mid + 1, right] 有序
        else:
            if nums[mid] < target <= nums[right]:
                left = mid + 1
            else:
                right = mid - 1
    return -1
```