---
title: 力扣 48. 旋转图像
date: 2022/7/30 上午11:56
tags: [leetcode]
categories: [leetcode]
---


# 矩阵处理(转置 + 列调换)

```python
def rotate(matrix):
    """
    Do not return anything, modify matrix in-place instead.
    """
    if not matrix or not matrix[0]:
        return
    
    height = len(matrix)

    for i in range(height):
        for j in range(i, height):
            matrix[i][j], matrix[j][i] = matrix[j][i], matrix[i][j]
    
    
    for i in range(height // 2):
        for j in range(height):
            matrix[j][i], matrix[j][height - 1 - i] = matrix[j][height - 1 - i],  matrix[j][i]

```