---
title: leetcode 292. Nim Game
date: 2017-05-06 19:14:11
updated: 2017-05-06 19:14:11
tags: [c++]
categories: leetcode
---
# 题目
    You are playing the following Nim Game with your friend: There is a heap of stones on the table, each time one of you take turns to remove 1 to 3 stones. The one who removes the last stone will be the winner. You will take the first turn to remove the stones.

    Both of you are very clever and have optimal strategies for the game. Write a function to determine whether you can win the game given the number of stones in the heap.

    For example, if there are 4 stones in the heap, then you will never win the game: no matter 1, 2, or 3 stones you remove, the last stone will always be removed by your friend.

<!--more-->

# 解法
    简单的博弈问题。如果最后剩下4个，那么先行者必输，那么对于n = 
    4*m,后行者都可以以同样的法子取得胜利。所以一旦n为4的倍数，则
    必输，相反则必赢。

```cpp
class Solution {
public:
    bool canWinNim(int n) {
        if( n % 4 == 0)
            return false;
        return true;
    }
};
```

