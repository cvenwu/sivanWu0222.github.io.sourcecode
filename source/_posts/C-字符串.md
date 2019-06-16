---
title: C-字符串
tags:
  - C
  - Grammar
share: true
categories:
  - C
  - Grammar
reward: true
comment: true
top: 1
repo: sivanWu0222 | CProgrammingLanguage
date: 2018-07-02 09:58:58
---





# 字符串

C/C++中，字符串有两种形式：
1. 字符串常量	"Hello World"
2. 字符串变量	存放于字符数组中，该字符数组中包含一个'\0'字符，代表字符串的结尾，将用来存放字符串数组的变量称为字符串变量。


## 字符串常量

注意事项：
1. 字符串常量内存中所占的字节数等于字符数目+1，因为末尾要存放一个'\0'字符，是二进制数0的ASCII码。C/C++中的字符串都是以'\0'结尾的
2. ""是合法的字符串常量，该字符串里面是一个空字符串，称为“空串”，但是仍会占用1个字节的存储空间用来存放代表结束为止的'\0'
3. 字符串常量中包括双引号时，双引号应该写为“\"”，而“\”字符在字符串出现时，应该写为“\\”，“\*”和“\\”仍只占用1个字节

## 字符串变量

1. 在使用scanf输入字符串时，输入的字符串不可以有空格，例如输入"Fox River"则为"Fox"，也就是将会读取空格前面的那部分。
2. 当我们想要将用户输入的一个或者包含多个空格的一整行当做一个字符串读入时，那么应该用gets(字符串变量)
3. 在C/C++对字符或者字符串进行处理时，碰到'\0'就认为字符串结束了。

### 函数gets
原型：char *gets(char *s)
功能：将用户输入的一整行，当做一个字符串读入到s中，并在后面自动加上'\0'

### 函数strcmp
功能：比较两个字符串是否一致，如果一致，返回0

### 函数strcpy
功能：将后面的字符串拷贝到前面那个字符串变量，要注意字符串变量一定要可以装得下后面的字符串。同时要特别注意，拷贝函数会在最后加上一个'\0'

```C


/*书上示例程序*/

#include<stdio.h>
#include<string.h>
int main()
{
	char szTitle[] = "Prison Break";
	char szHero[100] = "Michael scofield";
	char szPrisonName[100];
	char szResponse[100];
	printf("What's the name of the prison in %s?\n", szTitle);
	scanf( "%s", szPrisonName);
	if( strcmp( szPrisonName, "Fox-River") == 0 ) {		//如果返回结果为0，则说明比较结果一致 
		printf("Yeah! Do you love %s?\n", szHero);
	}
	else {
		strcpy( szResponse, "It seems you haven't watched it!\n");	//将后面的内容拷贝到前面的数组中。 
		printf( szResponse);
	}
	szTitle [0] = 't';
	szTitle [3] = 0; //等效于 szTitle [3] = ‘\0’; 因为C/C++在处理字符串时认为遇到\0就会就认为字符串结束了，因此将会输出tri 
	printf(szTitle);
	return 0;
 } 


 
```
 <!--more-->
```
/*编写一个函数，

	int MyItoa(char *s)
	其功能是将 s 中以字符串形式存放的非负整数，转换成相应整数返回。例如，如果 s
	中存放字符串 “1234”，则该函数的返回值就是 1234。假设 s 中的字符全是数字，且不考虑 s 是空串或 s 太长的情况
*/
#include<stdio.h>
#include<string.h>
int MyItoa(char *s)
{
	int num = 0,i;
	for(i = 0; i < sizeof(s) - 1; i++)
	{
		if((*(s + i) >= '0') && (*(s + i) <= '9'))
		{
			num *= 10;
			num += (*(s + i) - 48);
		}
		else if(*(s + i) == '\0')
		{
			printf("结束\n");
			return num;
		}
		else
		{
			printf("Error\n");
			return -1;
		}
	}
	return num;
}

int main()
{
	char num[] = "12034";
	int n = MyItoa(num);
	printf("%d\n",n);
	return 0;
}



```