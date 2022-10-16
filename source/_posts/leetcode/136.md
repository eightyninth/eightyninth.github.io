---
title: 力扣 136. 只出现一次的数字
date: 2022/8/8 上午11:02
tags: [leetcode, 位运算]
categories: [leetcode]
---

# 位运算
```python
def singleNumber(nums):
    # 位运算
    ret = nums[0]
    for i in range(1, len(nums)):
        ret = nums[i] ^ ret
    return ret 
```