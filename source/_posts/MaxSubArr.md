---
title: 最大子数组
date: 2020/7/9
categories:
- 算法
---

***基于《算法导论》中的伪代码 以及部分网络资源***



# 最大子数组

### 递归分治：

```c++
/*Method:
  Find the maxmium sub-arr in both sides
  plus together
  record index and sum*/
Res *FIND_MAX_CROSSING_SUBARRAY(int *data, int low, int mid, int high)
{
    //The initial sum should be infinite small
    int left_sum = -10000000;
    int sum = 0;
    int max_left = 0;
    for (int i = mid; i >= low; i--)
    {
        sum += data[i];
        if (sum > left_sum)
        {
            left_sum = sum;
            max_left = i;
        }
    }

    //The initial sum should be infinite small
    int right_sum = -10000000;
    sum = 0;
    int max_right = 0;
    for (int j = mid + 1; j <= high; j++)
    {
        sum += data[j];
        if (sum > right_sum)
        {
            right_sum = sum;
            max_right = j;
        }
    }

    Res *res = new Res(max_left, max_right, left_sum + right_sum);
    return res;
}

Res *FIND_MAXMIUM_SUBARRAY(int *data, int low, int high)
{
    //base case
    if (high == low)
    {
        Res *res = new Res(low, high, data[low]);
        return res;
    }
    else
    {
        int mid = floor((low + high) / 2);
        Res *left = FIND_MAXMIUM_SUBARRAY(data, low, mid);
        Res *right = FIND_MAXMIUM_SUBARRAY(data, mid + 1, high);
        Res *cross = FIND_MAX_CROSSING_SUBARRAY(data, low, mid, high);
        if ((left->sum >= right->sum) && (left->sum >= cross->sum))
        {
            return left;
        }
        else if ((right->sum >= left->sum) && (right->sum >= cross->sum))
        {
            return right;
        }
        else
        {
            return cross;
        }
    }
}

```



### 线性时间(基于算法导论4.1-5)

```c++
Res *FIND_MAXMIUM_SUBARRAY_WZOUT_RECUSION(int *data, int len)
{
    int max_left = 0;
    int max_right = 0;
    //recorded max sum
    int max_sum = -10000000;
    //sum thatchanging every loop
    int sum = 0;
    //record where the maxmium sub-arr start
    int cursor = 0;
    for (int i = 0; i < len; i++)
    {
        sum += data[i];
        if (sum > max_sum)
        {
            max_sum = sum;
            max_left = cursor;
            max_right = i;
        }
        if (sum < 0)
        {
            sum = 0;
            cursor = i + 1;
        }
    }

    Res *res = new Res(max_left, max_right, max_sum);
    return res;
}
```

