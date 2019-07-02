---
title: leetcode 448. Find All Numbers Disappeared in an Array
date: 2017-05-10 14:25:28
updated: 2017-05-10 14:25:28
tags: [c++]

---
# 题目
    Given an array of integers where 1 ≤ a[i] ≤ n (n = size of array), some elements appear twice and others appear once.

    Find all the elements of [1, n] inclusive that do not appear in this array.

    Could you do it without extra space and in O(n) runtime? You may assume the returned list does not count as extra space.
<!--more-->
# Example:
    Input:
    [4,3,2,7,8,2,3,1]

    Output:
    [5,6]

# 解法
    由于题目要求我们不使用额外的空间，也就是说，我们需要再原来的nums中做一个变化调整。
    由于nums中的数据是1-n，其空间也为n。所以我们可以遍历一遍nums，将出现的数字所在的位置上的数字调整为负，以此作为标记,同时也不会改变其中的数值内容。

```cpp
class Solution {
public:
    vector<int> findDisappearedNumbers(vector<int>& nums) {
        for(int i = 0 ; i < (int)nums.size(); ++i)
        {
            nums[abs(nums[i]) - 1] = nums[abs(nums[i]) - 1] > 0 ? -nums[abs(nums[i]) - 1]:nums[abs(nums[i]) - 1];
            //int m = abs(nums[i] - 1),出现过的数值作为下标，要从0开始。
        }
        std::vector<int> v;
        for(int i = 0; i< (int)nums.size(); ++i)
        {
            if(nums[i] > 0)
                v.push_back(i+1);
        }
        return v;
    }
};

```