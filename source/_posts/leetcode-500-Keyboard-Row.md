---
title: leetcode 500. Keyboard Row
date: 2017-03-19 20:01:13
tags: [Python]
categories: leetcode
---
### 题目
>Given a List of words, return the words that can be typed using letters of alphabet on only one row's of American keyboard like the image below.
<!--more-->

#### Exmaple:
>Input: ["Hello", "Alaska", "Dad", "Peace"]
>Output: ["Alaska", "Dad"]

#### 关于解法
1. 这道题用python还是很方便的，用一行写完
```python
class Solution(object):
    def findWords(self, words):
        """
        :type words: List[str]
        :rtype: List[str]
        """
        judge = lambda x : sum(set(x.lower()).issubset(r) for r in [set(r) for r in ["qwertyuiop","asdfghjkl","zxcvbnm"]])
        return filter(judge,words)
```