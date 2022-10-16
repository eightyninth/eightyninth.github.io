---
title: 力扣 84. 柱状图中最大的矩形
date: 2022/8/4 下午4:27
tags: [leetcode, 双指针, 动态规划, 单调栈]
categories: [leetcode]
---

# 双指针 + 动态规划

```python
def largestRectangleArea(heights):
    # 类似接雨水
    res, h_len = 0, len(heights)
    dp_left, dp_right = [i for i in range(h_len)], [i for i in range(h_len)]
    for i in range(1, h_len):
        while dp_left[i] > 0 and heights[i] <= heights[dp_left[i] - 1]:
            dp_left[i] = dp_left[dp_left[i] - 1]
    # print(dp_left)
    for i in range(h_len - 2, -1, -1):
        while dp_right[i] < h_len - 1 and heights[i] <= heights[dp_right[i] + 1]:
            dp_right[i] = dp_right[dp_right[i] + 1]
    # print(dp_right)
    for i in range(h_len):
        res = max(res, (dp_right[i] - dp_left[i] + 1) * heights[i])
    return res
```

# 单调栈

```python
def largestRectangleArea(heights):
    # 类似接雨水
    res, h_len = 0, len(heights)
    left, right = [0] * h_len, [0] * h_len

    tmp = []
    for i in range(h_len):
        while tmp and heights[tmp[-1]] >= heights[i]:
            tmp.pop()
        left[i] = tmp[-1] if tmp else -1
        tmp.append(i)
    tmp = []
    # print(left)
    for i in range(h_len - 1, -1, -1):
        while tmp and heights[tmp[-1]] >= heights[i]:
            tmp.pop()
        right[i] = tmp[-1] if tmp else h_len
        tmp.append(i)
    # print(right)
    for i in range(h_len):
        res = max(res, (right[i] - left[i] - 1) * heights[i])
    return res
```