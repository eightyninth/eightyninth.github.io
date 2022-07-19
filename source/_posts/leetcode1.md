---
title: 力扣 1. 两数之和
date: 2022-07-02 21:22:45
tags: [leetcode, 算法]
categories: [leetcode]
---

# 使用快速排序 + 原数组index记录 + 双指针

1. 快速排序 + 原数组index记录

```python
def quick_sort(left, right):
    if left >= right: return
    i, j = left, right
    while i < j:
        while nums[j] > nums[left] and i < j: j -= 1
        while nums[i] <= nums[left] and i < j: i += 1
        nums[i], nums[j] = nums[j], nums[i]
        idx[i], idx[j] = idx[j], idx[i]
    nums[left], nums[i] = nums[i], nums[left]
    idx[i], idx[left] = idx[left], idx[i]

    quick_sort(left, i - 1)
    quick_sort(i + 1, right)
```

2. 双指针

```python
i, j = 0, len(nums) - 1
while 0 <= i < j < len(nums):
    cur = nums[i] + nums[j]
    if cur > target:
        j -= 1
    elif cur < target:
        i += 1
    else:
        break
```


# 官方解法: hash表

```python
in_ = {}
for i in range(len(nums)):
    if target - nums[i] in in_:
        return [in_[target - nums[i]], i]
    else:
        in_[nums[i]] = i
```