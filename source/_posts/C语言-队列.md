---
title: C语言-队列
date: 2017-09-22 12:21:36
updated: 2017-09-22 12:21:36
tags: C语言
---
# 前言
emmm..
<!--more-->

# 代码
```cpp
#include <stdio.h>
#include <stdbool.h>
#include <malloc.h>

//循环队列
typedef struct Queue{
    int *pBase;
    int front;
    int rear;
}Queue;

void initQueue(Queue* pq)
{
    pq->front = -1;
    pq->rear = 0;
    pq->pBase = malloc(sizeof(int)*6);//限定队列长度为6
}

bool isfull(Queue pq)
{
    if((pq.rear)%6 == pq.front)
        return true;
    else
        return false;
}

bool isempty(Queue pq)
{
    if(pq.front == -1)
        return true;
    return false;
}

//入队
void insert(Queue* pq, int val)
{
    printf("insert:%d,%d\n",val ,pq->rear);
    if(isempty(*pq))
    {
        pq->pBase[pq->rear] = val;
        pq->rear = (pq->rear+1)%6;
        pq->front ++;
        return;
    }
    if(isfull(*pq))
    {
        printf("%d,%d\n",pq->front,pq->rear );
        printf("insert isfull\n");
        return;
    }
    pq->pBase[pq->rear] = val;
    pq->rear = (pq->rear+1)%6;
}

//遍历
void traverse(Queue *pq)
{
    if(isempty(*pq))
    {
        printf("traverse empty\n");
        return;
    }
    int i = pq->front;
    while(i%6!= pq->rear || i<6)
    {
        printf("%d,",pq->pBase[i]);
        i++;
    }
    printf("\n");
}

//出队
void out_queue(Queue* pq)
{
    if (isempty(*pq))
    {
        printf("out_queue,error\n");
        return;
    }
    pq->front = (pq->front + 1)%6;
    if(pq->front == pq->rear)
    {
        pq->front = -1;
        pq->rear = 0;
    }
}

int main()
{
    Queue Q;
    initQueue(&Q);  
    insert(&Q,1);  
    insert(&Q,2);  
    insert(&Q,3);  
    insert(&Q,4);  
    insert(&Q,5);  
    insert(&Q,6);
    insert(&Q,6);  
      
    traverse(&Q);  
    out_queue(&Q);
    
    out_queue(&Q);  
    out_queue(&Q);
    out_queue(&Q);
    out_queue(&Q);
    out_queue(&Q);
    out_queue(&Q);
    traverse(&Q);  
    return 0;  
}
```
