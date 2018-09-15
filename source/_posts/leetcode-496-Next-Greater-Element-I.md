---
title: leetcode 496. Next Greater Element I
date: 2017-05-03 22:27:20
tags: [c++]
categories: leetcode
---

### 题目
    You are given two arrays (without duplicates) nums1 and nums2 where nums1’s elements are subset of nums2. Find all the next greater numbers for nums1's elements in the corresponding places of nums2.
    The Next Greater Number of a number x in nums1 is the first greater number to its right in nums2. If it does not exist, output -1 for this number.
<!--more-->

#### Example1
    Input: nums1 = [4,1,2], nums2 = [1,3,4,2].
    Output: [-1,3,-1]
    Explanation:
        For number 4 in the first array, you cannot find the next greater number for it in the second array, so output -1.
        For number 1 in the first array, the next greater number for it in the second array is 3.
        For number 2 in the first array, there is no next greater number for it in the second array, so output -1

#### Example2
    Input: nums1 = [2,4], nums2 = [1,2,3,4].
    Output: [3,-1]
    Explanation:
    For number 2 in the first array, the next greater number for it in the second array is 3.
    For number 4 in the first array, there is no next greater number for it in the second array, so output -1.

#### 关于解法
* 说明
    - 因为num1是num2的子集，所以要求num1中元素在num2里的下一个最大，只需要把num2中的所有元素下一个最大都求出来。
    - 在num2中，若第二个元素比第一个元素小，则说明，该两个元素可能在后面有同一个下一个最大元素，所以将它们存到栈中，直到找到下一个最大元素，将其弹出栈并存储。
    - 一次num2的循环结束后，栈中残留的元素即为没有下一个最大元素的为-1;
    - 此时的时间复杂度为O(n)

```cpp
class Solution {
public:
    vector<int> nextGreaterElement(vector<int>& findNums, vector<int>& nums) {
        unordered_map<int,int> m;//无序表用来存储结果
        stack<int> s;
        std::vector<int> v;
        for(int i=0;i<(int)nums.size();i++)
        {
            while(s.size() && nums[i] > s.top())
            {
                m[s.top()] = nums[i];
                s.pop();
            }
            s.push(nums[i]);
        }        
        for(int n:findNums)
        {
            v.push_back(m.count(n) ? m[n] : -1);
                                //m.count(n)返回一个bool,表示m[n]是否存在
        }
        return v;
    }
};
```