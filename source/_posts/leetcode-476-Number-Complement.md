---
title: leetcode 476. Number Complement
date: 2017-03-19 14:25:36
tags: [C++,Python]
categories: leetcode
---
### 题目
>Given a positive integer, output its complement number. The complement 
strategy is to flip the bits of its binary representation.
<!--more-->

>Note:
The given integer is guaranteed to fit within the range of a 32-bit signed 
integer.
You could assume no leading zero bit in the integer’s binary representation.

#### Example1:
>Input: 5
Output: 2
Explanation: The binary representation of 5 is 101 (no leading zero bits), and 
its complement is 010. So you need to output 2.

#### Example2:
>Input: 1
Output: 0
Explanation: The binary representation of 1 is 1 (no leading zero bits), and 
its complement is 0. So you need to output 0.

#### 关于解法
1. 比较愚蠢的一种解法，将num从右边一位一位取出，然后存到中间数temp中，再把temp的数反过来。
```cpp
class Solution {
public:
    int findComplement(int num) {
        int temp = 0;
        int index = 0;
        while(num)
        {
            temp  = temp<<1;
            int a = num & 1;
            temp += !a; 
            num >>= 1;
            index++;
        }
        int result2 = 0;
        while(index--)
        {
            result2 <<= 1;
            int a = temp & 1;
            result2 += a;
            temp >>= 1;
        }
        return result2;
    }
};
```

2. 将num去亦或00001111（1的个数为num的位数）
```cpp
class Solution {
public:
    int findComplement(int num) {
        int index = 0;
        int test = num;
        //计算num的位数
        while(test)
        {
            test >>= 1;
            index ++;
        }
        int maks = 1<<index - 1;
        return num ^ maks;
    }
};
```

3. 算num位数有2种方法，愚蠢的就是跟上面一样一位一位取数，快一点的话，可以直接用log去求
```cpp
    class Solution {
public:
    int findComplement(int num) {
        return num ^ ((1 <<(int)log2(num)+1)-1);
    }
};
```

4. Python的方法要来的简单不少，毕竟python的函数比较多,但速度真心挺慢的。
```python
class Solution(object):
    def findComplement(self, num):
        """
        :type num: int
        :rtype: int
        """
        return 2 ** (len(bin(num)) - 2) - num - 1
        #bin(8)的输出为：0b1000,所以长度-2即为num的位数。
```