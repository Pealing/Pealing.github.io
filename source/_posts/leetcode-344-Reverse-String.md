---
title: leetcode 344. Reverse String
date: 2017-03-23 20:05:49
tags: [C++]
categories: leetcode
---
# 题目
    Write a function that takes a string as input and returns the string reversed
<!--more-->

# Example
    Given s = "hello", return "olleh".

# 关于解法

### 解法一
常规方法，用长度倒置，但我不知道为什么strlen会报错啊。
``` cpp
class Solution {
public:
    string reverseString(string s) {
        string a = "";
        for(int i=s.size()-1;i>=0;i--)
        {
            a += s[i];
        }
        return a;
    }
};
```

### 解法二
```cpp
class Solution {
public:
    string reverseString(string s) {
        int i = 0, j = s.size() - 1;
        while(i < j){
            swap(s[i++], s[j--]); 
        }
        
        return s;
    }
};
```

