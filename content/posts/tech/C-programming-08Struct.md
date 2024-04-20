---
title: "【嵌入式高级C编程】第八章 结构体，共用体，枚举"
date: 2023-10-01T00:17:58+08:00
lastmod: 2023-10-01T00:17:58+08:00
author: ["Eddy"]
keywords: 
- C programming
- Embedded Development
- Struct
- common body
- enumeration
- C语言
- C语言编程
- 嵌入式编程
- 结构体
- 共用体
- 枚举
categories: 
- 
tags: 
- C programming
- Embedded Development
- Struct
- common body
- enumeration
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


## 第八章 结构体，共用体，枚举

## INIT

<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Blog-Common-Images/INIT.jpg" alt = "INIT.jpg" width = "70%" height = "auto">
</div>

<br/>

>**INIT：本节内容正式开始。action!**




### 一、结构体类型的概念及定义

#### 1.1 基本概述

**构造类型：**
不是基本类型的数据结构也不是指针，它是若干个相同或不同类型的数据构成的集合
常用的构造类型有数组、结构体、共用体


数组用于保存多个相同类型的数据
结构体用于保存多个不同类型的数据



#### 1.2 结构体的概念

结构体是一种构造类型的数据结构，

是一种或多种基本类型或构造类型的数据的集合。



#### 1.3 结构体类型的定义

**1.3.1 先定义结构体类型，再去定义结构体变量**

```C
struct 结构体类型名{
  成员列表
};
```

例子：

```C
struct stu{
  int num;
  char name[20];
  char sex;
};
```

//有了结构体类型后，就可以用类型定义变量了

`struct stu lucy, bob, lilei;`    //定义了三个struct stu类型的变量，注意不要把struct省略

每个变量都有三个成员，分别是`num` `name` `sex`



**1.3.2 在定义结构体类型的时候顺便定义结构体变量，以后还可以定义结构体变量**

```C
struct 结构体类型名{
  成员列表;
}结构体变量1,变量2;
struct 结构体类型名 变量3，变量4；
```

例子：

```C
struct stu{
  int num;
  char name[20];
  char sex;
}lucy,bob,lilei;
struct stu xiaohong,xiaoming;
```

注意：一般结构体类型都会定义在全局，也就是main函数的外面。

所以在定义结构体类型的同时定义变量，这些变量一般都是全局变量。

定义完类型之后定义的结构体变量内存分配要看定义的位置。



**1.3.3 无名结构体的定义**

在定义结构体类型的时候，没有结构体类型名，顺便定义结构体变量，因为没有类型名，所以以后不能再定义相关类型的数据了

```C
struct {
  成员列表;
}变量1，变量2;
```

注意：无名结构体由于没有结构体名，所以定义完之后是无法在定义结构体变量的，只能在定义类型的同时定义结构体变量。

例子：

```C
struct {
  int num;
  char name[20];
  char sex;
}lucy,bob;
```



**1.3.4 给结构体类型取别名**

通常咱们将一个结构体类型重新起个类型名，用新的类型名替代原先的类型

```C
typedef struct 结构体名 {
  成员列表;
}重新定义的结构体类型名A;
```

注意：typedef主要用于给一个类型取别名，此时相当于给当前结构体重新起了一个类型名为A，相当于 struct 结构体名，所以如果结构体要取别名，一般不需要先给结构体定义名字，定义结构体变量时，直接使用A即可，不用加struct

例子：

```C
typedef struct stu{
  int num;
  char name[20];
  char sex;
}STU;
```

以后STU 就相当于 struct stu

`STU lucy;` 和`struct stu lucy;` 是等价的，所以可以不指定stu这个名字



### 二、结构体变量的定义初始化及使用

#### 2.1 结构体变量的定义和初始化

结构体变量，是个变量，这个变量是若干个数据的集合

注：

(1)：在定义结构体变量之前首先得有结构体类型，然后在定义变量

(2)：在定义结构体变量的时候，可以顺便给结构体变量赋初值，被称为结构体的初始化

(3)：结构体变量初始化的时候，各个成员顺序初始化

案例：

```C
#include <stdio.h>

  //定义结构体类型
struct stu{
  int id;
  char name[32];
  char sex;
  int age;
  //定义结构体变量之定义结构体类型的同时定义结构体变量
}zhangsan, lisi = {1002, "李四", 'B', 25};

  //使用typedef对结构体类型取别名
typedef struct{
  int a;
  int b;
  char c;
}MSG;

int main(int argc, char *argv[])
{
//定义结构体变量之类型定义完毕之后定义变量
  struct stu wangwu;
  //结构体变量的初始化
  struct stu zhaoliu = {1001, "赵六", 'B', 20};

  //如果使用typedef对结构体类型取别名
  //就无法在定义类型的同时定义结构体变量
  //在定义结构体变量的时候不用加struct
  MSG msg1, msg2 = {100, 200, 'w'};

  return 0;
}
```



#### 2.2 结构体变量的使用

结构体变量对成员调用的方式：

```C
结构体变量.结构体成员
```

注意：这地方说的结构体变量主要指的是普通结构体变量



**2.2.1 结构体变量的简单使用**

```C
#include <stdio.h>
#include <string.h>

struct stu{
  int id;
  char name[32];
  char sex;
  int age;
}zhangsan, lisi = {1002, "李四", 'B', 25};

typedef struct{
  int a;
  int b;
  char c;
}MSG;

int main(int argc, char *argv[])
{
  struct stu wangwu;
  struct stu zhaoliu = {1001, "赵六", 'B', 20};

  MSG msg1, msg2 = {100, 200, 'w'};

  //结构体变量的使用
  zhangsan.id = 1001;
  strcpy(zhangsan.name, "张三");
  zhangsan.sex = 'B';
  zhangsan.age = 18;
  printf("%d ‐ %s ‐ %c ‐ %d\n", zhangsan.id, zhangsan.name,zhangsan.sex, zhangsan.age);
  printf("%d ‐ %s ‐ %c ‐ %d\n", lisi.id, lisi.name,lisi.sex, lisi.age);
  printf("%d ‐ %s ‐ %c ‐ %d\n", zhaoliu.id, zhaoliu.name,zhaoliu.sex, zhaoliu.age);
  printf("%d ‐ %d ‐ %c\n", msg2.a, msg2.b, msg2.c);

  return 0;
}
```

执行结果

```C
1001 - 张三 - B - 18
1002 - 李四 - B - 25
1001 - 赵六 - B -20
100 - 200 - W
```



**Question：**

为什么这个时候不能直接给year赋值？

```C
typedef struct{ 
  int year;
  int month;
  int day;             
}BD;
```

**Answer：**

上述代码定义了一个结构体类型 `BD`，但它仅仅是一个类型的声明，没有创建任何具体的变量。因此，您不能直接给 `year`、`month` 或 `day` 赋值，因为这些成员是结构体内部的成员，需要通过结构体变量来访问和赋值。

您可以理解为 `BD` 类型是一种用户自定义的数据类型，类似于 `int` 或其他基本数据类型。在使用它时，需要创建具体的变量，然后通过这些变量来操作其成员。

例如，在使用 `BD` 类型时，您需要先创建一个 `BD` 类型的变量，然后通过该变量来访问和赋值其成员变量 `year`、`month` 和 `day`，就像前面的示例所示。

```C
BD birthday; // 创建一个BD类型的变量

birthday.year = 1990; // 给year成员变量赋值
birthday.month = 9;   // 给month成员变量赋值
birthday.day = 15;    // 给day成员变量赋值
```

这个变量 `birthday` 才是具体存储数据的实例，而 `BD` 只是定义了这种类型的数据结构。



**2.2.2 在结构体中嵌套结构体**

```C
//在结构体中嵌套结构体
typedef struct{
  int year;
  int month;
  int day;
}BD;

typedef struct{
  int id;
  char name[32];
  BD birthday;
}STU;

void test2()
{
  STU xiaoming;
  xiaoming.id = 1001;
  strcpy(xiaoming.name, "小明");
  //如果结构体中嵌套结构体，赋值时找到最内层的成员再进行赋值
  xiaoming.birthday.year = 2002;
  xiaoming.birthday.month = 12;
  xiaoming.birthday.day = 20;
  
  printf("%d ‐ %s ‐ %d‐%d‐%d\n", xiaoming.id, xiaoming.name,\
  xiaoming.birthday.year, xiaoming.birthday.month,\
  xiaoming.birthday.day);

  //嵌套的形式定义并初始化
  STU xiaoli = {1002, "小丽", 2000, 1, 1};
  printf("%d ‐ %s ‐ %d‐%d‐%d\n", xiaoli.id, xiaoli.name,\
  xiaoli.birthday.year, xiaoli.birthday.month,\
  xiaoli.birthday.day);
}
```

执行结果

```C
1001 - 小明 - 2002-12-20
1002 - 小丽 - 2000-1-1
```



#### 2.3 相同类型的结构体变量可以相互赋值

```C
#include <stdio.h>
#include <string.h>

struct stu{
  int id;
  char name[32];
  char sex;
  int age;
};

int main(int argc, char *argv[])
{
  struct stu zhangsan;
  zhangsan.id = 1001;
  strcpy(zhangsan.name, "张三");
  zhangsan.sex = 'B';
  zhangsan.age = 18;
  printf("%d ‐ %s ‐ %c ‐ %d\n", zhangsan.id, zhangsan.name,\
    zhangsan.sex, zhangsan.age);

  //相同类型的结构体变量之间可以直接赋值
  struct stu lisi;
  lisi = zhangsan;
  printf("%d ‐ %s ‐ %c ‐ %d\n", lisi.id, lisi.name,\
    lisi.sex, lisi.age);

  return 0;
}
```

这是我优化后的代码：

```C
#include<stdio.h>

typedef struct
{
    int id;
    char name[32];
    char sex;
    int age;
}STU;

int main(int argc, char *argv[])
{
    STU zhangsan = {1001, "张三", 'B', 18};
    printf("%d - %s - %c - %d\n",zhangsan.id,zhangsan.name,\
            zhangsan.sex,zhangsan.age);

    //相同类型的结构体变量之间可以直接赋值
    STU lisi;
    lisi = zhangsan;
    printf("%d - %s - %c - %d\n",lisi.id,lisi.name,\
            lisi.sex,lisi.age);
    
    return 0;
}
```

执行结果

```C
1001 - 张三 - B - 18
1001 - 张三 - B - 18
```



### 三、结构体数组

结构体数组是个数组，由若干个相同类型的结构体变量构成的集合

**1、结构体数组的定义方法**

```C
struct 结构体类型名 数组名[元素个数];
```

例子：

```C
struct stu{
  int num;
  char name[20];
  char sex;
};
  struct stu edu[3];
  定义了一个struct stu 类型的结构体数组edu，
  这个数组有3个元素分别是edu[0] 、edu[1]、edu[2]
```

**2、结构体数组元素的引用**

```C
数组名[下标]
```

**3、结构体数组元素对成员的使用**

```C
数组名[下标].成员
```

案例：

```C
#include <stdio.h>

typedef struct{
  int num;
  char name[20];
  float score;
}STU;

int main(int argc, char *argv[])
{
  //定义一个结构体数组
  STU edu[3] = {
    {101,"Lucy",78},
    {102,"Bob",59.5},
    {103,"Tom",85}
  };

  //输出结构体数组中的元素
  int j;
  for(j = 0; j < 3; j++)
  {
    printf("%d ‐ %s ‐ %.2f\n", edu[j].num, edu[j].name,\
    edu[j].score);
  }

  int i;
  float sum=0;
  for(i = 0; i < 3; i++)
  {
    sum += edu[i].score;
  }

  printf("平均成绩为%.2f\n",sum / 3);

  return 0;
}
```

执行结果

```C
101 - Lucy - 78.00
102 - Bob - 59.50
103 - Tom - 85.00
平均成绩为74.17
```



### 四、结构体指针

即结构体的地址，结构体变量存放内存中，也有起始地址

咱们定义一个变量来存放这个地址，那这个变量就是结构体指针变量。

1、结构体指针变量的定义方法：

```C
struct 结构体类型名 * 结构体指针变量名;
```

2、结构体指针变量对成员的引用

```C
 (*结构体指针变量名).成员
  结构体指针变量名‐>成员
```

```C
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

struct stu{
    int id;
    char name[32];
    char sex;
    int age;
};

int main(int argc, char *argv[])
{
    //定义一个结构体指针变量
    struct stu *s;
    //在堆区开辟结构体空间并将其地址保存在结构体指针变量中
    s = (struct stu *)malloc(sizeof(struct stu));

    s->id = 1001;
    strcpy(s->name, "张三");
    s->sex = 'B';
    s->age = 20;

    printf("%d - %s - %c - %d\n", s->id, s->name, s->sex, s->age);

    return 0;
}
```

执行结果

```C
 1001 - 张三 - B - 20
```



### 五、结构体内存分配问题

```C
#include<stdio.h>
struct stu{
  char sex;
  int age;
}lucy;
int main()
{
  printf("%d\n",sizeof(lucy));  //结果为 8？？？
  return 0;
}
```

**引言：**

结构体变量大小是它的所有成员之和，那么为什么上述例子中sizeof(lucy)不是1+4=5而是8呢？❓

结构体变量究竟是怎么分配内存的呢？❓

请看下文🕵‍(侦探)



实际给结构体变量分配内存的时候，是规则的

**规则1：以多少个字节为单位开辟内存**

给结构体变量分配内存的时候，会去结构体变量中找基本类型的成员

哪个基本类型的成员占字节数多，就以它大大小为单位开辟内存,

在gcc中出现了double类型的例外

(1)：成员中只有char型数据 ，以1字节为单位开辟内存。

(2)：成员中出现了short 类型数据，没有更大字节数的基本类型数据。以2字节为单位开辟内存

(3)：出现了int 、float 没有更大字节的基本类型数据的时候以4字节为单位开辟内存。

(4)：出现了double类型的数据

情况1：

在vc里，以8字节为单位开辟内存。

情况2：

在gcc里，以4字节为单位开辟内存。

无论是那种环境，double型变量，占8字节。

(5)：如果在结构体中出现了数组，数组可以看成多个变量的集合。

  如果出现指针的话，没有占字节数更大的类型的，以4字节为单位开辟内存。

(6)：在内存中存储结构体成员的时候，按定义的结构体成员的顺序存储。



**规则2：字节对齐**

(1)：char    1字节对齐 ，即存放char型的变量，内存单元的编号是1的倍数即可。

(2)：short    2字节对齐 ，即存放short int 型的变量，起始内存单元的编号是2的倍数即可。

(3)：int     4字节对齐 ，即存放int 型的变量，起始内存单元的编号是4的倍数即可。

(4)：long     在32位平台下，4字节对齐 ，即存放long int 型的变量，起始内存单元的编号是4的倍数即可。

(5)：float     4字节对齐 ，即存放float 型的变量，起始内存单元的编号是4的倍数即可。

(6)：double

a.vc环境下

8字节对齐，即存放double型变量的起始地址，必须是8的倍数，double变量占8字节。

b.gcc环境下

4字节对齐，即存放double型变量的起始地址，必须是4的倍数，double变量占8字节。

注意：

当结构体成员中出现数组的时候，可以看成多个变量。

开辟内存的时候，从上向下依次按成员在结构体中的位置顺序开辟空间



例子：temp占 8 个字节

```C
#include<stdio.h>
struct stu{
  char a;
  short int b;
  int c;
}temp;
int main()
{
  printf("%d\n",sizeof(temp));
  printf("%p\n",&(temp.a));
  printf("%p\n",&(temp.b));
  printf("%p\n",&(temp.c));
  return 0;
}
```

这是具体分配内存的图示

<br/>

<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Embedded_high-level_C_programming/08_Struct_common-body_enumeration_image%206.png" alt = "08_Struct_common-body_enumeration_image_6.png" width = "70%" height = "auto">
</div>

<br/>


结果分析：

a 的地址和 b 的地址差 2 个字节

b 的地址和 c 的地址差 2 个字节



例子：temp 的大小为 12 个字节

```C
struct stu{
  char a;
  int c;
  short int b;
}temp;
```

<br/>

<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Embedded_high-level_C_programming/08_Struct_common-body_enumeration_image%207.png" alt = "08_Struct_common-body_enumeration_image_7.png" width = "70%" height = "auto">
</div>

<br/>

结果分析:

a 和 c 的地址差 4 个字节

c 和 b 的地址差 4 个字节



例子：

```C
struct stu{
  char a;
  double b;
}temp;
```

结果分析;

在 vc 中占 16 个字节 a 和 b 的地址差 8 个字节

在 gcc 中占 12 个字节 a 和 b 的地址差 4 个字节



例子：

```C
struct stu{
  int num;
  int age;
}lucy;
  8字节

struct stu{
  char sex;
  int age;
}lucy;
  8字节

struct stu{
  char a;
  short int b;
  int c;
}temp;
  8字节

struct stu{
  char a;
  int c;
  short int b;
}temp;
  12字节

struct stu{
  char buf[10];
  int a;
}temp;
  16字节
  
struct stu{
  char a;
  double b;
};
  12字节
```

为什么要有字节对齐？

用空间来换时间，提高cpu读取数据的效率



看到这里的话，恭喜你，已经掌握了结构体的基本知识。🏅

本章已过完1/3，下一节，我们将对《位段》进行学习。⏩



### 六、位段

**引言：**

位段是偏底层的内容

目前做应用层的话只需了解即可

我目前也不知道这是做什么的😶‍🌫️(迷茫)



一、位段

在结构体中，以位为单位的成员，咱们称之为位段(位域)。

struct packed_data{

unsigned int a:2;

unsigned int b:6;

unsigned int c:4;

unsigned int d:4;

unsigned int i;

} data;

<br/>

<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Embedded_high-level_C_programming/08_Struct_common-body_enumeration_image%208.png" alt = "08_Struct_common-body_enumeration_image_8.png" width = "70%" height = "auto">
</div>


<br/>



注意：不能对位段成员取地址

```C
#include<stdio.h>
struct packed_data{
  unsigned int a:2;
  unsigned int b:6;
  unsigned int c:4;
  unsigned int d:4;
  unsigned int i;
} data;

int main()
{
  printf("%d\n",sizeof(data));
  printf("%p\n",&data);
  printf("%p\n",&(data.i));
  return 0;
}
```



**位段注意：**

1、对于位段成员的引用如下：

data.a =2

赋值时，不要超出位段定义的范围;

如段成员a定义为2位，最大值为3,即（11）2

所以data.a =5，就会取5的低两位进行赋值 101

2、位段成员的类型必须指定为整形或字符型

3、一个位段必须存放在一个存储单元中，不能跨两个单元

第一个单元空间不能容纳下一个位段，则该空间不用，而从下一个单元起存放该位段。



**位段的存储单元：**

(1)：char型位段 存储单元是1个字节

(2)：short int型的位段存储单元是2个字节

(3)：int的位段，存储单元是4字节

(4)：long int的位段，存储单元是4字节

```C
struct stu{
  char a:7;
  char b:7;
  char c:2;
}temp;//占3字节，b不能跨 存储单元存储
```

```C
#include<stdio.h>
struct stu{
  char a:7;
  char b:7;
  char c:2;
}temp;
int main()
{
  printf("%d\n",sizeof(temp));
  return 0;
}
  结果为：3 ，证明位段不能跨其存储单元存储
```

注意：不能 取 temp.b的地址，因为b可能不够1字节，不能取地址。



4、位段的长度不能大于存储单元的长度

(1)：char型位段不能大于8位

(2)：short int型位段不能大于16位

(3)：int的位段，位段不能大于32位

(4)：long int的位段，位段不能大于32位

```C
#include<stdio.h>
struct stu{
  char a:9;
  char b:7;
  char c:2;
}temp;

int main()
{
  printf("%d\n",sizeof(temp));
  return 0;
}
  分析：
  编译出错，位段a不能大于其存储单元的大小
```



5、如一个段要从另一个存储单元开始，可以定义：

unsigned char a:1;

unsigned char b:2;

unsigned char :0;

unsigned char c:3;(另一个单元)

由于用了长度为0的位段，其作用是使下一个位段从下一个存储单元开始存放

将a、b存储在一个存储单元中，c另存在下一个单元

```C
#include<stdio.h>
struct m_type{
  unsigned char a:1;
  unsigned char b:2;
  unsigned char :0;
  unsigned char c:3;
};
int main()
{
  struct m_type temp;
  printf("%d\n",sizeof(temp));
  return 0;
}
```



6、可以定义无意义位段,如：

unsigned a: 1;

unsigned : 2;

unsigned b: 3;

```C
struct data{
  char a:1;
  char b:1;
  char c:1;
  char d:1;
  char e:1;
  char f:1;
  char g:1;
  char h:1;
}temp;
int main()
{
  char p0;
  //p0=0x01;// 0000 0001
  temp.a=1;
  //p0=temp;//错的，类型不匹配
  //p0=(char)temp;//错的，编译器不允许将结构体变量，强制转成基本类型的。
  p0 = *((char *)(&temp));
}
```

​

指定对齐原则：

使用#pragma pack改变默认对其原则

格式：

#pragma pack (value)时的指定对齐值value。

📢 这个部分可以先不学习，一般情况下都是用的默认对齐原则



### 七、共用体

1：共用体和结构体类似，也是一种构造类型的数据结构。

- 既然是构造类型的，咱们得先定义出类型，然后用类型定义变量。

- 定义共用体类型的方法和结构体非常相似，把struct 改成union 就可以了。

- 在进行某些算法的时候，需要使几种不同类型的变量存到同一段内存单元中，几个变量所使用空间相互重叠。

    这种几个不同的变量共同占用一段内存的结构，在C语言中，被称作“共用体”类型结构

- 共用体所有成员占有同一段地址空间

- 共用体的大小是其占内存长度最大的成员的大小



**共用体的特点：**

1、同一内存段可以用来存放几种不同类型的成员，但每一瞬时只有一种起作用

2、共用体变量中起作用的成员是最后一次存放的成员，在存入一个新的成员后原有的

成员的值会被覆盖

3、共用体变量的地址和它的各成员的地址都是同一地址

4、共用体变量的初始化union data a={123}; 初始化共用体为第一个成员

```C
#include <stdio.h>

  //定义一个共用体
union un{
  int a;
  int b;
  int c;
};

int main(int argc, char *argv[])
{
  //定义共用体变量
  union un myun;
  myun.a = 100;
  myun.b = 200;
  myun.c = 300;

  printf("a = %d, b = %d, c = %d\n", myun.a, myun.b, myun.c);

  return 0;
}
```

执行结果

```C
a = 300, b = 300, c = 300
```



### 八、枚举

将变量的值一一列举出来，变量的值只限于列举出来的值的范围内

枚举类型也是个构造类型的



1、枚举类型的定义

```C
enum 枚举类型名{
  枚举值列表；
};
```

在枚举值表中应列出所有可用值,也称为枚举元素

枚举变量仅能取枚举值所列元素



2、枚举变量的定义方法

```C
enum 枚举类型名 枚举变量名;
```

① 枚举值是常量,不能在程序中用赋值语句再对它赋值

例如：sun=5; mon=2; sun=mon; 都是错误的.

② 枚举元素本身由系统定义了一个表示序号的数值

默认是从0开始顺序定义为0，1，2…

如在week中，mon值为0，tue值为1， …,sun值为6

③ 可以改变枚举值的默认值：如

```C
enum week //枚举类型
{
  mon=3，tue，wed，thu，fri=4，sat,sun
};

  mon=3 tue=4,以此类推
  fri=4 以此类推
```

注意：在定义枚举类型的时候枚举元素可以用等号给它赋值，用来代表元素从几开始编号

在程序中，不能再次对枚举元素赋值，因为枚举元素是常量。

```C
#include <stdio.h>

//定义一个枚举类型
enum week
{
    mon=8, tue, wed, thu=2, fri, sat, sun
};

int main(int argc, char *argv[])
{
  //定义枚举类型的变量
  enum week day = mon;
  printf("day = %d\n", day);
  
  day = fri;
  printf("day = %d\n", day);

  return 0;
}
```

执行结果

```C
day = 8
day = 3
```

## END

>**END：本节内容到此结束。**

个人提升之余，别忘了和小伙伴积极交流，很多人觉得他们在思考，而实际上他们只是在重新整理自己的偏见。请珍惜和他人交流讨论的机会。

<br/>

<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Blog-Common-Images/END1.jpg" alt = "END1.jpg" width = "70%" height = "auto">
</div>

<br/>

希望你每一天都有所收获，进步up up up。今天的我们并不比昨天更聪明，但一定要比昨天更睿智。

<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Blog-Common-Images/END2.jpg" alt = "END2.jpg" width = "70%" height = "auto">
</div>

<br/>




**彩蛋**🎁

我时常想，做学问，做事业，在人生中都只能算是第二桩事。人生第一桩事是生活。我所谓的“生活”是“领略”，是“培养生机”。假若为学问为事业而忘却生活，那种学问，事业在人生中便失去其真正意义与价值。

- 朱光潜《给青年的十二封信》

恭喜你🎉，完成了对第八章《结构体，共用体，枚举》部分的学习，下一章我们将学习链表。

⏩第九章《链表》



