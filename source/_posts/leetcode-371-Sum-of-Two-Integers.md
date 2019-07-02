---
title: leetcode 371. Sum of Two Integers
date: 2017-05-18 17:20:49
updated: 2017-05-18 17:20:49
tags: [c++]
categories: leetcode
---
# 题目
    Calculate the sum of two integers a and b, but you are not allowed to use the operator + and -.

## Example
    Given a = 1 and b = 2, return 3.

## 解法一
    对于位运算的加法而言。a^b结果为无进位加法，而a&b结果则为每位的进位结果。进位数要左移1位相加，当a&b == 0的时候表示无进位，即加法结束。


```cpp
class Solution {
public:
    int getSum(int a, int b) {
        int sum = a;
        while(b)
        {
            sum = a ^ b;
            b = (a & b) << 1;
            a = sum;
        }
        return sum;
    }
};
```
