---
title: 简单的栈和队列(真的很简)
date: 2020/7/13
categories:
- 算法
---

***基于《算法导论》中的伪代码 以及部分网络资源***



# Stack

```c++
class Stack
{
private:
    int *m_stack;
    int max_size;
    int m_top;

public:
    Stack(int size)
    {
        this->max_size = size;
        this->m_top = -1;
        this->m_stack = new int[this->max_size];
    };
    bool STACK_EMPTY();
    void PUSH(int x);
    int POP();
    void PRINTER();
    ~Stack() { delete[] this->m_stack; };
};
```

```c++
bool Stack::STACK_EMPTY()
{
    if (this->m_top == 0)
    {
        return true;
    }
    else
    {
        return false;
    }
}
```

```c++
void Stack::PUSH(int x)
{
    if (this->m_top <= this->max_size - 1)
    {
        this->m_top++;
        this->m_stack[this->m_top] = x;
    }
    else
    {
        //Full
        this->max_size++;
        this->m_top++;
        int *temp = new int[this->max_size];
        for (int i = 0; i < this->max_size - 1; i++)
        {
            temp[i] = this->m_stack[i];
        }
        temp[this->max_size - 1] = x;

        delete[] this->m_stack;
        this->m_stack = temp;
    }
}
```

```c++
int Stack::POP()
{
    if (this->STACK_EMPTY())
    {
        throw "Underflow";
    }
    else
    {
        this->m_top--;
        return this->m_stack[this->m_top + 1];
    }
}
```

```c++
void Stack::PRINTER()
{
    for (int i = 0; i <= this->m_top; i++)
    {
        cout << this->m_stack[i] << " ";
    }
    cout << endl;
}
```

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------



# Queue

```c++
class Queue
{
private:
    int *m_queue;
    int m_head;
    int m_tail;
    int max_size;
    int temp_size;

public:
    Queue(int size)
    {
        this->m_queue = new int[size];
        this->max_size = size;
        this->m_head = 0;
        this->m_tail = 0;
        //current num of elements in queue
        this->temp_size = 0;
    };
    ~Queue() { delete[] this->m_queue; };
    void ENQUEUE(int x);
    void PRINTER();
    int DEQUEUE();
};
```

```c++
void Queue::ENQUEUE(int x)
{
    if (this->m_head == this->m_tail && this->temp_size != 0)
    {
        throw "Queue if full";
    }

    this->m_queue[this->m_tail] = x;
    this->temp_size++;
    if (this->m_tail == this->max_size - 1)
    {
        this->m_tail = 0;
    }
    else
    {
        this->m_tail++;
    }
}
```

```c++
int Queue::DEQUEUE()
{
    if (this->temp_size == 0)
    {
        throw "Queue is empty";
    }

    int x = this->m_queue[this->m_head];
    if (this->m_head == this->max_size - 1)
    {
        this->m_head = 0;
    }
    else
    {
        this->m_head++;
    }

    this->temp_size--;
    return x;
}
```

```c++
//print the queue in order
void Queue::PRINTER()
{
    if (this->m_head < this->m_tail)
    {
        for (int i = this->m_head; i < this->m_tail; i++)
        {
            cout << this->m_queue[i] << " ";
        }
        cout << endl;
    }
    else if (this->m_head > this->m_tail)
    {
        for (int i = this->m_head; i < this->max_size; i++)
        {
            cout << this->m_queue[i] << " ";
        }
        for (int i = 0; i < this->m_tail; i++)
        {
            cout << this->m_queue[i] << " ";
        }
        cout << endl;
    }
    else if (this->temp_size == 0)
    {
        cout << "Queue is empty" << endl;
    }
    else if (this->m_head == this->m_tail && this->temp_size != 0)
    {
        cout << "Queue is full" << endl;
        for (int i = this->m_head; i < this->max_size; i++)
        {
            cout << this->m_queue[i] << " ";
        }
        for (int i = 0; i < this->m_tail; i++)
        {
            cout << this->m_queue[i] << " ";
        }
        cout << endl;
    }
}

```

