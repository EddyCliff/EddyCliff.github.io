---
title: "【嵌入式高级C编程】第十章 文件"
date: 2023-10-23T00:17:58+08:00
lastmod: 2023-10-23T00:17:58+08:00
author: ["Eddy"]
keywords: 
- C programming
- Embedded Development
- file
categories: 
- 
tags: 
- C programming
- Embedded Development
- file
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
## 一、文件的概念

文件用来存放程序、文档、音频、视频数据、图片等数据的。

文件就是存放在磁盘上的，一些数据的集合。在windows下可以通过写字板或记事本打开文本文件对文件进行编辑保存。写字板和记事

本是微软程序员写的程序，对文件进行打开、显示、读写、关闭。

作为一个程序员，必须掌握编程实现创建、写入、读取文件等操作

对文件的操作是经常要用到的知识，比如：写飞秋软件传送文件等



### 1.1 文件的定义

磁盘文件：（我们通常认识的文件）

指一组相关数据的有序集合,通常存储在外部介质(如磁盘)上，使用时才调入内存。

设备文件：

在操作系统中把每一个与主机相连的输入、输出设备看作是一个文件，把它们的输入、输出等同于对磁盘文件的读和写。

键盘：标准输入文件 屏幕：标准输出文件

其它设备：打印机、触摸屏、摄像头、音箱等

在Linux操作系统中，每一个外部设备都在/dev目录下对应着一个设备文件，咱们在程

序中要想操作设备，就必须对与其对应的/dev下的设备文件进行操作。



**标准io库函数对磁盘文件的读取特点**

通过标准IO库相当于在内存当中对我们的磁盘文件做输入输出操作，也就是读写操作。

![image.png](https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Embedded_high-level_C_programming/10_file_image.png)



**1、行缓冲**

标准io库函数，往标准输出（屏幕）输出东西的时候是行缓冲的

所谓的行缓冲就是缓冲区碰到换行符的时候才刷新缓冲区

如果不刷新缓冲区，无法对文件执行读写操作

举个例子：当地铁到站之后，只有所有门都关闭的时候，地铁才会出发，只要有一个门没关闭，地铁就不会出发。这就相当于我们的行缓存，如果不刷新缓冲区，就无法对文件执行读写操作。

行缓冲的刷新条件：

```C
#include <stdio.h>

int main(int argc, char const *argv[])
{
  //由于printf函数是一个标准io，所以只有刷新缓冲区才可以将数据输出到终端
  //printf("hello world");

  //刷新缓冲区方法1：使用\n
  //printf("hello world\n");

  //刷新缓冲区方法2：程序正常结束
  /*
  printf("hello world");
  return 0;
  */
  
  //刷新缓冲区方法3：使用fflush函数刷新缓冲区
  /*
  printf("hello world");
  fflush：刷新函数。可以刷新指定的缓冲区
  stdout：标准输出，就是对终端进行写操作
  fflush(stdout);
  */

  //刷新缓冲区方法4：当缓冲区满的时候自动刷新
  //默认行缓冲的大小为1024个字节
  /*
  int i;
  for(i = 1; i < 300; i++)
  {
    printf("%03d ", i);
  }
  */
  while(1);

  return 0;
}
```



> 没有刷新缓冲区，则无法进行输出

```C
#include <stdio.h>

int main(int argc, char const *argv[])
{
  //由于printf函数是一个标准io，所以只有刷新缓冲区才可以将数据输出到终端
  printf("hello world");
  //此时我们是没有任何输出结果的,因为我们没有加\n刷新缓冲区.

  while(1);

  return 0;
}
```



**2、全缓冲**

标准io库函数 ，往普通文件读写数据的，是全缓冲的，

碰到换行符也不刷新缓冲区，即缓冲区满了，才刷新缓冲区。

刷新缓冲区的情况

1.缓冲区满了，刷新缓冲区

2.人为刷新缓冲区 fflush(文件指针)

3.程序正常结束 会刷新缓冲区



**3.无缓冲**

在读写文件的时候通过系统调用io （read write）,对文件进行读写数据

这个时候是无缓冲的，即写数据会立马进入文件，读数据会立马进入内存



**写文件的流程**

应用程序空间 → 内核空间 → 驱动程序 → 硬盘上

应用程序和内核程序运行在不同的空间里，目的是为了保护内核。



**设置缓冲区的目的**

通过缓冲可以减少进出内核的次数，以提高效率。



### 1.2 磁盘文件的分类

一个文件通常是磁盘上一段命名的存储区

计算机的存储在物理上是二进制的，所以物理上所有的磁盘文件本质上都是一样的：以字节为单位进行顺序存储

从用户或者操作系统使用的角度（逻辑上）把文件分为：

文本文件：基于字符编码的文件

二进制文件：基于值编码的文件



**文本文件**

基于字符编码，常见编码有ASCII、UNICODE等

一般可以使用文本编辑器直接打开

例如：数5678的以ASCII存储形式为：

ASCII码：00110101 00110110 00110111 00111000

歌词文件(lrc):文本文件



**二进制码文件**

基于值编码,自己根据具体应用,指定某个值是什么意思

把内存中的数据按其在内存中的存储形式原样输出到磁盘上

一般需要自己判断或使用特定软件分析数据格式

例如：数5678的存储形式为：

二进制码：00010110 00101110

音频文件(mp3):二进制文件

图片文件（bmp）文件，一个像素点由两个字节来描述*****######&&&&&

*代表红色的值#代表绿色的值

&代表蓝色的值

二进制文件以位来表示一个意思。



**文本文件、二进制文件对比：**

**译码：**

文本文件编码基于字符定长，译码容易些；

二进制文件编码是变长的，译码难一些（不同的二进制文件格式，有不同的译码方

式）。

**空间利用率：**

二进制文件用一个比特来代表一个意思(位操作)；

而文本文件任何一个意思至少是一个字符。

二进制文件，空间利用率高。

**可读性：**

文本文件用通用的记事本工具就几乎可以浏览所有文本文件

二进制文件需要一个具体的文件解码器，比如读BMP文件，必须用读图软件。

**总结一下：**

文件在硬盘上存储的时候，物理上都是用二进制来存储的。

咱们的标准io库函数，对文件操作的时候，不管文件的编码格式（字符编码、或二进制），而是按字节对文件进行读写，所以咱们管文件又叫流式文件，即把文件看成一个字节流。



## 二、文件指针

文件指针就是用于标识一个文件的，所有对文件的操作都是用对文件指针进行操作的



**定义文件指针的一般形式为:**

FILE * 指针变量标识符；

本质上文件指针是一个结构体指针，结构体中包含了当前文件的很多信息，但是在实际编程时，

不需要关系结构体中的成员，只需要使用文件指针即可



**对文件操作的步骤：**

1、对文件进行读写等操作之前要打开文件得到文件指针

2、可以通过文件指针对文件进行读写等操作

3、读写等操作完毕后，要关闭文件，关闭文件后，就不能再通过此文件指针操作文件了



**c语言中有三个特殊的文件指针无需定义，在程序中可以直接使用**

**stdin：** 标准输入 默认为当前终端（键盘）

我们使用的scanf、getchar函数默认从此终端获得数据

**stdout：**标准输出 默认为当前终端（屏幕）

我们使用的printf、puts函数默认输出信息到此终端

**stderr：**标准错误输出设备文件 默认为当前终端（屏幕）

当我们程序出错使用:perror函数时信息打印在此终端



## 三、打开文件fopen

```C
#include <stdio.h>
FILE *fopen(const char *path, const char *mode);
功能：创建或者打开一个文件
参数：
  path：文件名，如果只写文件名，默认就是当前路径，也可以添加路径
  mode：文件权限
  r 只读，如果文件不存在则报错
  r+ 读写，如果文件不存在则报错
  w 只写，如果文件不存在则创建，如果文件存在则清空
  w+ 读写，如果文件不存在则创建，如果文件存在则清空
  a 只写，如果文件不存在则创建，如果文件存在则追加
  a+ 读写，如果文件不存在则创建，如果文件存在则追加
返回值：
  成功：文件指针
  失败：NULL
```



> 示例程序：对`file.txt`进行`fopen`操作

```C
#include <stdio.h>

int main(int argc, char *argv[])
{
  //使用fopen函数打开或者创建文件，返回文件指针
  FILE *fp;
  //以只读的方式打开文件，如果文件不存在则报错
  //fp = fopen("C:/Users/lzx/Desktop/file.txt", "r");

  //以只写的方式打开文件，如果文件不存在则创建，如果文件存在清空
  //fp = fopen("C:/Users/lzx/Desktop/file.txt", "w");

  //以只写的方式打开文件，如果文件不存在则创建，如果文件存在则追加
  fp = fopen("C:/Users/lzx/Desktop/file.txt", "a");
  if(fp == NULL)
  {
    printf("fail to fopen\n");
    return ‐1;
  }

  return 0;
}
```



## 四、关闭文件fclose

```C
#include <stdio.h>
int fclose(FILE *stream);
功能：关闭一个文件指针，无法在对当前文件进行操作
参数：
  stream：指定的文件指针，fopen函数的返回值
返回值：
  成功：0
  失败：EOF
注意：注意一个文件只能关闭一次，不能多次关闭。
  关闭文件之后就不能再文件指针对文件进行读写等操作了。
```



> 示例程序：对`file.txt`进行`fclose`操作

```C
#include <stdio.h>

int main(int argc, char *argv[])
{
  //使用fopen函数打开或者创建文件，返回文件指针
  FILE *fp;

  //以只写的方式打开文件，如果文件不存在则创建，如果文件存在则追加
  fp = fopen("C:/Users/lzx/Desktop/file.txt", "a");
  if(fp == NULL)
  {
    printf("fail to fopen\n");
    return ‐1;
  }

  //使用fclose关闭文件
  fclose(fp);
  return 0;
}
```



## 五、一次读写一个字符

### 5.1 fgetc

```C
#include <stdio.h>
int fgetc(FILE *stream);
功能：从文件指针标识的文件中读取一个字符
参数：
  stream：指定的文件指针
返回值：
  成功：读取的字符
  失败：EOF
  如果文件读取完毕，也会返回EOF
```



> `file.txt`文件内容

```C
hello
666
```



> 示例程序：对`file.txt`进行`fgetc`操作

```C
#include <stdio.h>

int main(int argc, char *argv[])
{
  FILE *fp;
  fp = fopen("C:/Users/lzx/Desktop/file.txt", "r");
  if(fp == NULL)
  {
    printf("fail to fopen\n");
    return ‐1;
  }

  //使用fgetc从文件中读取一个字符
  // int c = fgetc(fp);
  // printf("c = [%c] ‐ %d\n", c, c);

  // c = fgetc(fp);
  // printf("c = [%c] ‐ %d\n", c, c);

  //文件的每一行结束的位置都有一个标识，是一个换行符，称之为行结束符
  //fgetc可以读取到行结束符
  int c;
  //遍历文件中内容,每次读取一个字符,每读取一次就移动到下一个字符
  while((c = fgetc(fp)) != EOF) 
  {
    printf("c = [%c] ‐ %d\n", c, c);
  }
  //注意：打开文件的时候，默认读写位置在文件的开始，如果以 a 的方式打开读写位置在文件的末尾
  //咱们向文件中读取字节或写入字节的时候，读写位置会往文件的末尾方向偏移，读写多少个字节，读写位置就往
  //文件的末尾方向偏移多少个字节
    return 0;
}
```

> 执行结果：`file.txt`文件内容

```C
c = [h] - 104
c = [e] - 101
c = [l] - 108
c = [l] - 108
c = [o] - 111
c = [
] - 10
c = [6] - 54
c = [6] - 54
c = [6] - 54
```



### 5.2 fputc

```C
#include <stdio.h>
int fputc(int c, FILE *stream);
功能：向文件指针标识的文件中写入一个字符
参数：
  c：要写入的字符
  stream：指定的文件指针
返回值：
  成功：要写入的字符
  失败：EOF
```



> 示例程序：对`file.txt`进行`fputc`操作

```C
#include <stdio.h>

int main(int argc, char *argv[])
{
  FILE *fp;
  fp = fopen("C:/Users/lzx/Desktop/file.txt", "w");
  if(fp == NULL)
  {
    printf("fail to fopen\n");
    return ‐1;
  }

  //通过fputc函数向文件写入一个字符
  fputc('w', fp);
  fputc('h', fp);
  fputc('a', fp);
  fputc('t', fp);
  fputc('\n', fp);
  fputc('o', fp);

  return 0;
}
```

> 执行结果：`file.txt`文件内容

```C
what
o
```



## 六、一次读写一个字符串

### 6.1 fgets

```C
#include <stdio.h>
char *fgets(char *s, int size, FILE *stream);
功能：从文件中读取内容
参数：
  s：保存读取到的内容
  size：每次读取的最大个数
  stream：文件指针
返回值：
  成功：读取的数据的首地址
  失败：NULL
  如果文件内容读取完毕，也返回NULL
注意：从stream所指的文件中读取字符，在读取的时候碰到换行符或者是碰
     到文件的末尾停止读取，或者是读取了size‐1个字节停止读取，在读取
     的内容后面会加一个\0,作为字符串的结尾
```



> `file.txt`文件内容

```C
hello world
nihao beijing
```



> 示例程序：对`file.txt`进行`fgets`操作

```C
#include <stdio.h>

int main(int argc, char *argv[])
{
  FILE *fp;
  fp = fopen("C:/Users/lzx/Desktop/file.txt", "r");
  if(fp == NULL)
  {
    printf("fail to fopen\n");
    return ‐1;
  }

  //使用fgets读取文件内容
  //fgets每次读取时最多读取文件一行内容，只要遇到行结束符就立即返回
  //如果想要读取的字节数小于一行内容，则只会读取第二个参数‐1个字节，
  //最后位置补\0
  char buf[32] = "";
  //fgets(buf, 8, fp);
  fgets(buf, 32, fp);
  printf("buf = %s\n", buf);

  return 0;
}
```

> 执行结果：`file.txt`文件内容

```C
buf = hello world
```



### 6.2 fputs

```C
#include <stdio.h>
int fputs(const char *s, FILE *stream);
功能：向文件写入数据
参数：
  s：要写入的内容
  stream：文件指针
返回值：
  成功：写入文件内容的字节数
  失败：EOF
```



> 示例程序：对`file.txt`进行`fputs`操作

```C
#include <stdio.h>

int main(int argc, char *argv[])
{
  FILE *fp;
  fp = fopen("C:/Users/lzx/Desktop/file.txt", "w");
  if(fp == NULL)
  {
    printf("fail to fopen\n");
    return ‐1;
  }

  //通过fputs函数向文件写入数据
  fputs("66666666666666\n", fp);
  fputs("nihao", fp);

  return 0;
}
```

> 执行结果：`file.txt`文件内容

```C
66666666666666
nihao
```



## 七、读文件fread

```C
#include <stdio.h>
size_t fread(void *ptr, size_t size, size_t nmemb, FILE *stream);
功能：从文件中读取数据
参数：
  ptr：保存读取的数据
  size：每次读取的字节数
  nmemb：一共读取的次数
  stream：文件指针
返回值：
  成功：实际读取的次数（对象数、块数）
  失败：0
  如果文件内容读取完毕，返回0
```



> 对`fread`的说明

```C
例1：
int num;
num=fread(str,100,3,fp);
从fp所代表的文件中读取内容存放到str指向的内存中，读取的字节数为 ，每块100个字节，3块。
返回值num，
  如果读到300个字节返回值num为3
  如果读到了大于等于200个字节小于300个字节 返回值为2
  读到的字节数，大于等于100个字节小于200个字节 返回1
  不到100个字节返回0
```





> `file.txt`文件内容

```C
66666666666666
nihao
```



> 示例程序：对`file.txt`进行`fread`操作

```C
#include <stdio.h>

int main(int argc, char *argv[])
{
  FILE *fp;
  fp = fopen("C:/Users/lzx/Desktop/file.txt", "r");
  if(fp == NULL)
  {
    printf("fail to fopen\n");
    return ‐1;
  }

  //使用fread函数读取文件内容
  int num;
  char buf[128] = "";
  num = fread(buf, 5, 4, fp);

  printf("buf = %s\n", buf);
  printf("num = %d\n", num);

  return 0;
}
```

> 执行结果

```C
buf = 66666666666666
nihao
num = 4
```



## 八、写文件fwrite

```C
#include <stdio.h>
size_t fwrite(const void *ptr, size_t size, size_t nmemb, FILE *stream);
功能：向文件中写入数据
参数：
  ptr：要写入的数据
  size：一次写入的字节数
  nmemb：一共写入的次数
  stream：文件指针
返回值：
  成功：实际写入的次数
  失败：0
```



> 示例代码：对`file.txt`进行`fwrite`操作

```C
#include <stdio.h>

typedef struct{
  int a;
  int b;
  char c;
}MSG;

int main(int argc, char *argv[])
{
  FILE *fp;
  fp = fopen("C:/Users/lzx/Desktop/file.txt", "w+");
  if(fp == NULL)
  {
    printf("fail to fopen\n");
    return ‐1;
  }

  //使用fwrite向文件写入一个结构体
  MSG msg[4] = {1, 2, 'a', 3, 4, 'b', 5, 6,'c', \
  7, 8, 'd'};

  fwrite(msg, sizeof(MSG), 4, fp);
  //fwrite,打开file.txt会发现里面有乱码
  //因为我们的结构体在计算机中存储的方式和字符串是不一样的
  //如果我们想要得到file.txt的正确内容,我们下面用fread操作
  //fwite操作后,文件偏移量会移动到文件的末尾

  //将文件的偏移量设置为文件的起始位置
  //假如不进行rewind,那么fread会直接从文件的末尾开始读,会读出来为空
  rewind(fp);

  MSG rcv[4];
  fread(rcv, sizeof(MSG), 4, fp);
  int i;
  for(i = 0; i < 4; i++)
  {
    printf("%d ‐ %d ‐ %c\n", rcv[i].a, rcv[i].b, rcv[i].c);
  }
  
  return 0;
}
```

> 执行结果

- 第一次`fwrite`执行结果

![image1.png](https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Embedded_high-level_C_programming/10_file_image1.png)

- 第二次`fread`执行结果

```C
1 - 2 - a
3 - 4 - b
5 - 6 - c
7 - 8 - d
```



## 九、格式化读写文件函数

```C
函数调用:
fprintf(文件指针,格式字符串,输出表列);
fscanf(文件指针,格式字符串,输入表列);

函数功能:
  从磁盘文件中读入或输出字符

fprintf 和printf函数类似：
  printf是将数据输出到屏幕上（标准输出），
  fprintf函数是将数据输出到文件指针所指定的文件中。

fscanf和scanf 函数类似：
  scanf是从键盘（标准输入）获取输入，
  fscanf是从文件指针所标示的文件中获取输入。
```





> 示例程序：对`file.txt`进行`fprintf`和`fscanf`操作

```C
#include <stdio.h>

int main(int argc, char *argv[])
{
  FILE *fp;
  char ch1='a', ch2;
  int num1=50, num2;
  char string1[20]="hello", string2[20];
  float score1 = 85.5, score2;

  if((fp = fopen("C:/Users/lzx/Desktop/file.txt","w+"))==NULL)
  {
    printf("fail to fopen\n");
    return ‐1;
  }

  //使用fprintf向文件写入字符串
  fprintf(fp,"%c %d %s %f\n",ch1,num1,string1,score1);

  rewind(fp);

  //使用fscanf获取文件内容
  fscanf(fp,"%c %d %s %f\n",&ch2,&num2,&string2,&score2);
  printf("%c %d %s %f\n",ch2,num2,string2,score2);

  fclose(fp);
  return 0;
}
```



> 执行结果

- `file.txt`文件内容

```C
a 50 hello 85.500000
```

- 运行终端

```C
a 50 hello 85.500000
```



## 十、随机读写

前面介绍的对文件的读写方式都是顺序读写，即读写文件只能从头开始，顺序读写各个数据。

但在实际问题中常要求只读写文件中某一指定的部分，例如：读取文件第200--300个字节。

为了解决这个问题可以移动文件内部的位置指针到需要读写的位置，再进行读写，这种读写称为随机读写。

实现随机读写的关键是要按要求移动位置指针，这称为文件的定位。



### 10.1 rewind

```C
#include <stdio.h>
void rewind(FILE *stream);
功能：将文件位置定位到起始位置
参数：
  stream：文件指针
返回值：无
```



### 10.2 ftell

```C
#include <stdio.h>
long ftell(FILE *stream);
功能：获取当前文件的偏移量
参数：
  stream：文件指针
返回值：
  获取当前文件的偏移量
```



### 10.3 fseek

```C
#include <stdio.h>
int fseek(FILE *stream, long offset, int whence);
功能：设置文件位置指针的偏移量
参数：
  stream：文件指针
  offset：偏移量
  可正可负也可为0
  whence：相对位置
  SEEK_SET 文件起始位置
  SEEK_CUR 文件当前位置
  SEEK_END 文件末尾位置（最后一个字符后面一个位置）
返回值：
  成功：0
  失败：‐1

rewind(fp) <==> fseek(fp, 0, SEEK_SET);
这样fseek就可以实现rewind的功能
```



> 示例程序：对`file.txt`进行`rewind,ftell,fseek`操作

```C
#include <stdio.h>

int main(int argc, char *argv[])
{
  FILE *fp;
  if((fp = fopen("C:/Users/lzx/Desktop/file.txt","w+"))==NULL)
  {
    printf("fail to fopen\n");
    return ‐1;
  }

  fputs("123456789\n", fp);
  fputs("abcdefghijklmn", fp);

  //获取当前文件指针的读写位置
  printf("offset = %ld\n", ftell(fp));

  //将当前文件的读写文件设置到文件的起始位置
  //必须将当前文件的读写文件设置到文件的起始位置,因为fputs操作后读写位置已经到了文件的末尾,打印出来为空
  rewind(fp);
  fseek(fp, 0, SEEK_SET);

  //将当前文件的读写位置设置为倒数第五个位置
  //fseek(fp, ‐5, SEEK_END);
  
  char buf[32] = "";
  while(fgets(buf, 32, fp) != NULL)
  {
    printf("[%s]\n", buf);
  }

  return 0;
}
```



> 执行结果

- `file.txt`文件内容

```C
123456789
abcdefghijklmn
```

- 运行终端

```C
offset = 25
[jklmn]  
```

**彩蛋**🎁

整个春天，直至夏天，都是生命力独享风流的季节。长风沛雨，艳阳明月。那时田野被喜悦铺满，天地间充着生的豪情，风里梦里也全是不屈不挠的欲望。春天的美丽也正在于此，在于纯真和勇敢，在于未通世故。

- 史铁生《比如摇滚与写作》

恭喜你🎉，完成了对第十章《文件》部分的学习，下一章我们将学习Linux基础命令。

⏩第十一章《Linux基础命令》
