---
title: C 冒泡排序
tags:
  - Algorithm
  - C
share: true
categories:
  - Algorithm
  - C
reward: true
comment: true
top: 1
repo: sivanWu0222 | CProgrammingLanguage
date: 2018-07-01 09:56:34
---

## 冒泡排序
> 使用C语言进行冒泡排序，每次都把当前找到的最大的放到最后一个，所以只需要找前面的n - 1 -i个数

```C

#include<stdio.h>
#define MAX_NUM 5


/**
	@brief:打印数组的内容
	@param:数组的指针 
*/ 
void printArray(int *num)
{
	int i;
	for(i = 0; i < MAX_NUM; i++)
	{
		printf("%d\t",*(num + i));
	} 
	printf("\n");
}



/**
	@brief:为数组元素赋值
	@param:数组指针 
**/
void scanArray(int *num)
{
	int i;
	for(i = 0; i < MAX_NUM; i++)
	{
		scanf("%d",&(*(num + i)));
	}
}


/**
	@brief:选择排序算法
	@param:数组指针 
**/
void selectionSort(int *num)
{
	int i,j,min,temp;
	for(i = 0; i < MAX_NUM; i++)
	{
		min = i;
		for(j = i + 1; j < MAX_NUM; j++)
		{
			if(*(num+min) > *(num+j))
			{
				min = j;
			}
		}
		
		temp = *(num+i);
		*(num+i) = *(num+min);
		*(num+min) = temp;
	}
}


/**
	@brief:冒泡排序
	@param:数组指针 
**/
void bubbledSort(int *num)
{
	int i,j,min;
	for(i = 0; i < MAX_NUM - 1; i++)
	{
		for(j = 0; j < MAX_NUM - 1 - i; j++)			
		//这里需要注意是每次都会把最大放到最后，所以每次只需要管前面的就可以 
		{
			if(*(num + j) > *(num + j + 1))
			{
				min = *(num + j + 1);
				*(num + j + 1) = *(num + j);
				*(num + j) = min;
			}
		}
		printArray(num);
	}
}

int main()
{
	int num[MAX_NUM];
	scanArray(num);
	bubbledSort(num);
	printArray(num);
	return 0;
 } 

```

<!--more-->