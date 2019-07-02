---
title: leetcode 461. Hamming Distance
date: 2017-03-19 14:26:29
updated: 2017-03-19 14:26:29
tags: [C++,Python]
categories: leetcode
---
### 题目：
>The Hamming distance between two integers is the number of positions at which the corresponding bits are different.
>Given two integers x and y, calculate the Hamming distance.
<!--more-->

>Note:
>0 ≤ x, y < 231.

##### Example
>Input: x = 1, y = 4
>Output: 2
Explanation:
1   (0 0 0 1)
4   (0 1 0 0)
>↑   ↑
>The above arrows point to positions where the corresponding bits are different.

#### 关于解法
1. 从右到左按位取出，分别于1,10,100……做与运算，判断两个结果是否相等：
```cpp
class Solution {
public:
    int hammingDistance(int x, int y) {
        int num = 0;
        for (int i=0;i<32;i++)
        {
            int a = x & (1<<i);
            int b = y & (1<<i);
            if(a != b)
                num ++ ;
        }
        return num;
    }
};
```

2. 直接把两个数相异或，去判断结果中有多少个1（这个应该是比较好的解法）
```cpp
class Solution {
public:
    int hammingDistance(int x, int y) {
        return __builtin_popcount(x^y);
    }
};
//__builtin_popcount 判断有多少个1
```

3. Python的写法，用bin可以将数字转换成2进制.当然python的运行速度要比C++慢了不少。
```python
class Solution(object):
def hammingDistance(self, x, y):
"""
:type x: int
:type y: int
:rtype: int
"""
return bin(x^y).count('1')
```
