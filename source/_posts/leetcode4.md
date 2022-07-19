---
title: 力扣 4. 寻找两个正序数组的中位数
date: 2022-07-02 21:22:45
tags: [leetcode, 算法]
categories: [leetcode]
---

# 双指针

1. 双指针

```python
len_1, len_2 = len(nums1), len(nums2)
len_1a2 = len_1 + len_2
mid = len_1a2 >> 1

l1, l2 = 0, 0
s, m = 0, 0
while mid >= 0:
    if l1 < len_1 and l2 < len_2:
        if nums1[l1] > nums2[l2]:
            cur = nums2[l2]
            l2 += 1
        else:
            cur = nums1[l1]
            l1 += 1
    elif l1 < len_1:
        cur = nums1[l1]
        l1 += 1
    else:
        cur = nums2[l2]
        l2 += 1
    s, m = m, cur
    mid -= 1
return float(s + m) / 2 if not len_1a2 % 2 else float(m)
```