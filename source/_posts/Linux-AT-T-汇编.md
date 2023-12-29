---
title: Linux AT&T 汇编
date: 2023-05-21 12:30:48
tags: 
- Linux
- AT&T
- ASM
---
## 开始之前

大名鼎鼎的**Hello World**

```s
.data 
  msg : .string "Hello Word!\n"
  len = . -msg

.text
.global _start 

_start :
  movl $len,%edx  # len -> edx
  movl $msg,%ecx  # msg -> ecx
  movl $1,%ebx
  movl $4,%eax    # 系统调用号
  int $0x80       # 调用  

  movl $0,%ebx    # 退出代码
  movl $1,%eax
  int $0x80
```


## 通用寄存器

汇编就是在几个寄存器间倒来倒去

寄存器有两种概念，逻辑上的和物理上的，分别是：

* 架构相关寄存器（architectural register）
* 物理寄存器（physical register）  
  
前者是指令集（ISA）提供给编译器可见的，相当于API接口规范，一共16个通用寄存器；后者是硬件上实际设计的，软件领域不直接接触。最新的CPU可能有上百个实际的物理寄存器。对软件开发人员来说，我们只需要关注逻辑上的通用寄存器。

这16个逻辑上的通用寄存器如下所示：

| 寄存器 | 约束         | 惯例                                        |
| ------ | ------------ | ------------------------------------------- |
| rax    | 否           | 系统调用时，调用号                          |
|        |              | 函数返回值                                  |
|        |              | 除法运算中，存放除数、以及运算结果的商      |
|        |              | 乘法运算中，存放被乘数、以及运算结果        |
| rbx    | 被调用者保存 | 在32位模式下，用来存放GOT的地址             |
| rcx    | 否           | 函数调用时，第4个参数                       |
|        |              | 有时用作counter                             |
| rdx    | 否           | 函数调用时，第3个参数                       |
|        |              | 除法运算中，存放运算结果的余数              |
|        |              | 乘法运算中，存放运算结果溢出的部分          |
| rbp    | 被调用者保存 | frame pointer，存放当前函数调用时栈的基地址 |
| rsp    | 被调用者保存 | 时刻指向栈顶                                |
| rdi    | 否           | 函数调用时，第1个参数                       |
|        |              | rep movsb中的目的寄存器                     |
| rsi    | 否           | 函数调用时，第2个参数                       |
|        |              | rep movsb中的源寄存器                       |
| r8     | 否           | 函数调用时，第5个参数                       |
| r9     | 否           | 函数调用时，第6个参数                       |
| r10    | 否           |                                             |
| r11    | 否           |                                             |
| r12    | 被调用者保存 |                                             |
| r13    | 被调用者保存 |                                             |
| r14    | 被调用者保存 |                                             |
| r15    | 被调用者保存 |                                             |

## 指令

### 1. 前缀、操作数的方向、操作码的后缀

关于前缀，AT&T 汇编中：

    寄存器：“%”
    立即数：“$”
    十六进制数：“0x”


在AT&T 语境中说到386的通用寄存器时，会这样描述：8个32-bit寄存器 %eax，%ebx，%ecx，%edx，%edi，%esi，%ebp，%esp。比如：在stage1.s中，有这么一个定义：

    #define ABS(x) （x-_start+0x7c00）

那么你就会知到0x7c00是个十六进制数（_start函数的入口地址就位于内存的0x7c00处）。而在设置int 0x13的0x42功能号时，它是这么说的：

    movb $0x42,%ah

这句告诉了我们一些不同之处：

    首先，操作码的后缀l表示的是操作码的大小：
    l是长整数32位，
    movw是16位，
    movb是8位；
    
    其次，立即数是用$前缀来表示的，就像$0x42；
    再次，寄存器的名字是有%前缀的，像例子中的%ah；
    最后，操作数的方向有点不一样，比如把立即数$0x42放 到寄存器%ah中，用的是movb $0x42,%ah，也即源操作数在前，目的操作数在后，这一点和intel汇编语法正好相反。

对于内存单元操作数来说，在AT&T 中是把寄存器用（）括起来，而非[]。比如：

    movl %ebx,8（%si）

将ebx寄存器里的值放到内存地址是8（%si）的内存单元上。正好，这里同时遇到了另一个问题，就是在AT&T 汇编中，间接寻址方式是有别于intel汇编的。上例中的8（%si）就相当于intel汇编中的[si+8]。

### 2，运算指令

一些常见的命令：

    add %r10,%r11 # r10 + r11，结果放到r11 
    add $5,%r10 # 5 + r10，结果放到r10
    div %r10 # rax 除以r10，商放到rax，余数放到rdx
    inc %r10 # r10 加1
    mul %r10 # 将rax乘以 r10, 将结果放到rax中，溢出部分放到rdx 

## 3，拷贝指令

这些拷贝有从寄存器到寄存器、从寄存器到内存

    mov %r10,%r11 #将r10寄存器的值赋值给r11 ；
    mov $99,%r10 #将立即数99赋值给r10寄存器； 
    mov %r10,(%r11) # 将r10的值拷贝到r11寄存器中的数值指向的内存地址上； 
    mov (%r10),%r11 # 将r10中数值指向的内存地址上的内容拷贝到r11；push %r10 # 将r10的值放到栈上 ；
    pop %r10 # 将栈顶的值pop到r10寄存器上。

## 4，流程控制指令

label（标号）程序控制语句

    cmpb $GRUB_INVALID_DRIVE,%al
        je 1f
        movb %al,%dl
    1:
        pushw %dx

上面就用到了标号，je 1f，前面的两个数进行比较，如果相等就跳转到1的位置。
注意，1后面的f表示的是forward，即从je指令后继续往前走来寻找1这个标号。所以，如果程序中有好几个叫做1的标号，就要看是1f还是1b了，b代表backward，方向和f相反。CivilNet BBS里有这么一个例子可以更好的帮助我们理解：

    1: cmp  $0,  (%si)  
      je  1f        # 跳转到后面的1标示的地方，也就是第6行
      movsb  
      stosb  
      jmp  1b       # 跳转到前面1表示的地方  ，也就是第1行
    1: jmp  1b      # 跳转到前面1表示的地方，第6行，其实就是个死循环

    cmp %r10,%r11   # 比较r10 和 r11，根据比较结果来设置CPU的状态寄存器，从而影响后面的jump语句；
    cmp $99,%r11    # 比较99和r11，根据比较结果来设置CPU的状态寄存器，从而影响后面的jump语句；
    jmp label       #跳转到label je label #如果相等，跳转到label jne label       # 如果不相等，跳转到label 
    jl label        # 如果小于，跳转到label 
    jg label        # 如果大于，跳转到label 
    call label      # 调用函数
    ret             # 从函数调用返回
    syscall         #系统调用 (32位模式下, 使用"int $0x80" 软中断)

## AT&T 汇编指示符 Assembler Directive

所有的汇编指示符都由句号（'.'）开头。命令名的其余是字母,通常使用小写

### 1，.byte 表达式（expression_rs）

.byte可不带参数或者带多个表达式参数，表达式之间由逗点分隔。每个表达式参数都被汇编成下一个字节。在stage1.s中，有这么一段代码：

    after_BPB:
    CLI
    .byte 0x80,0xca

那么编译器在编译时，就会在cli指令的下面接着放上0x80和0xca，因为每个表达式要占用1个字节，所以此处一共占用2个字节。

### 2，.word 表达式

这个表达式表示任意一节中的一个或多个表达式（同样用逗号分开），表达式占一个字（两个字节）。类似的还有.long。例：

    .word 0x800

### 3，.file 字符（string）

.file 通告编译器我们准备开启一个新的逻辑文件。 string 是新文件名。

    .file ”stage1.s”

### 4，.text 小节（subsection）

通知as编译器把后续语句汇编到编号为subsection的正文子段的末尾，subsection是一个纯粹的表达式。如果省略了参数subsection，则使用编号为0的子段。例：

    .text

### 5，.code16

告诉编译器生成16位的指令

### 6，.globl

.globl使得连接程序（ld）能够看到symbol，如果gemfield在局部程序中定义了symbol，那么与这个局部程序链接的局部程序也能存取symbol，例：

    .globl SYMBOL_NAME(idt) 

定义idt为全局符号。

### 7，.fill repeat , size , value

生成**size**个字节的**repeat**个副本。  
repeat, size 和value都必须是纯粹的表达式。  
repeat 可以是0或更大的值。
size 可以是0或更大的值, 但即使size大于8,也被视作8，以兼容其它的汇编器。

例如，在linux初始化的过程中，对全局描述符表GDT进行设置的最后一句为：

    .fill NR_CPUS*4,8,0


## 编译和链接

linux下有两种方式，一种是使用汇编程序GAS和链接程序ld：

    as filename.s –o filename.o
    ld filename.o –o filename 

最终将源代码转换为目标文件.o再连接为可执行文件filename。另一种就是更上层的gcc（内部使用了as）：

    gcc –o filename filename.S

