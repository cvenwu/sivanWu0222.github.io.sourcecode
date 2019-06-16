---
title: C++ primer 中文版 5th edition Unit 5--语句篇
tags:
  - C++
  - Grammar
share: true
categories:
  - C++
  - Grammar
reward: true
comment: true
top: 1
repo: sivanWu0222 | CppProgramming
date: 2018-06-28 19:11:53
---

> 对C++中常用的结构和语句进行描述，包括选择结构，顺序结构，循环结构等和一些常用的语句！

## 空语句

> 空语句是最简单的语句，只有一个单独的分号

```C++
	;//这是一个空语句
```

### 使用
1. 当我们在coding时，语法上需要一条语句而逻辑上并不需要，此时我们应该使用空语句
2. 当我们在循环的全部工作可以在条件语句中完成时，这时我们就会用到空语句


```C++

/*重复读入数据直至到达文件末尾或者某次输入的值等于sought*/
	while(cin >> s && s != sought)
		;

```

注意事项：
1. 当我们使用空语句的时候应该加上注释，从而让阅读这段代码的人知道我们是故意省略的
2. 当使用空语句的时候应该注意分号的位置。
3. 多余的空语句并非总是乌海的
```
	while(iter != svec.end())	;		//由于多了个分号，循环将会变为死循环
		++iter;							//不属于循环的一部分
```

## 悬垂else

> 当我们遇到if语句多于else语句时，如何确定哪个if语句和哪个else语句匹配，这个问题便称为悬垂else(dangling else)

在那些既有if语句又有if else语句中，这是个普遍的问题，不同语言中解决思路不同，C++中，规定else与离他最近的尚未匹配的if匹配，从而消除程序的二义性


<!--more-->

## switch语句

> switch语句对紧跟在后面的表达式求值，可以是一个初始化的变量声明，然后表达式的值转换成整数类型，与每个case的值进行比较

注意：case标签必须是常量表达式

```C++
switch(ch)
{
	case 3.14:	//错误，case标签不是一个整数
	case ival:	//错误，case标签不是一个常量
	
}
```

### 统计元音字母出现次数

> 这里有两种写法，重点关注switch语句中的写法

写法一：
```
#include<iostream>
#include<string>
using namespace std;

/**
 * 判断单词中元音字母的个数
 * @return [description]
 */

int main()
{
	unsigned vowelCnt = 0;
	string str;
	cin >> str;
	while ( !str.empty() )
	{
		for (auto s : str)
		{
			switch (s)
			{
				case 'a':
				case 'e':
				case 'i':
				case 'o':
				case 'u':
					vowelCnt++;
					break;
			}
		}
	}
	cout << vowelCnt;
	return 0;

}

```
> 上面代码中，几个case连在一起没有break语句，只要ch是元音字母，就会执行上面所述的代码

写法二：
```
#include<iostream>
#include<string>
using namespace std;

/**
 * 得到元音字母个数的另一种写法
 * @return [description]
 */

int main()
{
  string str;
  unsigned int vowelCnt = 0;
  cin >> str;
  if (!str.empty())
  {
    for (auto s : str)
    {
      switch(s){
        case 'a': case 'e': case 'i': case 'o': case 'u':   //这里可以放到一行，需要注意一下
          vowelCnt ++;
      }
    }
  }


  cout << vowelCnt << endl;

  return 0;
}

```

注意事项：当我们省略case后面的break语句，最好加一段注释说明程序的逻辑

### default标签
> 我们在switch语句中写出一个空的deafult语句也是有用的，因为我们可以告诉其他人，我们已经考虑到了默认情况





## 异常语句

throw表达式：throw引发（raise）异常

try语句块内声明的变量在块外部无法访问，特别是在catch子句内也无法访问。

### 异常的处理过程
异常发生之后，首先搜索异常附近的函数。没找到匹配的catch语句，终止当前正在寻找的函数的寻找，将会向调用者继续寻找，一步一步进行找，直到找到为止！

注意：
1.try语句块内声明的变量无法在外部访问，特别是在catch子句内部也无法访问
2.异常中断了程序的正常流程：当程序遇到异常而中断时，一部分程序不会执行，从而导致对象处于无效或者未完成的状态，如果在发生异常时，进行了正常的清理的程序称作异常安全代码
3.没有try语句块时发生异常，系统会调用terminate函数并终止当前程序的执行


```C++
#include<iostream>
#include<stdexcept>
using namespace std;

/**
 * 修改你的程序，使得当第2个数为0时，抛出异常，
 * 先不要设定catch语句，运行程序并真的为除数输入0
 * @return [description]
 */
int main()
{
    int num1, num2;
    while(cin >> num1 >> num2)
    {
        try {
            if(num2 == 0)
                throw runtime_error("Exception：num2 must not be Zero.");
            double num3 = 1.0 * num1 / num2;
            cout << "The result is " << num3;
            break;
        } catch (runtime_error err) {
            cout << err.what() << "\nTry again? Enter y or n" <<endl;
            char c;
            cin >> c;
            if(!cin || c == 'n')
                break;
        }
    }
    return 0;
}

```

### what()函数
> what函数返回c风格的字符串的内容与异常对象的类型有关，如果异常类型是一个字符串初始值，则返回该字符串，对于其他无初始值的异常类型来说，what返回的内容由编译器决定