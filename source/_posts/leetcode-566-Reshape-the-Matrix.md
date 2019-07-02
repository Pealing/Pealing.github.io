---
title: leetcode 566. Reshape the Matrix
date: 2017-05-01 23:27:40
updated: 2017-05-01 23:27:40
tags: [C++]
categories: leetcode
---
### 题目
    In MATLAB, there is a very useful function called 'reshape', which can reshape a matrix into a new one with different size but keep its original data.
    You're given a matrix represented by a two-dimensional array, and two positive integers r and c representing the row number and column number of the wanted reshaped matrix, respectively.
    The reshaped matrix need to be filled with all the elements of the original matrix in the same row-traversing order as they were.
    If the 'reshape' operation with given parameters is possible and legal, output the new reshaped matrix; Otherwise, output the original matrix.

<!--more-->

#### Example1:
    Input: 
    nums = 
    [[1,2],
     [3,4]]
    r = 1, c = 4
    Output: 
    [[1,2,3,4]]
    Explanation:
    The row-traversing of nums is [1,2,3,4]. The new reshaped matrix is a 1 * 4 matrix, fill it row by row by using the previous list.

#### Example2:
    Input: 
    nums = 
    [[1,2],
     [3,4]]
    r = 2, c = 4
    Output: 
    [[1,2],
     [3,4]]
    Explanation:
    There is no way to reshape a 2 * 2 matrix to a 2 * 4 matrix. So output the original matrix.

#### Note:
    1.The height and width of the given matrix is in range [1, 100].
    2.The given r and c are all positive.

#### 关于解法
1. 按照顺序将原矩阵中的数一个一个读入新的矩阵中。
```cpp
class Solution {
public:
    vector<vector<int>> matrixReshape(vector<vector<int>>& nums, int r, int c) {
        int row = nums.size();
        int col = nums[0].size();
        if(((row * col) != (r*c)) || (row == r && col == c))
            return nums;
        vector<vector<int>> newnums;
        int num = 0;
        for(int i = 0; i < r;i++)
        {
            std::vector<int> v;
            for(int j = 0; j < c; j ++)
            {
                int nr = num / col;
                int nc = num % col;
                v.push_back(nums[nr][nc]);
                num++;
            }
            newnums.push_back(v);
        }
        return newnums;
    }
};
```
