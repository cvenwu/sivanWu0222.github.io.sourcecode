---
title: C 选择排序
tags:
  - C
  - Algorithm
share: true
categories:
  - Algorithm
  - C
reward: true
comment: true
top: 1
repo: sivanWu0222 | CProgrammingLanguage
date: 2018-06-30 22:53:25

---

## 排序模板

```C
#include<stdio.h>
#define MAX_NUM 10

/*输出数组*/ 
void printArray(int *n)
{
	int i;
	for( i = 0; i < MAX_NUM; i++ )
	{
		printf("%d\t", *(n + i));
	}
}

/*输入数组*/
void scanArray(int *n)
{
	int i;
	for (i = 0; i < MAX_NUM; i++)
	{
		scanf("%d",&(*(n+i)));
	}
}

int main()
{
	int an[MAX_NUM];
	scanArray(an);
	printArray(an);
} 


```

<!--more-->

-------

## 选择排序
> 所谓选择排序，就是每次选择当前的那个与后面未排序的最小的进行交换



之前自己写的错误的选择排序：
```C
#include<stdio.h>
#define MAX_NUM 10

/*输出数组*/ 
void printArray(int *n)
{
	int i;
	for( i = 0; i < MAX_NUM; i++ )
	{
		printf("%d\t", *(n + i));
	}
	printf("\n");
}

/*输入数组*/
void scanArray(int *n)
{
	int i;
	for (i = 0; i < MAX_NUM; i++)
	{
		scanf("%d",&(*(n+i)));
	}
}

int main()
{
	int an[MAX_NUM];
	scanArray(an);
	
	int i,j,temp,index,min;			//min存放每次循环最小的数，temp存放临时数字，index存放最小的数对应的索引 
	for(i = 0; i < MAX_NUM; i++)
	{
		
		min = an[i];
		printArray(an);

		printf("min：%d\t", min);

		for(j = i + 1; j < MAX_NUM; j++)
		{
			if(min > an[j])
			{
				min = an[j];
				index = j;	
			}	
		}
		
		
		printf("min：%d\t", min);
		temp = an[i];
		an[i] = min;
		an[index] = temp;
		
		printArray(an);

			
	} 
	
	
	printArray(an);
} 


```


改正之后的程序：
```C
#include<stdio.h>
#define MAX_NUM 10

/*输出数组*/ 
void printArray(int *n)
{
	int i;
	for( i = 0; i < MAX_NUM; i++ )
	{
		printf("%d\t", *(n + i));
	}
	printf("\n");
}

/*输入数组*/
void scanArray(int *n)
{
	int i;
	for (i = 0; i < MAX_NUM; i++)
	{
		scanf("%d",&(*(n+i)));
	}
}

int main()
{
	int an[MAX_NUM];
	scanArray(an);
	
	int i,j,temp,index,min;			//min存放每次循环最小的数，temp存放临时数字，index存放最小的数对应的索引 
	for(i = 0; i < MAX_NUM; i++)
	{
		
		min = an[i];
		index = i;				//之前的问题就是这里没有初始化的问题 

		printArray(an);

		printf("min：%d\t", min);

		for(j = i + 1; j < MAX_NUM; j++)
		{
			if(min > an[j])
			{
				min = an[j];
				index = j;	
			}	
		}
		
		
		printf("min：%d\t", min);
		temp = an[i];
		an[i] = min;
		an[index] = temp;
		
		printArray(an);

			
	} 
	
	
	printArray(an);
} 

```


书上的程序：
```C
#include<stdio.h>
#define MAX_NUM 10

/*输出数组*/ 
void printArray(int *n)
{
	int i;
	for( i = 0; i < MAX_NUM; i++ )
	{
		printf("%d\t", *(n + i));
	}
	printf("\n");
}

/*输入数组*/
void scanArray(int *n)
{
	int i;
	for (i = 0; i < MAX_NUM; i++)
	{
		scanf("%d",&(*(n+i)));
	}
}

int main()
{
	int an[MAX_NUM];
	scanArray(an);
	
	int i,j,temp,index,min;			//min存放每次循环最小的数，temp存放临时数字，index存放最小的数对应的索引 
	for(i = 0; i < MAX_NUM; i++)
	{
		min = i;
		printArray(an);

		printf("1min：%d\t", min);

		for(j = i + 1; j < MAX_NUM; j++)
		{
			if(an[min] > an[j])
			{
				min = j;
			}	
		}
		temp = an[i];
		an[i] = an[min];
		an[min] = temp;	
		printf("min：%d\t", min);
		printArray(an);
	} 
	
	printArray(an);
} 


```
