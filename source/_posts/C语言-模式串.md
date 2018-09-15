---
title: C语言-模式串
date: 2017-09-22 14:38:54
tags: C语言
---
# 前言
> BF算法和KMP算法
<!--more-->

# KMP
> 需要计算较短串的失效函数f(n)，即，当前位置i前方有几个子串：
> A   B   C   A   B   C   A   A   A
> -1  0   0   0   1   2   3   4   1

# 代码
```cpp
#include <stdio.h>
#include <stdbool.h>
#include <malloc.h>
#include <string.h>

int BF_find(const char *ob, const char *pat)
{
    int i=0,j=0;
    while((i < strlen(ob) && j < strlen(pat)) && strlen(pat) - j <= strlen(ob) - i)
    {
        if(ob[i] == pat[j])
        {
            i++;
            j++;
        }
        else
        {
            i = i-j + 1;
            j = 0;
        }
    }
    if(j >= strlen(pat))
        return i - j;
    else
        return -1;
    //BF时间复杂度O(m*n)
}
void GetFailure(const char*pat, int f[])
{
    //设置pat的f[]
    int i=0,j =-1;
    f[0] = -1;
    while(i<strlen(pat) - 1)
    {
        if(j == -1 || pat[i] == pat[j])
        {
            f[++i] = ++j;
        }
        else
        {
            j = f[j];
        }
    }
}
int KMP(const char*op, const char* pat)
{
    //重点:失效函数.f[j]表示模式串在下标j前面的字符串中的真子串长度。
    int *f = malloc(sizeof(int)*strlen(pat));
    GetFailure(pat, f);
    int i=0,j=0;
    while((i < strlen(op) && j < strlen(pat)) && strlen(pat) - j <strlen(op)-i)
    {
        if(j == -1 || op[i] == pat[j])
        {
            i++;
            j++;
        }
        else
        {
            j = f[j];
        }
    }
    if(j >= strlen(pat))
    {
        return i-j;
    }
    else
    {
        return -1;
    }
    //时间复杂度O(ob.size)

}
int main()
{
    char s1[] = "zwwwzwz";
    char s2[] = "zwz";
    printf("%d\n",KMP(s1,s2));
    return 0;
}

```
