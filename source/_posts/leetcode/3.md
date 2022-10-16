---
title: 力扣 3. 无重复字符的最长子串
date: 2022-07-02 21:22:45
tags: [leetcode, hash表, 双指针]
categories: [leetcode]
---

# 哈希表 + 双指针

```python
if not s: return 0
res, bi, left, right = 1, set(), 0, 0
bi.add(s[0])
for i in range(1, len(s)):
    right += 1
    if s[i] in bi:
        while left <= right:
            if s[left] != s[i]:
                bi.remove(s[left])
                left += 1
            else:
                left += 1
                break
    bi.add(s[i])
    
    res = max(right - left + 1, res)
return res
```