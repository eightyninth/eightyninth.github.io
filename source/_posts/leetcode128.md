---
title: 力扣 128. 最长连续序列
date: 2022/8/8 上午10:26
tags: [leetcode, 动态规划, hash表]
categories: []
---

# hash表

```python
def longestConsecutive(nums):
    ret = 0
    nums_set = set(nums)
    for num in nums_set:
        if num - 1 in nums_set:
            continue
        cur, cur_num = 1, num + 1
        while cur_num in nums_set:
            cur += 1
            cur_num += 1
        ret = max(ret, cur)
    return ret
```

# 动态规划

```python
def longestConsecutive(nums):
    num_dict, ret = {}, 0
    for num in nums:
        if num not in num_dict:
            left = num_dict.get(num - 1, 0)
            right = num_dict.get(num + 1, 0)
            cur = left + right + 1
            ret = max(ret, cur)

            num_dict[num] = cur
            num_dict[num - left] = cur
            num_dict[num + right] = cur
    return ret
```