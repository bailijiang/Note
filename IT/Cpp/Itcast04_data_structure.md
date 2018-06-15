#### 1. 算法效率评估
* 大O表示法
    - 时间复杂度: 指令执行条数
        - 2层嵌套循环: O($n^2$)
    - 空间复杂度: 内存占用大小

#### 2. 提高效率的方法: 用空间换时间
* 2层将嵌套for循环拆分成2个单独的for循环 O($n^2$) --> O(n)
 
#### 3. STL map insert返回值
* map.insert 返回值为   `std::pair<iterator,bool> insert( P&& value );`
可以利用其 bool 值判断是否insert成功
* 在一个由自然数1-1000中某些数字所组成的数组中，每个数字可能出现零次或者多次。
    设计一个算法，找出出现次数最多的数字。
```
#define _CRT_SECURE_NO_WARNINGS

#include <iostream>
#include<map>

using namespace std;

void search_max(int* array, int len) {
    map<int, int> M;
    for (int i = 0; i < len; i++) {
        pair<map<int, int>::iterator, bool> pairIt = M.insert(pair<int, int>(array[i], 1));
        if (pairIt.second != true) {
            M[array[i]]++;
        }
    }

    int max = 0;
    int data = 0;
    for (int i = 0; i < len; i++) {
        if (M[array[i]] > max) {
            max = M[array[i]];
            data = array[i];
        }
    }
    cout << "max times data: " << data << endl;
    cout << "max times: " << max << endl;
}

int main() {

    int t[] = { 1,2,3,5,4,6,8,7,9,7,8,4,5,6,3,2,12,1,1,1,1,2,2,2,2,5,5,5,5 };
    int len = sizeof(t) / sizeof(int);
    
    search_max(t, len);

}
```

#### 4. 根据数据之间的关联关系来选择数据结构
* 1:1   线性表, 数组, vector, list链表, queue, dequeue
* 1:N   二叉树/红黑树, map/mutimap set/multiset
* N:N   Graph

#### 5. 实现线性表数据结构
* C语言没有bool类型, 1 真, 0 假
* 参数中需要写操作带指针\*, 只需要读操作不带\*
* 程序要尽量能够被复用, 功能越单一,被复用的几率就越高
* 写程序要考虑到临界状态的处理: 越界, 为空...
* 实现过程想不明白时要画图
* 操作线性表需要保证线性表的生命周期， 整个操作过程中其不能被析构

#### 6. 函数传参 2级指针
* 要在函数内部操作外边的1级指针， 应该用2级指针传参
* 2级指针传的是1级指针的地址
* 如果用1级指针传参， 将会发生值拷贝， 函数内部对其分配地址是操作的局部变量， 与外边的1级指针没有关系
* 在函数内部， 对2级指针解引用，就可以直接操作外边的1级指针

#### 7. 线性表通用数据类型void*操作
* SqList.h:
```
#ifndef _SQLIST_H
#define _SQLIST_H

#define MAXSIZE 50  // 最大容量
typedef unsigned int ElemType;  // 数据类型
                                // 定义线性表结构体
typedef struct SQLIST
{
    //ElemType data[MAXSIZE];   // 数组
    void* data[MAXSIZE];
    int length; //数组长度 -- 放进去了多少个
}SqList;

#if 0
// 初始化，建立一个空的线性表L。
void InitList(SqList *L);
// 若线性表为空，返回true，否则返回false
int ListEmpty(SqList L);
// 将线性表清空
void ClearList(SqList *L);
// 将线性表L中的第pos个位置的元素返回给e
void GetElem(SqList L, int pos, ElemType *e);
// 在线性表L中的第pos个位置插入新元素e
void ListInsert(SqList *L, int pos, ElemType e);
// 删除线性表L中的第pos个位置元素，并用e返回其值
void ListDelete(SqList *L, int pos, ElemType *e);
// 返回线性表L的元素个数
int ListLength(SqList L);


#else
// 指针是unsigned int 类型
// 初始化，建立一个空的线性表L。
void InitList(SqList *L);
// 若线性表为空，返回true，否则返回false
int ListEmpty(SqList L);
// 将线性表清空
void ClearList(SqList *L);
// 将线性表L中的第pos个位置的元素返回给e
void GetElem(SqList L, int pos, void** e);
// 在线性表L中的第pos个位置插入新元素e
void ListInsert(SqList *L, int pos, void* e);
// 删除线性表L中的第pos个位置元素，并用e返回其值
void ListDelete(SqList *L, int pos, void **e);
// 返回线性表L的元素个数
int ListLength(SqList L);

#endif


#endif // _SQLIST_H
```
* SqList.c:
```
#include "SqList.h"
#include <string.h>

void InitList(SqList * L)
{
    L->length = 0;
    memset(L->data, 0, sizeof(L->data));
}

int ListEmpty(SqList L)
{
    if (L.length == 0) {
        return 1;
    }
    return 0;
}

void ClearList(SqList * L)
{
    InitList(L);
}

void GetElem(SqList L, int pos, void ** e)
{
    if (pos < 0 || pos >= L.length) {
        return;
    }
    if (L.length == 0) {
        return;
    }
    *e = L.data[pos];
}

void ListInsert(SqList * L, int pos, void* e)
{
    //over
    if (pos < 0 || pos > L->length) {
        return;
    }
    //full
    if (L->length == MAXSIZE) {
        return;
    }
    //move
    for (int i = L->length - 1; i >= pos; --i) {
        L->data[i + 1] = L->data[i];
    }
    //=
    L->data[pos] = e;
    //length++
    L->length++;
}

void ListDelete(SqList * L, int pos, void ** e)
{
    //over
    if (pos < 0 || pos >= L->length) {
        return;
    }
    //null
    if (L->length == 0) {
        return;
    }

    *e = L->data[pos];
    for (int i = pos + 1; i < L->length; ++i) {
        L->data[i - 1] = L->data[i];
    }
    L->length--;

}

int ListLength(SqList L)
{
    return L.length;
}
```
* main.c:
```
#include<stdio.h>
#include<stdlib.h>
#include"SqList.h"
#include<string.h>

void main() {
    SqList list;
    InitList(&list);
    char* arr[] = {
        "aaa",
        "bbb",
        "ccc"
    };

    for (int i = 0; i < 3; ++i) {
        ListInsert(&list, i, (void*)arr[i]);
    }
    for (int i = 0; i < ListLength(list); ++i) {
        char* tmp;
        GetElem(list, i, (void**)&tmp);
        printf("Elem: %s\n", tmp);
    }
    
    while (ListEmpty(list) != 1)
    {
        char *tmp;
        ListDelete(&list, 0, (void**)&tmp);
        printf("delete Elem: %s\n", tmp);
    }

    system("pause");

}
```

#### 8. 带头链表LinkList:
* 管理链表结构体
```
typedef struct LINKLIST
{
    int length;
    Node header; // 此处不应该用指针*， 如果用指针*， 就要给header分配内存空间
}LinkList;
```
* 遍历链表需要辅助指针\*， 类型为Node\*， 指向头节点header
* 插入节点，先连后边的指针(newNode.next)
* 清空链表：
```
void ClearList(LinkList * L)
{
    int tmp;
    while (L->length)
    {
        ListDelete(L, 0, &tmp);
    }
}
```

#### 9. 通用数据类型链表的实现
* 使用小链表， 间接将业务节点_LISTNODE链接起来(多了一层NODE)
```
typedef struct NODE
{
    struct NODE *next;  // 指向后继节点的指针
}Node;

// 业务节点(包含一个小链表节点)
typedef struct _LISTNODE
{
    Node node;  // 小链表节点(放在第一个位置, 以便进行强转获取数据)
    void *data; // 指向数据地址的指针
}ListNode;

typedef struct LINKLIST
{
    int length;
    ListNode header;
}LinkList;
```

#### 10. 内存操作
* 内存偏移之前要进行强转来保持规格统一
    - sizeof 的结果是以char为单位的
    - 但Value v不是以char为单位的, 因此必须进行强转
```
Value v;
int a = *(int *)((char*)&v + sizeof(Test));
```

#### 11. 循环链表
* 通常将尾部节点的next指向第一个数据节点
* 游标: 当前辅助指针, 通过游标遍历链表
* 游标移动到链表的下一节点, 但返回的是移动前的节点(游标++)
    - `CircleListNode* CircleList_Next(CircleList* list);`
* 删除节点时, 游标向后移动到有效节点
    - 删除的时头节点时, 先找到尾节点, 使其指向头节点的下一个节点(新的头节点)
* 头部插入时, 要先找到尾部节点, 使其指向新节点pNew, pNew->(head->next), head->pNew
    - 链表为空时, pNew->pNew

#### 12. 双向链表
* 需要逆序访问链表时, 使用双向链表
* 自己画画图, CRUD操作

#### 13. 顺序栈stack
* 先选出栈顶, 通常在栈的尾部

#### 14. SqList / LinkList / DLinkList / SqStack / Tree ... 的特点和使用场景
* ![](image\dataStructure_usage.PNG)

#### 15. 2种链表结构对比
* 2种链表相同点：外部业务数据的构造和析构与链表无关
* 不同点：
    - 数据域和指针域在一起的(数据节点不包括小链表节点)：需要在链表api中开辟和释放小链表Node节点(需要在api中malloc/free Node)
    - 数据域和指针域分开的(数据节点包括小链表节点)：不需要在链表api中开辟和释放小链表Node节点(不需要在api中malloc/free Node)

#### 16. 栈Stack操作
* 在取栈顶元素时, 一定要先做判空操作
```
if (!st.empty()) {
                st.pop();
            }
```
* 判断stack长度: st.size() == 1

#### 17. 队列 Queue
* FIFO 先进先出
* 队尾入栈, 队头出栈
* 队列不能被遍历, 不能insert/delete中间的元素
* 数组队列
    - 队头队尾没有区别
    - 管理队列结构体struct的设计理念: 如何操作这个队列, 需要哪些属性(front, rear, void* data[MAXSIZE])
    - 存储任意数据类型的指针数组: `void* data[MAXSIZE];` 不需要malloc开辟空间

#### 18. 树 二叉树
* 左孩子, 右兄弟表示法, 将多叉树转换成二叉树
* 完全二叉树
    - 满二叉树是完全二叉树的一种特殊情况
    - 完全二叉树: 比如有n层, 从1~n-1层是满二叉树, 第n层最后一个节点前面都挂满了节点
    - ![](image\完全二叉树.PNG)
    - 完全二叉树的顺序存储:
        + 性质5: 对完全二叉树，若从上至下、从左至右编号，则编号为i 的结点，其左孩子编号必为2i，其右孩子编号必为2i＋1；其双亲的编号必为i/2（i＝1 时为根,除外）。

        + ![](image\二叉树的顺序存储.PNG)
        + 便于还原二叉树

* 普通二叉树
    - 采用链式存储
    - 叶子节点一定要把左右孩子赋值为NULL, 即递归退出的条件

#### 19. 遍历二叉树
![](image\二叉树案例.PNG)
```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef struct _tag_BiTNode {
    char data;
    struct _tag_BiTNode* lChild;
    struct _tag_BiTNode* rChild;

}BiTNode;

void xianxuBL(BiTNode *root) {
    if (root == NULL) {
        return;
    }
    printf("%c ", root->data);
    xianxuBL(root->lChild);
    xianxuBL(root->rChild);
}

void zhongxuBL(BiTNode *root) {
    if (root == NULL) {
        return;
    }
    zhongxuBL(root->lChild);
    printf("%c ", root->data);
    zhongxuBL(root->rChild);
}

void zhongxuBL_Leaf_Count(BiTNode *root, int *sum) {
    if (root == NULL) {
        return;
    }
    
    zhongxuBL_Leaf_Count(root->lChild, sum);
    if (!root->lChild && !root->rChild) {
        printf("%c ", root->data);
        *sum += 1;
    }
    zhongxuBL_Leaf_Count(root->rChild, sum);
}

void houxuBL(BiTNode *root) {
    if (root == NULL) {
        return;
    }
    houxuBL(root->lChild);
    houxuBL(root->rChild);
    printf("%c ", root->data);
}
int treeDepth(BiTNode *root) {
    if (root == NULL) {
        return 0;
    }
    int left = treeDepth(root->lChild);
    int right = treeDepth(root->rChild);
    int max = left > right ? left : right;

    return ++max;
}

BiTNode* copyTree(BiTNode *root) {
    if (root == NULL) {
        return NULL;
    }

    BiTNode *newRoot = (BiTNode*)malloc(sizeof(BiTNode));
    BiTNode *left = copyTree(root->lChild);
    BiTNode *right = copyTree(root->rChild);

    newRoot->data = root->data;
    newRoot->lChild = left;
    newRoot->rChild = right;

    return newRoot;
}

void main() {
    BiTNode nodeA, nodeB, nodeC, nodeD, nodeE, nodeF, nodeG, nodeH;
    memset(&nodeA, 0, sizeof(BiTNode));
    memset(&nodeB, 0, sizeof(BiTNode));
    memset(&nodeC, 0, sizeof(BiTNode));
    memset(&nodeD, 0, sizeof(BiTNode));
    memset(&nodeE, 0, sizeof(BiTNode));
    memset(&nodeF, 0, sizeof(BiTNode));
    memset(&nodeG, 0, sizeof(BiTNode));
    memset(&nodeH, 0, sizeof(BiTNode));

    nodeA.data = 'A';
    nodeA.lChild = &nodeB;
    nodeA.rChild = &nodeC;

    nodeB.data = 'B';
    nodeB.lChild = &nodeD;
    nodeB.rChild = &nodeE;

    nodeC.data = 'C';
    nodeC.lChild = &nodeF;
    nodeC.rChild = &nodeG;

    nodeD.data = 'D';
    nodeD.lChild = &nodeH;

    nodeE.data = 'E';
    nodeF.data = 'F';   
    nodeG.data = 'G';
    nodeH.data = 'H';

    printf("先序\n");
    xianxuBL(&nodeA);
    printf("\n中序\n");
    zhongxuBL(&nodeA);
    printf("\n后序\n");
    houxuBL(&nodeA);
    printf("\n");

    printf("中序统计叶子节点数:\n");
    int sum = 0;
    zhongxuBL_Leaf_Count(&nodeA, &sum);
    printf("sum: %d\n", sum);
    
    int depth = treeDepth(&nodeA);
    printf("depth: %d\n", depth);

    BiTNode *newRoot = copyTree(&nodeA);
    printf("copyTree\n");
    xianxuBL(&nodeA);
    printf("\n");
}
```
* 非递归算法中序遍历二叉树

![](image\非递归算法中序遍历二叉树.PNG)

#### 20. 递归算法
* 在递归算法中, 要首先写出递归的退出条件
* 正序操作(printf/compute...)在递归调用之前
* 逆序操作(printf/compute...)在递归调用之后

#### 21. 
