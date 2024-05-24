---
title: "C++编程入门：函数提高-函数重载"
date: 2024-04-30T00:17:58+10:00
lastmod: 2024-04-30T00:17:58+10:00
author: ["Eddy"]
keywords: 
- CPP Programming
- 引用
categories: 
- 
tags: 
- C++
- C++ 函数
- C++ 函数重载
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

## 3 函数提高

### 3.1 函数默认参数

在C++中，函数的形参列表中的形参是可以有默认值的。

语法：`返回值类型 函数名 （参数= 默认值）{}`

> 示例代码

```C++
#include<iostream>
using namespace std;

//函数默认参数
//如果我们自己传入数据，就用自己的数据，如果没有，那么用默认值
//语法：返回值类型 函数名(形参 = 默认值){}
int func(int a,int b = 20,int c =30)
{
	return a + b + c;
}

//注意事项：
//1、如果某个位置已经有了默认参数，那么这个位置往后，从左往右都必须有默认值
//int func2(int a = 10,int b,int c) //此时会报错
//{
//	return a + b + c;
//}

//2、如果函数声明有默认参数，函数实现就不能有默认参数
//声明和实现只能有一个有默认参数
//int func2(int a = 10, int b = 10);
//int func2(int a = 20, int b = 20) //此时会报错
//{
//	return a + b;
//}

int main()
{
	cout << func(10) << endl;

	system("pause");
	return 0;
}
```

> 运行结果

```C++
60
```



### 3.2 函数占位参数

C++中函数的形参列表里可以有占位参数，用来做占位，调用函数时必须填补该位置

**语法：** `返回值类型 函数名 (数据类型){}`

在现阶段函数的占位参数存在意义不大，但是后面的课程中会用到该技术

> 示例代码

```C++
#include<iostream>
using namespace std;

//占位参数
//返回值类型 函数名(数据类型){}

//目前阶段的占位参数，我们还用不到，后面课程中会用到

void func(int a,int)
{
	cout << "this is func" << endl;
}

//占位参数 还可以有默认参数
void func1(int a, int = 10)
{
	cout << "this is func1" << endl;
}

int main()
{
	func(10,10);
	func1(10);

	system("pause");
	return 0;
}
```

> 运行结果

```C++
this is func
this is func1
```



### 3.3 函数重载

#### 3.3.1 函数重载概述

**作用：**函数名可以相同，提高复用性

**函数重载满足条件：**

- 同一个作用域下

- 函数名称相同

- 函数参数**类型不同** 或者 **个数不同** 或者 **顺序不同**

**注意:** 函数的返回值不可以作为函数重载的条件

> 示例代码

```C++
#include<iostream>
using namespace std;

//函数重载
//可以让函数名相同，提高复用性

//函数重载满足条件
//1、同一个作用域下
//2、函数名称相同
//3、函数参数类型不同，或者个数不同，或者顺序不同

void func()
{
	cout << "func的调用" << endl;
}

void func(int a)
{
	cout << "func的调用(int a)" << endl;
}

void func(double a)
{
	cout << "func的调用(double a)" << endl;
}

void func(int a, double b)
{
	cout << "func的调用(int a, double b)" << endl;
}

void func(double a, int b)
{
	cout << "func的调用(double a, int b)" << endl;
}

//注意事项
//函数的返回值不可以作为函数重载的条件
/*
int func(double a, int b)
{
	cout << "func的调用(double a, int b)" << endl;
}
*/

int main()
{
	func();
	func(10);
	func(3.14);
	func(10, 3.14);
	func(3.14, 10);

	system("pause");
	return 0;
}
```

> 运行结果

```C++
func的调用
func的调用(int a)
func的调用(double a)
func的调用(int a, double b)
func的调用(double a, int b)
```



#### 3.3.2 函数重载注意事项

- 引用作为重载条件

- 函数重载碰到函数默认参数



1、引用作为重载的条件。

> 示例代码

```C++
#include<iostream>
using namespace std;

//函数重载的注意事项
//1、引用作为重载的条件
void func(int& a)
{
	cout << "fun(int &a)的调用" << endl;
}

void func(const int& a)
{
	cout << "fun(const int &a)的调用" << endl;
}

int main()
{
	int a = 10;
	func(a); //调用无const的func
	func(10); //调用有const的func
	
	system("pause");
	return 0;
}
```

> 运行结果

```C++
fun(int &a)的调用
fun(const int &a)的调用
```



2、函数重载遇到默认参数。

> 示例代码

```C++
#include<iostream>
using namespace std;

//函数重载的注意事项
//2、函数重载遇到默认参数
void func2(int a , int b = 10)
{
	cout << "func2(int a,int b = 10)的调用" << endl;
}
void func2(int a)
{
	cout << "func2(int a)的调用" << endl;
}

int main()
{
	//func2(10); //当函数重载碰到默认参数，出现二义性，报错，尽量避免这种情况
    func2(10,10);
	system("pause");
	return 0;
}
```

> 运行结果

```C++
func2(int a,int b = 10)的调用
```

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