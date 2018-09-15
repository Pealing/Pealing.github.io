---
title: C语言-动态内存分配
date: 2017-09-21 20:26:18
tags: C语言
---

# 前言
> ...
<!--more-->

# 代码
```CPP
#include <stdio.h>
#include <math.h>
#include <string.h>
#include <stdbool.h>
#include <malloc.h>

typedef struct arr
{
    int len;
    int cnu;
    int* pBase;
} arr;

//初始化
void init_array(arr *pArray, int len)
{
    pArray->pBase=(int*)malloc(sizeof(int) * len); //分配int字节长 
    if(NULL == pArray->pBase)
    {
        printf("内存分配失败\n");
    }
    else
    {
        pArray->len = len;
        pArray->cnu = 0;
    }
}
//判断数组是否为空
bool isempty(arr *pArray)
{
    if(pArray->cnu == 0)
    {
        return true;
    }
    else
        return false;   
}

//判断数组是否满
bool isfull(arr *pArray)
{
    if(pArray->len == pArray->cnu)
        return true;
    else
        return false;
}
//打印数组
void show_array(arr *pArray)
{
    if(isempty(pArray))
        printf("数组为空\n");
    else
    {
        int i;
        for (i=0; i<pArray->cnu;i++)
        {
            printf("%d\n",pArray->pBase[i]);
        }
        printf("-----------\n");
    }
}
//添加元素
bool append(arr *pArray, int val)
{
    if(isfull(pArray))
    {
        printf("数组已满\n");
        return false;
    }
    else
    {
        pArray->pBase[pArray->cnu] = val;
        pArray->cnu ++;
        return true;
    }
}
//插入元素
bool insert(arr *pArray, int pos, int val)
{
    if(pos <1 || pos > pArray->len+1)
    {
        printf("位置有误\n");
        return false;
    }
    if(isfull(pArray))
    {
        printf("数组已满\n");
        return false;
    }
    int i;
    for (i=pArray->cnu;i>=pos;--i)
    {
        pArray->pBase[i] = pArray->pBase[i-1];
    }
    pArray->pBase[pos-1] = val;
    pArray->cnu++;
    return true;
}
//删除
bool delete(arr *pArray, int pos, int *val)
{
    if(pos<1 || pos>pArray->cnu)
    {   
        printf("位置不合法\n");
        return false;
    }
    int i;
    *val = pArray->pBase[pos-1];
    for(i=pos+1;i<=pArray->cnu;i++)
    {
        pArray->pBase[i-2] = pArray->pBase[i-1];
    }
    pArray->cnu--;
    return true;

}

//数组倒置
bool inverse(arr *pArray)
{
    if(isempty(pArray))
    {
        printf("数组为空\n");
        return false;
    }
    int i = 0;
    int j = pArray->cnu-1;
    while(i<j)
    {
        int tmp;
        tmp = pArray->pBase[j];
        pArray->pBase[j] = pArray->pBase[i];
        pArray->pBase[i] = tmp;
        i++;
        j--;
    }
    return true;
}
int main()
{
    arr pArray;
    init_array(&pArray, 6);
    append(&pArray, 1);
    append(&pArray, 2);
    append(&pArray, 3);
    append(&pArray, 4);
    show_array(&pArray);
    insert(&pArray, 2,88);
    show_array(&pArray);
    int val;
    delete(&pArray,1,&val);
    printf("delete %d\n", val);
    show_array(&pArray);
    inverse(&pArray);
    show_array(&pArray);
    return 0;
}
```