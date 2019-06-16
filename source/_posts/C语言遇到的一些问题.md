---
title: C语言遇到的一些问题
tags:
  - C
  - ApplyMaster
share: true
categories:
  - CCNU
  - C
  - ApplyMaster
reward: true
comment: true
top: 1
repo: sivanWu0222 | ApplyMaster
date: 2018-10-28 14:40:53

---

# 函数调用时候形参赋值

> 函数调用时，实参复制给形参的时候是***从右到左***复制, 并不是从左到右

```C

#include <stdio.h>
void compare (int a, int b);     
void main ( )
{   
    int i = 2;
    compare ( i, i++ );           
    printf ("i = %d\n", i);
}

void compare ( int a, int b ) 
{   
    printf ("a = %d  b = %d\n", a, b);
    if ( a > b)  
        printf ("a > b\n");
    else 
      if (a == b)  
          printf ("a = b\n");
      else 
          printf ("a < b\n");
}

```

### 运行结果

```C
a = 3  b = 2
a > b
i = 3
```



------------

# 字符串转换为整型
> 在控制台执行C语言程序的时候有时候需要传递参数但是我们传递的是const char *类型的参数，我们需要获得参数并将其赋值给整型变量的时候就需要进行类型转换,可以使用atoi()函数

## atoi函数介绍

原型：int  atoi （const  char  *nptr）

用法：#include  <stdlib.h>

功能：将字符串转换成整型数；atoi()会扫描参数nptr字符串，跳过前面的空格字符，直到遇上数字或正负号才开始做转换，而再遇到非数字或字符串时（'\0'）才结束转化，并将结果返回。

说明：atoi()函数返回转换后的整型数。

举例：

```C
/**
 *  编写文本文件显示程序，在命令行中指定文本文件显示的范围。如下所示
 * 
 * type filename m n
 * 其中type为执行文件，filename是要显示的文本文件名，
 * m和n指定了显示的范围，即显示从m行到n行的内容
 * 当m和n不指定的时候，显示文件全部内容
 **/

#include <stdio.h>
#include <stdlib.h>

void main(int argc, char const *argv[])
{
    int m, n;
    m = atoi(argv[2]);
    n = atoi(argv[2]);
	printf("%d %d\n", m, n);
}



```

 
> 转载：https://blog.csdn.net/youbang321/article/details/7887990 

-------------------------------------

# fgets()文件读取函数的一些注意点（变色的重点）

fgets函数读取文件的时候在一行结束的时候,也***会把该行的末尾的'\n'读取进去***，并随后加入'\0'，因此***在使用puts()输出的时候首先会输出字符串中的换行,然后puts()在自动输出的时候也会自动附加换行****

-------------------------------------

# strlen 与 sizeof 的区别

参考: https://blog.csdn.net/SunnyYoona/article/details/39118465

----------------------------------------

# return 与 exit 的区别
参考: https://blog.csdn.net/ligeforrent/article/details/45362959

----------------------------------------

# getch getche 以及 getchar 的区别

参考: https://blog.csdn.net/cxyol/article/details/628324

----------------------------------------

# C语言结构体数组中两个元素的值交换

```C
#include <stdio.h>
#include <stdlib.h>

#define NUM 100

struct Score_Tab
{
    int rank;
    char no[11];
    char name[21];
    int score;
    int time;
};

void main(int argc, char const *argv[])
{
    struct Score_Tab score[100], temp;
    FILE *fp;
    int i, j, k, n, s, t;

    if((fp = fopen("score_tab.txt", "r")) == NULL)              //打开文件失败
    {
        printf("can not open source file: score_tab.txt\n");
        exit(0);
    }

    //从文件中读取数据到结构体数组score中
    n = 0;
    while(fscanf(fp, "%s%s%d%d", score[n].no, score[n].name, &score[n].score, &score[n].time) != EOF)
        n++;
    
    //对结构体数组score按分数降序排列,分数相同则按所用时间升序排列
    for(i = 0; i < n-1; i++)
    {
        k = i;
        for(j = i + 1; j < n; j++)
            if(score[j].score > score[k].score || score[j].score==score[k].score && score[j].time < score[k].time)
                k = j;

        if(k != i)
        {
            temp = score[i];
            score[i] = score[k];
            score[k] = temp;
        }
    }

    //排名次
    j = 0;
    s = 0;
    t = 0;
    for(i = 0; i < n; i++)
    {
        if(score[i].score != s || score[i].time != t)
        {
            j++;
            score[i].rank = j;
            s = score[i].score;
            t = score[i].time;
        }
        score[i].rank = j;
    }

    //显示排名结果
    for(i = 0; i < n; i++)
        printf("%4d %-10s %-20s %3d %3d\n", score[i].rank, score[i].no, score[i].name, score[i].score, score[i].time);
    

    fclose(fp);

}


```


----------------------------------------



<!--more-->