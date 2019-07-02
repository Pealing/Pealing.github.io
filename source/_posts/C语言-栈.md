---
title: C语言-栈
date: 2017-09-21 23:46:31
updated: 2017-09-21 23:46:31
tags: C语言
---
# 前言
>
<!--more-->

# 代码
```cpp
#include <stdio.h>
#include <stdbool.h>
#include <malloc.h>

typedef struct Node{
    int data;
    struct Node* next;
}Node;

typedef struct Stack
{
    Node* top;
    Node* bottom;
}Stack;

//初始化
void init (Stack* pstack)
{
    pstack->bottom = malloc(sizeof(Node));
    pstack->bottom->next = NULL;
    pstack->top = pstack->bottom;
    pstack->top->next = NULL;
}
//遍历
void traverse(Stack pstack)
{
    if(pstack.bottom == pstack.top)
    {
        printf("stack empty\n");
        return;
    }
    Node *p = pstack.top;
    while(p->next != NULL)
    {
        printf("%d,", p->data);
        p = p->next;
    }
    printf("\n");
}
//入栈
void push(Stack* pstack, int val)
{
    Node* pnew = malloc(sizeof(Node));
    pnew->data = val;
    pnew->next = pstack->top;
    pstack->top = pnew;
}
void pop(Stack* pstack, int *val)
{
    if(pstack->bottom == pstack->top)
    {
        printf("pop error\n");
        return ;
    }
    *val = pstack->top->data; 
    Node* ptop = pstack->top;
    pstack->top = pstack->top->next;
    free(ptop);
    ptop = NULL;
}
//清空
void clear(Stack* pstack)
{
    while(pstack->top != pstack->bottom)
    {
        int val;
        pop(pstack,&val);
    }
}
int main()
{
    Stack stack;
    init(&stack);
    push(&stack,6);
    push(&stack,7);
    push(&stack,8);
    traverse(stack);
    int val;
    pop(&stack,&val);
    printf("num:%d\n",val);
    traverse(stack);
    clear(&stack);
    traverse(stack);
    push(&stack,7);
    traverse(stack);
    return 0;
}
```