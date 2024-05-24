---
title: "C++编程入门：类和对象-封装"
date: 2024-04-30T00:17:58+09:00
lastmod: 2024-04-30T00:17:58+09:00
author: ["Eddy"]
keywords: 
- CPP Programming
- 引用
categories: 
- 
tags: 
- C++
- C++ 封装
- C++ 类和对象
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

## 4 类和对象

C++面向对象的三大特性为：**封装、继承、多态。**

C++认为**万事万物都皆为对象**，对象上有其属性和行为。

**例如：**

人可以作为对象，属性有姓名、年龄、身高、体重...，行为有走、跑、跳、吃饭、唱歌...。

车也可以作为对象，属性有轮胎、方向盘、车灯...,行为有载人、放音乐、放空调...。

具有相同性质的**对象**，我们可以抽象称为**类**，人属于人类，车属于车类。



### 4.1 封装

#### 4.1.1 封装的意义

封装是C++面向对象三大特性之一。

封装的意义：

- 将属性和行为作为一个整体，表现生活中的事物。

- 将属性和行为加以权限控制。

**封装意义一：**

在设计类的时候，属性和行为写在一起，表现事物。

**语法：** `class 类名{ 访问权限： 属性 / 行为 };`

**示例1：**设计一个圆类，求圆的周长。

> 示例代码

```C++
#include<iostream>
using namespace std;

const double PI = 3.14;

//设计一个圆类，求圆的周长
//圆求周长的公式:2 * PI * 半径

//class代表设计一个类，类后面紧跟着的是类名称
class Circle
{
	//访问权限
	//公共权限
public:

	//属性
	//半径
	int m_r;

	//行为
	//获取圆的周长
	double calculatrZC()
	{
		return 2 * PI * m_r;
	}
};

int main()
{
	//通过圆类 创建具体的圆(对象)
	//实例化(通过一个类 创建一个对象的过程)
	Circle c1;
	//给圆对象 的属性进行赋值
	c1.m_r = 10;
	
	//2 * PI * 10 = 62.8
	cout << "圆的周长为：" << c1.calculatrZC() << endl;
  
	system("pause");
	return 0;
}
```

> 运行结果

```C++
圆的周长为：62.8
```



**示例2：**设计一个学生类，属性有姓名和学号，可以给姓名和学号赋值，可以显示学生的姓名和学号。

> 示例代码

```C++
#include<iostream>
#include<string>
using namespace std;

//类中的属性和行为 我们统一称为 成员
// 属性 成员属性 成员变量
// 行为 成员函数 成员方法

class Student
{
	//权限
public:

	//属性
	string m_name; //姓名
	int m_Id; //学号

	//行为
	//显示姓名和学号
	void showStudent()
	{
		cout << "姓名：" << m_name << " 学号：" << m_Id << endl;
	}
	//给学生信息赋值
	void setStudent(string name,int id)
	{
		m_name = name;
		m_Id = id;
	}

};

int main()
{
	//创建一个具体学生  实例化对象
	Student s1;
	//给s1对象 进行属性赋值操作
	s1.m_name = "张三";
	s1.m_Id = 10;
	//显示学生信息
	s1.showStudent();

	Student s2;
	s2.setStudent("李四", 20);
	s2.showStudent();

	system("pause");
	return 0;
}
```

> 运行结果

```C++
姓名：张三  学号：10
姓名：李四  学号：20
```



**封装意义二：**

类在设计时，可以把属性和行为放在不同的权限下，加以控制。

访问权限有三种：

1. public 公共权限

2. protected 保护权限

3. private 私有权限

> 示例代码

```C++
#include<iostream>
#include<string>
using namespace std;

//访问权限
//三种
//公共权限 public    成员 类内可以访问 类外可以访问 
//保护权限 protected 成员 类内可以访问 类外不可以访问 儿子可以访问父亲中的访问内容
//私有权限 private   成员 类内可以访问 类外不可以访问 儿子不可以访问父亲的私有内容

class Person
{
public:
	//公共权限
	string m_Name; //姓名
protected:
	//保护权限
	string m_Car; //汽车
private:
	//私有权限
	int m_Password; //银行卡密码

public:
	void func()
	{
		m_Name = "张三";
		m_Car = "拖拉机";
		m_Password = 123456;
	}

};

int main()
{
	Person p1;
	p1.m_Name = "李四";
	//p1.m_Car = "奔驰"; //保护去权限内容，在类外访问不到
	//p1.m_Password = 123; //私有权限内容，类外访问不到
	system("pause");
	return 0;
}
```



#### 4.1.2 struct和class区别

在C++中 struct和class唯一的**区别**就在于 **默认的访问权限不同。**

区别：

- struct 默认权限为公共

- class 默认权限为私有

> 示例代码

```C++
#include<iostream>
using namespace std;

//strcut 和class的区别
//struct 默认权限是 public 
//public 默认权限是 private 

class C1 
{
	int m_A; //默认权限 是私有
};

struct C2
{
	int m_A; //默认权限 是公有
};

int main()
{
	C1 c1;
	// c1.m_A = 100;  //在class里默认权限为私有 ，因此类外不可以访问

	C2 c2;
	c2.m_A = 100; //在struct默认的权限为公共，因此可以访问

	system("pause");
	return 0;
}
```



#### 4.1.3 成员属性设置为私有

**优点1：**将所有成员属性设置为私有，可以自己控制读写权限

**优点2：**对于写权限，我们可以检测数据的有效性

> 示例代码

```C++
#include<iostream>
#include<string>
using namespace std;
//成员属性设置私有
//1、可以自己控制读写权限
//2、对于写可以检测数据有效性

//人类
class Person
{
public:
	//设置姓名
	void setName(string name)
	{
		m_Name = name;
	}
	//获取姓名
	string getName()
	{
		return m_Name;
	}
	
	//获取年龄
	int getAge()
	{
		return m_Age;	
	}
	//设置年龄 0~150之间
	void setAge(int age)
	{
		if (age < 0 || age > 150)
		{
			cout << "年龄" << age << "输入有误，赋值失败" << endl;
			return;
		}
		m_Age = age;
	}

	//设置偶像
	void setIdol(string idol)
	{
		m_Idol = idol;
	}

private:
	string m_Name; //姓名 可读可写
	int m_Age = 20; //年龄 只读 也可以写 要求年龄在0~150之间
	string m_Idol; //偶像 只写

};
int main()
{
	Person p;
	//姓名设置
	p.setName("张三");
	cout << "姓名：" << p.getName() << endl;

	//年龄设置
	p.setAge(160);
	//年龄获取
	cout << "年龄：" << p.getAge() << endl;

	//偶像设置
	p.setIdol("张曼玉");
	//偶像只可写入 不可读取

	system("pause");
	return 0;
}
```

> 运行结果

```C++
姓名：张三
年龄160输入有误，赋值失败
年龄：20
```



**练习案例1：设计立方体类**

设计立方体类(Cube)

求出立方体的面积和体积

分别用全局函数和成员函数判断两个立方体是否相等。

![4-类和对象.png](4-类和对象.png)

**思路：**

1）创建立方体类

2）设计属性 长，高，宽

3）设计行为 获取立方体面积和体积

4）分别利用全局函数和成员函数 判断两个立方体是否相等

> 示例代码

```C++
#include<iostream>
using namespace std;
//立方体类设计
//1、创建立方体类
//2、设计属性
//3、设计行为 获取立方体面积和体积
//4、分别利用全局函数和成员函数 判断两个立方体是否相等

class Cube
{
public:
	//设置长
	void setL(int l)
	{
		m_L = l;
	}
	//获取长
	int getL()
	{
		return m_L;
	}

	//设置宽
	void setW(int w)
	{
		m_W = w;
	}
	//获取宽
	int getW()
	{
		return m_W;
	}

	//设置高
	void setH(int h)
	{
		m_H = h;
	}
	//获取高
	int getH()
	{
		return m_H;
	}

	//获取立方体面积
	int caculateS()
	{
		return (2 * m_L * m_W) + (2 * m_H * m_W) + (2 * m_L * m_H);
	}
	//获取立方体体积
	int caculateV()
	{
		return m_L * m_W * m_H;
	}
	//利用成员函数判断两个立方体是否相等
	bool isSameByClass(Cube &c)
	{
		if (m_L == c.getL() && m_W == c.getW() && m_L == c.getL())
		{
			return true;
		}
		else
			return false;
	}

private:
	int m_L; //长
	int m_W; //宽
	int m_H; //高
};

//利用全局函数判断 两个立方体是否相等
bool isSame(Cube &c1,Cube &c2)
{
	if (c1.getL() == c2.getL() && c1.getW() == c1.getW() && c1.getH() == c2.getH())
	{
		return true;
	}
	else
		return false;
}

int main()
{
	//创建立方体对象
	Cube c1;
	c1.setL(10);
	c1.setW(10);
	c1.setH(10);

	cout << "c1的面积为：" << c1.caculateS() << endl;
	cout << "c1的体积为：" << c1.caculateV() << endl;

	//创建第二个立方体
	Cube c2;
	c2.setL(10);
	c2.setW(10);
	c2.setH(10);

	cout << "c2的面积为：" << c2.caculateS() << endl;
	cout << "c2的体积为：" << c2.caculateV() << endl;

	bool ret = isSame(c1, c2);
	if (ret)
	{
		cout << "c1和c2是相等的" << endl;
	}
	else
	{
		cout << "c1和c2是不相等的" << endl;
	}

	//利用成员函数判断
	ret = c1.isSameByClass(c2);
	if (ret)
	{
		cout << "成员函数判断：c1和c2是相等的" << endl;
	}
	else
	{
		cout << "成员函数判断：c1和c2是不相等的" << endl;
	}

	system("pause");
	return 0;
}
```

> 运行结果

```C++
c1的面积为：600
c1的体积为：1000
c2的面积为：600
c2的体积为：1000
c1和c2是相等的
成员函数判断：c1和c2是相等的
```



**练习案例2：点和圆的关系**

设计一个圆形类（Circle），和一个点类（Point），计算点和圆的关系。

![4-类和对象-1.png](4-类和对象-1.png)

**思路：**

1）创建点类`point.h`和`point.cpp`

2）创建圆类`circle.h`和`circle.cpp`

3）点和圆的关系判断`void isIncircle(Circle &c,Point &p)`

点到圆心的距离 == 圆心 （点在圆上）

点到圆心的距离 < 圆心 （点在圆内）

点到圆心的距离 > 圆心 （点在圆外）



> 示例代码

我们将点类用`point.cpp`和`point.h`实现。将圆类用`circle.cpp`和`circle.h`实现。

> 点类头文件`point.h`

```C++
#pragma once
#include<iostream>
using namespace std;
class Point
{
public:
	//设置x坐标
	void setX(int x);
	
	//读取x坐标
	int getX();
	
	//设置y坐标
	void setY(int y);
	
	//读取y坐标
	int getY();
	
private:
	int m_x;
	int m_y;
};
```

> 点类代码实现`point.c`

```C++
#include"point.h"

//设置x坐标
void Point::setX(int x)
{
	m_x = x;
}
//读取x坐标
int Point::getX()
{
	return m_x;
}
//设置y坐标
void Point::setY(int y)
{
	m_y = y;
}
//读取y坐标
int Point::getY()
{
	return m_y;
}
```

> 圆类头文件`circle.h`

```C++
#pragma once
#include<iostream>
#include"point.h"
using namespace std;

class Circle
{
public:
	//设置半径
	void setR(int r);
	
	//读取半径
	int getR();
	
	//设置圆心
	void setCenter(Point center);
	
	//获取圆心
	Point getCenter();
	
private:
	int m_R; //半径
	//在类中可以让另一个类，作为本类中的成员
	Point m_center; //圆心

};
```

> 圆类代码实现`circle.cpp`

```C++
#include"circle.h"

//设置半径
void Circle::setR(int r)
{
	m_R = r;
}
//读取半径
int Circle::getR()
{
	return m_R;
}
//设置圆心
void Circle::setCenter(Point center)
{
	m_center = center;
}
//获取圆心
Point Circle::getCenter()
{
	return m_center;
}
```

> 主文件`点和圆的关系.cpp`

```C++
#include<iostream>
#include"circle.h"
#include"point.h"
using namespace std;
//点和圆的关系案例

//判断点和圆的关系
void isIncircle(Circle &c,Point &p)
{
	//计算两点距离的平方
	int dist =
	(c.getCenter().getX() - p.getX()) * (c.getCenter().getX() - p.getX()) +
	(c.getCenter().getY() - p.getY()) * (c.getCenter().getY() - p.getY());
	
	//计算半径的平方
	int rDistance = c.getR() * c.getR();

	//判断两点距离的平方和半径的平方的关系
	if (dist == rDistance)
	{
		cout << "点在圆上" << endl;
	}
	else if (dist > rDistance)
	{
		cout << "点在圆外" << endl;
	}
	else
	{
		cout << "点在圆内" << endl;
	}
}
int main()
{
	//创建圆
	Circle c1;
	c1.setR(10);
	Point center;
	center.setX(10);
	center.setY(0);
	c1.setCenter(center);

	//创建点
	Point p1;
	p1.setX(10);
	p1.setY(9);

	Point p2;
	p2.setX(10);
	p2.setY(10);

	Point p3;
	p3.setX(10);
	p3.setY(11);

	//判断关系
	isIncircle(c1, p1);
	isIncircle(c1, p2);
	isIncircle(c1, p3);

	system("pause");
	return 0;
}
```

> 运行结果

我们设置了一个圆，圆心为（10,0），半径为10。
我们设置了三个点（10,9）（10,10）（10,11）。

```C++
点在圆内
点在圆上
点在圆外
```



### 4.2 对象的初始化和清理

- 生活中我们买的电子产品都基本会有出厂设置，在某一天我们不用时候也会删除一些自己信息数据保证安全。

- C++中的面向对象来源于生活，每个对象也都会有初始设置以及 对象销毁前的清理数据的设置。

#### 4.2.1 构造函数和析构函数

对象的**初始化和清理**也是两个非常重要的安全问题。

一个对象或者变量没有初始状态，对其使用后果是未知。

同样的使用完一个对象或变量，没有及时清理，也会造成一定的安全问题。

c++利用了**构造函数**和**析构函数**解决上述问题，这两个函数将会被编译器自动调用，完成对象初始化和清理工作。

对象的初始化和清理工作是编译器强制要我们做的事情，因此如果**我们不提供构造和析构，编译器会提供**

**编译器提供的构造函数和析构函数是空实现。**

- 构造函数：主要作用在于创建对象时为对象的成员属性赋值，构造函数由编译器自动调用，无须手动调用。

- 析构函数：主要作用在于对象**销毁前**系统自动调用，执行一些清理工作。

**构造函数语法：**`类名(){}`

1. 构造函数，没有返回值也不写void

2. 函数名称与类名相同

3. 构造函数可以有参数，因此可以发生重载

4. 程序在调用对象时候会自动调用构造，无须手动调用,而且只会调用一次

**析构函数语法：** `~类名(){}`

1. 析构函数，没有返回值也不写void

2. 函数名称与类名相同,在名称前加上符号 ~

3. 析构函数不可以有参数，因此不可以发生重载

4. 程序在对象销毁前会自动调用析构，无须手动调用,而且只会调用一次

> 示例代码

```C++
#include<iostream>
using namespace std;
//对象的初始化和清理
//1、构造函数 进行初始化操作
//2、析构函数 进行清理的操作
class Person
{
public:
	//1.1、构造函数
	//没有返回值 不用void
	//函数名 与类名称相同
	//构造函数可以有参数，可以发生重载
	//创建对象的时候，构造函数会自动调用，且只调用一次
	Person()
	{
		cout << "Person 构造函数的调用" << endl;
	}

	//1.2、析构函数
	//没有返回值 不写void
	//函数名和类名相同 在名称前加~
	//析构函数不可以有参数，不可以发生重载
	//对象销毁前 会自动调用析构函数 而且只会调用一次
	~Person()
	{
		cout << "Person 析构函数的调用" << endl;
	}
};

void test01()
{
	Person P;
}
int main()
{
	test01();
	system("pause");
	return 0;
}
```

> 运行结果

```C++
Person 构造函数的调用
Person 析构函数的调用
```



#### 4.2.2 构造函数的分类及调用

两种分类方式：

1）按参数分为： 有参构造和无参构造

2）按类型分为： 普通构造和拷贝构造

三种调用方式：

1）括号法

2）显示法

3）隐式转换法

> 构造函数的分类

```C++
#include<iostream>
using namespace std;

//1构造函数的分类及调用
//分类
//按照参数分类 无参构造(默认构造) 和 有参构造
//按照类型分类 普通构造 和 拷贝构造
class Person
{
public:
	int age;
	//构造函数
	Person()
	{
		cout << "Person 无参构造函数的调用" << endl;
	}
    //有参构造函数
	Person(int a)
	{
		age = a;
		cout << "Person 有参构造函数的调用" << endl;
	}
	//拷贝构造函数
	Person(const Person &p)
	{
		//将传入的人身上的所有属性，拷贝在我的身上 
		age = p.age;
		cout << "Person 拷贝构造函数的调用" << endl;
	}
	//析构函数
	~Person()
	{
		cout << "Person 析构函数的调用" << endl;
	}
};
```



> 构造函数的调用

1）构造函数的调用：括号法

```C++
void test01()
{
	//1、括号法
	Person p1; //默认构造函数的调用
	Person p2(10); //有参构造函数的调用
	Person p3(p2); //拷贝构造函数的调用

	//注意事项1
	//调用默认构造函数时候，不用加()
	//因为下面这行代码，编译器会认为是一个函数的声明
	//Person p1();

	cout << "p2的年龄为：" << p2.age << endl;
	cout << "p3的年龄为：" << p3.age << endl;
}

int main()
{
	test01();
	system("pause");
	return 0;
}
```

> 运行结果

```C++
Person 无参构造函数的调用
Person 有参构造函数的调用
Person 拷贝构造函数的调用
p2的年龄为：10
p3的年龄为：10
Person 析构函数的调用
Person 析构函数的调用
Person 析构函数的调用
```



2）构造函数的调用：显示法

```C++
void test01()
{
	//2、显示法
	Person p1;
	Person p2 = Person(10); //有参构造
	Person p3 = Person(p2); //拷贝构造

	//Person(10); //匿名对象 特点：当前行执行结束后，系统会立即回收掉匿名对象

	//注意事项2
	//不要利用拷贝构造函数 初始化匿名对象 编译器会认为 Person(p3); 等价于 Person p3; 重定义，报错。
	//Person(p3);

	cout << "p2的年龄为：" << p2.age << endl;
	cout << "p3的年龄为：" << p3.age << endl;
}
int main()
{
	test01();
	system("pause");
	return 0;
}
```

> 运行结果

```C++
Person 无参构造函数的调用
Person 有参构造函数的调用
Person 拷贝构造函数的调用
p2的年龄为：10
p3的年龄为：10
Person 析构函数的调用
Person 析构函数的调用
Person 析构函数的调用
```



3）构造函数的调用：

```C++
void test01()
{
	//3、隐式转换法
    Person p4 = 10; //相当于 写了 Person(10); 
    Person p5 = p4; //拷贝构造

    cout << "p4的年龄：" << p4.age << endl;
    cout << "p5的年龄：" << p5.age << endl;
}
int main()
{
	test01();
	system("pause");
	return 0;
}
```

> 运行结果

```C++
Person 有参构造函数的调用
Person 拷贝构造函数的调用
p4的年龄：10
p5的年龄：10
Person 析构函数的调用
Person 析构函数的调用
```



#### 4.2.3 拷贝构造函数调用时机

C++中拷贝构造函数调用时机通常有三种情况。

- 使用一个已经创建完毕的对象来初始化一个新对象。

- 值传递的方式给函数参数传值。

- 以值方式返回局部对象。

> 示例代码

```C++
#include<iostream>
using namespace std;

//拷贝构造函数调用时机

//1、使用一个已经创建完毕的对象来初始化一个新对象

//2、值传递的方式给函数参数传值

//3、值方式返回局部对象
class Person
{
public:
	int m_Age;
	Person()
	{
		cout << "Person默认构造函数的调用" << endl;
	}
	Person(int age)
	{
		m_Age = age;
		cout << "Person有参构造函数的调用" << endl;
	}
	Person(const Person& p)
	{
		m_Age = p.m_Age;
		cout << "Person拷贝构造函数的调用" << endl;
	}
	~Person()
	{
		cout << "Person默认析构函数的调用" << endl;
	}
};
```



1、使用一个已经创建完毕的对象来初始化一个新对象。

```C++
//1、使用一个已经创建完毕的对象来初始化一个新对象
void test01()
{
	Person  p1(20);
	Person  p2(p1);
	cout << "p2的年龄为：" << p2.m_Age << endl;
}
int main()
{
	test01();
	system("pause");
	return 0;
}
```

> 运行结果

```C++
Person有参构造函数的调用
Person拷贝构造函数的调用
p2的年龄为：20
Person默认析构函数的调用
Person默认析构函数的调用
```



2、值传递的方式给函数参数传值

```C++
//2、值传递的方式给函数参数传值
//相当于Person p1 = p;
void doWork(Person p1)
{

}
void test02()
{
	Person p;
	doWork(p);
}
```

> 运行结果

```C++
Person默认构造函数的调用
Person拷贝构造函数的调用
Person默认析构函数的调用
Person默认析构函数的调用
```



3、值方式返回局部对象

```C++
//3、值方式返回局部对象
Person doWork03()
{
	Person p1;
	cout << "p1的地址为：" << (int*)&p1 << endl;
	return p1;
}
void test03()
{
	Person p = doWork03();
	cout << "p的地址为：" << (int*)&p << endl;
}
```

> 运行结果

```C++
Person默认构造函数的调用
010FF894
Person拷贝构造函数的调用
Person默认析构函数的调用
010FF98C
Person默认析构函数的调用
```

> 运行结果（VS2022）编译器会优化

```C++
Person默认构造函数的调用
p1的地址为：000000B083EFF5D4
p的地址为：000000B083EFF5D4
Person默认析构函数的调用
```



#### 4.2.4 构造函数调用规则

默认情况下，c++编译器至少给一个类添加3个函数。

1．默认构造函数(无参，函数体为空)。

2．默认析构函数(无参，函数体为空)。

3．默认拷贝构造函数，对属性进行值拷贝。

构造函数调用规则如下：

- 如果用户定义有参构造函数，c++不在提供默认无参构造，但是会提供默认拷贝构造。

- 如果用户定义拷贝构造函数，c++不会再提供其他构造函数。

> 示例代码

```C++
#include<iostream>
using namespace std;
//构造函数的调用规则
//1、创建一个类，C++编译器会给每个类都添加至少三个函数
//默认构造(空实现)
//析构函数(空实现)
//拷贝构造(值拷贝)

//2、
// 如果我们写了有参构造函数，编译器就不再提供默认构造，依然提供拷贝构造
// 如果我们写了拷贝构造函数，编译器就不再提供其他普通构造函数了

class Person
{
public:
	int m_Age;
	Person()
	{
		cout << "Person默认构造函数的调用" << endl;
	}
	Person(int age)
	{
		m_Age = age;
		cout << "Person有参构造函数的调用" << endl;
	}
	/*Person(const Person& p)
	{
		m_Age = p.m_Age;
		cout << "Person拷贝构造函数的调用" << endl;
	}*/
	~Person()
	{
		cout << "Person默认析构函数的调用" << endl;
	}
};

void test01()
{
	Person p;
	p.m_Age = 18;

	Person p2(p);
	cout << "p2的年龄为：" << p2.m_Age << endl;
}

int main()
{
	test01();
	system("pause");
	return 0;
}
```

> 运行结果

```C++
Person默认构造函数的调用
p2的年龄为：18
Person默认析构函数的调用
Person默认析构函数的调用
```

可以看到我们定义了有参构造函数，而我们没有定义拷贝构造函数，但是编译器为我们提供了拷贝构造函数，将p.m_Age = 18值拷贝给了p2。



#### 4.2.5 深拷贝与浅拷贝

深浅拷贝是面试经典问题，也是常见的一个坑。

浅拷贝：简单的赋值拷贝操作。

深拷贝：在堆区重新申请空间，进行拷贝操作。

> 示例代码

```C++
#include<iostream>
using namespace std;
//深拷贝与浅拷贝

class Person
{
public:
	int m_Age;
	int *m_Height; //身高
public:
	Person() 
	{
		cout << "Person的默认构造函数调用" << endl;
	}
	Person(int age, int height)
	{
		cout << "Person的有参构造函数调用" << endl;
		m_Age = age;
		m_Height = new int(height);
	}
	//自己实现拷贝构造函数，解决浅拷贝带来的问题
	Person(const Person& p)
	{
		cout << "Person 拷贝构造函数调用" << endl;
		m_Age = p.m_Age;
		//m_Height = p.m_Height; 编译器默认实现就是这行代码
		//深拷贝操作
		m_Height = new int(*p.m_Height);
	}
	~Person()
	{
		//析构代码，将堆区开辟数据做释放操作
		if (m_Height != NULL)
		{
			delete m_Height;
			m_Height = NULL;
		}
		cout << "Person的默认析构函数调用" << endl; 
	}
};

void test01()
{
	Person p1(18,160);
	Person p2(p1);

	cout << "p1的年龄为："  << p1.m_Age << " 身高为：" << *p1.m_Height << endl;
	
	cout << "p2的年龄为：" << p2.m_Age << " 身高为：" << *p2.m_Height << endl;
}

int main()
{
	test01();

	system("pause");
	return 0;
}
```

> 运行结果

```C++
Person的有参构造函数调用
Person 拷贝构造函数调用
p1的年龄为：18 身高为：160
p2的年龄为：18 身高为：160
Person的默认析构函数调用
Person的默认析构函数调用
```



![4-类和对象-2.png](4-类和对象-2.png)

假如，我们没有自行设计拷贝构造函数，那么编译器会默认为浅拷贝，也就是说首先p2析构，内存释放，然后p1析构，内存释放，然而此时对应内存已经释放过了，无法释放第二次，所以会报错。

浅拷贝的意思就是 p1.m_Height在堆区开辟内存，对应的地址为0x0011，而拷贝给p2.m_Height，它对应的地址也是0x0011。

而深拷贝也就是说， p1.m_Height在堆区开辟内存，对应的地址为0x0011，而拷贝给p2.m_Height，p2.m_Height开辟了一个新地址，0x0022，但是0x0011和0x0022的内容都是160。

> 总结：如果属性有在堆区开辟的，一定要自己提供拷贝构造函数，防止浅拷贝带来的问题。



#### 4.2.6 初始化列表

**作用：**

C++提供了初始化列表语法，用来初始化属性。

**语法：**`构造函数()：属性1(值1),属性2（值2）... {}`

> 示例代码

1、传统初始化操作。

```C++
#include<iostream>
using namespace std;

//初始化列表
class Person
{
public:
	//传统初始化操作
	Person(int a, int b, int c)
	{
		m_A = a;
		m_B = b;
		m_C = c;
	}
    void PrintPerson()
    {
      cout << "m_A：" << m_A << endl;
      cout << "m_B：" << m_B << endl;
      cout << "m_C：" << m_C << endl;
    }
private:
	int m_A;
	int m_B;
	int m_C;
};

void test01()
{
	Person p(1,2,3);
    p.PrintPerson();
}

int main()
{
	test01();
	system("pause");
	return 0;
}
```

> 运行结果

```C++
m_A：1
m_B：2
m_C：3
```



2、初始化列表初始化属性。

```C++
#include<iostream>
using namespace std;

//初始化列表
class Person
{
public:
	//初始化列表初始化属性
	Person(int a, int b, int c) :m_A(a), m_B(b), m_C(b){}
	void PrintPerson()
	{
		cout << "m_A：" << m_A << endl;
		cout << "m_B：" << m_B << endl;
		cout << "m_C：" << m_C << endl;
	}

private:
	int m_A;
	int m_B;
	int m_C;
};

void test01()
{
	Person p(10,20,30);
	p.PrintPerson();
}

int main()
{
	test01();
	system("pause");
	return 0;
}
```

> 运行结果

```C++
m_A：10
m_B：20
m_C：20
```



#### 4.2.7 类对象作为类成员

C++类中的成员可以是另一个类的对象，我们称该成员为 对象成员。

例如：

```C++
class A {}
class B
{
    A a；
}
```

B类中有对象A作为成员，A为对象成员。

那么当创建B对象时，A与B的构造和析构的顺序是谁先谁后？



> 实验代码

```C++
#include<iostream>
#include<string>
using namespace std;

//类对象作为类成员
//手机类
class Phone
{
public:
	Phone(string PName)
	{
		cout << "Phone的构造函数调用" << endl;
		m_PName = PName;
	}
	~Phone()
	{
		cout << "Phone的析构函数调用" << endl;
	}
	
	string m_PName;

};


class Person
{
public:
	//Phone m_Phone = pName 隐式转换法
	Person(string name, string pName):m_Name(name),m_Phone(pName)
	{
		cout << "Person的构造函数调用" << endl;
	}
	~Person()
	{
		cout << "Person的析构函数调用" << endl;
	}
	//姓名
	string m_Name;

	//手机
	Phone m_Phone;

};

//其他类对象作为本类成员，构造时候先构造类对象，再构造自身，析构的顺序与构造相反
void test01()
{
	Person p("张三", "苹果MAX");
	cout << p.m_Name << "拿着：" << p.m_Phone.m_PName << endl;
}

int main()
{
	test01();
	system("pause");
	return 0;
}
```

> 运行结果

```C++
Phone的构造函数调用
Person的构造函数调用
张三拿着：苹果MAX
Person的析构函数调用
Phone的析构函数调用
```



结论：其他类对象作为本类成员，构造时候先构造类对象，再构造自身，析构的顺序与构造相反。



#### 4.2.8 静态成员

静态成员就是在成员变量和成员函数前加上关键字`static`，称为静态成员。 

静态成员分为：

- 静态成员变量

    - 所有对象共享同一份数据

    - 在编译阶段分配内存

    - 类内声明，类外初始化

- 静态成员函数

    - 所有对象共享同一个函数

    - 静态成员函数只能访问静态成员变量



> 实验代码-静态成员变量

> 实验一 静态成员变量的特点

```C++
#include<iostream>
using namespace std;

//静态成员变量
class Person
{
public:
	//1 所有对象都共享同一份数据
	//2 编译阶段就分配内存
	//3 类内声明 类外初始化操作
	static int m_A;

};
int Person::m_A = 100;

void test01()
{
	Person p;
	cout << p.m_A << endl; //输出100

	Person p2;
	p2.m_A = 200;
	cout << p2.m_A << endl; //输出200

	cout << p.m_A << endl; //输出200
}

int main()
{
	test01();	
	return 0;
}
```

> 运行结果

```C++
100
200
200
```

> 结论

1. 静态成员变量需要类内声明 类外初始化操作。

2. 所有对象都共享同一份数据，从输出结果为100，200，200即可看出。

3. 编译阶段就分配内存。



> 实验二 静态成员变量的访问方式

```C++
#include<iostream>
using namespace std;

//静态成员变量
class Person
{
public:
	static int m_A;
};
int Person::m_A = 100;

void test02()
{
	//静态成员变量 不属于某个对象上 所有对象都共享同一份数据
	//因此 静态变量有2种访问方式

	//1、通过对象进行访问
	Person p;
	cout << p.m_A << endl;

	//2、通过类名进行访问
	cout << Person::m_A << endl;
}

int main()
{
	test02();	
	return 0;
}
```

> 运行结果

```C++
100
100
```

> 结论

静态成员变量 不属于某个对象上 所有对象都共享同一份数据。因此 静态变量有2种访问方式

1. 通过对象进行访问。

2. 通过类名进行访问。



> 实验三 静态成员变量的访问权限

```C++
#include<iostream>
using namespace std;

//静态成员变量
class Person
{
public:
	static int m_A;

private:
	static int m_B;
};
int Person::m_A = 100;
int Person::m_B = 200;

void test02()
{
	cout << Person::m_A << endl;
	//cout << Person::m_B << endl; 类外访问不到私有静态成员变量
}

int main()
{
	test02();	
	return 0;
}
```

> 运行结果

```C++
100
```

> 结论

静态成员变量也是有访问权限的。类外访问不到私有静态成员变量



> **静态成员变量-总结**

**静态成员变量的特点**

1. 所有对象都共享同一份数据。

2. 编译阶段就分配内存。

3. 类内声明 类外初始化操作。

**静态成员变量的访问方式**

1. 通过对象进行访问。

2. 通过类名进行访问。

**静态成员变量的访问权限**

1. 类外访问不到私有静态成员变量。



> 实验代码-静态成员函数

> 实验二 静态成员函数可以访问哪些静态成员变量

```C++
#include<iostream>
using namespace std;

//静态成员函数
//所有对象共享同一个函数
//静态成员函数只能访问静态成员变量

class Person
{
public:
	//静态成员函数
	static void func()
	{
		cout << "static void func()的调用" << endl;
		m_A = 200; //静态成员函数 可以访问 静态成员变量
		//m_B = 200; 静态成员函数 不可以访问 非静态成员变量，无法区分到底是哪个对象的m_B属性
	}
	static int m_A; //静态成员变量
	int m_B = 100;
};

int Person::m_A = 100;

int main()
{
	return 0;
}

```

> 结论

1. 静态成员函数 可以访问 静态成员变量。

2. 静态成员函数 不可以访问 非静态成员变量。

**为什么静态成员函数不可以访问非静态成员变量？**

解释：非静态成员必须与特定的对象相对，举个例子，先`Person p1`，然后调用`p1.func()`，再`Perosn p2`，然后调用`p2.func()`，此时编译器是无法判断你的`m_B`是和哪个对象对应的，所以说静态成员函数 不可以访问 非静态成员变量。

同样也可以解释为什么静态成员函数 可以访问 静态成员变量，因为`m_A`是所有对象共享的。

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

