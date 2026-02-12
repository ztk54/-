---
tags:
  - C语言
  - rand函数
时间: 2026-02-09T17:33:00
---
rand()函数用于生成随机数，可是实际生成的是伪随机数，是基于种子的随机数，因此我们需要让种子随机化，可以运用srand函数，srand函数可以生成不同的种子。同时我们基于time.h库函数可以实现以下功能。
srand函数接收一个unsigned int 类型的数值，所以传参的时候要进行强制转换。
```c
	#include <stdio.h>
	#include <stdlib.h>
	#include <time.h>
	srand((unsigned int)time(NULL));
	int a=rand()%100+1;//这里就能实现1~100随机数的生成;
```

> [!tips] 要注意在一次程序中不要频繁调用`srand()`函数，因为这个函数也是基于算法生成的，频繁使用会表现出规律性，可能会不符合随机数生成的要求。

接下来我将用代码简单实现猜数游戏的功能
```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <stdlib.h>
#include <time.h>
void menu();
void game();
int main()
{
	srand((unsigned int)time(NULL));
	int choice = 1;
	do
	{
		menu();
		scanf("%d", &choice);
		switch (choice)
		{
		case 1:
			game();
			break;
		case 0:
			printf("退出游戏");
			break;
		default :
			printf("输入错误");
		}
	} while (choice);
}
void menu()
{
	printf("*****************************\n");
	printf("*********输入1开始游戏********\n");
	printf("*********输入0退出游戏********\n");
	printf("*****************************\n");
}

void game()
{
	int count = 0;
	int m;
	do
	{
		printf("请问您要尝试几次\n");
		m=scanf("%d", &count); 
		if (m!=1)
		{
			printf("输入有错，请您重新输入\n");
			while (getchar() != '\n');//清除缓冲区
			continue;
		}
	} while (m!=1);
	int number = rand() % 100 + 1;
	do
	{
		int guess = 0;
		printf("请输入您的猜测\n");
		scanf("%d", &guess);
		if (guess > number)
		{
			printf("您猜大了\n");
		}
		else if (guess == number)
		{
			printf("您猜对了\n");
		}
		else
		{
			printf("您猜小了\n");
		}
		count--;
	} while (count);
	if (count == 0)
	{
		printf("很遗憾，您输了，这个数字是%d", number);
	}
}
```
> [!warning]
>猜数字游戏里面出现了两个逻辑漏洞
>* 在猜对数字的时候少加了break跳出循环
>* 在增强程序健壮性那里使用了一个死循环。