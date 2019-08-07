​                    










## 目  录

### 第1章 概述	- 4 -
1.1 HELLO简介	- 4 -
1.2 环境与工具	- 4 -
1.3 中间结果	- 4 -
1.4 本章小结	- 4 -
### 第2章 预处理	- 5 -
2.1 预处理的概念与作用	- 5 -
2.2在UBUNTU下预处理的命令	- 5 -
2.3 HELLO的预处理结果解析	- 6 -
2.4 本章小结	- 6 -
### 第3章 编译	- 7 -
3.1 编译的概念与作用	- 7 -
3.2 在UBUNTU下编译的命令	- 7 -
3.3 HELLO的编译结果解析	- 7 -
3.4 本章小结	- 13 -
### 第4章 汇编	- 14 -
4.1 汇编的概念与作用	- 14 -
4.2 在UBUNTU下汇编的命令	- 14 -
4.3 可重定位目标ELF格式	- 14 -
4.4 HELLO.O的结果解析	- 19 -
4.5 本章小结	- 22 -
### 第5章 链接	- 23 -
5.1 链接的概念与作用	- 23 -
5.2 在UBUNTU下链接的命令	- 23 -
5.3 可执行目标文件HELLO的格式	- 23 -
5.4 HELLO的虚拟地址空间	- 25 -
5.5 链接的重定位过程分析	- 26 -
5.6 HELLO的执行流程	- 29 -
5.7 HELLO的动态链接分析	- 29 -
5.8 本章小结	- 31 -
### 第6章 HELLO进程管理	- 32 -
6.1 进程的概念与作用	- 32 -
6.2 简述壳SHELL-BASH的作用与处理流程	- 32 -
6.3 HELLO的FORK进程创建过程	- 33 -
6.4 HELLO的EXECVE过程	- 33 -
6.5 HELLO的进程执行	- 34 -
6.6 HELLO的异常与信号处理	- 35 -
6.7本章小结	- 37 -
### 第7章 HELLO的存储管理	- 38 -
7.1 HELLO的存储器地址空间	- 38 -
7.2 INTEL逻辑地址到线性地址的变换-段式管理	- 38 -
7.3 HELLO的线性地址到物理地址的变换-页式管理	- 40 -
7.4 TLB与四级页表支持下的VA到PA的变换	- 40 -
7.5 三级CACHE支持下的物理内存访问	- 42 -
7.6 HELLO进程FORK时的内存映射	- 43 -
7.7 HELLO进程EXECVE时的内存映射	- 44 -
7.8 缺页故障与缺页中断处理	- 44 -
7.9动态存储分配管理	- 46 -
7.10本章小结	- 49 -
### 第8章 HELLO的IO管理	- 50 -
8.1 LINUX的IO设备管理方法	- 50 -
8.2 简述UNIX IO接口及其函数	- 50 -
8.3 PRINTF的实现分析	- 52 -
8.4 GETCHAR的实现分析	- 54 -
8.5本章小结	- 54 -
结论	- 55 -
附件	- 56 -
参考文献	- 57 -



# 第1章 概述
#### 1.1 Hello简介
最初我们什么都没有（0），通过我们键盘输入，写出一个.c程序。我们的hello.c程序(Program),经过预处理，编译，汇编，链接4个步骤后变为可执行文件，然后通过shell输入./hello，父进程为我们创建子进程（Process）,这就是P2P.
之后通过加载器和execve函数加载程序的数据代码到进程的虚拟内存空间，进入程序入口载入物理内存，进入main函数执行目标代码，CPU执行逻辑控制流，程序结束父进程回收hello进程，内核删除相关数据结构，程序变为0，为020过程。
#### 1.2 环境与工具
硬件信息：Intel Core i7-77000HQ, 8GRAM, 256GSSD
软件信息：VMware WorkStation 14  Ubuntu 18.04.1 LTS
使用工具：edb ,gdb ,Objdump ,readelf,gcc,ld 
#### 1.3 中间结果
hello.c：源程序文件
hello.i：预处理后的文本文件
hello.s：编译后的汇编文件
hello.o：汇编后的可重定位文件
hello：链接后的可执行文件
#### 1.4 本章小结
介绍了hello的P2P，O2O过程，介绍了实验的环境工具和产物。



# 第2章 预处理
#### 2.1 预处理的概念与作用
 ##### 1.概念
预处理是指在进行编译的第一遍扫描（词法扫描和语法分析）之前所作的工作。预处理器会对以字符#开头的预处理指令（宏定义，文件包含，条件编译）做解释，正如我们hello.c文件中的三个头文件，#include<stdio.h>命令告诉预处理器读取系统头文件stdio.h中的内容，并把它直接插入程序文本，就得到一个通常以.i结尾的预处理文件。
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229005148841.png)
（图2-1  hello中预处理指令）
#####  2.作用
代码的可移植性强，代码修改方便。


#####  2.2在Ubuntu下预处理的命令
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229005202454.png)
（图2-2 生成预处理文件）
gcc的-E选项，可以让编译器在预处理后停止，并输出预处理结果。利用 gcc -E hello.c -o test.i 生成预处理后的C语言程序






####  2.3 Hello的预处理结果解析
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229005229704.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mjk3MDQ1Ng==,size_16,color_FFFFFF,t_70)
（图2-3  预处理后的.i文本文件）
我们可以发现，经过预处理后，前面3个#include消失了，取而代之的是一长串以#开头的字符串，这个应该是我们原来的三个系统头文件，由于3个头文件代码过于庞大，这里截图只展示了我们hello源代码附近的。
#### 2.4 本章小结
预处理是c语言程序变为可执行文件4个阶段的第一个阶段，主要是处理以#开头的预处理指令，包括宏定义，文件包含，条件编译。



# 第3章 编译
##  3.1 编译的概念与作用

#### 1.	概念 
编译器把文本文件（hello.i）翻译成 包含汇编语言程序的文本文件(hello.s)
#### 2.	作用     
把高级的c语言代码翻译成为了低级的机器语言指令。
## 3.2 在Ubuntu下编译的命令

![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229005257668.png)

（图3-1 根据.i文件生成.s包含汇编代码的文件）



## 3.3 Hello的编译结果解析
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229005309968.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mjk3MDQ1Ng==,size_16,color_FFFFFF,t_70)
（图3-2 汇编代码1）
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229005318274.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mjk3MDQ1Ng==,size_16,color_FFFFFF,t_70)
（图3-3 汇编代码2）

解析：(这里先没有完全按照程序执行顺序解析，因为像赋值，加法等操作，像for循环等很多地方都有用到，如果放在后面解析在解析for时就会有问题)
#### 3.3.1全局变量

![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229005329544.png)
（3-3 c程序中定义的全局变量）
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229005336138.png)

（3-4 在汇编中体现）
#### 3.3.2 局部变量
局部变量是存放在函数自己的堆栈段，只有函数自己申请了堆栈后才能把局部变量初始化。hello中的局部变量i，就是在分配堆栈后，利用mov语句把0放在-4(%rbp)的位置

 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229005619255.png)
（3-5 局部变量i）
#### 3.3.3 赋值
赋值指令的实现通过的就是条件传送指令mov. mov a b 的效果就是把a的值给b,其中movb,movw,movl,movq分别是传送1，2，4，8个字节
#### 3.3.4 加法
加法的实现利用的是add指令。 add a b 的意思是 b = b+a,即为加法运算。
其中addb,addw,addl,addq分别为字节，字，双字和四字的加法。 
#### 3.3.5 小于
小于的实现利用的是cmp，cmp S1 S2 执行 S2 – S1,如果结果为0，设置ZF 0标志条件码寄存器中的值为1，如果结果为负数，设置SF负数条件码寄存器为1.如原程序中要比较i<10,就利用cmp 9 i,如果SF为1，则小于
#### 3.3.6 函数中参数传递
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229005631203.png)
（3-6 c语言中函数及其参数）
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229005639910.png)
（3-7 汇编代码中参数调用）
对于函数来说，通过寄存器或者栈来传递参数，最多可以通过寄存器传递6个参数，并且在64位汇编中，第一个参数放在%rdi,第二个放在%rsi,第三个%rdx，第四个%rcx，第五个%r8，第六个%r9,其余的应该通过栈传递，而这里我们只有两个参数，argv放在rdi中，argv放在rsi中。我们在函数分配栈空间后，利用mov指令把这个两个参数传送到函数的堆栈段。
#### 3.3.7 if语句
根据图3-6 我们知道这个if语句意思是如果argc不等于3,则执行里面的语句，否则不执行。
观察3-7中的汇编代码，首先是利用cmp 语句，比较3与-20(%rbp)位置数的大小，-20(%rbp)存放的是%rdi中的值，%rdi存放函数传入的第一个参数，即我们的argc, cmp S1 S2 执行 S2 – S1,如果结果为0，设置ZF 0标志条件码寄存器中的值为1.下一步为je指令，je指令的跳转条件为ZF，意思是ZF为1时跳转，即S1与S2相等时跳转，L2中即为我们if语句里面的汇编代码
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229005658757.png)
（图3-8 .c文件中if代码）
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229005705342.png)
（图3-9 if语句是否执行的判断）
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229005734424.png)
（图3-10 跳转后的结果）

#### 3.3.8 函数调用
如果我们进入了if语句，我们这时需要调用两个函数，函数调用的第一步就是把函数需要的参数放入相应的寄存器中，由前面解析我们已经知道，函数的第一个参数存放在%rdi中，所以这里首先把.LC0中数据放入edi,由图  知里面的数据就是我们的输出然后再call puts函数。call Q 指令会把地址A压入栈中，并将PC设置位Q的起始地址，压入栈中的地址称位返回地址，是call指令后面的那条指令的地址。同理可得exit(0)的调用过程。
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/2018122900581225.png)
（3-11 if里函数调用）
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229005821687.png)
（3-12 汇编中函数调用）
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229005837104.png)
（3-13  .LC0处的数据）





#### 3.3.9 for循环
汇编语言中对for的实现其实也是一系列的判断跳转。首先我们把0赋值给i（-4(%rbx)）,然后跳转到L3，判断i与9的大小，不大于9的话跳转到L4处执行for循环里面的代码，执行完毕后用add语句令i++,再与9比较，不大于的话继续到L4中执行，直到i大于9，跳出循环，即执行L3后面的语句，不再跳转进入L4.
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229005847539.png)
（3-14  c程序中for语句）
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/2018122900585969.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mjk3MDQ1Ng==,size_16,color_FFFFFF,t_70)
（3-15  for的汇编代码）







此部分是重点，说明编译器是怎么处理C语言的各个数据类型以及各类操作的。应分3.3.1~ 3.3.x等按照类型和操作进行分析，只要hello.s中出现的属于大作业PPT中P4给出的参考C数据与操作，都应解析。

## 3.4 本章小结
汇编代码与我们的高级语言已有了很大的不同，里面涉及到了很多如寄存器等真正在计算机上如何实现的过程，基本上是计算机真正如何执行我们的程序，可能一个简单的for循环，if语句，函数调用，在汇编语言中会花费比高级语言多的多的语句来实现，就是一个简单的hello程序，汇编代码就花费了几百行来实现。幸运的是我们不用自己编写汇编代码，不过懂得汇编代码如何运行对于我们对程序的理解和以后面对一些在高级语言中难以发现的错误都大有脾益。
（第3章2分）

# 第4章 汇编
## 4.1 汇编的概念与作用

概念：通过汇编器，把汇编语言翻译成机器语言
作用：通过汇编这个过程，把汇编代码转化成了计算机完全能够理解的机器代码，这个代码也是我们程序在计算机中表示。
## 4.2 在Ubuntu下汇编的命令
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229005932172.png)
（图4-1 生成.o文件）
## 4.3 可重定位目标elf格式
利用readelf -a 命令，列出所有信息
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229010106848.png)
（图4-2  readelf命令）





### 1.	ELF头
ELF头首先以一个16字节的序列开始，这个序列描述了生成该文件的系统的字的大小和字节顺序。剩下部分就如下图所示，列出了包含帮助链接器语法分析和解释目标文件的信息。其中包括ELF头的大小（64字节），目标文件的类型（REL可重定位文件），机器类型（AMD X86-64），节头部表的文件偏移，以及节头部表中条目的大小和数量。
![在这里插入图片描述](https://img-blog.csdnimg.cn/2018122901012362.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mjk3MDQ1Ng==,size_16,color_FFFFFF,t_70)
 （图4-3 elf头）

### 2.	节
在ELF后的都是节，下图也列出了节和它的一些基本信息，这里在写出每个节里应该存放的东西。
.text:  已编译的机器代码，类型为PROBITS,意思是程序数据，旗标为AX,下面也给出了解释，意思是分配内存且可执行

.rela.text  一个.text节中位置的列表(下一节会重点解释)

.data:  这个里面是已初始化的全局变量和静态c变量，类型也为PROBITS，旗标WA意思是分配内存且可修改

.bss： 这里面放的是未初始化的全局变量和静态c变量，类型NOBITS，意思是暂时没有存储空间，说明这个节在开始是不占据实际的空间
。
.rodata:  只读数据，如printf中的格式串和switch中的跳转表，我们hello程序中的printf中的格式串就存放在这里。

.comment:  这个节中包含了版本控制信息

.note.GNU_stack: 用来标记executable stack（可执行堆栈）
.eh_frame: This section contains information necessary for frame unwinding during exception handling.（这个节的意思我没有找到中文的解释）主要就是用来处理异常
.rela.eh_frame:.eh_frame的重定位信息
.symtab:，装载符号信息
.strtab: 一个字符串表，其内容包括.symtab和.debug节中的符号表，以及节头部的节名字。
.shstrtab:该区域包含节区名称
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229010137406.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mjk3MDQ1Ng==,size_16,color_FFFFFF,t_70)
（图4-4 节的信息）










### 3符号表
每个可重定位目标模块都有一个符号表，它包含m的定义和引用的符号的信息。符号表是由汇编器构造，使用编译器输出到汇编语言.s文件中的符号。每个符号表是一个条目的数组，每个条目包含下面部分
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229010148954.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mjk3MDQ1Ng==,size_16,color_FFFFFF,t_70)
（图4-5 符号表定义）
然后我们可以看到我们hello程序的符号表
举个例子，我们通过下图可以看到对全局符号sleepsecs的定义，它是一个位于.data段偏移量位0（value值）处一个大小为4个字节的变量，全局符号main是一个位于.text段，段偏移为0，大小125字节的函数
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229010253421.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mjk3MDQ1Ng==,size_16,color_FFFFFF,t_70)
（图4-6 hello程序的符号表）











### 4重定位节
在rela.text里面有我们的重定位条目，这个条目能告诉链接器目标文件合并成可执行文件时如何修改引用。
一个重定位条目包含以下信息
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229010308900.png)
（图4-7 重定位节定义）

我们利用objdump能够得到我们hello文件的重定位条目，这里最后有我们的符号名称，说明这是哪个的重定位条目，偏移量是指被修改的引用的节偏移x
类型这里有两种，R_X86_64_32意思是重定位时使用一个32位的绝对地址的引用，通过绝对寻址，CPU直接使用在指令中编码的32位值作为有效地址，不需要进一步修改。R_X86_64_PC32意思是重定位时使用一个32位PC相对地址的引用。一个pc相对地址就是据程序计数器的当前运行值的偏移量。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229010318797.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mjk3MDQ1Ng==,size_16,color_FFFFFF,t_70)
（图4-8 hello程序的重定位节）











### 4.4 Hello.o的结果解析
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229010330985.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mjk3MDQ1Ng==,size_16,color_FFFFFF,t_70)
（图 4-9 hello.o的反汇编）

objdump -d -r hello.o  分析hello.o的反汇编，并请与第3章的 hello.s进行对照分析。
机器语言就是一系列的二进制代码，一般包括操作码字段和地址码字段。每一种cpu都有自己的机器指令集\汇编指令集，在这个指令集中，汇编代码中操作数，寄存器等都会对应一个机器码，就如上图中retq的机器码是0xc3.
观察上图和我们第三章给出的汇编代码可以发现，机器代码反汇编后得到的汇编代码与我们原来的有些地方不同，这里来展示哪些地方不同及其原因。
首先我们观察整体，发现机器代码里没有再分.L1这些段了，而是在一串连续地址里，这点对我们下面跳转和函数调用很重要
（1）
  ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229010353601.png)
  ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229010405586.png)
（图4-10 汇编代码与机器代码不同1）
第一个不同是堆栈大小，我们能够发现机器代码中申请的堆栈大小为0x20,而原来汇编代码里为0x32
（2）
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229010414807.png)
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229010425858.png)

（图4-11 汇编代码与机器代码不同2）
第二个不同就是跳转，汇编代码中有不同的段，跳转就是跳转到下一个段，而机器代码你没有这个了，我们需要根据机器代码地址来跳转。通过后面反汇编的代码我们知道我们需要跳转到0x29这个偏移的位置，机器码里展示为 74 14,74是je的意思，14则代表我们要跳转到的地址比下一条指令的起始地址大0x14,即0x15+0x14 = 0x29
(3)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229010438665.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229010450582.png)


（图4-12 汇编代码与机器代码不同3）
第三个不同则是函数调用，函数参数传递时，根据我们上一节知道的，printf的格式串存放在.rodata，根据后面的.rodata_0x0查询重定位节，如下图，根据偏移确定重定位符号的位置 
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229010517209.png)
（图4-13 ）
就是图中地址0x16位置，我们可以看到，从0x16到0x1a被空了出来，这时给链接时需要放入的值把位置腾出来，这里先不解释怎么操作，在链接的时候会详细介绍。


（4）下面这个不同也是函数调用的不同，分析与上面相同
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229010543300.png)
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229010558420.png)

（图4-14 汇编代码与机器代码不同3）
（5）全局变量的调用也是一样，通过sleepses-0x4去查重定位节，找到相对PC的偏移量。这个偏移量这个时候我们也还不知道，所以也被空出来，等到下一节链接时确定了每个节的地址才可以知道。
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229010612316.png)
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229010622286.png)

（图4-15 汇编代码与机器代码不同4）



说明机器语言的构成，与汇编语言的映射关系。特别是机器语言中的操作数与汇编语言不一致，特别是分支转移函数调用等。
## 4.5 本章小结
当我们从汇编代码变为了机器代码，程序就真正变成了计算机可以理解的程序，我们也知道了我们的程序真正在计算机中是以什么存储的。机器代码与汇编代码会根据cpu的指令集，产生一个对应，我们也能通过objdump这样的反汇编工具查看机器码对应的汇编码，不过这里对代码已经与我们.s里的汇编代码有了些不同，已经在汇编过程中我们的代码变成了ELF格式，代码被放在代码段，全局变量放在.data段，通过重定位条目得到每个符号不同偏移量，去不同的段找到我们想要的信息。


# 第5章 链接
## 5.1 链接的概念与作用
概念：
作用：把可重定位目标文件和命令行参数作为输入，产生一个完全链接的，可以加载运行的可执行目标文件。
注意：这儿的链接是指从 hello.o 到hello生成过程。
## 5.2 在Ubuntu下链接的命令
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/2018122901063446.png)
（图5-1 使用ld命令链接）
使用ld的链接命令，应截图，展示汇编过程！ 注意不只连接hello.o文件
## 5.3 可执行目标文件hello的格式
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229010644470.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mjk3MDQ1Ng==,size_16,color_FFFFFF,t_70)
（图5-2 ELF头）

 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229010654550.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mjk3MDQ1Ng==,size_16,color_FFFFFF,t_70)
（图5-3 各段信息）
这个时候我们能够发现，这里比原来hello.o要多出很多来，这是因为我们计算机进行了动态链接，这里也把多出来的一些段做一些解释
1.	interp段：动态链接器在操作系统中的位置不是由系统配置决定，也不是由环境参数指定，而是由 ELF 文件中的 .interp 段指定。该段里保存的是一个字符串，这个字符串就是可执行文件所需要的动态链接器的位置，常位于 /lib/ld-linux.so.2。（通常是软链接）
2.	dynamic段：该段中保存了动态链接器所需要的基本信息，是一个结构数组，可以看做动态链接下 ELF 文件的“文件头”。存储了动态链接会用到的各个表的位置等信息。
3.	dynsym段：该段与 “.symtab”段类似，但只保存了与动态链接相关的符号，很多时候，ELF文件同时拥有 .symtab 与 .synsym段，其中 .symtab 将包含 .synsym 中的符号。该符号表中记录了动态链接符号在动态符号字符串表中的偏移，与.symtab中记录对应。
4.	dynstr段：该段是 .dynsym 段的辅助段，.dynstr 与 .dynsym 的关系，类比与 .symtab 与 .strtab 的关系
5.	hash段：在动态链接下，需要在程序运行时查找符号，为了加快符号查找过程，增加了辅助的符号哈希表，功能与 .dynstr 类似
6.	rel.dyn段：对数据引用的修正，其所修正的位置位于 “.got”以及数据段（类似重定位段 "rel.data"）
7.	rel.plt段：对函数引用的修正，其所修正的位置位于 “.got.plt
    分析hello的ELF格式，用readelf等列出其各段的基本信息，包括各段的起始地址，大小等信息。
5.4 hello的虚拟地址空间
用edb加载hello,然后在Data Dump 中查看虚拟地址的信息。
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229010707980.png)
（图5-4 edb查看虚拟地址信息）
Data Dump里向我们展示处，hello的虚拟地址从0x400000 – 0x401000，并给我们列出了整个文件在内存上存储的是什么。我们可以通过5.3节的每段起始地址，找到每一段的信息。
比如.interp段，根据5.3的分析，这段应该是存储动态链接器的位置，我们在它的起始地址0x400200查看
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229010716458.png)
（图5-5）
果然这个地方存储了一个字符串，代表动态存储器的位置。
再比如0x400500处是.text节，我们也可以查看到我们代码段到底是什么，与5.5节使用objdump反汇编后得到的结果可以对照。
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229010728922.png)
（图5-6  .text段内容）

    使用edb加载hello，查看本进程的虚拟地址空间各段信息，并与5.3对照分析说明。   
## 5.5 链接的重定位过程分析
观察反汇编结果，首先我们能看到比原来只有.text段，这里多了.init段和.plt段，这些都是在动态链接过程中产生的，在5.6节会详细分析这个部分。
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229010739484.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mjk3MDQ1Ng==,size_16,color_FFFFFF,t_70)
（图5-7  反汇编后的.init段）
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229010747853.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mjk3MDQ1Ng==,size_16,color_FFFFFF,t_70)
（图5-8 .plt段）
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229010757581.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mjk3MDQ1Ng==,size_16,color_FFFFFF,t_70)
（图5-9）
下面我们重点关注我们.text的代码经过链接后发生了什么变化。
1.	调用rodata的数据
原来代码中在这个地方需要访问一个rodata的数据，我们注意到机器代码中bf后面并没有东西，这是因为此时还没有链接，无法得知现在是什么。
   ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229010819597.png)
（图5-10）
而在我们的可执行文件中，这里已经链接完毕了，bf后面也有了相应的值，那么0x400634这个值怎么得到的？
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229010831692.png)
（图5-11 ）
由上一章的重定位条目，我们知道这个符号的重定位条目，这个字段告诉链接器要修改从偏移量0x16处开始的绝对引用，而0x400547刚好就是代码起始位置0x400300+0x16后的那个位置，引用的位置为.rodata起始处，rodata节的起始位置为0x400630,我们所需数据在0x400630,于是得到这个值的由来。
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229010841180.png)
（图5-12 重定位条目）
2.	对动态链接函数的引用
在调用puts函数时，相应的位置也被空了出来，
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229010852717.png)
（图5-13 ）
而链接后的结果为0xffffff5f,注意这个地方是相对偏移，即用下一条指令首地址加这个偏移，得到的结果为0x4004b0,
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229010904125.png)
（图5-14 ）
而在0x4004b0的位置，刚好就是我们的动态链接的put的地址。
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229010919697.png)
（图5-15）
objdump -d -r hello 分析hello与hello.o的不同，说明链接的过程。
结合hello.o的重定位项目，分析hello中对其怎么重定位的。
## 5.6 hello的执行流程
-----------------------------------------------
| 名称                                | 地址            |
| ----------------------------------- | --------------- |
| ld-2.27.so! dl_start                | 0x7ffee5aca680  |
| ld-2.27.so! dl_init                 | 0x7f9f48629630  |
| hello!_start                        | 0x0x400500      |
| ld-2.27.so!_libc_start_main         | 0x7f9f48249ab0  |
| libc-2.27.so! cxa_atexit            | 0x7f4523fd6af7  |
| libc-2.27.so! lll_look_wait_private | 0x7f4523ff8471  |
| libc-2.27.so!_new_exitfn            | 0x7f87ff534220  |
| hello!_libc_csu_init                | 0x7f87ff512b26  |
| libc-2.27.so!_setjmp                | 0x7f87ff512b4a  |
| libc-2.27.so!_sigsetjmp             | 0x7f87ff52fc12  |
| libc-2.27.so!__sigjmp_save          | 0x7f87ff52fbc3  |
| hello_main                          | 0x400532        |
| hello!puts@plt                      | 0x4004b0        |
| hello!exit@plt                      | 0x4004e0        |
| hello!printf@plt                    | 0x400587        |
| hello!sleep@plt                     | 0x400594        |
| hello!getchar@plt                   | 0x4005a3        |
| dl_runtime_resolve                  | 0x7f169ad84750  |
| libc-2.27.so!exit                   | 0x7fce 8c889128 |


使用edb执行hello，说明从加载hello到_start，到call main,以及程序终止的所有过程。请列出其调用与跳转的各个子程序名或程序地址。
## 5.7 Hello的动态链接分析
   我们程序中调用了很多由共享库定义的函数，编译器没有办法预测这个函数运行时的地址，因为定义它的共享模块在运行时可以加载到任何位置，对于这些PIC函数，GNU采用延迟绑定技术，将过程地址的绑定推迟到第一次调用该过程。运用两个数据结构，PLT和GOT。在我们的可执行ELF文件中，也有这两个节，
首先我们看plt节，plt是一个数组，每个条目是个16字节的代码，每个可被执行的库函数都有自己的PLT条目，每个条目负责调用一个具体的函数。比如我们0x4004b0这个位置的函数，它代表的就是puts这个函数，这里跳转到*0x200b62(%rip)会跳转到一个GOT条目，这个条目对于的就是我们puts函数真正的地址，0x200b62的意义是这个GOT条目与下一条指令的固定距离为0x200b62.
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229011226498.png)
（图5-16 .plt段信息）
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229011238750.png)
（图5-17 plt条目）
我们再来看dl_init前后的一些变化
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229011251822.png)
（图7-18 dl_init前的got.plt节）
通过前面的节地址，我们找到got.plt节，这个节里面，每8个字节是一个条目，与plt联合使用时，GOT[0],GOT[1]是包含动态链接器在解析函数地址时会使用的信息，GOT[2]是动态链接器ld-linux.so模块的入口。在dl_init前这里是空的，如图5-18，而在dl_init后这里
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229011305864.png)
（图7-19 dl_init后的got.plt节）
这里GOT[2]变为0x7f169ad84750,找到这个地址，对应的是一个叫dl_runtime_resolve的函数，所有动态库函数在第一次调用时，都是通过XXX@plt -> 公共@plt -> _dl_runtime_resolve调用关系做地址解析和重定位的。
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229011316391.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mjk3MDQ1Ng==,size_16,color_FFFFFF,t_70)
（图 7-20 dl_runtime_resolve函数 ） 
GOT[1]变为0x7f169af96170,如图5-21黄色部分，对应一个重定位表。
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229011338646.png)


## 5.8 本章小结
链接(linking) 是将各种代码和数据片段收集并组合成为一个单一文件的过程，这个文件可被加载（复制）到内存并执行。链接可以执行于编译时(compile time), 也就是在源代码被翻译成机器代码时；也可以执行于加载时(load time), 也就是在程序被加载器(loader)加载到内存并执行时；甚至执行于运行时(run time), 也就是由应用程序来执行。在早期的计算机系统中，链接是手动执行的。在现代系统中，
链接是由叫做链接器(linker) 的程序自动执行的。
   链接器在软件开发中扮演着一个关键的角色，因为它们使得分离编译(separate compilation)成为可能。我们不用将一个大型的应用程序组织为一个巨大的源文件，而是可以把它分解为更小、更好管理的模块，可以独立地修改和编译这些模块。当我们改变这些模块中的一个时，只需简单地重新编译它，并重新链接应用，不必重新编译其他文件  



# 第6章 hello进程管理
## 6.1 进程的概念与作用
进程的经典定义是一个执行中程序的实例，系统的每个程序都运行在某个进程的上下文。上下文是由程序正确运行所需的状态组成的，这个状态包括存放在内存里的程序的代码和数据，它的栈，通用目的寄存器的内容，程序计数器，环境变量以及打开文件描述符的集合。
通过进程，我们会得到一种假象，好像我们的程序是当前唯一运行的程序，我们的程序独占处理器和内存，我们程序的代码和数据好像是系统内存中唯一的对象。
## 6.2 简述壳Shell-bash的作用与处理流程
shell俗称壳，它是指UNIX系统下的一个命令解析器；主要用于用户和系统的交互。UNIX系统上有很多种Shell。首个shell，即Bourne Shell，于1978年在V7(AT&T的第7版)UNIX上推出。后来，又演变出C shell、bash等不同版本的shell。
bash，全称为Bourne-Again Shell。它是一个为GNU项目编写的Unix shell。bash脚本功能非常强大，尤其是在处理自动循环或大的任务方面可节省大量的时间。bash是许多Linux平台的内定Shell。
处理流程：
1 新建文件test.sh
$ touch test.sh
2 添加可执行权限
$ chmod +x test.sh
3 编辑test.sh，test.sh内容如下：
#!/bin/bash
echo "hello bash"
exit 0
说明：
#!/bin/bash : 它是bash文件声明语句，表示是以/bin/bash程序执行该文件。它必须写在文件的第一行！
echo "hello bash" : 表示在终端输出“hello bash”
exit 0 : 表示返回0。在bash中，0表示执行成功，其他表示失败。
4 执行bash脚本
$ ./bash
在终端输出“bash hello”
## 6.3 Hello的fork进程创建过程
我们在shell上输入./hello,这个不是一个内置的shell命令，所以shell会认为hello是一个可执行目标文件，通过条用某个驻留在存储器中被称为加载器的操作系统代码来运行它。当shell运行一个程序时，父进程通过fork函数生成这个程序的进程。这个子进程几乎与父进程相同，子进程得到与父进程相同的虚拟地址空间（独立）的一个副本，包括代码，数据段，堆，共享库以及用户栈。唯一的不同是与父进程的PID不同。
6.4 Hello的execve过程
execve函数在当前进程的上下文中加载并运行一个新程序

![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229011431228.png)

（图6-1 execve函数定义）

execve函数加载并运行可执行文件filename(hello),且带参数列表argv和环境变量envp.当加载器运行时，它创建一个类似与图 6-2 的内存映像。在程序头部表的引导下，加载器将可执行文件的片复制到代码段和数据段，接下来，加载器跳转到程序的入口，_start函数的地址，这个函数是在系统目标文件ctrl.o中定义的，对所有的c程序都一样。_start函数调用系统启动函数，_libc_start_main,该函数定义在libc.so里，初始化环境，调用用户层的main函数，处理main函数返回值，并且在需要的时候返回给内核。
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229011438901.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mjk3MDQ1Ng==,size_16,color_FFFFFF,t_70)
（图6-2 内存映像）
## 6.5 Hello的进程执行
在hello执行的某些时刻，内核可以决定抢占当前进程，并重新开始一个先前被抢占了的进程，这种决策就叫做调度。是由内核中称为调度器的代码处理。当内核选择了一个新的进程，我们说内核调度了这个进程。调度这个进程后，就抢占了当前进程，并通过上下文切换的方式来转移控制新的进程。
上下文切换：
1）	保存当前的上下文
2）	恢复某个先前被抢占的进程被保存的上下文
3）	将控制传递给这个新恢复的进程
结合进程上下文信息、进程时间片，阐述进程调度的过程，用户态与核心态转换等等。
在我们的hello中，如果执行sleep,它显示的请求让进程休眠，这个时候系统就会调度，实现上下文切换。
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229011452900.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mjk3MDQ1Ng==,size_16,color_FFFFFF,t_70)
（图6-3 内存保存不同进程上下文）
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229011506613.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mjk3MDQ1Ng==,size_16,color_FFFFFF,t_70)
（图6-4  进程间调度）
## 6.6 hello的异常与信号处理


如果乱按过程中没有回车，这个时候只是把输入屏幕的字符串缓存起来，如果输入最后是回车,getchar把回车读入，并把回车前的字符串当作shell输入的命令
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229011705416.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mjk3MDQ1Ng==,size_16,color_FFFFFF,t_70)
（图6-5  键盘乱按）
如下图，如果在程序运行过程中输入Ctrl+C,会让内核发送一个SIGINT信号给到前台进程组中的每个进程，结果是终止前台进程，通过ps命令发现这时hello进程已经被回收。
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/2018122901171527.png)
（图6-6 Cltr-C命令）

如下图，如果输入Ctrl+Z会发送一个SIGTSTP信号给前台进程组的每个进程，，结果是停止前台作业，即我们的hello程序
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229011746169.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mjk3MDQ1Ng==,size_16,color_FFFFFF,t_70)
（图6-6 Ctrl-Z命令）
fg 1 的意思是使第一个后台作业变为前台，第一个后台作业是我们的hello，所以输入fg 1 后hello程序又开始运行，并且是继续刚才的进程，输出剩下的7个字符串。
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229011808965.png)
(图6-7 fg命令)
hello执行过程中会出现哪几类异常，会产生哪些信号，又怎么处理的。
 程序运行过程中可以按键盘，如不停乱按，包括回车，Ctrl-Z，Ctrl-C等，Ctrl-z后可以运行ps  jobs  pstree  fg  kill 等命令，请分别给出各命令及运行结截屏，说明异常与信号的处理。
## 6.7本章小结
进程是一个程序执行的实例，我们的程序其实就是磁盘中的一串二进制代码，通过fork为hello创建进程，并为这个进程分配它的上下文，让我们的hello看起来独享整个虚拟内存空间。在进程执行过程中，程序可能会遇到各种的异常，如在键盘的输入，这时会通过信号机制，调用信号处理函数，处理我们的信号。
（第6章1分）

# 第7章 hello的存储管理
## 7.1 hello的存储器地址空间
逻辑地址：逻辑地址是指机器语言指令中，用来指定一个操作数或者是一条指令的地址。这里callq 0x4004b0的0x4004b0就是逻辑地址
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229012101101.png)
（7-1 hello中的逻辑地址）
线性地址：跟逻辑地址类似，它也是一个不真实的地址，假设逻辑地址是相应的硬件平台段式管理转换前地址的话，那么线性地址则相应了硬件页式内存的转换前地址
物理地址：计算机系统的主存被组织成一个由M个连续的字节大小单元组成的数组，每字节都有一个唯一的物理地址。
虚拟地址：也就是线性地址，现代计算机通过虚拟地址寻址，通过MMU（内存管理单元）翻译成物理地址。Hello程序中，各个节的地址都是指的虚拟地址
结合hello说明逻辑地址、线性地址、虚拟地址、物理地址的概念。
## 7.2	Intel逻辑地址到线性地址的变换-段式管理
一个逻辑地址由两部分组成，段标识符，段内偏移量。段标识符是一个16位长的字段组成，称为段选择符，其中前13位是一个索引号。后面三位包含一些硬件细节。
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229012112230.png)
（图7-2 段选择符）
索引号，这里可以直接理解成数组下标，它对应的“数组”就是段描述符表，段描述符具体描述了一个段地址，这样，很多段描述符就组成段描述符表。可以通过段标识符的前13位，直接在段描述符表中找到一个具体的段描述符，这个描述符就描述了一个段。
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/201812290121213.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mjk3MDQ1Ng==,size_16,color_FFFFFF,t_70)
（图7-3 段描述符）
这里面，我们只用关心Base字段，它描述了一个段的开始位置的线性地址。
Intel设计的本意是，一些全局的段描述符，就放在“全局段描述符表(GDT)”中，一些局部的，例如每个进程自己的，就放在所谓的“局部段描述符表(LDT)”中。
GDT在内存中的地址和大小存放在CPU的gdtr控制寄存器中，而LDT则在ldtr寄存器中。
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229012130616.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mjk3MDQ1Ng==,size_16,color_FFFFFF,t_70)
（图7-4）
首先，给定一个完整的逻辑地址[段选择符：段内偏移地址]，
1、看段选择符的T1=0还是1，知道当前要转换是GDT中的段，还是LDT中的段，再根据相应寄存器，得到其地址和大小。我们就有了一个数组了。
2、拿出段选择符中前13位，可以在这个数组中，查找到对应的段描述符，这样，它了Base，即基地址就知道了。
3、把Base + offset，就是要转换的线性地址了。
## 7.3 Hello的线性地址到物理地址的变换-页式管理
计算机利用页表，通过MMU来完成从虚拟地址到物理地址的转换。
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229012148696.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mjk3MDQ1Ng==,size_16,color_FFFFFF,t_70)
（图7-5 虚拟地址转换为物理地址）
PTBR是cpu里的一个控制寄存器，指向当前页表，n位的虚拟地址包括p位的虚拟页面偏移VPO和n-p位的虚拟页号VPN。MMU通过VPN来选择适当的PTE,将页表条目中的PPN（物理页号）和虚拟地址的VPO串联起来，就得到相应的物理地址。
## 7.4 TLB与四级页表支持下的VA到PA的变换
每次cpu产生一个虚拟地址，MMU需要查询一个PTE,如果运气不好，需要从内存中取得，这需要花费很多时间，通过TLB（翻译后备缓冲器）能够消除这些开销。TLB是一个小的，虚拟寻址的缓存，在MMU里，其每一行都保存着一个单个PTE组成的块，TLB通常具有高度相联度。
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229012202953.png)

（图7-6  虚拟地址组成）
用于组选择和行匹配的索引和标记字段是从虚拟地址中的虚拟页号中提取出来的。

如果是32位系统，我们有一个32位地址空间，4KB的页面和一个4字节的PTE,我们总需要一个4MB的页表驻留在内存中，而对于64位系统，我们甚至需要8PB的空间来存放页表，这显然是不现实的。用来压缩页表的常见方式就是使用层次结构的页表。
如果是如图7-7所示的二级页表，第一级页表的每个PTE负责一个4MB的块，每个块由1024个连续的页面组成。二级页表每一个PTE负责一个4KB的虚拟地址页面。这样的好处在于，如果一级页表中有一个PTE是空，那么二级页表就不会存在，这样会有巨大的潜在节约，因为4GB的地址空间大部分都是未分配的。
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229012213874.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mjk3MDQ1Ng==,size_16,color_FFFFFF,t_70)
（图7-7 多级页表）
现在的64位计算机采用4级页表，如图7-8，36位的VPN被封为4个9位的片，每个片被用作一个页面的偏移，CR3寄存器包含L1页表的物理地址。VPN1提供到一个L1PET的偏移量，这个PTE包含L2页表的基地址，VPN2提供一个到L2PTE的偏移量，以此类推。
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/2018122901222418.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mjk3MDQ1Ng==,size_16,color_FFFFFF,t_70)
(图7-8 64位操作系统的4级页表 )

## 7.5 三级Cache支持下的物理内存访问
当我们通过MMU得到了物理地址后，我们就需要去内存里去相应的数据，当从内存直接取数据速度太慢，计算机利用cache（高度缓存）来加快访存速度。它位于CPU与内存之间，访问速度比内存块很多，需要从内存里取数据时，先考虑是否在cache里有缓存。图    是一个典型的cache结构。
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229012235694.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mjk3MDQ1Ng==,size_16,color_FFFFFF,t_70)
（图7-9  一个典型的cache结构）

那么我们又是如何确定物理地址所对应的是高速缓存中的哪个部分，如图7-10 ，
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229012244460.png)
（图7-10 利用物理地址确定cache中位置）
根据高速缓存的大小，我们把物理地址分割成这些部分，其中S = 2^s,B = 2^b,剩下的t位都是标记位，得到一个物理地址后，通过组索引部分可以确定在cache里的哪一组，通过标记位确定看是否与组里的某一行标记相同，如果有，通过块偏移位确定具体是哪个数据块，从而得到我们的数据。如果没有找到，则需要从内存里去数据，并找到cache里的一行替换，对于L1,L2这样的组相联cache，替换策略通常有LFU(最不常使用)，LRU(最近最少使用)。
## 7.6 hello进程fork时的内存映射 
Linux通过将一个虚拟内存区域与一个磁盘上的对象关联起来，以初始化这个虚拟内存区域，这个过程称为内存映射。
我们知道Linux为每个进程维护了一个单独的虚拟地址空间，通过图   的数据结构来包含每个进程的信息
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229012253380.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mjk3MDQ1Ng==,size_16,color_FFFFFF,t_70)
（7-11 每个进程虚拟内存的数据结构）
当我们的hello程序被fork后，内存为我们的hello创建如图7-11的数据结构，并分配给一个唯一的PID,为了给我们的新进程hello创建虚拟内存，它创建了当前进程mm_struct,区域结构和页表的原样副本，将两个进程的每个页面标记为只读，并将两个进程中的每个区域结构都标记为写时复制。当fork在新进程里返回时，新进程现在的虚拟内存刚好和调用fork时相同。
## 7.7 hello进程execve时的内存映射 
当我们用./hello运行可执行文件hello时，先fork创建虚拟内存区域，当这个区域和父进程还是完全一样的，然后会调用execve(“hello”,NULL,NULL),加载并运行可执行文件hello,用hello程序有效替换了当前程序，加载并运行hello的步骤如下：
1）	删除已存在的用户区域
2）	映射私有区域（为hello程序的代码，数据，bss,栈区域创建新的区域结构），都是私有，写时复制的
3）	映射共享区域，如我们的hello需要与libc.so动态链接，那么这些对象动态链接到这些程序，然后再映射到用户虚拟地址空间中的共享区域。
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229012303497.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mjk3MDQ1Ng==,size_16,color_FFFFFF,t_70)
（图7-12 一个进程的虚拟内存空间）
## 7.8 缺页故障与缺页中断处理  
虚拟内存中，DRAM缓存不命中称为缺页。如图7-13，CPU需要引用VP3中的一个字，通过读取PTE3，发现有效位为0，说明不在内存里，这时就发生了缺页异常。缺页异常发生时，通常会调用内核里的缺页异常处理程序，该程序会选择一个牺牲页，这里是存放在PP3的VP4,如果VP4已经被修改，内核就会将它复制回磁盘。无论哪种情况，内核都会修改VP4的页表条目，反映出VP4不再缓存在内存里。
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/2018122901231971.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mjk3MDQ1Ng==,size_16,color_FFFFFF,t_70)
（图7-13  发生缺页中断）
接下来，内核从磁盘复制VP3到内存中的PP3,更新PTE3,随后返回。当异常处理程序返回时，它会重新启动导致缺页的指令。
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229012336329.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mjk3MDQ1Ng==,size_16,color_FFFFFF,t_70)

（图7-14 替换后的结构）
缺页处理程序不是直接就替换，它会经过一系列的步骤：
1）	虚拟地址是合法的吗？如果不合法，它就会触发一个段错误
2）	试图进行的内存访问是否合法？意思就是进程是否有读，写或者执行这个区域的权限
3）	经过上述判断，这时才能确定这是个合法的虚拟地址，然后才会执行上述的替换。
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/2018122901234859.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mjk3MDQ1Ng==,size_16,color_FFFFFF,t_70)
（图7-15 缺页时的各种情况）
## 7.9动态存储分配管理
当我们的C程序需要额外的虚拟内存区域，就会使用动态内存分配来得到，动态内存分配器维持着一个进程的虚拟内存区域，称为堆，分配器将堆视作一组大小不同的块，每个块就是一个连续的虚拟内存块，这些块要么是已分配的，要么是空闲的。
对于C程序，使用的是显式分配器，能够显式地释放任何已分配的块。C标准库通过malloc程序包的显示分配器。
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229012400807.png)
（图7-16 malloc函数）
如图7-16，malloc函数能够返回一个至少包含size大小的内存块的指针，这里的size不一定是我们输入的大小，因为块大小需要数据对齐。32位中返回的地址总位8的倍数，64位中总是16的倍数
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229012409945.png)
（图7-17 free函数）
free函数把当前地址对应的块给释放变成空闲块。
这个malloc和free又是怎么实现的呢
一个分配器的设计中，我们需要考虑以下问题：
1）	空闲块组织
2）	放置，如何找到一个合适的空闲块来放置新的块
3）	分割，分配后空闲块的剩余部分如何处理
4）	合并，如何处理刚刚释放的块
现在主要有3种方式来构造分配器
##### 1）	隐式空闲链表
它的数据结构如图    所示
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229012421344.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mjk3MDQ1Ng==,size_16,color_FFFFFF,t_70)
（图7-18 隐式空闲链表）
这种结构下块与块之间是通过头部的块大小隐式连接在一起，意思是我们要找到下一个块，只需要用当前块地址加上块大小即可。
这种结构的优点就是简单，当任何操作，如放置分配的块，都需要对整个链表搜索，全是线性级。
我们在搜索一个可以用户请求的空闲块时，一般有三种策略
1.	首次适配：从头搜索的第一个合适的块
2.	下一次适配：从上一次分配结束的位置开始的首次适配
3.	最佳适配：检查所有块，选择最适合的那个块
##### 2）	显式空闲链表  
如图7-19，这种结构比隐式多了两个指针，一个指针指向前一个空闲块，一个指针指向后一个空闲块，这样做的好处是在查找空闲块时，不需要进行很多无用的查找操作（隐式查找时需要看每一个块，这里只用看空闲块）
![在这里插入图片描述](https://img-blog.csdnimg.cn/2018122901243015.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mjk3MDQ1Ng==,size_16,color_FFFFFF,t_70)

（图7-19 显式空闲链表）
组织这种链表有两种方式，一种是LIFO,最新释放的空闲块放在链表最前面。一种是按地址大小来维护，链表中的块地址都小于它后继节点的地址。
##### 3）	分离的空闲链表
这种是在显式空闲链表的基础上又进行优化，把所有的块大小分为一些等价类，如图7-20然后每个等价类都有一个链表，我们寻找空闲块时就只需要在对应大小的链表里找就好了
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229012438780.png)
（图7-10 大小类）
但是这种链表也有一个问题，如果我们分割了一个空闲块，那么剩下的空闲块大小可能不应该在这个链表里，有两种方法来处理这个问题，一种是简单分离适配，每个空闲链表里的块大小都是这个大小类里最大元素的大小。然后处理时不会分割，直接取出链表的第一个块。另一种方法是分离适配，把分割后的块再插入适当的空闲链表。
合并策略：
     在频繁的申请释放后，我们的堆中会有很多碎片，我们需要通过合并来减少碎片的产生。我们需要决定什么时候合并，一般来说有两种考虑，一是立即合并，每次释放一个块就合并，二时推迟合并，直到分配请求失败，再扫描整个堆进行合并。
如图     ，这种结构下，我们能够知道下一个块的位置，但不知道上一个块的位置，那么如果要与上一个块合并就会比较困难，这里有一个叫边界标记的技术可以解决这个问题，如图      ，这里多增加了一个脚部，脚部和头部内容完全一样，我们可以通过脚部，找到上一个块的脚部，从而知道上一个块的大小和是否已分配。
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229012447985.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mjk3MDQ1Ng==,size_16,color_FFFFFF,t_70)
（图7-21 边界标记技术）
利用边界标记技术，我们很容易就可以处理如图7-22的全部4中合并情况
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229012456949.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mjk3MDQ1Ng==,size_16,color_FFFFFF,t_70)
（图7-22  四种合并情况）
## 7.10本章小结
在这一章中，主要介绍了程序的存储结构，通过段式管理在逻辑地址到虚拟地址，页式管理从虚拟地址到物理地址。程序访问过程中的cache结构和页表结构，进程如何加载自己的虚拟内存空间，内存映射和动态内存分配。
这些所有的结构，都是为了两个目标，加快访问的速度，为每个进程分配一个独立的虚拟内存空间而不不会受到其他进程影响


# 第8章 hello的IO管理
## 8.1 Linux的IO设备管理方法
设备的模型化：所有的IO设备都被模型化为文件，而所有的输入和输出都被当做对相应文件的读和写来执行，这种将设备优雅地映射为文件的方式，允许Linux内核引出一个简单低级的应用接口，称为Unix I/O。
## 8.2 简述Unix IO接口及其函数  
所有的输入输出以一种统一且一致的方式来执行：
1）。打开文件。一个应用程序通过要求内核打开相应的文件，来宣告它想要访问一个I/O设备，内核返回一个小的非负整数，叫做描述符，它在后续对此文件的所有操作中标识这个文件，内核记录有关这个打开文件的所有信息。
2）Shell创建的每个进程都有三个打开的文件：标准输入，标准输出，标准错误。
3）改变当前的文件位置：对于每个打开的文件，内核保持着一个文件位置k，初始为0，这个文件位置是从文件开头起始的字节偏移量，应用程序能够通过执行seek，显式地将改变当前文件位置k。
4）读写文件：一个读操作就是从文件复制n>0个字节到内存，从当前文件位置k开始，然后将k增加到k+n，给定一个大小为m字节的而文件，当k>=m时，触发EOF。类似一个写操作就是从内存中复制n>0个字节到一个文件，从当前文件位置k开始，然后更新k。
5）关闭文件，内核释放文件打开时创建的数据结构，并将这个描述符恢复到可用的描述符池中去。
打开文件

 ![在这里插入图片描述](https://img-blog.csdnimg.cn/2018122901252249.png)
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229012534694.png)
（图8-1  open函数）

open函数将filename转换为一个文件描述符，并且返回描述符数字，返回的描述符数字是在当前进程中没有打开的最小描述符。Flags参数指明了该进程打算如何访问这个文件。

关闭文件：

 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229012544486.png)
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229012554425.png)
（图8-2 close函数）

读文件：

 ![在这里插入图片描述](https://img-blog.csdnimg.cn/2018122901260371.png)
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229012618344.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mjk3MDQ1Ng==,size_16,color_FFFFFF,t_70)
（图8-3 read函数）

read函数从描述符为fd的当前文件位置复制最多n个字节到内存位置buf,返回值-1,表示一个错误，返回0表示EOF.否则，返回值表示的是实际传送的字节数量。














写文件：
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181229012644808.png)
 在这里插入图片描述

（图8-4 write函数）
write函数从内存位置buf复制至多n个字节到描述符fd的当前文件位置。
## 8.3 printf的实现分析
首先我们查看printf函数函数体：
```c
//
1.	int printf(const char *fmt, ...)   
2.	{   
3.	int i;   
4.	char buf[256];   
5.	      
6.	     va_list arg = (va_list)((char*)(&fmt) + 4);   
7.	     i = vsprintf(buf, fmt, arg);   
8.	     write(buf, i);   
9.	      
10.	     return i;   
11.	    }  
```
这里…是可变形参，参数个数不确定时使用。那我们怎么知道具体形参个数？
这里va_list是一个字符指针，(char*)(&fmt) + 4) 表示的是...中的第一个参数。
我们再来看vsprintf
```c
12.	int vsprintf(char *buf, const char *fmt, va_list args)   
13.	   {   
14.	    char* p;   
15.	    char tmp[256];   
16.	    va_list p_next_arg = args;   
17.	     
18.	    for (p=buf;*fmt;fmt++) {   
19.	    if (*fmt != '%') {   
20.	    *p++ = *fmt;   
21.	    continue;   
22.	    }   
23.	     
24.	    fmt++;   
25.	     
26.	    switch (*fmt) {   
27.	    case 'x':   
28.	    itoa(tmp, *((int*)p_next_arg));   
29.	    strcpy(p, tmp);   
30.	    p_next_arg += 4;   
31.	    p += strlen(tmp);   
32.	    break;   
33.	    case 's':   
34.	    break;   
35.	    default:   
36.	    break;   
37.	    }   
38.	    }   
39.	     
40.	    return (p - buf);   
41.	   } 
42.	  
```
vsprintf返回一个长度，就是我们的字符串长度。Vsprintf的作用就是格式化，它接收确定输出格式的格式字符串fmt,用格式字符串读个数变化的参数进行格式化，产生格式化输出。
然后是wirte函数
如果追踪write函数：
```
43.	write:   
44.	     mov eax, _NR_write   
45.	     mov ebx, [esp + 4]   
46.	     mov ecx, [esp + 8]   
47.	     int INT_VECTOR_SYS_CALL 
```
这个地方int要调用中断门来实现特定的系统服务。我们可以找到INT_VECTOR_SYS_CALL的实现：
init_idt_desc(INT_VECTOR_SYS_CALL,DA_386IGate,sys_call,PRIVILEGE_USER)
这个函数我们不用知道什么意思，我们只需知道它通过系统调用sys_call这个函数，再来看sys_call
```
48.	sys_call:   
49.	     call save   //保存中断前进程的状态
50.	      
51.	     push dword [p_proc_ready]   
52.	      
53.	     sti   
54.	      
55.	     push ecx   //要打印出的元素个数
56.	     push ebx   //要打印出的buf数组中的第一个元素
57.	     call [sys_call_table + eax * 4]   
58.	     add esp, 4 * 3   
59.	      
60.	     mov [esi + EAXREG - P_STACKBASE], eax   
61.	      
62.	     cli   
63.	      
64.	     ret  
```
syscall将字符串中的字节“Hello 1170300825 lidaxin”从寄存器中通过总线复制到显卡的显存中，显存中存储的是字符的ASCII码。
字符显示驱动子程序将通过ASCII码在字模库中找到点阵信息将点阵信息存储到vram中。
显示芯片会按照一定的刷新频率逐行读取vram，并通过信号线向液晶显示器传输每一个点（RGB分量）。
于是我们的打印字符串“Hello 1170301006 韩啸”就显示在了屏幕上。
## 8.4 getchar的实现分析
键盘中断的处理过程：
当用户按键时，键盘接口会得到一个代表该按键的键盘扫描码，同时产生一个中断请求。键盘中断服务程序先从键盘接口取得按键的扫描码，然后根据其扫描码判断用户所按的键并作相应的处理，最后通知中断控制器本次中断结束并实现中断返回。
若用户按下双态键(如：Caps Lock、Num Lock和Scroll Lock等)，则在键盘上相应LED指示灯的状态将发生改变；
若用户按下控制键(如：Ctrl、Alt和Shift等)，则在键盘标志字中设置其标志位；
若用户按下功能键(如：F1、F2、…等)，再根据当前是否又按下控制键来确定其系统扫描码，并把其系统扫描码和一个值为0的字节存入键盘缓冲区；
若用户按下字符键(如：A、1、+、…等)，此时，再根据当前是否又按下控制键来确定其系统扫描码，并得到该按键所对应的ASCII码，然后把其系统扫描码和ASCII码一起存入键盘缓冲区；

getchar函数落实到底层调用了系统函数read，通过系统调用read读取存储在键盘缓冲区中的ASCII码直到读到回车符然后返回整个字串，getchar进行封装，大体逻辑是读取字符串的第一个字符然后返回。
## 8.5本章小结
本章主要介绍了Linux的IO设备管理方法、Unix IO接口及其函数，分析了printf函数和getchar函数。通过这章，我们知道，就算是我们从键盘输入一串字符串，然后要在屏幕上显示出来，这中间的过程也是异常复杂的，需要经过很多的处理，也不得不叹服计算机设计者的伟大。

# 结论
Hello程序在计算机中从出生到死亡整个过程：
1	写程序：写下我们hello程序的代码
2.	预处理：对带#的指令解析，生成hello.i文件
3.	编译：把我们的C语言程序编译成汇编语言程序，生成.s文件
4.	汇编：把汇编语言转换成机器代码，生成重定位信息，生成.o文件。
5.	链接：与动态库链接，生成可执行文件hello
6.	创建进程：在shell利用./hello运行hello程序，父进程通过fork函数为hello创建进程
7.	加载程序：通过加载器，调用execve函数，删除原来的进程内容，加载我们现在进程的代码，数据等到进程自己的虚拟内存空间。
8.	执行指令：CPU取指令，顺序执行进程的逻辑控制流。这里CPU会给出一个虚拟地址，通过MMU从页表里得到物理地址， 在通过这个物理地址去cache或者内存里得到我们想要的信息
9.	异常（信号）：程序执行过程中，如果从键盘输入Ctrl-C等命令，会给进程发送一个信号，然后通过信号处理函数对信号进行处理。
10.	结束：程序执行结束后，父进程回收子进程，内核删除为这个进程创建的所有数据结构。




_到此，我们对hello程序的分析已经全部结束。其实一直以来，我都想要知道一个程序在计算机内部到底是怎么执行的，我们现在有了各种各样的IDE，我们只需要写好代码，按一下build键，然后我们的程序就编译好输出了，当这中间的所有过程，我们都一点也不清楚，如果以后是在这些过程中发生了错误，那么我们会不知所措，这次大作业算是初步了解整个过程，也算是对一个程序的执行有了大体的认识，当还有许许多多的地方，只能算是一笔带过，实现上的细节还远远不清楚，还需要以后的学习中继续努力。_


附件
hello.c：源程序文件
hello.i：预处理后的文本文件
hello.s：编译后的汇编文件
hello.o：汇编后的可重定位文件
hello：链接后的可执行文件
（中间有些文件显示在shell，不过文中都有截图）



参考文献
为完成本次大作业你翻阅的书籍与网站等
[1] Linux GCC 常用命令 
https://www.cnblogs.com/ggjucheng/archive/2011/12/14/2287738.html 
[2]  ELF中与动态链接相关的段
https://blog.csdn.net/virtual_func/article/details/48792087
[3]  linux bash总结
http://www.cnblogs.com/skywang12345/archive/2013/05/30/3106570.html.
[4] x86在逻辑地址，线性地址，理解虚拟地址和物理地址
 https://www.cnblogs.com/bhlsheji/p/4868964.html
[5] printf函数实现的深入剖析
https://www.cnblogs.com/pianist/p/3315801.html
[6]  键盘的中断处理
https://blog.csdn.net/xumingjie1658/article/details/6965176
[7]聊聊Linux动态链接中的PLT和GOT（３）——公共GOT表项
https://blog.csdn.net/linyt/article/details/51637832
[8] 深入理解计算机系统（第三版） Randal E.Bryant  David R.O’Hallaron