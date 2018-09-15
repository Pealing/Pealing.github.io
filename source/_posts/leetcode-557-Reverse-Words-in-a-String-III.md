---
title: leetcode 557. Reverse Words in a String III
date: 2017-05-03 22:50:09
tags: [c++,python]
categories: leetcode
---

## 题目
    Given a string, you need to reverse the order of characters in each word within a sentence while still preserving whitespace and initial word order.

<!--more-->

### Example 1:
    Input: "Let's take LeetCode contest"
    Output: "s'teL ekat edoCteeL tsetnoc"

### 解法：
1.c++
```cpp
class Solution {
public:
    string reverseWords(string s) {
        for(int i = 0;i<s.length();i++)
        {
            if(s[i] != ' ')
            {
                int j = i;
                for(;j<s.length() && s[j]!=' '; j++);
                reverse(s.begin() + i,s.begin() + j);
                i = j - 1;
            }
        }
        return s;
    }
};
```

2.python
```python
    class Solution(object):
    def reverseWords(self, s):
        """
        :type s: str
        :rtype: str
        """
        return " ".join(map(lambda x: x[::-1],s.split()))
```
