---
title: leetcode 412. Fizz Buzz
date: 2017-03-23 19:29:26
updated: 2017-03-23 19:29:26
tags: [C++]
categories: leetcode
---
# 题目
    Write a program that outputs the string representation of numbers from 1 to n.
    But for multiples of three it should output “Fizz” instead of the number and for the multiples of five output “Buzz”. For numbers which are multiples of both three and five output “FizzBuzz”.

<!--more-->

# Example
     n = 15,
    Return:
    [
        "1",
        "2",
        "Fizz",
        "4",
        "Buzz",
        "Fizz",
        "7",
        "8",
        "Fizz",
        "Buzz",
        "11",
        "Fizz",
        "13",
        "14",
        "FizzBuzz"
    ]

# 关于解法

```cpp
    class Solution {
public:
    vector<string> fizzBuzz(int n) {
        vector<string> result;
        for(int i=1;i<n+1;i++)
        {
            if(i % 15  == 0)
            {
                result.push_back("FizzBuzz");
            }
            else if(i % 3 == 0)
            {
                result.push_back("Fizz");
            }
            else if(i % 5 == 0)
            {
                result.push_back("Buzz");
            }
            else
            {
                string a = to_string(i);
                result.push_back(a);
            }

        }
    return result;
    }
};
```