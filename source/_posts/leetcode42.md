---
title: 力扣 42. 接雨水
date: 2022/7/30 上午9:51
tags: [leetcode, 动态规划, 双指针, 栈]
categories: [leetcode]
---

# 按列求 N/A

```python
def trap(height):
    # 按列求
    ret, h_len = 0, len(height)
    for i in range(1, h_len - 1):
        left, right = 0, 0
        for l in range(0, i):
            left = max(height[l], left)
        for r in range(i + 1, h_len):
            right = max(right, height[r])
        
        cur = min(left, right)

        if cur > height[i]:
            ret += (cur - height[i])

    return ret
```


# 动态规划 + 按列求

```python
def trap(height):
    # 动态规划 + 按列求
    ret, h_len = 0, len(height)
    left, right = [0] * h_len, [0] * h_len
    left[0] = height[0]
    for l in range(1, h_len):
        left[l] = max(left[l - 1], height[l])
    right[h_len - 1] = height[h_len - 1]
    for r in range(h_len - 2, -1, -1):
        right[r] = max(right[r + 1], height[r])

    for i in range(1, h_len - 1):
        cur = min(left[i], right[i])

        if cur > height[i]:
            ret += (cur - height[i])

    return ret
```


# 双指针 + 按列求

```python
def trap(height):
    # 双指针 + 按列求
    ret, h_len = 0, len(height)
    max_left, max_right = height[0], height[h_len - 1]
    left, right = 1, h_len - 2
    for i in range(1, h_len - 1):
        if height[left - 1] < height[right + 1]:
            max_left = max(max_left, height[left - 1])
            if max_left > height[left]:
                ret += (max_left - height[left])
            left += 1
        else:
            max_right = max(max_right, height[right + 1])
            if max_right > height[right]:
                ret += (max_right - height[right])
            right -= 1

    return ret
```


# 栈

```python
def trap(height):
    # 栈
    ret, h_len = 0, len(height)
    stack = [0]
    for i in range(1, h_len):
        while stack and height[i] > height[stack[-1]]:
            bottom = stack.pop()
            if stack:
                ret += (min(height[stack[-1]], height[i]) - height[bottom]) * (i - stack[-1] - 1)
        stack.append(i)

    return ret
```