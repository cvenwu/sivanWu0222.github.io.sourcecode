---
title: '[LeetCode]171-Excel表列序号.md'
tags:
  - LeetCode
  - Job
share: true
categories:
  - LeetCode
reward: true
comment: true
top: 2
repo: 'sivanWu0222 | LeetCode-MySolution'
date: 2019-09-10 13:08:53
description:
---

# 171-Excel表列序号

[题目内容](https://leetcode-cn.com/problems/excel-sheet-column-number/)

> 其实这道题就是一个进制转换题，把26个字母看作26进制，只是进制从1开始到26


## 第一次提交

```C
int titleToNumber(char * s){
    int i = 0, sum = 0;
    while (s[i] != '\0')
    {
        sum = sum * 26 + s[i] - 64;             //在输入最后一个测试用例的时候("FXSHRXW")首先会产生溢出，之后才会执行减去64的操作
        i++;
    }
    return sum;
}

```


> 结果：999 / 1000 个通过测试用例
> 执行出错信息：Line 5: Char 24: runtime error: signed integer overflow: 2147483624 + 87 cannot be represented in type 'int' (solution.c)
> 最后执行的输入："FXSHRXW"



总结：在最后一个测试用例执行的时候，发生了int溢出，因为FXSHRXW执行到最后一个字母时，sum*26刚执行完还没有发生溢出，直到加上s[i]才发生溢出，所以考虑将***-64移动到前面，避免溢出***

分析：
1. FXSHRX执行完之后，sum取值是82595524
2. sum * 26 = 2147483624
3. sum * 26 + s[i] = 2147483624 + 87 因为此时s[i]是最后一个字母W对应的ASCII编码，发生溢出
4. 故猜想把后面的-64提到前面避免溢出，最后跑了一遍可行



## 第二次提交


```c
int titleToNumber2(char * s){
    int i = 0, sum = 0;
    while (s[i] != '\0')
    {
        // sum = sum * 26 + s[i] - 64;
        // 为了防止溢出，修改加的顺序, 要把-64提到前面
        sum = sum * 26 - 64 + s[i];
        i++;
    }
    return sum;
}

```

> 第二次提交：成功
> 执行用时 :8 ms, 在所有 C 提交中击败了35.51%的用户
> 内存消耗 :6.7 MB, 在所有 C 提交中击败了30.43%的用户

![第二种解法](./2.png)


## 总结
``C 语言int 范围-2147483648 ~ 2147483647``


<!--more-->