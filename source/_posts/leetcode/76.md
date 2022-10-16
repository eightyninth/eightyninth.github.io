---
title: 力扣 76. 最小覆盖子串
date: 2022/8/2 下午6:33
tags: [leetcode, hash表, 滑动窗口]
categories: [leetcode]
---

# hash表 + 滑动窗口

```python
def minWindow(s, t):
    if len(s) < len(t):
        return ""
    # 构建hash表
    hash_t, hash_s, count = {}, {}, 0
    for c in t:
        hash_t[c] = hash_t.get(c, 0) + 1
    
    j, t_len, res = 0, len(t), ""
    for i in range(len(s)):
        hash_s[s[i]] = hash_s.get(s[i], 0) + 1
        if hash_s[s[i]] <= hash_t.get(s[i], 0):
            count += 1
        while j <= i and hash_s.get(s[j], 0) > hash_t.get(s[j], 0):
            hash_s[s[j]] -= 1
            j += 1
        if count == t_len:
            if not res or i - j + 1 < len(res):
                res = s[j: i + 1]
    return res
```