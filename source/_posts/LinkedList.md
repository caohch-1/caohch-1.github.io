---
title: 简单的双向链表
date: 2020/7/13
categories:
- 算法
---

***基于《算法导论》中的伪代码 以及部分网络资源***



# LinkedList

#### 链表中的节点

```c++
struct LinkedNode
{
    LinkedNode(int x)
    {
        key = x;
        prev = nullptr;
        next = nullptr;
    }
    int key;
    LinkedNode *prev;
    LinkedNode *next;
};
```



#### 链表类

```c++
class LinkedList
{
private:
    LinkedNode *head;
    LinkedNode *tail;
    int size;

public:
    LinkedList(int x);
    ~LinkedList();

    //print all nodes in linkedlist
    void PRINTER();

    //find the node whose key==k
    LinkedNode *LIST_SEARCH(int k);

    //insert a node as the head
    void PUSHFRONT(int x);

    //insert a node as the tail
    void PUSHBACK(int x);

    //delete the first node
    void POPFRONT();

    //delete the last node
    void POPBACK();

    //delete the node whose key==x
    void LIST_DELETE(int x);

    //insert a node whose key==x after the node whose key==x
    void LIST_INSERT(int x, int key);
};
```



#### 构造&&析构 

```c++
LinkedList::LinkedList(int x)
{
    LinkedNode *temp = new LinkedNode(x);
    this->head = temp;
    this->tail = temp;
    this->size = 1;
}

LinkedList::~LinkedList()
{
    LinkedNode *temp = this->head;
    while (temp)
    {
        this->head = this->head->next;
        delete temp;
        temp = this->head;
    }
    this->head = nullptr;
    this->tail = nullptr;
    this->size = 0;
}
```



#### 头删&&尾删

```c++
void LinkedList::POPFRONT()
{
    if (this->head == nullptr)
    {
        cout << "Empty linkedlist" << endl;
    }

    LinkedNode *temp = this->head;

    //assign class_head to second node in original linked list
    this->head = this->head->next;
    this->head->prev = nullptr;
    delete temp;
    this->size--;
}

void LinkedList::POPBACK()
{
    //similar to POPFRONT()
    if (this->tail == nullptr)
    {
        cout << "Empty linkedlist" << endl;
    }

    LinkedNode *temp = this->tail;

    this->tail = this->tail->prev;
    this->tail->next = nullptr;
    delete temp;
    this->size--;
}
```



#### 头插&&尾插

```c++
void LinkedList::PUSHFRONT(int x)
{
    LinkedNode *temp = new LinkedNode(x);

    //construct new_head-->old_head
    temp->next = this->head;

    if (this->head != nullptr)
    {
        //construct old_head-->new_head
        this->head->prev = temp;
    }
    //assign class_head to new_head
    this->head = temp;

    //if original linked list is empty, we also should assign tail to new head
    if (this->tail == nullptr)
    {
        this->tail = this->head;
    }

    temp->prev = nullptr;
    this->size++;
}

void LinkedList::PUSHBACK(int x)
{
    //similar to PUSHFRONT()
    LinkedNode *temp = new LinkedNode(x);

    temp->prev = this->tail;

    if (this->tail != nullptr)
    {
        this->tail->next = temp;
    }

    this->tail = temp;

    if (this->head == nullptr)
    {
        this->head = this->tail;
    }

    temp->next = nullptr;
    this->size++;
}
```



#### 删除特定值节点

```c++
void LinkedList::LIST_DELETE(int x)
{
    //first locate the node will be deleted
    LinkedNode *target = this->LIST_SEARCH(x);
    if (target == nullptr)
    {
        return;
    }
    else if (target == this->head)
    {
        this->POPFRONT();
    }
    else if (target == this->tail)
    {
        this->POPBACK();
    }
    else
    {
        //node_a --- target_node --- node_b

        //construct node_a --> node_b
        target->next->prev = target->prev;

        //construct node_a <-- node_b
        target->prev->next = target->next;

        //now: node_a --- node_b
        delete target;
        this->size--;
    }
}
```



#### 在特定值节点后插入新节点

```c++
void LinkedList::LIST_INSERT(int x, int key)
{
    //first locate the node will be inserted
    LinkedNode *location = this->LIST_SEARCH(x);
    if (location == this->tail)
    {
        this->PUSHBACK(key);
    }
    else if (location)
    {
        LinkedNode *temp = new LinkedNode(key);
        //node_a(location) --- node_b

        //construct new_node --> node_b
        temp->next = location->next;

        //construct new_node <-- node_b
        location->next->prev = temp;

        //construct node_a --> new_node
        location->next = temp;

        //construct node_a <-- new_node
        temp->prev = location;

        //now: node_a --- new_node --- node_b
        this->size++;
    }
}
```



#### 根据值查询某节点位置

```c++
LinkedNode *LinkedList::LIST_SEARCH(int k)
{
    //traverse linked list
    LinkedNode *x = this->head;
    while (x != nullptr && x->key != k)
    {
        x = x->next;
    }

    if (x == nullptr)
    {
        cout << "No such element in this linkedlist" << endl;
    }

    return x;
}
```



#### 打印全链表

```c++
void LinkedList::PRINTER()
{
    LinkedNode *temp = this->head;
    if (this->head == nullptr && this->tail == nullptr)
    {
        cout << "Empty linkedlist" << endl;
        return;
    }

    cout << "Node number: " << this->size << endl;
    while (temp)
    {
        cout << temp->key << " --- ";
        temp = temp->next;
    }
    cout << endl;
}
```

