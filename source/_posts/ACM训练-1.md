---
title: ACM训练-1
tags:
  - ACM
  - Algorithm
share: true
categories:
  - ACM
  - Easy
reward: true
comment: true
top: 1
repo: sivanWu0222 | CppProgramming
date: 2018-06-29 18:52:46
---

## 打印点坐标

> 假如在二维坐标系中有很多点，每一个点都有坐标(x,y),打印在以(100,100)为圆心，100为半径里面所有的点的坐标。

```C++

/**
 * 假如在二维坐标系中有很多点，每一个点都有坐标(x,y),
 * 打印在以(100,100)为圆心，100为半径里面所有的点的坐标。
 */
#include<iostream>
#include<cmath>
using namespace std;
int main()
{
  for (int x = 0, y = 0; x <= 200; x++)
  {
    y = 0;
    while(y <= 200)
    {
      if(sqrt(pow(x - 100, 2) + pow(y -100, 2)) <= 100)
        cout << "(" << x << "," << y << ")" << "\n";
      y++;
    }
  }

}

```

## 求100以内的水仙花数

> 求1000以内所有的“水仙花数”，”水仙花“是一个三位数，个位，十位，白位的数字的立方和等于本身。例如153 = 1+ 125 + 27

```C++
/**
 * 求1000以内的水仙花数：个位 十位 百位 立方和等于本身
 */

#include<iostream>
#include<cmath>
using namespace std;
int main()
{
  for (int i = 999; i >= 100 && i <= 999; i--)
  {
    if (i == pow(i%10, 3) + pow(i/100, 3) + pow(i/10%10, 3))
    {
      cout << i << endl;
    }
  }
  return 0;
}

```

<!--more-->

## 求质数
> 输入n,求n以内所有的质数，(使用最优算法)

```C++
/**
 * 输入n，求n以内的所有质数
 */
#include<iostream>
#include<cmath>
using namespace std;
int main()
{
  int n = 0, temp = 0;
  cout << "Input a number:";
  cin >> n;
  for (int j = 2; j <= n; j++)
  {
    temp = 0;   //表示质数
    for (int i = 2; i <= sqrt(j); i++)
    {
      if (j % i == 0)
	  {
		  temp = 1;
		  break;
	  }
    }
    if(temp == 0)
      cout << j << endl;
  }

  return 0;
}

```


## 拆分成正整数的和
> 连续正整数之和，一个整数有可能可以表示为n个连续的正整数之和，写一个程序，输入任意数，找出所有可能的序列

```C++
/**
 * 连续正整数之和，一个整数有可能可以表示为n个连续的正整数之和，
 * 写一个程序，输入任意数，找出所有可能的序列。
 */


#include<iostream>
using namespace std;
int main()
{
  int num;
  cin >> num;
  //n就是个数，表示的个数
  for (int n = 2; n <= num / 2; n++)
  {

    if (num % n == (n * (n - 1)) / 2 % n) //判断可否被拆分
    {
      for (int i = (num * 2 - (n - 1) * n) / (2 * n), count = 1; count <= n && i > 0; count++,i++)
      {
        cout << i << "\t";
      }
      cout << endl;
    }
  }


// n * s + (n - 1) * n / 2 = num;
// n * s * 2 + (n - 1) * n = num * 2
// s = (num * 2 - (n - 1) * n) / (2 * n)
// n * s + (n - 1)! = num
//用n个数表示则为 最小的那个数乘以n + (n-1)!
// 条件就是 给定的数 % n = (n * (n - 1)) / 2 % n;
//最小的数 * n + (n * (n - 1)) / 2;
// n ^ 2 - n + 2 * n * 最小的数 / 2 = num
// (num * 2 - 2 * ( n ^ 2 ) + 2 * n) / (2 * n)
// 给定的数 * 2 / n = n - 1 + 2 * 最小的数


  return 0;
}


```

## 打印九九乘法表

```C++
/**
 * 打印一个9*9乘法表。打印到开发板。
 */

#include<iostream>
using namespace std;
int main()
{
  for (int i = 1; i <= 9; i++)
  {
    for (int j = 1; j <= i; j++)
      cout << i << "*" << j << "=" << j * i << "\t";
    cout << endl;
  }
  return 0;
}

```

## 写一个能输入10个数的冒泡排序
```C++
/**
 * 写一个能输入10个数的冒泡排序
 */

#include<iostream>
using namespace std;

int main()
{
  int num[10];
  for (int i = 0; i <= 9; i++)
    cin >> num[i];

  int temp = 0;
  for(int i = 0; i <= 9; i++)
  {
    for(int j = i + 1; j <= 9; j++)
    {
      if(num[i] > num[j])
      {
        temp = num[i];
        num[i] = num[j];
        num[j] = temp;
      }
    }
  }

  for(int i = 0; i <= 9; i++)
    cout << num[i] << "\t";

  return 0;
}

```
