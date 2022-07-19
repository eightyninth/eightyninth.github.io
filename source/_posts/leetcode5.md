---
title: 力扣 5. 最长回文子串
date: 2022-07-02 21:22:45
tags: [leetcode, 算法]
categories: [leetcode]
---

# 中心扩散法

```python
def longestPalindrome(s):
    def expend_substr(left, right):
        while left >= 0 and right < s_l and s[left] == s[right]:
            left -= 1
            right += 1
        return left + 1, right - 1
    
    # 最终回文的始终
    start, end, s_l = 0, 0, len(s)
    for i in range(s_l):
        # 偶数回文
        left, right = expend_substr(i, i + 1)

        if right - left > end - start:
            start, end = left, right

        # 奇数回文
        left, right = expend_substr(i, i)

        if right - left > end - start:
            start, end = left, right
    
    return s[start: end + 1]
```


# 动态规划

```python
def longestPalindrome(s):
    s_len = len(s)
    if s_len < 2:
        return s
    dp = [[False] * s_len for _ in range(s_len)]
    for i in range(s_len):
        dp[i][i] = True
    
    max_len, start = 1, 0
    # 回文长度
    for l in range(2, s_len + 1):
        # 回文字符起始位置
        for st in range(s_len):
            ed = l + st - 1
            if ed >= s_len:
                break
            if s[st] != s[ed]:
                dp[st][ed] = False
            else:
                if l <= 3:
                    dp[st][ed] = True
                else:
                    dp[st][ed] = dp[st + 1][ed - 1]
            
            if dp[st][ed] and l > max_len:
                max_len = l
                start = st
    return s[start: start + max_len]
```