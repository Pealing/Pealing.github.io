---
title: C语言-链表
date: 2017-09-21 22:25:54
updated: 2017-09-21 22:25:54
tags: C语言
---
# 前言
>emmm..
<!--more-->

# 代码
```cpp
#include <stdio.h>
#include <stdbool.h>
#include <malloc.h>

typedef struct Node
{
    int data;
    struct Node *next;
}Node;

void InitList(Node **PHead)
{
    *PHead = NULL;
}

Node *CreateList(Node **pHead)
{
    Node *p1;
    Node *p2;

    p1 = p2 = (Node*)malloc(sizeof(Node));
    if(p1 == NULL || p2 == NULL)
    {
        printf("分配失败\n");
        exit(0);
    }
    memset(p1,0,sizeof(Node));//把p1前sizeof(Node)的内存设置成0
    scanf("%d", &p1->data);
    p1->next = NULL;

    while(p1->data > 0)//当输入非负数就继续输入
    {
        if(*pHead == NULL)
        {
            *pHead = p1;
        }
        else
        {
            p2->next = p1;
        }
        p2 = p1;
        p1 = (Node*)malloc(sizeof(Node));
        if(p1 == NULL)
        {
            printf("分配失败\n");
            exit(0);
        }
        memset(p1,0,sizeof(Node));
        scanf("%d",&p1->data);
        p1->next = NULL;
    }
    return *pHead;
}

void printList(Node* pHead)//打印链表
{
    if(NULL == pHead)
    {
        printf("print error\n");
        return;
    }
    else
    {
        Node *p;
        p = pHead;
        while(p != NULL)
        {
            printf("%d," ,p->data);
            p = p->next;
        }
        printf("\n");
    }
}
void clearLisr(Node *pHead)
{
    Node *p;
    if(pHead == NULL)
    {
        printf("列表为空\n");
        return;
    }
    else
    {
        p = pHead;
        while(p != NULL)
        {
            Node *x;
            x = p;
            free(p);
            p = x->next;
        }
        p = NULL;
    }
}

int sizeList(Node *pHead)
{
    int size = 0;
    if(pHead = NULL)
    {
        return size;
    }
    Node *p = pHead;
    while(p != NULL)
    {
        size++;
        p = p->next;
    }
    return size;
}

int main()
{
    Node* pHead = NULL;
    int length = 0;

    InitList(&pHead);
    CreateList(&pHead);
    printList(pHead);
    return 0;

}
```
