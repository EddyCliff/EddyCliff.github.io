---
title: "【嵌入式高级C编程】第五章 指针"
date: 2023-10-01T00:17:58+08:00
lastmod: 2023-10-01T00:17:58+08:00
author: ["Eddy"]
keywords: 
- C programming
- Embedded Development
- pointer
categories: 
- 
tags: 
- C programming
- Embedded Development
- pointer
description: 
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
    image: "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/BlogCover/c2.jpg" #图片路径例如：posts/tech/123/123.png
    caption: "" #图片底部描述
    alt: ""
    relative: false
---

## 第五章 指针

### 一、关于内存那点事

**存储器：存储数据器件**

**外存**

外存又叫外部存储器，长期存放数据，掉电不丢失数据

常见的外存设备：硬盘、flash、rom、u盘、光盘、磁带

**内存**

内存又叫内部存储器，暂时存放数据，掉电数据丢失

常见的内存设备：ram、DDR



物理内存：实实在在存在的存储设备

虚拟内存：操作系统虚拟出来的内存，当一个进程被创建的时候，或者程序运行的时候都会

分配虚拟内存，虚拟内存和物理内存之间存在映射关系。



操作系统会在物理内存和虚拟内存之间做映射。

在32位系统下，每个进程（运行着的程序）的寻址范围是4G,0x00 00 00 00 ~0xff ff ff ff

在写应用程序的，咱们看到的都是虚拟地址。



在运行程序的时候，操作系统会将 虚拟内存进行分区。

1.堆

在动态申请内存的时候，在堆里开辟内存。

2.栈

主要存放局部变量（在函数内部，或复合语句内部定义的变量）。

3.静态全局区

1）：未初始化的静态全局区

静态变量（定义的时候，前面加static修饰），或全局变量 ，没有初始化的，存在此区

2）：初始化的静态全局区

全局变量、静态变量，赋过初值的，存放在此区

4.代码区

存放咱们的程序代码

5.文字常量区

存放常量的。

内存以字节为单位来存储数据的，咱们可以将程序中的虚拟寻址空间，看成一个很大的一维的字符数组



本章所接触的内容，涉及到的内存都是虚拟内存，更准确来说是虚拟内存的用户空间



### 二、指针的相关概念

操作系统给每个存储单元分配了一个编号，从0x00 00 00 00 ~0xff ff ff ff

这个编号咱们称之为地址。

指针就是地址。

![05pointer_image_1.png](https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Embedded_high-level_C_programming/05pointer_image%20_1.png)

指针变量：是个变量，是个指针变量，即这个变量用来存放一个地址编号

在32位平台下，地址总线是32位的，所以地址是32位编号，所以指针变量是32位的即4个字节。



注意：

1：无论什么类型的地址，都是存储单元的编号，在32位平台下都是4个字节，即任何类型的指针变量都是4个字节大小

2：对应类型的指针变量，只能存放对应类型的变量的地址

举例：整型的指针变量，只能存放整型变量的地址



扩展：

字符变量 char ch; ch占1个字节，它有一个地址编号，这个地址编号就是ch的地址

整型变量 int a; a占4个字节，它占有4个字节的存储单元，有4个地址编号。

![05pointer_image_2.png](https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Embedded_high-level_C_programming/05pointer_image%20_2.png)

`Int a=0x00 00 23 4f`



### 三、指针的定义方法

1.简单的指针

数据类型 * 指针变量名;

`int * p;`  //定义了一个指针变量p

在 定义指针变量的时候 * 是用来修饰变量的，说明变量p是个指针变量。

变量名是 p

2.关于指针的运算符

& 取地址 、 *取值

&：获取一个变量的地址 

*：在定义一个指针变量时，起到标识作用，标识定义的是一个指针变量

除此之外其他地方都表示获取一个指针变量保存的地址里面的内容

![05pointer__image_3.png](https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Embedded_high-level_C_programming/05pointer_image%20_3.png)

```C
#include <stdio.h>

int main(int argc, char *argv[])
{
  //定义一个普通变量
  int a = 100;
  //定义一个指针变量
  int *p;

  //给指针变量赋值
  //将a的地址保存在p中
  p = &a;

  printf("a = %d %d\n", a, *p);
  printf("&a = %p %p\n", &a, p);

  return 0;
}
```

执行结果

```C
a = 100 100
&a = 0029FEA8 0029FEA8
```



扩展：如果在一行中定义多个指针变量，每个指针变量前面都需要加*来修饰

`int *p,*q;`  //定义了两个整型的指针变量p和q

`int * p,q;`  //定义了一个整型指针变量p，和整型的变量q



3、指针大小

在32位系统下，所有类型的指针都是4个字节。

因为不管地址内的空间多大，但是地址编号的长度是一样的，所以在32位操作系统中，地址都是四个字节。

```C
#include <stdio.h>

int main(int argc, char *argv[])
{
  char *a;
  short *b;
  int *c;
  long *d;
  float *e;
  double *f;

  printf("sizeof(a) = %d\n", sizeof(a));
  printf("sizeof(b) = %d\n", sizeof(b));
  printf("sizeof(c) = %d\n", sizeof(c));
  printf("sizeof(d) = %d\n", sizeof(d));
  printf("sizeof(e) = %d\n", sizeof(e));
  printf("sizeof(f) = %d\n", sizeof(f));

  return 0;
}
```

执行结果

```C
sizeof(a) = 4
sizeof(b) = 4
sizeof(c) = 4
sizeof(d) = 4
sizeof(e) = 4
sizeof(f) = 4
```



### 四、指针的分类

按指针指向的数据的类型来分。

**1:字符指针**

字符型数据的地址

`char *p;`  //定义了一个字符指针变量，只能存放字符型数据的地址编号

`char ch;`

`p= &ch;`

**2：短整型指针**

`short int *p;`  //定义了一个短整型的指针变量p，只能存放短整型变量的地址

`short int a;`

`p =&a;`

**3：整型指针**

`int *p;` //定义了一个整型的指针变量p，只能存放整型变量的地址

`int a;`

`p =&a;`

注：多字节变量，占多个存储单元，每个存储单元都有地址编号，

c语言规定，存储单元编号最小的那个编号，是多字节变量的地址编号。

**4：长整型指针**

`long int *p;`  //定义了一个长整型的指针变量p，只能存放长整型变量的地址

`long int a;`

`p =&a;`

**5：float 型的指针**

`float *p;`  //定义了一个float型的指针变量p，只能存放float型变量的地址

`float a;`

`p =&a;`

**6：double型的指针**

`double *p;`  //定义了一个double型的指针变量p，只能存放double型变量的地址

`double a;`

`p =&a;`

7：函数指针

8、结构体指针

9、指针的指针

10、数组指针

总结:无论什么类型的指针变量，在32位系统下，都是4个字节，只能存放对应类型的变量的地址编号。



### 五、指针和变量的关系

指针可以存放变量的地址编号

在程序中，引用变量的方法

1:直接通过变量的名称

`int a;`

`a = 100;`

2:可以通过指针变量来引用变量

`int * p;`  //在定义的时候，*不是取值的意思，而是修饰的意思，修饰p是个指针变量

`p = &a;`  //取a的地址给p赋值，p保存了a的地址，也可以说p指向了a

`*p = 100;`  //在调用的时候*是取值的意思，*指针变量 等价于指针指向的变量

注：指针变量在定义的时候可以初始化

`int a;`

`int * p = &a;` //用a的地址，给p赋值，因为p是指针变量

指针就是用来存放变量的地址的。

*+指针变量 就相当于指针指向的变量



指针变量只能保存开辟好空间的地址，不能随意保存地址

```C
#include <stdio.h>

int main(int argc, char *argv[])
{
  int *p1,*p2,temp,a,b;
  p1=&a;
  p2=&b;

  printf("请输入:a b的值:\n");
  scanf("%d %d", p1, p2);  //给p1和p2指向的变量赋值

  temp = *p1;  //用p1指向的变量（a）给temp赋值
  *p1 = *p2;   //用p2指向的变量（b）给p1指向的变量（a）赋值
  *p2 = temp;  //temp给p2指向的变量（b）赋值

  printf("a=%d b=%d\n",a,b);
  printf("*p1=%d *p2=%d\n",*p1,*p2);

  return 0;
}
```

执行结果

```C
请输入:a b的值:
70 90
a=90 b=70
*p1=90 *p2=70
```



扩展：

对应类型的指针，只能保存对应类型数据的地址，

如果想让不同类型的指针相互赋值的时候，需要强制类型转换。

```C
#include <stdio.h>

int main(int argc, char *argv[])
{
  int a=0x1234,b=0x5678;
  char *p1,*p2;
  printf("%#x %#x\n",a,b);
  p1=(char *)&a;
  p2=(char *)&b;
  printf("%#x %#x\n",*p1,*p2);
  p1++;
  p2++;
  printf("%#x %#x\n",*p1,*p2);

  return 0;
}
```



**彩蛋：**🎁

如果你来访我，我不在，请和我门外的花坐一会儿，它们很温暖，我注视它们很多日子了。

- 汪曾祺《人间草木》

恭喜你🎉，完成了对第五章《指针》部分的学习，下一章我们将学习动态内存申请。

⏩第六章 《动态内存申请》
