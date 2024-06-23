---
title: SCAU数据结构历年编程题
categories: 
    - 数据结构
---
## 2016

2. 编写算法，设计判断一棵二叉树是否是完全二叉树的算法。

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
统一使用如下函数名：int IsCompleteTree(BTree T)

```c++
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

2. 一个具有 n 个结点的完全二叉树采用顺序存储方式，其数据存放在整型一维数组 `a` 中，编写非递归算法对其进行先序遍历。算法可以直接调用栈的基本操作：

- 初始化：`InitStack(SqStack &S)`
- 入栈：`Push(SqStack &S, int e)`
- 出栈：`Pop(SqStack &S, int &e)`
- 判断栈空：`Empty(SqStack S)`

统一使用函数名：`void preorder(int a[], int n)`

```cpp
//ANSWER
void preorder(int a[], int n) {
    SqStack S;
    InitStack(S);
    int index = 0;

    while (index < n || !Empty(S)) {
        while (index < n) {
            // 访问当前结点
            printf("%d ", a[index]);
            // 当前结点入栈
            Push(S, index);
            // 访问左孩子
            index = 2 * index + 1;
        }
        if (!Empty(S)) {
            // 结点出栈
            Pop(S, index);
            // 访问右孩子
            index = 2 * index + 2;
        }
    }
}
```