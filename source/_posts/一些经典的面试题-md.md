---
title: 一些经典的面试题.md
date: 2017-04-17 11:25:32
updated: 2017-04-17 11:25:32
tags:
---
# 题目
    输入数字n,按顺序打印出从1最大的n位十进制数。比如输入3，则打印出1，2，3一直到最大3位
    数999

<!--more-->

## 考虑
    如果直接从1开始循环打印,当n很大的时候，即使用long 
    long也会溢出，此时我们需要用string来解决这个问题。

## 解法
```cpp
bool Increment(char *number)
{
    int ntakeover = 0;
    for(int i = strlen(number)-1;i>=0;i--)
    {
        int nsum = ntakeover + number[i] - '0';
        if(i == strlen(number)-1)
            nsum ++;
        if(nsum >= 10)
        {
            ntakeover = 1;
            if(i == 0)
                return true;
            else
            {
                nsum -= 10;
                number[i] = '0' + nsum;
            }
        }
        else
        {
            number[i] = '0' + nsum;
        }
    }
}
void printNumber(char* number)
{
    bool start = false;
    for(int i=0;i<strlen(number);i++)
    {
        if(!start)
        {
            if(number[i] != '0')
            {
                cout<<number[i];
                start = true;
            }
        }
        else
        {
            cout<<number[i];
        }
    }
    cout<<endl;
}
void Print1ToMAxOf(int n)
{
    if(n < 0)
        return;
    char number[n+1];
    for(int i=0;i<n;i++)
    {
        number[i] = '0';
    }
    number[n] = '\0';
    while(!Increment(number))
    {
        printNumber(number);
    }
     
}
```

# 题目
    给定单向链表的头指针和一个结点指针，定义一个函数在O(1)时间删除该结点。

#解法
    一般来说，删除某个结点，需要找到其前一个结点，则需要遍历链表即O(n)。
    O(1)时间内，我们可以将该结点的下一个结点值复制到该结点中，修改指针。

```cpp
void DeleteNode(ListNode** pListHead,ListNode* pToBeDeleted)
{
    if(!pListHead || !pToBeDeleted)
        retuen;
    //要删除的结点不是尾结点
    if(pToBeDeleted->next != NULL)
    {
        ListNode* pNext = pToBeDeleted->next;
        pToBeDeleted->value = pNext->value;
        pToBeDeleted->next = pNext->next;
        
        delete pNext;
        pNext = NULL;
    }
    //该链表只有一个结点
    else if(*pListHead == pToBeDeleted)
    {
        delete pToBeDeleted;
        pToBeDeleted = NULL;
        pListHead->next = NULL;
    }    
    //该结点是尾结点
    else
    {
        ListNode* p = *pListHead;
        while(pNode->next != pToBeDeleted)
        {
            pNode = pNode->next;
        }
        pNode->next = pToBeDeleted->next;
        delete pToBeDeleted;
        pToBeDeleted = NULL;
    }
}
```

# 题目
    输入一个链表，输出该链表中倒数第k个结点。

```cpp
ListNode* FindKthToTail(ListNode* pListHead,unsigneed int k)
{
    if(k == 0 || pListHead == NULL)
        return NULL;
    ListNode* pFirst = pListHead;
    ListNode* pSecond = pListHead;
    for(unsigneed int i=0;i<k-1;++i)
        if(pSecond->next != NULL)
            pSecond = pSecond->next;
        else
            return NULL;
    while(pSecond->next != NULL)
    {
        pFirst = pFirst->next;
        pSecond = pSecond->next;
    }
    return pFirst;
}
```

# 题目
    定义一个函数，输入一个链表的头结点，翻转该链表并输出翻转后的头结点。

```cpp
ListNode* ReverseList(ListNode* pHead)
{
    ListNode* pReversedhead = NULL;
    ListNode* pNode = pHead;
    ListNode* pPrev = NULL;
    while(pNode != NULL)
    {
        ListNode* pNext = pNode->next;
        if(pNext == NULL)
            pReversedhead = pNode;
        pNode->next = pPrev;
        pPrev = pNode;
        pNode = pNext;
    }
    return pReversedHead;
}


```

# 题目
    输入一个数字，求出其二进制中有多少个1

```cpp
// 二进制中1的个数

// 解法一：该方法运行到溢出，即unit32次。
int NumberOf1(int n)
{
     int count = 0;
     unsigned int flag = 1;
     while(flag)
     {
        if(n & flag)
            count ++;
        flag = flag<<1;
     }
     return count;
}
// 解法二：一个整数减1，和原整数&，可以把该整数最右边的1变成0
int NumberOf1(int n)
{
    int count = 0;
    int m = n;
    while(m)
    {
        m = (m - 1) & m;
        ++count;
    }
    return count;
}
//解法三：直接与1与，然后右移。
int NumberOf1(int n)
{
    int count = 0;
    int m = n;
    while(m)
    {
        if(m & 1)
            ++count;
        m >>= 1;
    }
    return count;
}
```

# 一道关于指针的题目
    这是我之前面试的一道笔试题目，关于指针和地址错综复杂。
    虽然答案写对了，但由于不确定，被面试官怼得一塌糊涂。
    下面的答案是出于个人理解，如果有错，欢迎联系我改正，谢谢！

### 题目
    写出下面4段程序的输出结果。

```cpp
//1
void function(char *p)
{
    p = (char *)malloc(100);
}
int main()
{
    char* str = NULL;
    function(str);
    strcpy(str,"hello,word");
    printf("%s\n",str);
}
```

```cpp
//2
char* function()
{
    char* p =  (char *)malloc(100);
    return p;
}
int main()
{
    char* str = NULL;
    str = function();
    strcpy(str,"hello,word");
    printf("%s\n",str);
}
```

```cpp
//3
void function(char** p,int num)
{
    *p = (char *)malloc(num);
}

int main()
{
    char* str = NULL;
    function(&str,100);
    strcpy(str,"hello,word");
    printf("%s\n",str);
}
```

```cpp
//4
int main()
{
    char* str = NULL;
    str = (char*)malloc(100);
    strcpy(str,"hello");
    free(str);
    if(str ! = NULL)
        strcpy(str,"world");
    printf("%s\n",str);
}
```

### 答案
1. error:数组越界。str == null;
2. hello,world
3. hello,world
4. error:数组越界。str != null;

### 解释
1.这个比较容易，function里的参数p是一个局部变量，不会影响到外界的str，所以str依旧是空。
2.function返回一个地址，被str接收，所以虽然局部变量p被销毁了，但地址传给了str,且内存里的hello,world还在，所以能够输出。
3.是一个二级指针，p指向*存放指针*str的地址，\*P就是str指向的地址了，\*\*p就是\*str，所以\*p申请了内存，就相当于给str申请内存了。
4.free(str)释放了str的内存，但str还是有地址的，只是这块地址没有内存，所以会进入到if
(str != NULL)中，但是此时它没有内存了，所以strcpy会出错。

最后补充一点，malloc和free，跟new和delete的区别在于：后者是c++端口下的，所以如果你创建的是一个类空间，那么new和delete会调用构造函数和析构函数，而malloc和free不会。

# 题目：
    输入两个递增排序的链表，合并这两个链表，并使新链表中的结点仍然是按照递增排序的。

```cpp
//这一题需要注意的主要是代码的鲁棒性，即当输入的其中一个链表为空的时候需要作出反应。
ListNode * Merge(ListNode* pHead1,ListNode* pHead2)
{
    if(pHead1 == NULL)
        return pHead2;
    if(pHead2 == NULL)
        return pHead1;
    ListNode* pHead3 = NULL;
    if(pHead1->value < pHead2->value)
    {
        pHead3 = pHead1;
        pHead3->next = Merge(pHead1->next,pHead2);
    }
    else
    {
        pHead3 = pHead2;
        pHead3->next = Merge(pHead1,pHead2->next);
    }
    return pHead3;
}
```

# 题目
    输入两棵二叉树A和B，判断B是不是A的子结构。

```cpp
//遍历A树，找到与B根节点相同的点。
bool HasSubtree(BinryTreeNode* pRoot1,BinaryTreeNode* pRoot2)
{
    bool result = false;
    if(pRoot1 !=NULL && pRoot2 != BULL)
    {
        if(pRoot1->value == pRoot2->value)
            result = DoseTree1HaveTree2(pRoot1,pRoot2);
        if(!result)
            result = HasSubtree(pRoot1->left,pRoot2);
        if(!result)
            result = HasSubtree(pRoot1->right,pRoot2);
    }
    return result;
}
//判断和B有同样根节点的子树是不是和B相同
bool DoesTree1HavaTree2(BinaryTreeNode* pRoot1,BinaryTreeNode pRoot2)
{
    if(pRoot2 == NULL)
        return true;
    if(pRoot1 == NULL)
        return false;
    if(pRoot1->value != pRoot2->value)
        return false;
    return DoesTree1HavaTree2(pRoot1->left,pRoot2->left) && DoesTree1HavaTree2(pRoot1->right,pRoot2->right);
}

```

# 题目
    输入一个二叉树，输出它的镜像

```cpp
    void mimrrorrecursively(BinaryTreeNode* pNode)
    {
        if(pNode == NULL)
            return;
        if(pNode->left == NULL && p->Node->right == NULL)
            return;
        BinaryTreeNode* pTemp = pNode->left;
        pNode->left = pNode->right;
        pNode-<right = pTemp;
        
        if(pNode->left)
            mirrorrecursively(pNode->left);
        if(pNode->right)
            mirrorrecursively(pNode->right);
    }
```

# 题目
    输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字。

```cpp
    //通过画图我们可以发现，每一圈循环的起点都是（x,x)，即横纵坐标相同的点。
    //最后一圈循环的起点是该矩阵
```