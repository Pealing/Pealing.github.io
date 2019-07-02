---
title: leetcode 463. Island Perimeter
date: 2017-05-06 16:54:18
updated: 2017-05-06 16:54:18
tags: [c++]
categories: leetcode
---

# 题目
    You are given a map in form of a two-dimensional integer grid where 1 represents land and 0 represents water. Grid cells are connected horizontally/vertically (not diagonally). The grid is completely surrounded by water, and there is exactly one island (i.e., one or more connected land cells). The island doesn't have "lakes" (water inside that isn't connected to the water around the island). One cell is a square with side length 1. The grid is rectangular, width and height don't exceed 100. Determine the perimeter of the island.

<!--more-->

# Example
    [[0,1,0,0],
     [1,1,1,0],
     [0,1,0,0],
     [1,1,0,0]]
    Answer: 16

# 关于解法

## 解法
    一块陆地（一个正方形）有4条边，一旦它的四周（上下左右）中有一个同样为陆地，则需要-1
    条边，所以，比较复杂的写法：时间复杂度为O(n)。

```cpp
class Solution {
public:
    int islandPerimeter(vector<vector<int>>& grid) {
     
        int row = (int)grid.size();
        int col = (int)grid[0].size();
        int sum = 0;
        for(int i = 0; i < row; i ++)
        {
            for(int j = 0; j < col ; j ++ )
            {
                if(grid[i][j] == 1)
                {
                    sum += 4;
                    if(i != 0)//不是第一行，则看看其上方是否有陆地
                        if(grid[i - 1][j] == 1)
                            sum -- ;
                    if(i != row - 1)//不是最后一行，则看其下方是否有陆地
                        if(grid[i + 1][j] == 1)
                            sum -- ;
                    if(j != 0)//不是最左边，则看其左方是否有陆地
                        if(grid[i][j - 1] == 1)
                            sum -- ;
                    if(j != col - 1)//不是最右边，则看其右方是否有陆地
                        if(grid[i][j + 1] == 1)
                            sum -- ;
                }

            }
        }
        return sum;
    }
};
```

