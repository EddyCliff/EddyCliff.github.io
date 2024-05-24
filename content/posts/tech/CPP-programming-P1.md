---
title: "C++编程入门：内存分区模型"
date: 2024-04-30T00:17:58+12:00
lastmod: 2024-04-30T00:17:58+12:00
author: ["Eddy"]
keywords: 
- CPP Programming
- 内存分区模型
categories: 
- 
tags: 
- C++
- C++ 内存分区模型
description: ""
weight: 
slug: ""
draft: false # 是否为草稿
comments: false
reward: false # 打赏
mermaid: true #是否开启mermaid
showToc: true # 显示目录
TocOpen: true # 自动展开目录
hidemeta: false # 是否隐藏文章的元信息，如发布日期、作者等
disableShare: true # 底部不显示分享栏
showbreadcrumbs: true #顶部显示路径
cover:
    image: "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/BlogCover/CPP.png" #图片路径例如：posts/tech/123/123.png
    caption: "" #图片底部描述
    alt: ""
    relative: false
---

<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Blog-Common-Images/%E7%9F%A5%E8%AF%86%E8%92%99%E8%94%BD%E4%BA%86%E6%88%91%E7%9A%84%E5%8F%8C%E7%9C%BC.jpg" alt = "闪亮登场.jpg" width = "70%" height = "auto">
</div>
<br>

## INIT

<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Blog-Common-Images/INIT.jpg" alt = "INIT.jpg" width = "70%" height = "auto">
</div>
<br>

>**INIT：本节内容正式开始。action!**

## 1 内存分区模型

C++程序在执行时，将内存大方向划分为**4个区域**

- 代码区：存放函数体的二进制代码，由操作系统进行管理的。

- 全局区：存放全局变量和静态变量以及常量。

- 栈区：由编译器自动分配释放, 存放函数的参数值,局部变量等。

- 堆区：由程序员分配和释放,若程序员不释放,程序结束时由操作系统回收。



**内存四区意义：**

不同区域存放的数据，赋予不同的生命周期, 给我们更大的灵活编程。



### 1.1 程序运行前

在程序编译后，生成了exe可执行程序，**未执行该程序前**分为两个区域

**代码区：**

存放 CPU 执行的机器指令

代码区是**共享**的，共享的目的是对于频繁被执行的程序，只需要在内存中有一份代码即可。

人话就是：比如你运行一个游戏exe，你的这个游戏exe不会每次在你的内存中都生成一份新的代码，一直就是一份代码。



代码区是**只读**的，使其只读的原因是防止程序意外地修改了它的指令。

人话就是：你的游戏里面一个道具是10个银币，不能说你第二次点开这个游戏的时候，这个道具就变成100个银币了。



**全局区：**

全局变量和静态变量存放在此。

全局区还包含了常量区, 字符串常量和其他常量也存放在此。

该区域的数据在程序结束后由操作系统释放。

> 示例代码

```C++
#include<iostream>
using namespace std;

// 全局变量
int g_a = 10;
int g_b = 10;

//const修饰的全局变量，全局常量
const int c_g_a = 10;
const int c_g_b = 10;

int main()
{
	//局部变量
	int a = 10;
	int b = 10;

	//全局区
	//全局变量，静态变量，常量
	
	cout << "局部变量a的地址为：" << (int)&a << endl;
	cout << "局部变量b的地址为：" << (int)&b << endl;

	cout << "全局变量g_a的地址为：" << (int)&g_a << endl;
	cout << "全局变量g_b的地址为：" << (int)&g_b << endl;

	//静态变量
	static int s_a = 10;
	static int s_b = 10;

	cout << "静态变量s_a的地址为：" << (int)&s_a << endl;
	cout << "静态变量s_b的地址为：" << (int)&s_b << endl;

	//常量
	//字符串常量
	cout << "字符串常量地址为：" << (int)&"hello world" << endl;
	cout << "字符串常量地址为： " << (int)&"hello world1" << endl;

	//const修饰的变量
	//const修饰的全局变量，局部变量
	cout << "全局常量c_g_a地址为： " << (int)&c_g_a << endl;
	cout << "全局常量c_g_b地址为： " << (int)&c_g_b << endl;

	//c- const g- global l- local 
	const int c_l_a = 10;
	const int c_l_b = 10;
	cout << "局部常量c_l_a地址为： " << (int)&c_l_a << endl;
	cout << "局部常量c_l_b地址为： " << (int)&c_l_b << endl;
	system("pause");
	return 0;
}
```

> 运行结果

![1-内存分区模型.png](1-内存分区模型.png)

![1-内存分区模型-1.png](1-内存分区模型-1.png)

总结：

- C++中在程序运行前分为全局区和代码区。

- 代码区特点是共享和只读。

- 全局区中存放全局变量、静态变量、常量。

- 常量区中存放 const修饰的全局常量 和 字符串常量。



### 1.2 程序运行后

**栈区：**

由编译器自动分配释放, 存放函数的参数值,局部变量等。

注意事项：不要返回局部变量的地址，栈区开辟的数据由编译器自动释放。

> 示例代码

```C++
#include<iostream>
using namespace std;

//栈区数据注意事项 - 不要返回局部变量的地址
//栈区的数据由编译器管理开辟和释放

int* func()
{
	int a = 10; //局部变量 存放在栈区，栈区的数据在函数执行完后自动释放
	return &a;  //返回局部变量的地址
}

int main()
{
	//接收func函数的返回值
	int* p = func();

	cout << *p << endl; //第一次可以打印正确的数据，是因为编译器做了保留
	cout << *p << endl; //第二次这个数据就不再保留了

	system("pause");
	return 0;
}
```



**堆区：**

由程序员分配释放，若程序员不释放，程序结束时由操作系统回收。

在C++中主要利用new在堆区开辟内存。

> 示例代码

```C++
#include<iostream>
using namespace std;

int* func()
{
	//利用new关键字 可以将数据开辟到堆区
	//指针 本身也是局部变量 放在栈上，指针保存的数据是放在堆区
	int *p = new int(10);
	return p;
}

int main()
{
	int* p = func();
	cout << *p << endl;

	system("pause");
	return 0;
}
```

**总结：**

堆区数据由程序员管理开辟和释放。

堆区数据利用new关键字进行开辟内存。

## END

>**END：本节内容到此结束。**

个人提升之余，别忘了和小伙伴积极交流，很多人觉得他们在思考，而实际上他们只是在重新整理自己的偏见。请珍惜和他人交流讨论的机会。

<br>
<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Blog-Common-Images/END1.jpg" alt = "END1.jpg" width = "70%" height = "auto">
</div>
<br>

希望你每一天都有所收获，进步up up up。今天的我们并不比昨天更聪明，但一定要比昨天更睿智。

<br>
<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Blog-Common-Images/END2.jpg" alt = "END2.jpg" width = "70%" height = "auto">
</div>