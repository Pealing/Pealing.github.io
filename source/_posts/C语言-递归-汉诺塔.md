---
title: C语言-递归-汉诺塔
date: 2017-09-22 13:01:27
tags: C语言
---
# 前言
>emm
<!--more-->

# 代码
```cpp
#include <stdio.h>
#include <stdbool.h>
#include <malloc.h>


int hanota(int num,char A,char B,char C)  
{  
    int sum = 0;
 //如果只有一个元素，那么直接把这个元素，移动到C  
 if(1==num)  
 {  
     printf("at %d, %c from %c\n",num,A,C); 
     sum ++; 
 }else{  
 //如果不是第一个元素，先把前n-1个元素，借助C移动到B  
   sum += hanota(num-1,A,C,B);  
 //然后把A最下面的元素移动到C  
   sum ++;
   printf("at %d, %c from %c\n",num,A,C);  
   //然后再把B上的元素借助A移动到C  
   sum += hanota(num-1,B,A,C);  
 }  
 return sum;
}  
int main()  
{  
    char A='A';  
    char B='B';  
    char C='C';  
    printf("%d\n", hanota(4,A,B,C));  
    return 0;  
}  


```
