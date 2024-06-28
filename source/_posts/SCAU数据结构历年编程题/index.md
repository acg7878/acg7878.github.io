---
title: SCAU数据结构历年编程题
categories: 
    - 数据结构
tags:
    - 数据结构
cover: "https://raw.githubusercontent.com/acg7878/hexo_picture/master/blog/SCAU数据结构历年编程题/2024-06-27.jpg"
---
## 2022

### 第一题

在链表存储结构上设计实现**直接插入排序**算法。

链表存储结构如下：

```c
typedef struct LNode {
    int data;           // 结点的数据域
    struct LNode *next; // 结点的指针域
} LNode, LinkList;
```

统一使用函数名：`void straightinsertsort(LinkList  &L)`

**答案：**

```c
void InsertSort(LinkedList head) {
    if (head->next == NULL) // 若链表为空，则返回
        return;
    LNode *sorted = head->next; // 表示已排序链表，指向链表第一个元素
    LNode *curr = sorted->next; // 表示待排元素
    while (curr != NULL) {
        if (sorted->data <=
            curr->data) { // 若已排最后一个元素元素小于待排元素，则不需要排序
            sorted = sorted->next;
        } else {
            LNode *prev = head; // 头结点
            while (prev->next->data <=
                   curr->data) { // 从头结点开始遍历，找到小于待排元素的结点
                prev = prev->next;
            } // while
            sorted->next = curr->next; // 插入待排结点
            curr->next = prev->next;
            prev->next = curr;
        } // else
        curr = sorted->next;
    } // while
    return head;
} // void
```



###  第二题

假设`二叉排序树`采用二叉链表存储结构存储，设计一个算法，求出二叉排序树某个节点node的双亲结点

```c
typedef struct BSNode { // 二叉链表定义
    int key;
    struct BiNode *lchild, *rchild;
} BSTNode, *BSTree;
```

统一使用函数名： `BSTNode * Parent(BSTNode * node)`

**答案：**

```C
BSTNode *Parent(BSTNode *node, BSTree T) {
    TreeNode *p = T; // 先创建指针p使其指向根节点root
    if (node == T) {
        return NULL;
    } else {
        while (p->lchild != node && p->rchild != node) {
            if (node->key < p->key) {
                p = p->lchild;
            } else if (node->key > p->key) {
                p = p->rchild;
            }
        }
        return p; 
    }
}
```



## 2016

### 第二题

编写算法，设计判断一棵二叉树是否是完全二叉树的算法。

二叉树的存储结构定义如下：

```c++
typedef struct BTNode{
  int data;
  struct BTNode *lchild,*rchild;
}BTNode,*BTree;
```
其中队列数据结构的基本操作如下:

**InitQueue(Queue &Q);** 构造一个空队Q
**QueueEmpty(Queue Q)**; //若队列为空返回TRUE，否则返回FALSE
**EnQueue(Queue &Q,e);** 入队
**DeQueue(Queue &Q,e);** 出队，并用e返回 
假定上述操作已经实现，直接调用即可。
统一使用如下函数名：`int IsCompleteTree(BTree T)`

**答案：**
```C
//ANSWER
int IsCompleteTree(BTree T) {
    if (!T) return 1; // 空树是完全二叉树

    Queue Q;
    InitQueue(Q);
    EnQueue(Q, T);
    bool flag = false; // 标志是否遇到了空节点

    while (!QueueEmpty(Q)) {
        BTNode* node;
        DeQueue(Q, node);

        if (node) {
            if (flag) {
                // 如果已经遇到了空节点，后续不能再有非空节点
                return 0;
            }
            EnQueue(Q, node->lchild);
            EnQueue(Q, node->rchild);
        } else {
            // 遇到空节点，设置标志
            flag = true;
        }
    }

    return 1;
}
```



## 2015

### 第二题

一个具有 n 个结点的完全二叉树采用顺序存储方式，其数据存放在整型一维数组 `a` 中，编写非递归算法对其进行先序遍历。算法可以直接调用栈的基本操作：

- 初始化：`InitStack(SqStack &S)`
- 入栈：`Push(SqStack &S, int e)`
- 出栈：`Pop(SqStack &S, int &e)`
- 判断栈空：`Empty(SqStack S)`

统一使用函数名：`void preorder(int a[], int n)`

**答案：**
```c
void preorder（int a[], int n）
{
   SqStack S;
   InitStack(S);
   Push(S,0);
   int i = 1;
   int j = 2*i;
   visit(a[i]);
   Push(S,i);
   while(!Empty(S))
   {
      while(j<=n)
      {
         Push(S,j);
         i=j;  j=2*i;
         visit(a[i]);
       }
       Pop(S,i);
       j = 2*i+1;
    }
}
```

## 2014

### 第二题

编写算法，设计判断一棵二叉树是否是二叉排序树的算法

二叉排序树（Binary Search Tree，BST）的存储结构定义如下：

```C
typedef struct BSTNode {
    int data;
    struct BSTNode *lchild, *rchild; 
} BSTNode, *BSTree;
```
统一使用如下函数名：`int  IsSearchTree(BSTree t)`  

**答案：**
```C
int IsSearchTree(BSTree t) // 递归遍历二叉树是否为二叉排序树
{
    if (!t) // 空二叉树情况
        return 1;
    else if (!(t->lchild) && !(t->rchild)) // 左右子树都无情况
        return 1;
    else if ((t->lchild) && !(t->rchild)) // 只有左子树情况
    {
        if (t->lchild->data > t->data)
            return 0;
        else
            return IsSearchTree(t->lchild);
    } else if ((t->rchild) && !(t->lchild)) // 只有右子树情况
    {
        if (t->rchild->data < t->data)
            return 0;
        else
            return IsSearchTree(t->rchild);
    } else // 左右子树全有情况
    {
        if ((t->lchild->data > t->data) || (t->rchild->data < t->data))
            return 0;
        else
            return (IsSearchTree(t->lchild) && IsSearchTree(t->rchild));
    }
}

```

## 2013

### 第一题

编写算法删除单链表L中值最小的结点

单链表存储结构定义如下：

```C
struct LNode {
    int data;
    struct LNode *next;
};
typedef struct LNode *LinkList;
```

**答案：**
```C
void deleteMinNode(LinkList* head) {
    if (*head == NULL) {
        return;
    }

    struct LNode *minNode = *head;
    struct LNode *minNodePrev = NULL;
    struct LNode *current = *head;
    struct LNode *prev = NULL;

    while (current != NULL) {
        if (current->data < minNode->data) {
            minNode = current;
            minNodePrev = prev;
        }
        prev = current;
        current = current->next;
    }

    if (minNodePrev == NULL) {
        // 如果最小节点是头节点
        *head = minNode->next;
    } else {
        minNodePrev->next = minNode->next;
    }

    free(minNode);
}
```

### 第二题

编写算法，从大到小输出二叉排序树中所有结点的值。

二叉排序树的存储结构定义如下：

```c
typedef struct BSTNode {
    int data;
    struct BSTNode *lchild, *rchild; 
} BSTNode, *BSTree;
```

**答案：**

```C
void trav（BsTree T）
{
  if(T)
  {
     trav(T->rchild);
     printf("%d ", T->data);
     trav(T->lchild);
  }
} //反向中序遍历即可
```



## 2012.1
### 第一题

给定（已生成）一个带表头结点的单链表，设head为头指针，结点的结构为(data,next)，试写出算法：按递增次序输出单链表中各结点的数据元素，并释放结点所占的存储空间。(要求：不允许使用数组作辅助空间)
单链表定义如下：

  ```c
typedef struct LNode {
    int data;
    struct LNode *next;
} LNode, *LinkList;
  ```
  统一使用如下函数名：`void  MiniDelete(LinkedList  head)`


**答案：**
```C
void MiniDelete(LinkedList head)
// head是带头结点的单链表的头指针，本算法按递增顺序输出单链表中各结点的数
// 据元素，并释放结点所占的存储空间。
{
    while (head->next != null) // 循环到仅剩头结点
    {
        pre = head;    // pre为元素最小值结点的前驱结点的指针
        p = pre->next; // p为工作指针
        while (p->next != null) {
            if (p->next->data < pre->next->data)
                pre = p; // 记住当前最小值结点的前驱
            p = p->next;
        }
        printf(pre->next->data); // 输出元素最小值结点的数据
        u = pre->next;
        re->next = u->next;
        free(u); // 删除元素值最小的结点,放结点空间
    }

    free(head);
} // 释放头结点

```

### 第二题

已知一二叉树中结点的左右孩子为lchild和rchild，p指向二叉树的某一结点。请用C语言编一个非递归函数PostFirst (p)，求p所对应子树（即以p为根的子树）的第一个后序遍历结点。

二叉树采用二叉链表表示，定义如下：

```C
typedef struct BiTNode {
    TElemType data;
    struct BiTNode *lchild, *rchild;
} BiTNode, *BiTree;
```

统一使用如下函数名：`BiTree PostFirst(p)`

**答案：**

```C
BiTree PostFirst(BiTree p) {
    BiTree q = p;
    if (!q) return nullptr;
    
    while (q->lchild || q->rchild) { // 找最左下的叶子结点
        if (q->lchild) {
            q = q->lchild; // 优先沿左分枝向下去查“最左下”的叶子结点
        } else {
            q = q->rchild; // 沿右分枝去查“最左下”的叶子结点
        }
    }
    
    return q;
}

```

