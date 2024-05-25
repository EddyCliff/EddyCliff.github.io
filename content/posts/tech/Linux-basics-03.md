---
title: "Linux入门：文件IO"
date: 2024-04-21T00:17:58+08:00
lastmod: 2024-04-21T00:17:58+08:00
author: ["Eddy"]
keywords: 
- Linux
- Operating System
- Operating system command
- 操作系统
- 操作系统命令
- 操作系统基本命令
- Linux命令
- Linux基本命令
- Linux新手必看
categories: 
- 
tags: 
- Linux
- 操作系统
- 文件IO
description: ""
weight: 3
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
    image: "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/BlogCover/linux.png" #图片路径例如：posts/tech/123/123.png
    caption: "" #图片底部描述
    alt: ""
    relative: false
---

## INIT

<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Blog-Common-Images/INIT.jpg" alt = "INIT.jpg" width = "70%" height = "auto">
</div>
<br>

>**INIT：本节内容正式开始。action!**

## 库的制作与使用

### **库文件的定义**

库文件是计算机软件中包含一系列函数、类、数据结构和资源的文件，它们被编译成机器码，可以在运行时被其他程序链接和调用。库文件能够提供一些常用的功能，使得程序员不必“重新发明轮子”，可以更高效地开发软件。
库文件主要有以下几种类型：
1. **静态库（Static Libraries）**：在程序编译链接时，静态库的代码会被直接整合进最终的可执行文件中。在程序运行时，不需要再次加载静态库。常见的静态库文件扩展名有 .a（Linux和Unix系统）和 .lib（Windows系统）。
2. **动态库（Dynamic Libraries）**：与静态库不同，动态库在程序编译时不会包含在可执行文件中，而是在程序运行时动态加载。这样多个程序可以共享同一个库文件，节省内存和磁盘空间。常见的动态库文件扩展名有 .so（Linux系统）、.dylib（macOS系统）和 .dll（Windows系统）。
3. **共享库（Shared Libraries）**：通常与动态库同义，但在某些操作系统中，共享库可能指的是可以被多个程序共享的库，这个概念可以同时包括静态库和动态库。

库文件通过提供预先编写和测试的代码，可以大大提高软件开发效率，确保软件质量，并简化维护工作。在遵守相关开源协议或商业许可的前提下，开发者可以合理使用库文件来构建自己的应用程序。

🤔**静态库 VS 动态库**

比如你晚上做一个菜，叫小炒黄牛肉。

- **静态库**：就像你白天查了小炒黄牛肉的菜谱，然后把整个菜谱的内容记在脑子里。到了晚上做菜的时候，你不需要再查阅菜谱，因为所有的信息都已经在你脑子里了。同样地，静态库在程序编译链接时就已经被整合到可执行文件中，程序运行时不需要再次加载库。
    
- **动态库**：这就像是你在白天查到了小炒黄牛肉的菜谱，并且记下了网页地址。晚上做菜时，你再次访问那个网页，按照网页上的步骤来做菜。同样地，动态库在程序编译时不会包含在可执行文件中，而是在程序运行时动态加载，这样不同的程序可以共享同一个库文件，只有在需要的时候才会被载入内存。

**静态库（类似于书店，只卖不借）**

在编译程序的时候静态库的内容会被完成的复制到程序的内部。

优点

- 当我们运行该程序是不会出现缺失的问题

缺点

- 不利于更能的更新
- 需要占用更多的内存

**动态库（类似于图书馆，只借不卖）**

在编译的时候动态库并没被复制到程序中，而是检查将否是否异常*（参数、返回值、函数名..

优点

- 相对于静态库来说占用更少的内存
- 对程序执行的效率有一定的提升
 
缺点

- 当程序执行的时候需要有动态库的支持


📋**举个例子（什么是库文件）**

如果你开发了一个图像识别的功能，并且将其编译成库文件，比如动态库或静态库，你可以授权给其他公司使用这个库文件，而无需提供源代码。这样，其他公司可以在他们的应用程序中调用你的库提供的功能，从而实现图像识别。

由于库文件是编译后的二进制形式，外界无法直接从库文件中读取源代码，这在一定程度上保护了你的知识产权。尽管存在反编译工具和技术，可以将二进制文件转换为汇编语言或高级语言，但这种转换通常不会生成易于理解的源代码，且反编译过程可能涉及法律风险和复杂的技术挑战。

此外，你还可以选择仅提供托管服务（SaaS），让客户通过API访问您的图像识别功能，而不直接提供库文件，这样也能有效控制功能的访问和使用。


### **库文件的命名**

- 必须使用lib作为前：比如`libDeployPkg.so.0`和`libhgfs.so.0`.....
- 静态库一般以.a为后，动态库一般以.so为后
- 库文件会有不同的版本，一般写在后后面，比如lib.a.so.0.1.2

```markdown
libc.so.1.03
	1ib     库文件的前缀
	c 库的名字(链接库文件时，只需要写上该名字)
	.so 后缀(so为动态库/共享库，a则是静态库)
	.1 库文件的版本号
	.0.3 修正号
```

### **如何制作库文件**

#### **静态库的制作**

📋举例：以`demo`项目为例。

```txt
demo/
├── bin (可执行文件)
├── inc (头文件)
│   ├── main.h
│   ├── max.h
│   └── swap.h
├── lib (库文件)
└── src (源代码)
    ├── main.c
    ├── max.c (封装进静态库)
    └── swap.c (封装进静态库)
```

这个项目就是在`main.c`中调用`swap.c`实现交换a和b的值，调用`max.c`实现找出a,b,c中的最大值，而我们要把`swap.c`和`max.c`的功能封装进静态库，最终`src`源代码只保留一个`main.c`。

`main.h`

```
#ifndef __MAIN_H

#define __MAIN_H

#include <stdio.h>

#include "max.h"

#include "swap.h"

#endif
```

`max.h`

```
#ifndef __MAX_H

#define __MAX_H

#include <stdio.h>

/*
功  能： 比较获得较大的值
参  数： @a 比较值1  @b 比较值2 @c 比较值3
返回值： 三个参数中较大的
作  者：
修改日期：
更新内容：
联系方式：
版  本：
*/

int max( int a , int b , int c ) ;

#endif
```

`swap.h`

```
#ifndef __SWAP_H

#define __SWAP_H

#include <stdio.h>

/*
功  能：交换a和b的值
参  数：@a @b
返回值：无
作  者：
修改日期：
更新内容：
联系方式：
版  本：
*/

void swap( int * a , int * b );

#endif
```

`swap.c`
```c
# include "swap.h"

void swap(int * a, int * b)
{
	int temp = *a; 
	*a = *b; 
	*b = temp;
}
```

`max.c`

```c
#include "max.h"

int max(int a, int b, int c)
{
	int max = a > b ? a : b;
	max = max > c ? max : c;
	return max;
}
```

`main.c`
```c
#include    "main.h"

int main(int argc, char const *argv[])
{
    int x = 129 ;

    int y = 245;

    int z = 350;
    
    int num = max( x , y , z ) ; 

    printf("max:%d\n" , num ); //打印a,b,c中的最大值

    swap( &x , &y);

    printf("x:%d y:%d\n" , x , y ); //打印a,b交换后的值

    return 0;

}
```

下面演示针对`demo`这个项目制作静态库文件。

1）先获得.o文件

```shell
gcc swap.c -o swap.o -c -fPIC -I../inc 
gcc max.c -o max.o -c -fPIC -I../inc
# -c 是让编译在生成可重定位文件后停止工作
# -fPIC 是告诉编译器生成与地址无关的可执行文件
```

2）把以上生成的两个.o文件一起编译成静态库文件

```shell
ar -rcs libmy_lib.a max.o swap.o
```

3）如何使用静态库文件

```markdown
$ gcc src/main.c -I./inc -L./lib -lmy_lib -o bin/main
- `gcc` 编译器
- `src/main.c` 源文件
- `-I./inc`  -I 指定头文件路径为 ./inc
- `-L./lib` -L 指定库文件路径为 ./lib
- `-lmy_lib` -l 链接库名为 my_lib
- `-o bin/main` -o 指定生成二进制文件为 bin/main 
```

💡**GCC：**

GCC编译器常用选项

```markdown
`-I<PATH>` 指定头文件路径 gcc hello.c -o hello -I./include
`-L<PATH>` 指定库文件路径 gcc hello.c -o hello -L./lib
```

GCC链接

```markdown
$ gcc hello.o -o hello -lc -lgcc 
编译`hello.o`文件，生成名为`hello`的可执行文件，并链接C标准库和GCC的库。

- `gcc` 
- `hello.o`  编译过程中的一个目标文件（object file），它是源代码文件（比如`hello.c`）经过编译器编译后生成的。
- `-o` hello 这个选项告诉GCC，将编译后的可执行文件命名为`hello`。如果省略这个选项，GCC默认会生成名为`a.out`的可执行文件。
- `-lc`  -l 链接 c 标准C库
- `-lgcc` -l 链接 gcc 链接GCC的库
```

#### 动态库的制作

1）先获得.o文件

```shell
gcc swap.c -o swap.o -c -fPIC -I../inc 
gcc max.c -o max.o -c -fPIC -I../inc
# -c 是让编译在生成可重定位文件后停止工作
# -fPIC 是告诉编译器生成与地址无关的可执行文件
```

2）把.o文件制作成动态链接库文件

```markdown
`$ gcc -shared -fPIC -o libmy_lib.so *.o`  
- `gcc` 编译器  
- `-shared` 生成动态链接库  
- `-fPIC` 生成于地址无关的二进制文件  
- `-o libmy_lib.so` 输出一个动态链接库名字， libmy_lib.so.0  
- `*.o` 所有的.o文件
```

3）如何使用动态链接

- 如何编译

```shell
gcc src/*.c -I./inc -L./lib -lmy_lib -o bin/mian
```

- 如何运行



<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Linux-Basics/Linux-%E6%96%87%E4%BB%B6IO-41.png" alt = "INIT.jpg" width = "70%" height = "auto">
</div>
<br>

🤔问题：

因为动态库在我们运行程序的时候需要加载到内存中，加载的动作由系统来完成。系统并不知道我们的库在哪里，因此加载失败。

🔑如何解决：

方法一（推荐使用）：

系统虽然不知道我们自己的库在哪里，但是它有一些默认路径（在环境变量中预先写好的路径）。因此我们可以把自己的动态库文件放到系统默认的路径下。（`/usr/lib`或者`/lib`）

```shell
sudo cp lib/libmy_lib.so /usr/lib/   # 把自己的库文件拷贝到`/usr/lib`
```


方法二：

把库所在的路径写入（添加）到环境变量中

- 先确定自己的库路径在哪里（不推荐使用共享路径）

- 打开配置文件（.bashrc）

```shell
vim ~/.bashrc
```

- 在文件末尾添加一行
```markdown
`export LD_LIBRARY_PATH=home/zhipu/my_lib:$LD_LIBRARY_PATH`

- `export` 临时修改环境变量，因为每次都会运行这个.bashrc，所以也就实现了永久修改
- `LD_LIBRARY_PATH` 修改的是库的路径
- `home/zhipu/my_lib` 需要添加的具体你自己的库路径
- `:` 分隔符
- `$LD_LIBRARY_PATH` 重新引用原有的路径 类似于 a += b，假如不添加这个的话，那就只会引用 home/zhipu/my_lib 这一个库，而不会引用原本的库了 
```



<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Linux-Basics/Linux-%E6%96%87%E4%BB%B6IO-42.png" alt = "INIT.jpg" width = "70%" height = "auto">
</div>
<br>

- 可以重新打开新的终端或者重新生效配置脚本

```shell
source ~/.bashrc #重新生效 `~./bashrc`文件
```

方法三：

在系统的默认路径下添加一个新路径

- 先确定自己的库路径在哪里（不推荐使用共享路径）

- 打开配置文件（libc.conf）

```shell
sudo vim /etc/ld.so.conf.d/libc.conf
```

- 在文件末尾添加一行，添加你自己的库路径



<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Linux-Basics/Linux-%E6%96%87%E4%BB%B6IO-43.png" alt = "INIT.jpg" width = "70%" height = "auto">
</div>
<br>

- 重新生效库的配置文件

```shell
sudo ldconfig
```

📢注意：

- 如果动态库与静态库名字一样，而且放一起，则编译器默认使用动态链接。

- 如何指定让编译器使用静态库 -static
```shell
gcc src/*.c -o bin/main -I./inc -L./lib -lmy_lib -static
# `- static` 让编译器选择静态编译，程序会变得很大
# 以后运行的时候完全不需要任何的动态库支持
```

🤔到目前为止，可以看到我们的命令行的文件路径越来越长了，有没有办法缩短一些呢？



<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Linux-Basics/Linux-%E6%96%87%E4%BB%B6IO-44.png" alt = "INIT.jpg" width = "70%" height = "auto">
</div>
<br>

🔑在`~/.bashrc`修改来Linux命令行文件路径隐藏



<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Linux-Basics/Linux-%E6%96%87%E4%BB%B6IO.png" alt = "INIT.jpg" width = "70%" height = "auto">
</div>
<br>

比如我们添加一行

```shell
export PS1 = `[\u@\h \W]\$`
```

可以看到我们命令行的文件路径变短了。



<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Linux-Basics/Linux-%E6%96%87%E4%BB%B6IO-45.png" alt = "INIT.jpg" width = "70%" height = "auto">
</div>
<br>

这个改法有很多，大家可以自行研究。


🤔到目前为止，可以看到我们的编译命令越来越复杂了，有没有办法改进一下呢？

下面我们开始讲 Makefile 它将解决这个问题。


## Makefile

### 工程管理器Make

当我们需要编译一个比较大的项目时，编译命令会变得越来越复杂， 需要编译的文件越来越多。其次就是项目中并不是每一次编译都需要把所有文件都重新编译，比如没有被修改过的文件则不需要重新编译。工程管理器就帮助我们来优化这两个问题。

MakeFile就类似于make工程管理的工作的脚本。 用来告诉工程管理器如何正确的编译我们的程序。

📜原文：

当我们要编译成千上万个源程序文件的时候，光靠手工地使用GCC工具来达到目的也许就会很没有效率，我们需要一款能够帮助我们自动检查文件的更新情况，自动进行编译的软件，GNU make（工程管理器 make 在不同环境有很多版本分支，比如Qt下的gmake，Windows下的nmake等，下面提到的make指的是Linux下的GNUmake）就是这样的一款软件。

而 Makefile ，是 make 的配置文件，用来配置运行 make 的时候的一些相关细节，比如指定编译选项，指定编译环境等等。一般而言，一个工程项目不管是简单还是复杂，每一个源代码子目录都会有一个 Makefile 来管理，然后一般有个所谓的顶层 Makefile 来统一管理所有的子目录 Makefile 。

在撸起袖子准备大干一场之前，明确学习目的非常重要，因为 Makefile 的语法相对晦涩，尤其对于没有任何 Linux 编程和 Shell 编程经验的新手而言，第一次打开 Makefile 阅读常常有以为是乱码的幻觉！因此面对这样的东西初学者如果抱着对每一个细节“死追不放“的心态可能会死得很惨，信心将被大大挫败，而信心和兴趣的缺失是学习最大的敌人。

假如你是实用主义者，为的是在Linux编程开发不被 Makefile 难倒，那我们学习Makefile的程度仅限于看得懂就行了，顶多有时会对某些大型项目的Makefile进行修改，但绝对不需要你像对C语言那样达到“精通到骨子”的程度，而这一节的内容就是为这样的人准备的。另一方面，如果你是学院派，需要对工程管理做学术型研究，那可能除了阅读以下内容之外还需要阅读其他专门探讨该专题的文献，但不管你是哪一类人，以下内容作为学习Makefile 的入门及提高的读物，应该算是这个地球上你能找得到的最贴心的资料了。

好了，下面通过一个经典例子，说明一下我们为什么需要 make 来管理工程项目：

📋示例：

假设我们有一个工程，这个工程总共有4个源文件，姑且叫做a.c、b.c以及×.c和y.c吧，他们最终将会链接生成可执行文件image


<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Linux-Basics/Linux-%E6%96%87%E4%BB%B6IO-46.png" alt = "INIT.jpg" width = "70%" height = "auto">
</div>
<br>


   在开发的过程当中，假设我们对`×.c`这个源文件进行了修改，那么，为了在最终的`image`当中体现出来，我们必须重新编译生成`×.o`，然后必须重新编译链接生成`image`文件，此过程中，其他未经修改的文件以及他们的目标文件都不需要改动：



<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Linux-Basics/Linux-%E6%96%87%E4%BB%B6IO-47.png" alt = "INIT.jpg" width = "70%" height = "auto">
</div>
<br>

由于文件比较少，我们用肉眼就可以简单地别，究竟哪些要编译哪些不需要重新再搞一遍，甚至所有文件重新编译一次也不是什么十恶不赦的事情。但是考虑一下一个由成千上万个源文件组成的庞大工程，比如Linux源码，一旦我们对若干个地方进行了修改，重新编译的文件则需要精心地挑选，否则如果整体编译必将会浪费大量时间，这个“精心挑选“的任务，就留给make帮我们来实现。

### 依赖与目标的关系

在 Makefile 中依赖与目标是相互的， 并不是绝对。



<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Linux-Basics/Linux-%E6%96%87%E4%BB%B6IO-48.png" alt = "INIT.jpg" width = "70%" height = "auto">
</div>
<br>

在上面的例子中，image是最终的标，其依赖是四个可重定位文件，而对于每一个可重定位文件而言，他们自己本身也是标，依赖于其相对应的.c源程序文件。在make的眼中，所有的文件都有这么一层一层推的日标-依赖关系，然后通过对比日标和依赖的时间截来决定下一步动作，这就是make的最基本的工作原理。

### 安装make

```shell
sudo apt install make
```

#### 第一个Makefile

语法：
```markdown
目标:依赖
	命令
```

📢注意：
- 目标必须存在
- 依赖可以没有
- 命令前面必须是一个制表符“TAB”
- Makefile 文件的命名一般是 Makefile 没有前缀也没有后缀

以下两行就是一套规则。


<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Linux-Basics/Linux-%E6%96%87%E4%BB%B6IO-49.png" alt = "INIT.jpg" width = "70%" height = "auto">
</div>
<br>

当我们输入make的时候  

1）工程管理器，先会默认在当前路径下寻找一个名为Makefile的文件  

2）确认Makefile文件中的目标（最终）  

3）检查最终目标是否已经存在  

- a.如果目标存在，则检查规则中是否存在依赖。  
	- i.如果规则中没有依赖，则不运行规则。  
	- ii.如果有依赖，则检查依赖是否真实存在。  
		- 1.依赖真实存在，检查时间截判断是否执行规则。  
		- 2.依赖不真实存在，则直接报错。  
- b.如果目标不存在，则直接运行规则。  


📢注意：

- 如果规则中没有写依赖，则无论如何该规则都会执行。

📋以下面这个makefile为例。

```Makefile
Even:
	echo "Hello Makefile"
```

然后我们在终端输入一些指令

```shell
[zhipu] $ make
echo "Hello Makefile"
Hello Makefile
```

可见规则中无依赖，无论如何规则都会执行，由于没有用`@`隐藏不输出当前命令，所以会先打印该命令，再打印该命令的结果。

- 如果目标已经存在 ，然后也没有写依赖则不执行该规则。

📋以下面这个makefile为例。

```Makefile
Even:
	echo "Hello Makefile"
```

然后我们在终端输入一些指令

```shell
[zhipu]$ make
Hello Makefile
[zhipu] $ touch Even
[zhipu] $ make
make: 'Even' is up to date
```



<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Linux-Basics/Linux-%E6%96%87%E4%BB%B6IO-50.png" alt = "INIT.jpg" width = "70%" height = "auto">
</div>
<br>

你可以理解为你的目标是月入百万的话，假如你已经实现了，那你还有目标吗，那就没有目标了，因为已经实现了。

- 如果目标文件已经存在，规则中有写依赖并且依赖文件比目标文件更新，则规则会被执行。

📋以下面这个makefile为例。

```Makefile
Even:Jacy
	echo "Hello Makefile"
```

我们先创建两个文件Even和Jacy。



<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Linux-Basics/Linux-%E6%96%87%E4%BB%B6IO-51.png" alt = "INIT.jpg" width = "70%" height = "auto">
</div>
<br>

可以看到目标 `Even`比依赖`Jacy`更新，所以该规则不会执行。

- 注意制表符后面紧接着的是一个Shell命令。

#### 目标与依赖的互相嵌套(类似函数调用)

Makefile代码如下：

```makefile
Even:Jacy
	@echo:"Hello Makefile"

Jacy:Cuihua
	@echo:"Hello Even"

Cuihua:
	@echo:"Hello Jacy"
```

执行

```shell
[zhipu] $ make #工程管理器把第一个目标当做最终目标 `Even`
Hello Jacy
Hello Even
Hello Makefile
[zhipu] $ make Jacy # 告诉工程管理器 `Jacy` 是我们的最终目标
Hello Jacy
Hello Even
```

**下面我们使用makefile去编译之前的项目**

```txt
demo/
├── bin (可执行文件)
├── inc (头文件)
│   ├── main.h
│   ├── max.h
│   └── swap.h
├── lib (库文件)
│   ├── libmy_lib.a
│   ├── libmy_lib.so
└── src (源代码)
    └── main.c
```

对于以上工程

目标：`demo/bin/main`

依赖：`demo/src/*.c`

makefile代码如下：
```makefile
demo/bin/main:demo/src/*.c
	gcc demo/src/*.c -o demo/bin/main -I./inc -L./lib -lmy_lib
```

### 变量

在Makefile中变量属于弱类型， 在Makefile中变量就是一个名字（像是C语言中的宏），代表个文本字符串 （变量的值），在Makefile的目标、 依赖、命令中都可以引用变量。

在Makefile中变量的特征有以下几点：

1）变量和函数的展开（除规则的命令行以外），是在make读取Makefile文件时进行的，这里的变量包括了使用 "=" 定义和使用指示符"define"定义的变量。

2）变量可以用来代表一个文件名列表、编译选项列表、程序运行的选项参数列表、搜索源文件的目录列表、编译输出的目录列表和所有我们能够想到的事物。

3）变量名不能包括":"、"#"、"="、前置空白和尾空白的任何字符串。需要注意的是尽管在GNUmake中没有对变量的命名有其它的限制，但定义一个包含除字母、数字和下划线以外的变量的做法也是不可取的，因为除字母、数字和下划线以外的其它字符可能会在以后的make版本中被赋予特殊含义，并且这样命名的变量对于一些Shell来说不能作为环境变量使用。

  4）变量名是大小写敏感的。变量"foo"、"Foo"和"FOO" 指的是三个不同的变量。Makefile传统做法是变量名是全采用大写的方式。 推荐的做法是在对于内部定义的一般变量（例如： 目标文件列表objects） 使用小写方式，而对于一些参数列表（例如：编译选项CFLAGS） 采用大写方式， 这并不是要求的。但需要强调一点：对于一个工程，所有Makefile
  中的变量命名应保持一种风格，否则会显得你是一个蹩脚的开发者（就像代码的变量命名风格一样），随时有被鄙视的危险。
  
5）另外有一些变量名只包含了一个或者很少的几个特殊的字符（符号）。称它们为自动化变量。像`"<"`，`"@"`，`"?"`，`"*"`，`"@D"`，    `"%F"`，`"^D"`等等，后面会详述之。

6）变量的引用跟Shell脚本类似，使用美元符号和圆括号，比如有个变量叫A，那么对他的引用则是`$(A)`，有个自动化变量叫@，则对他的引用是`$(@)`，有个系统变量是CC则对其引用的格式是`$(CC)`。 对于前面两个变量而言，他们都是单字符变量，因此对他们引用的括号可以省略，写成`$A`和`$@`。

**自定义变量**

顾名思义就是用户自己定义的变量。

版本一：

```makefile
A = apple # 定义并赋值变量
B = I love Apple
C = $(A) tree  #$( ) 则是对某一个变量进行引用

Even:
	@echo $(A)
	@echo $(B)
	@echo $(C)
```

版本二：

```makefile
TAG = ./bin/main
SRC = ./src/*.c
CC = gcc
O = -o
CONFIG = -I./inc -L./lib -lmy_lib

$(TAG):$(SRC)
	$(CC) $(SRC) $(O) $(TAG) $(CONFIG)

clean:
	rm ./bin/*
```

**系统预定义变量**



<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Linux-Basics/Linux-%E6%96%87%E4%BB%B6IO-52.png" alt = "INIT.jpg" width = "70%" height = "auto">
</div>
<br>

**自动化变量**
`<`、`@`、`?`、`#`等等，这些特殊的变量之所以称为自动化变量，是因为他们的值会“自动地”发生变化。



<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Linux-Basics/Linux-%E6%96%87%E4%BB%B6IO-53.png" alt = "INIT.jpg" width = "70%" height = "auto">
</div>
<br>



<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Linux-Basics/Linux-%E6%96%87%E4%BB%B6IO-54.png" alt = "INIT.jpg" width = "70%" height = "auto">
</div>
<br>



<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Linux-Basics/Linux-%E6%96%87%E4%BB%B6IO-55.png" alt = "INIT.jpg" width = "70%" height = "auto">
</div>
<br>

我们可以根据自动化变量对刚刚的Makefile做一些修改。

```Makefile
TAG = ./bin/main
SRC = ./src/*.c
CC = gcc
O = -o
CONFIG = -I./inc -L./lib -lmy_lib

$(TAG):$(SRC)
	$(CC) $(^) $(O) $(@) $(CONFIG)

# .PHONY:clean 表示 clean 是一个伪目标，它不是一个文件 
.PHONY: clean 

# 定义 clean 目标，用于清理生成的文件 
clean: 
	rm ./bin/*
```

#### Makefile中定义变量有以下几种不同方式

1）递归定义

```Makefile
A = I love $(B) # 在第一行使用到变量B 但是还没有定义，以此管理器进行全文搜索找到B并引用
B = Apple
```

2）直接定义

```Makefile
B = Apple 
A := I love $(B)
```

此处，定义A时用的是所谓的“直接”定义方式，说白了就是如果其定义里出现有对其他变量的引用的话，只对其前面的语句进行搜导 （不包含自己所在的那一行），而不是搜寻整个文件，因此，如果此处将变量A和变量B的定义交换一个位置：

```makefile
A := I love $(B) #A在B之前引用B，则为空
B = Apple
```

3）条件定义方式

有时我们需要先判断一个变量是否已经定义了，如果已经定义了则不作操作，如果没有定义再来定义它的值，这时最方便的方法就是采用所谓的条件定义方式：

```Makefile
A = apple
B ?= I love apple 
```

此处对A进行了两次定义，其中第二次是条件定义，其含义是：如果A在此之前没有定义，则定义为"I love apple"，否则维持原有的值。

4）多行命令定义方式

```makefile
define commands 
	echo "thank you!"
	echo "you are welcome"
endef
```

此处定义了一个包含多行命令的变量commands，我们利用它的这个特点实现一个完整命令包的定义。 注意其语法格式：以`define`开头，以`endef`结束。所要定义的变量名必须在指示符`define` 的同一行之后，指示符`define`所在行的下一行开始一直到`end` 所在行的上一行之间的若干行，是变量的值。这种方式定义的所请命令包，可以理解为编程语言中的函数。

#### Makefile中对变量进行操作

1）追加变量的值

例如

```makefile
A = apple
A += tree
```

这样，变量A的值就是apple tree。

2）修改变量的值

例如

```makefile
A = srt.c string.c tcl.c
B = $(A:%.c=%.o)
```

📋案例：我们演示一个含有多个`.c`文件的项目。

我们会利用到`B = $(A:%.c=%.o)`去修改makefile中变量的值。

项目如下：

```markdown
- demo
	- bin
	- src
		- Input.c
		- Output.c
		- Oper.c
		- main.c
	- inc
		- Input.h
		- Output.h
		- Oper.h
		- main.h
```

Makefile代码如下：

```makefile
TAG = ./bin/main
SRC = ./src/Input.c ./src/main.c ./src/Oper.c ./src/Output.c
OBJ = $(SRC:%.c=%.o)
CC=gcc
O=-o
CONFIG=-I./inc

$(TAG):$(OBJ)
	$(CC) $(OBJ) $(O) $(@) $(CONFIG)
	
%.o:%.c
	$(CC) $< -o $(@) $(CONFIG) -c

clean:
	$(RM) ./bin/* ./src/*.o
```

执行：
比如说我们更改了`Output.c`。

```shell
$ make
gcc src/Output.c -o src/Output.o -I./inc -c
gcc src/Input.o src/main.o src/Oper.o src/Output.o -o bin/main -I./inc
```

可以看到这个Makefile的目的是，只针对我们改动的`.c`文件编译生成新的`.o`文件，然后和原有的几个`.o`文件一起链接生成新的可执行文件。

所以步骤可以看到，`%.o:%.c` ：只针对我们改动的`.c`文件编译生成新的`.o`文件，`$(TAG):$(OBJ)` 然后和原有的几个`.o`文件一起链接生成新的可执行文件。

甚是精妙。👏

这就和之前的Makefile中，从源代码src到bin中的可执行文件一步到位不同。

这是之前一步到位的Makefile。

```makefile
$(TAG):$(SRC)
	$(CC) $(SRC) $(O) $(TAG) $(CONFIG)
	#作用：gcc src/main.c -o bin/main -I./inc
```


### 函数

📌**函数：subst**

```makefile
$(subst FROM,TO,TEXT)
```

功能：

将字符串TEXT中的字符FROM替换为TO

返回：

替换之后的新字符串

范例：

Makefile代码如下：

```makefile
$(subst pp,PP,apple tree)
```

替换之后变量A的值是"aPPle tree"

可以看到`subst`的作用和我们上一个makefile中`OBJ = $(SRC:%.c=%.o)`有异曲同工之妙。

📌**函数：wildcard**

```makefile
$(wildcard PATTERN)
```

功能：

获取匹配模式为PATTERN的文件名。

返回：

匹配模式为PATTERN的文件名。

范例：

Makefile代码如下：

```makefile
A = $(wildcard *.c)
```

假设当前路径下有两个.c文件，a.c和b.c，则处理后A的值为："a.c b.c"


📌**函数：override** 

作用：实现在makefile中增加或修改命令行参数。`override`在Makefile中用于强制执行特定的命令或变量设置，忽略任何外部尝试覆盖这些命令或变量的行为。

范例：

Makefile代码如下：

```makefile
A = apple tree
all:
	@echo $(A)
```

执行：

```shell
[zhipu] $ make A="apple"
apple
```

我们在`make`时，通过命令行重新指定Makefile中的变量。

而使用`override`呢？

```makefile
override A = apple tree
all:
	@echo $(A)
```

执行：

```shell
[zhipu] $ make A="apple"
apple tree
```

可以看到`override`强制指定变量A为"apple"，而没有被外部覆盖。

📌**函数：.PHONY** 

功能：`.PHONY`用于声明某些目标并不是文件名，而是特殊目标。这些特殊目标通常用于执行一些特定的操作，比如清理编译生成的文件。

```makefile
.PHONY clean:

clean:
	$(RM) ./bin/*	
```

- `.PHONY`: 这是一个指示符，告诉`make`工具，接下来的目标`clean`并不是一个文件，而是一个伪目标。
- 当你在命令行中运行`make clean`时，`make`工具会执行`clean`目标下的命令，即删除`./bin/`目录下的所有文件，而不会去检查是否存在一个名为`clean`的文件。
- 这个指令定义了一个名为`clean`的伪目标，用于清理编译生成的文件，确保项目的整洁。

📋案例：我们根据刚刚学习到的一些函数对之前的Makefile做一些改进。

项目如下：

```markdown
- demo
	- bin
	- src
		- Input.c
		- Output.c
		- Oper.c
		- main.c
	- inc
		- Input.h
		- Output.h
		- Oper.h
		- main.h
```

Makefile代码如下：

```Makefile
TAG = ./bin/main
SRC = $(wildcard src/*.c)
OBJ = $(SRC:%.c=%.o)
CC=gcc
O=-o
override CONFIG += -I./inc

$(TAG):$(OBJ)
	$(CC) $(OBJ) $(O) $(@) $(CONFIG)
	
%.o:%.c
	$(CC) $< -o $(@) $(CONFIG) -c

clean:
	$(RM) ./bin/* ./src/*.o

.PHONY:clean
```

### 总结



<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Linux-Basics/Linux-%E6%96%87%E4%BB%B6IO-56.png" alt = "INIT.jpg" width = "70%" height = "auto">
</div>
<br>



## 参考
（1）《Linux环境编程图文指南》

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