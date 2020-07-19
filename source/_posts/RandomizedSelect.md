---
title: 选择算法(随机)
date: 2020/7/12
categories:
- 算法
---

***基于《算法导论》中的伪代码 以及部分网络资源***



# 期望为线性时间的选择算法

```c++
#include "quicksort.hpp"
//Based on QUICKSORT->RANDOMIZED_PARTITION() in 各类排序算法（学习ing）
int RANDOMIZED_SELECT(int *arr, int p, int r, int index)
{
    if (p == r)
    {
        return arr[p];
    }
    int q = RANDOMIZED_PARTITION(arr, p, r);
    int k = q - p + 1;

    //Recursively search for one of the three areas
    if (index == k)
    {
        return arr[q];
    }
    else if (index < k)
    {
        return RANDOMIZED_SELECT(arr, p, q - 1, index);
    }
    else
    {
        return RANDOMIZED_SELECT(arr, q + 1, r, index - k);
    }
}
```

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------