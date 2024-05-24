---
title: "C++编程入门：引用"
date: 2024-04-30T00:17:58+11:00
lastmod: 2024-04-30T00:17:58+11:00
author: ["Eddy"]
keywords: 
- CPP Programming
- 引用
categories: 
- 
tags: 
- C++
- C++ 引用
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

## 2 引用

### 2.1 引用的基本使用

**作用：** 给变量起别名

**语法：** `数据类型 &别名 = 原名`

> 示例代码

```C++
#include<iostream>
using namespace std;

int main()
{
	//引用基本语法
	//数据类型 &别名 = 原名

	int a = 10;
	//创建引用
	int& b = a;

	cout << "a = " << a << endl;
	cout << "b = " << b << endl;

	b = 100;

	cout << "a = " << a << endl;
	cout << "b = " << b << endl;

	system("pause");
	return 0;
}
```

> 运行结果

```C++
a = 10
b = 10
a = 100
b = 100
```

![2-引用.png](2-引用.png)



### 2.2 引用注意事项

- 引用必须初始化

- 引用在初始化后，不可以改变

> 示例代码

```C++
#include<iostream>
using namespace std;

int main()
{
	int a = 10;
	//1、引用必须初始化
	//int &b; //错误，必须要初始化

	int& b = a;

	//2、引用在初始化后，不可以改变
	int c = 20;
	b = c;  //赋值操作，而不是更改引用

	cout << "a = " << a << endl;
	cout << "b = " << a << endl;
	cout << "c = " << a << endl;

	system("pause");
	return 0;
}
```

> 运行结果

```C++
a = 20
b = 20
c = 20
```

![2-引用-1.png](2-引用-1.png)



### 2.3 引用做函数参数

**作用：**函数传参时，可以利用引用的技术让形参修饰实参。

**优点：**可以简化指针修改实参。

> 示例代码

```C++
#include<iostream>
using namespace std;

//交换函数

//1、值传递
void mySwap01(int a, int b)
{
	int temp = a;
	a = b;
	b = temp;

	//cout << "swap01 a = " << a << endl;
	//cout << "swap01 b = " << b << endl;
}

//2、地址传递
void mySwap02(int* a, int* b)
{
	int temp = *a;
	*a = *b;
	*b = temp;
}

//3、引用传递
void mySwap03(int& a, int& b)
{
	int temp = a;
	a = b;
	b = temp;
}

int main()
{
	int a = 10;
	int b = 20;
	//mySwap01(a, b); //值传递，形参不会修饰实参
	//mySwap02(&a, &b); //值传递，形参不会修饰实参
	mySwap03(a, b);//引用传递，形参会修饰实参

	cout << "a = " << a << endl;
	cout << "b = " << b << endl;

	system("pause");
	return 0;
}
```

> 运行结果

```C++
a = 20
b = 10
```



### 2.4 引用做函数返回值

作用：引用是可以作为函数的返回值存在的。

注意：**不要返回局部变量引用。**

用法：函数调用作为左值。



> 示例代码

```C++
#include<iostream>
using namespace std;

//引用做函数的返回值
//1、不要返回局部变量的引用
int& test01()
{
	int a = 10; //局部变量存放在内存四区中 的栈区
	return a;
}

//2、函数的调用可以作为左值
int& test02()
{
	static int a = 10; //静态变量，存放在全局区，全局区上的数据在程序结束后系统释放
	return a;
}

int main()
{
	//int& ref = test01();

	//cout << "ref = " << ref << endl;//第一次结果正确，是因为编译器做了保留
	//cout << "ref = " << ref << endl;//第一次结果错误，因为a的内存已经释放

	int& ref2 = test02();
	cout << "ref2 = " << ref2 << endl;

	test02() = 1000; //如果函数的返回值是引用，那么这个函数调用可以作为左值 
	cout << "ref2 = " << ref2 << endl;

	system("pause");
	return 0;
}
```

> 运行结果

```C++
ref2 = 10
ref2 = 1000
```



### 2.5 引用的本质

本质：**引用的本质在c++内部实现是一个指针常量.**

> 示例代码

```C++
//发现是引用，转换为 int* const ref = &a;
void func(int& ref){
	ref = 100; // ref是引用，转换为*ref = 100
}
int main(){
	int a = 10;
    
    //自动转换为 int* const ref = &a; 指针常量是指针指向不可改，也说明为什么引用不可更改
	int& ref = a; 
	ref = 20; //内部发现ref是引用，自动帮我们转换为: *ref = 20;
    
	cout << "a:" << a << endl;
	cout << "ref:" << ref << endl;
    
	func(a);
	return 0;
}
```

![2-引用-2.png](2-引用-2.png)

结论：C++推荐用引用技术，因为语法方便，引用本质是指针常量，但是所有的指针操作编译器都帮我们做了



### 2.6 常量引用

**作用：**常量引用主要用来修饰形参，防止误操作

在函数形参列表中，可以加const修饰形参，防止形参改变实参

> 示例代码

```C++
#include<iostream>
using namespace std;

//常量引用
//使用场景：用来修饰形参，防止误操作

//打印数据函数
void showValue(const int &val)
{
	cout << "val = " << val << endl;
}
int main()
{
	//int& ref = 10;  引用本身需要一个合法的内存空间，因此这行错误
	//加入const就可以了，编译器优化代码，int temp = 10; const int& ref = temp;
	const int& ref = 10;

	//ref = 100;  //加入const后不可以修改变量
	cout << "ref = " << ref << endl;

	int a = 100;
	showValue(a);

	system("pause");
	return 0;
}
```

> 运行结果

```C++
ref = 10
val = 100
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