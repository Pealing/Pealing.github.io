---
title: leecode 136. Single Number
date: 2017-05-06 19:47:10
updated: 2017-05-06 19:47:10
tags: [c++]
categories: leetcode
---
# 题目
    Given an array of integers, every element appears twice except for one. Find that single one.
<!--more-->

# 解法一：
    用无序图来存储数据，即m[2] = 1代表数字2在列表中出现了一次。时间复杂度o(n)

```cpp
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        unordered_map<int,int> m;
                //如果这里使用map的话，则为o(nlogn)，因为map查找一次为logn
        for (int i = 0; i < nums.size(); ++i)
        {
            if(m.count(nums[i]))
                m[nums[i]] += 1;
            else
                m[nums[i]] = 1; 
        }
        for(int i = 0; i < nums.size(); ++i)
            if(m[nums[i]] == 1)
                return nums[i];
    }
};
```

# 解法二
    速度比较快的方法，用位运算可以大大提高运算速度。
    xor(异或)的位运算,x ^ x = 0; 0 ^ y = y;

```cpp
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int re = nums[0];
        for(int i = 1;i<(int)nums.size();i++)
        {
            re ^= nums[i]
        }
        return re;
    }
};
```