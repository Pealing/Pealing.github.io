---
title: leetcode 520. Detect Capital
date: 2017-05-08 13:10:13
tags: [c++]
categories: leetcode
---
# 题目
    Given a word, you need to judge whether the usage of capitals in it is 
    right or not.

    We define the usage of capitals in a word to be right when one of the 
    following cases holds:

    All letters in this word are capitals, like "USA".
    All letters in this word are not capitals, like "leetcode".
    Only the first letter in this word is capital if it has more than one 
    letter, like "Google".
    Otherwise, we define that this word doesn't use capitals in a right way.

<!--more-->

## Example
    Input: "USA"
    Output: True

    Input: "FlaG"
    Output: False

## 解法
1.时间复杂度O(n)
```cpp
    class Solution {
public:
    bool detectCapitalUse(string word) {
        if(word.length() == 1)
            return true;
        if(word[0]>='A' && word[0]<='Z')
        {
            if(word[1]>='A' && word[1]<='Z')//是否全是大写字母
            {
                for(int i = 1; i < word.length(); ++i)
                    if(word[i] >= 'a' && word[i] <= 'z')
                        return false;
                return true;
            }
            if(word[1]>='a' && word[1]<='z')//是否只是大写字母开头
            {
                for(int i = 1; i < word.length(); ++i)
                    if(word[i] >= 'A' && word[i] <= 'Z')
                        return false;
                return true;
            }
        }
        if(word[0]>='a' && word[0]<='z')
        {
            for(int i = 1; i < word.length(); ++i)
            {
                if((word[i]>='A' && word[i]<='Z'))
                    return false;
            }
            return true;
        }
    }
};
```
2.统计大写字母个数，时间复杂度O(n)
```cpp
class Solution {
public:
    bool detectCapitalUse(string word) {
        if(word.length() == 1)
            return true;
        int cnt = 0;
        for(int i = 0 ;i < word.length(); ++i)
        {
            if('Z' - word[i] >= 0)
                cnt ++;
        }
        if('Z' - word[0] >= 0)
        {
            if(cnt == 1 || cnt == word.length())
                return true;
            return false;
        }
        else
        {
            if(cnt)
                return false;
            return true;
        }
    }
};
```