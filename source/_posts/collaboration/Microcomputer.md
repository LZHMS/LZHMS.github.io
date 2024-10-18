---
title: 微型计算机原理与系统设计
date: 2023-10-01 18:34:32
toc: true
tags:
    - Microcomputer
    - Collaboration Project
categories: collaboration
cover: https://lzhms.oss-cn-hangzhou.aliyuncs.com/images/covers/xdu/wp6272742-computer-hardware-and-network-hd-wallpapers.jpg
excerpt: 计科院微型计算机原理与系统设计课程学习笔记
---
## 第一章 绪论：微型计算机概述

### 第一讲 微型计算机概述

#### 基本概念

| 微型计算机系统的三个层次：微处理器、微型计算机、微型计算机系统 |
| :------------------------------------------------------------: |

<center><img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Courses/image.6ro9u72wnvw0.webp" alt="微型计算机系统层次结构" width="50%"/></center>

> + 微处理器：由 1 片或几片大规模集成电路组成的中央处理器，也即微型计算机中的 CPU (中央处理单元），具体包括控制器、运算器、寄存器以及连接三者的片内总线；
> + 微型计算机：微处理器、内存、I/O 接口以及连接三者的系统总线或芯片组的集合，也即裸机；
> + 微型计算机系统：以微型计算机为中心，配以相应的外围设备以及控制微型计算机工作的软件，简称为微机；
> + 软件：系统软件和应用软件；
> + 外设：外存和 I/O 设备；
> + 单片机：CPU、内存、I/O接口以及使三者互连的总线在一个芯片上的集成，也即微型计算机在一个芯片上的集成；
> + 单片机系统：由单片机、专用软件和 I/O设备组成的系统，常用于特定任务的控制或处理；

| 单片机系统具有专用性，微型计算机系统具有通用性 |
| :--------------------------------------------: |

#### 微处理器概述

+ Intel微处理器发展

|  时间  |   型号   | 位宽 |    主频    |  制造工艺  |
| :----: | :-------: | :--: | :--------: | :--------: |
| 1971年 |   4004   | 4位 |   108KHz   | 10$\mu$ |
| 1972年 |   8008   | 8位 | 500-800KHz | 10$\mu$ |
| 1974年 |   8080   | 8位 |    2MHz    |  6$\mu$  |
| 1978年 |   8086   | 16位 |    5MHz    |  3$\mu$  |
| 1979年 |   8088   | 16位 |    5MHz    |  3$\mu$  |
| 1982年 |   80286   | 16位 |    6MHz    | 1.5$\mu$ |
| 1985年 |   80386   | 32位 |   16MHz   | 1.5$\mu$ |
| 1989年 |   80486   | 32位 |   25MHz   |  1$\mu$  |
| 1993年 | Pentium I | 32位 |   66MHz   | 0.8$\mu$ |
| 2001年 |  Itanium  | 64位 |   66MHz   | 0.8$\mu$ |

## 第二章 Intel 单核/多核处理器

### 第一讲 单核处理器（Intel 8086处理器）

#### 8086/8088 处理器功能特性

+ 16 位微处理器（内部数据总线）
+ 引进指令级流水
+ 引入分段管理机制，扩大寻址空间
+ 只有整数运算能力，配套数值协处理器 8087、输入/输出协处理器 8089，具备较强大计算能力和 I/O处理能力
+ 8086：16位；8088：8位（外部数据总线）

#### 8086处理器的体系架构

<center><img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Courses/image.5p95dnxwm8g0.webp" alt="8086处理器体系架构" width="70%"/></center>

【组成部分】
           8086处理器分为两个部分：执行单元 (Execution Unit, EU) 和总线接口单元 (Bus Interface Unit, BIU)

+ 执行单元
  + EU负责指令的执行，包括ALU（运算器）、通用寄存器组和状态寄存器
  + 执行的运算：算术、逻辑、移位运算及段内偏移地址（即有效地址）的计算
+ 总线接口单元
  + BIU负责与主存和I/O设备的接口，由段寄存器、指令指针、地址加法器和指令队列缓冲器等组成
  + 主要操作：取指令、与主存或I/O设备交换数据

【工作机理】

+ EU与BIU并行工作

  BIU在指令队列缓冲器有2个以上字节时就不断从主存连续地址单元中取得指令送入指令队列缓冲器中，EU则不断从指令队列缓冲器中取出指令加以译码执行
+ 异常情况:

  + 当6个字节的指令队列缓冲器满，且EU没有主存或I/O访问请求时，BIU进入空闲状态；
  + 当EU执行访存或I/O指令时，BIU在执行完当前周期后，暂停取指令操作，在下一总线周期执行EU所要求的主存或I/O读写操作，之后再继续BIU的取指操作
  + 当EU执行转移、调用、返回等程序跳转类指令时，BIU会清除之前读入指令队列缓冲器的无效指令，并根据EU提供的跳转指令，重新获取跳转后的程序段指令

【创新特点】
         8086处理器引入指令队列缓冲器使得预取指令变为现实，取指令和执行指令可以并行执行，从而加速了程序的运行.

#### 8086处理器的寄存器

  8086处理器有14个寄存器：8个通用寄存器（4个数据寄存器、2个指针寄存器和2个变址寄存器）、2个控制寄存器和4个段寄存器.

<center><img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Courses/image.1eah2r4ojh6o.webp" alt="8086处理器的寄存器配置" width="40%"/></center>

【数据寄存器】

+ 16位数据寄存器：AX、BX、CX和DX，可以存放16位的源操作数或目的操作数
+ 为了支持字节操作，每个寄存器又分为两个8位的高、低字节寄存器：AH、AL、BH、BL、CH、CL、DH、DL

【指针寄存器】

+ SP(Stack Pointer)堆栈指针寄存器用于存放主存中堆栈区的偏移地址，指示堆栈的**当前操作位置**
+ BP(Base Pointer)基数指针寄存器用于存放主存的基本偏移地址

【变址寄存器】

+ SI(Source Index)源变址寄存器，指向源操作数，具有自动修改内容的功能
+ DI(Destination Index)目的变址寄存器，指向目的操作数，具有自动修改内容的功能

【控制寄存器】

+ IP(Instruction Pointer)指令指针寄存器，指示当前指令所在存储单元的段内偏移地址；当8086 CPU根据CS和IP取得一个指令字节后，IP便自动加1(顺序执行)，指向下一条待读取指令

**分析**
8086处理器采用分段存储管理机制，CS段寄存器存放当前指令所在内存段的首地址，IP存放当前指令所在内存段内偏移地址，以此来获取指令的地址；操作系统执行程序时，将首地址加载至CS和IP中，转移类指令执行时：如果目标地址与程序首地址所在同一段，就用目标地址修改IP；如果目标地址与程序首地址在不同段，就用目标地址同时修改CS和IP.

+ PSW(Program Status Word)程序状态字，也称程序寄存器或标志寄存器，存放CPU工作过程中的状态信息

<center><img src='https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Courses/image.3xmogz7v2mw0.webp' alt='程序状态字' width="60%"></center>

  【标志位】——16位暂定义了9个标志位

+ C——进位标志位，加减运算时标识最高位出现进位或借位，受逻辑运算、位移和循环指令的影响
+ P——奇偶标志位，标识运算结果低8位中1的个数是否为偶数
+ A——半加进位标志位，加减运算时标识低4位向高4位是否进位或借位，用于对BCD结果的校正
+ Z——零标志位，标识运算结果是否为全0
+ S——符号标志位，标识运算结果是否为负数，当运算结果最高位为1时，该标志位置为1
+ T——单步标志位（陷阱标志位），标志位置为1时，8086处理器进入单步执行指令方式；每条指令执行完毕时，CPU测试T标志位：若T=1则在当前指令执行后产生单步中断（陷阱中断），CPU执行陷阱中断处理程序：该程序能够显示当前指令执行结果，为程序调试提供必要的信息
+ I——中断允许标志位，该标志位为1时，CPU可响应可屏蔽中断请求，否则不响应可屏蔽中断请求
+ D——方向标志位，若该标志位为1，则SI和DI在串操作指令执行中自动减量，即从高地址到低地址处理字符串，否则在串操作指令执行中自动增量
+ O——溢出标志位，标识带符号数运算结果是否超出8位或16位表示范围

【段寄存器】
          组成：代码段寄存器 CS(Code Segment)、数据段寄存器 DS(Data Segment)、堆栈段寄存器 SS(Stack Segment)和附加段寄存器 ES(Extra Segment)
         作用：存储不同属性段的段地址，与有效的段内偏移地址一起确定主存的物理地址; CS指示程序区，DS和ES指示数据区，SS指示堆栈区

#### 8086 主存储器和 I/O结构

【主存分段存储管理机制】
         8086处理器的主存物理地址由段地址和段内偏移地址确定，且主存空间: $1MB$，每段大小: $64KB$，则可以划分的虚拟段数为:

$$
\frac{1MB}{64KB}=\frac{2^{20}B}{2^{16}B}=2^4
$$

因此 1MB 主存可以划分为$2^4$ 个不重叠的存储段，对于段起始地址（有效高四位）采用低位对齐策略即段起始地址的低4位全为 0，可以使得 $20$ 位段地址降低为 $16$ 位，调整段起始地址又可以指定不同存储段因此又可以划分为 $2^{16}$ 个重叠的存储段；段内偏移地址（有效低16位）可以决定段内的地址.
$BIU$ 中有一个地址加法器，作用是将 $16$ 位段地址左移 $4$ 位，然后与 $16$ 位段内偏移地址相加，生成 $20$ 位的物理地址:

$$
Memory\ Address=CS\times16+IP
$$

每个存储单元的地址标识：$20$ 位物理地址/逻辑地址（段地址 $16$ 位：段内偏移地址 $16$ 位）
由上式可以看出，8086处理器可以提供20位的地址，对主存单元寻址使用全部20位地址，对I/O设备端口寻址使用其低16位地址：1M的主存存储空间、64K的I/O设备端口空间
【主存结构设计】

+ 支持字节操作：8086处理器采用字节编址方式，即主存或I/O的一个地址单元内存储一个字节，实现根据地址进行1字节的存储器或I/O设备的读/写操作
+ 支持16位数据操作：字节编址方式下使用两个连续地址来访问16位数据，并按小段模式存储，实现16位存储器或I/O操作

【主存和I/O系统的分体结构】
        主存的存储空间和I/O的端口空间均采用奇、偶双体存储器，偶地址对应低字节的访问，通过低字节允许信号 $A_0=0$ 选择；奇地址空间对应高字节的访问，通过高字节允许信号 $\overline{BHE}=0$ 选择；

| $\overline{BHE}$ | $A_0$ |                                  操作说明                                  |
| :----------------: | :-----: | :------------------------------------------------------------------------: |
|       $0$       |  $0$  | 高字节体与低字节体同时有效，给定地址$n^*$，从主存或 I/O 空间读写16位数据 |
|       $0$       |  $1$  |        高字节体有效，给定奇地址$n$，从主存或 I/O 空间读写8位数据        |
|       $1$       |  $0$  |        低字节体有效，给定偶地址$n$，从主存或 I/O 空间读写8位数据        |
|       $1$       |  $1$  |              高字节体和低字节体无效，不能访问主存或 I/O 空间              |

注 $^*$:
当 $n$ 为偶地址时，仅需一个总线周期就可以完成 $2$ 字节的读/写，其中 地址 $n$ 读/写数据低 $8$ 位，地址 $n+1$ 读/写数据高 $8$ 位（主存按字节编址）；当 $n$ 为奇地址时，则需要两个总线周期才可以完成 $2$ 字节的读/写，其中第一个总线周期从地址 $n$ 读/写数据低 $8$ 位，第二个总线周期从地址 $n+1$ 读/写数据高 $8$ 位（主存按字节编址）；
（总线周期：CPU通过系统总线对主存或 I/O 设备进行一次读/写访问所需的时间）

【数据按属性分段存储】
8086处理器设置了 $4$ 个属性的存储段：程序段（代码段）、数据段、堆栈段和附加段，并用段寄存器 $CS$、$DS$、$SS$ 和 $ES$ 分别为 $4$ 个属性段提供段地址. 在访存操作中，段地址由“默认”或“指定”的段寄存器提供：

+ 段寄存器的默认使用情况

<center><img src='https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Courses/image.1rt305phzo1s.webp' alt='段寄存器的默认使用情况' width="40%"></center>

+ 段寄存器的指定使用情况

  通过在指令中增加一个字节的段超越前缀指令来实现：$MOV\ AL,ES:[BX]$(从存储单元 ($ES$:$BX$) 中读取数据)

### 8086处理器芯片引脚

|  关注方面  |                      作用                      |
| :--------: | :--------------------------------------------: |
|  引脚功能  |              引脚信号的定义、作用              |
| 信号的流向 |                输出、输入、双向                |
|  有效电平  | 高电平有效、低电平有效、上升沿有效、下降沿有效 |
|  三态能力  |              低电平、高电平、高阻              |

【8086 CPU 引脚分析——工作条件】

|         名称         |       方向       | 有效电平 |             功能             |
| :-------------------: | :--------------: | :------: | :---------------------------: |
|        $VCC$        |    $Input$    | $+5V$ |           工作电源           |
|        $GND$        |    $Input$    |  $1$  |            接地端            |
|        $CLK$        |    $Input$    | $5MHz$ |           时钟信号           |
|       $RESET$       |    $Input$    |  $1$  |           复位信号           |
|        $MN$        |    $Input$    |  $1$  |         最小工作模式         |
|        $MX$        |    $Input$    |  $0$  |         最大工作模式         |
| $AD_0\sim AD_{15}$ | $Input/Output$ |   三态   |        地址或数据总线        |
| $A_{16}\sim A_{19}$ |    $Output$    |   三态   |           地址总线           |
|        $BHE$        |    $Output$    |   三态   |        高字节允许信号        |
|       $INTR$       |    $Input$    |  $1$  |        可屏蔽中断信号        |
|       $INTA$       |    $Output$    |   三态   | 对$INTR$ 请求信号的响应信号 |
|       $READY$       |    $Input$    |  $1$  |         准备就绪信号         |
|       $TEST$       |    $Input$    |  $0$  |           测试信号           |
|  $DT/\overline{R}$  |    $Output$    |   三态   |     数据发送/接受控制信号     |
|        $DEN$        |    $Output$    |   三态   |         数据有效信号         |
|        $ALE$        |    $Output$    |  $1$  |         地址锁存信号         |

#### I. 两种工作模式下的共用信号

【$A_{16}\sim A_{19}/S_3\sim S_6$】
$A_{16}\sim A_{19}$ 与 $S_3\sim S_6$ 分时复用信号，$S_6$ 始终为0，$S_5$ 表示中断允许标志的状态，$S_4$、$S_3$ 指示 CPU 正在使用的寄存器；
【$\overline{BHE}/S_7$】
分时复用信号，$\overline{BHE}$ 在总线周期的 $T_1$ 时钟周期起作用；$S_7$ 为备用状态；
【$RESET$】
复位信号，当 $RESET$ 返回低电平时，CPU重新启动；
【$READY$】
CPU读写主存或 I/O 设备时，在总线周期的 $T_3$ 时钟周期采样 $READY$ 信号；当为低电平时，需要在 $T_3$ 周期之后插入等待周期 $T_{WAIT}$ ；
【$INTR$】
CPU 在每条指令执行的最后一个时钟周期采样该信号，以决定是否进入中断响应周期；
【$NMI$】
非屏蔽中断请求信号，上升沿有效，中断不可被屏蔽，$NMI$ 请求的优先级高于 $INTR$ 请求；
【$\overline{INTA}$】
在响应中断过程中，由 $\overline{INTA}$ 送出两个负脉冲，在第二个 $INTA$ 周期 CPU 获得外部中断源的中断向量码；
【$DT/\overline{R}$】
数据发送/接受控制信号，高电平控制数据发送；低电平控制数据接受；
【$\overline{DEN}$】
数据有效信号，表示 $D_0\sim D_{15}$ 的数据是否有效；该信号在最小模式下由 8086 提供，**低电平**有效，在最大模式下由 8288 提供，**高电平**有效；
【$ALE$】
地址锁存信号，高电平有效，表示 $A_0\sim A_{19}$ 上的地址有效；

#### II. 最小工作模式

当$MN/\overline{MX}=1$ 时， 8086 CPU工作在最小模式，微机中只有一个处理器，系统总线仅由CPU信号形成；

【$M/\overline{IO}$(输出、三态)】
低电平访问 I/O 设备；高电平访问主存；
【$\overline{RD}$(输出、三态)】
低电平有效，表示CPU正在读主存或 I/O 接口；
【$\overline{WR}$(输出、三态)】
低电平有效，表示CPU正太写主存或 I/O 接口；
【$HOLD$(输入)】
保持请求信号，高电平有效，表示某总线设备请求使用系统总线；
【$HLDA$(输出、三态)】
保持允许信号，高电平有效，表示 CPU 对 HOLD 请求的响应信号；

#### III. 最大工作模式

当$MN/\overline{MX}=0$ 时， 8086 CPU工作在最大模式，微机中除了主处理器8086，还允许接入其他协处理器（运算处理器8087、I/O 处理器8089）构成多微处理器系统，系统总线由 8086 和总线控制器 8288 提供的信号共同形成；

【$\overline{S_0}$、$\overline{S_1}$、$\overline{S_2}$(输出、三态)】
表示该总线周期存取哪种设备的状态信号，是 $8288$ 产生控制信号的依据；

【$8288$ 不同之处】

+ 8288 的 $DEN$ 信号为高电平有效；
+ 8288 通过 $\overline{MRDC}$(存储器读信号)，$\overline{MWTC}$(存储器写信号)、$\overline{IORC}$(I/O 读信号)、$\overline{IOWC}$(I/O 写信号)来控制对主存或I/O的访问；

【$\overline{LOCK}$(输出、三态)】
总线索存信号，低电平有效，信号有效期间，总线请求信号被封锁；

【$QS_0, QS_1$(输出)】
表示指令队列缓冲器存取的状态信号:

|QS1|QS0|性能|
|:-----:|:-----:|:-----:|
|0|0|无操作|
|0|1|队列中操作码的第一个字节|
|1|0|队列空|
|1|1|队列中非第一个操作码字节|


### 8086处理器工作时序

【指令周期】
在冯诺依曼计算机中，将CPU取得并执行一条指令所花的时间定义为一个指令周期。
【总线周期】
CPU通过系统总线对主存或I/O设备进行一次读/写访问所需的时间。8086 CPU的一个总线周期由 4 个时钟周期（T1、T2、T3、T4）组成。
【基本总线时序】
等待周期 $T_{WAIT}$ 一般插入到 $T_3$ 后面

#### I. 写总线周期(Concise Mode)

$T_1:\ Load\ Address$

<center>$A_{16}\sim A_{19}/S_3\sim S_6\Longrightarrow A_{16}\sim A_{19}$</center>
<center>$\overline{BHE}/S_7\Longrightarrow \overline{BHE}$</center>
<center>$AD_0\sim AD_{15}\Longrightarrow A_0\sim A_{15}$</center>
<center>$A_0\sim A_{19}, \overline{BHE}\stackrel{ALE}\Longrightarrow$ 锁存器</center>

$T_1:\ Select\ Interface$

<center>$M/\overline{IO}==1\Longrightarrow A_0\sim A_{19}(Memory\ Unit)$</center>
<center>$M/\overline{IO}==0\Longrightarrow A_0\sim A_{15}\ with\ A_{16}\sim A_{19}=0(IO\ Interface)$</center>

$T_2:\ Load\ Data$
$AD_0\sim AD_{15}\Longrightarrow D_0\sim D_{15}$</center>

<center>$\overline{WR},\ M/\overline{IO},\ A_0\sim A_{19}\Longrightarrow Memory/IO(Minimal\ Mode)$</center>
<center>$\overline{MWTC},\ \overline{IOWC},\ A_0\sim A_{19}\Longrightarrow Memory/IO(Maximal\ Mode)$</center>

$T_3:\ Continue\ T_2$
READY信号：解决主存或I/O接口实际写入时间长于CPU提供的时间的问题

<center>$(T_3$ 开始时刻即下降沿 $):\ Test\ READY==0$</center>
<center>$\Longrightarrow Insert\ T_{WAIT}$</center>
<center>$(T_{WAIT}$ 开始时刻即下降沿 $):\ Test\ READY==0$</center>
<center>$\Longrightarrow Insert\ T_{WAIT}\Rightarrow Continue\ Loop$</center>
<center>$Test\ READY==1\Longrightarrow T_4$</center>

$T_4:\ Restore\ States$

#### II. 读总线周期(Concise Mode)——对比写总线周期

$DT/\overline{R}$

<center>$DT/\overline{R}==0,Bus\stackrel{data}\Longrightarrow CPU(Read\ Mode)$</center>
<center>$DT/\overline{R}==1,CPU\stackrel{data}\Longrightarrow Bus(Write\ Mode)$</center>

#### III. 中断响应周期(Concise Mode)

$Response\ Conditions:$

<center>$INTR == 1\ and\ IF == 1\ (Request\ and\ permission)$</center>

$INTA\ Period:\ from\ T_2\ to\ T_4$
$First\ INTA\ Period$:

<center>通知提出 $INTA$ 请求的中断源，请求已得到响应，并封锁总线</center>

$Second\ INTA\ Period$:

<center>总线封锁信号 $\overline{LOCK}$ 无效，中断源 $\stackrel{中断向量码}\Longrightarrow D_0\sim D_7$</center>
<center>$D_0\sim D_7\stackrel{中断向量码}\Longrightarrow CPU$</center>

### 8086系统总线的形成

系统总线：地址总线(AB)、数据总线(DB)、控制总线(CB)

#### I. 最小模式的系统总线

8086 CPU 工作在最小模式下，系统总线信号全部来自CPU

+ 地址总线 AB: $A_0\sim A_{19}$、$\overline{BHE}/S_7$ (ALE信号由 8086 CPU 提供)
+ 双向数据总线  DB: $D_0\sim D_{15}$
+ 控制总线 CB: 8086 CPU 在最小模式下提供的所有控制信号

<img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Courses/image.22o30zd5hjk0.webp" alt="8086在最小模式下的系统总线形成" width="70%"/>


#### II. 最大模式的系统总线

8086 CPU 工作在最大模式下，系统总线信号来自 CPU 和总线控制器 8288

+ 地址总线 AB: $A_0\sim A_{19}$、$\overline{BHE}/S_7$ (ALE信号由总线控制器 8288 提供)
+ 双向数据总线  DB: $D_0\sim D_{15}$
+ 控制总线 CB: 最大模式下的控制总线信号由 8086 CPU 和总线控制器 8288 共同提供

<img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Courses/image.6e8tcdyyjm80.webp" alt="8086在最大模式下的系统总线形成" width="70%"/>

#### III. 常用芯片引脚分布
【74LS373——8位锁存器】
<img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Courses/image.65m29a4hvf80.webp" alt="74LS373芯片引脚分布" width="20%"/>

【74LS245——双向驱动器】
<img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Courses/image.5pp02v89gzs0.webp" alt="74LS245芯片引脚分布" width="20%"/>

【74LS244——单向驱动器】
<img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Courses/image.1nl1zf4kvlkw.webp" alt="74LS244芯片引脚分布" width="20%"/>

## 第三章 Intel 处理器指令系统及汇编语言

### 第一讲 指令寻址方式

#### 操作数的寻址方式

指令格式：操作码+操作数（操作数本身/操作数地址/地址的一部分/操作数地址的指针/其他操作数信息）

+ 立即寻址

{% message color:info size:default%}
    Example: MOV AX, im
{% endmessage %}

指令格式：操作数包含在指令中
存储形式：操作码 $[n]+imL[n+1]+imH[n+2]$（小端存储）
存储位置：与操作码一起存放在代码段区域中
指令用途：主要用来给寄存器/存储器赋初值

{% message color:info size:default%}
    Operand = AH << 8 + AL = imH << 8 + imL
{% endmessage %}

+ 直接寻址

{% message color:info size:default%}
    Example: MOV AX, DS:[offset]
{% endmessage %}

指令格式：16位段内偏移地址包含在指令中
存储形式：操作码 $[n]+offsetL[n+1]+offsetH[n+2]$（小端存储）
存储位置：与操作码一起存放在代码段区域中

{% message color:info size:default%}
    Address = DS << 4 + offset
    Operand = AH << 8 + AL = Mem[Address] << 8 + Mem[Address+1]
{% endmessage %}

+ 寄存器寻址

{% message color:info size:default%}
    Example: MOV DS, AX
{% endmessage %}

指令格式：操作数包含在CPU的内部寄存器中
存储位置：存放在寄存器中

{% message color:info size:default%}
    Operand = (Reg)
{% endmessage %}

+ 寄存器间接寻址

{% message color:info size:default%}
    Example: MOV AX, [SI]
{% endmessage %}

指令格式：操作数的16位段内偏移地址存放在 $SI/DI/BP/BX$ 中, 其中 $SI/DI/BX$ 间接寻址时操作数通常存放在现行**数据段**中; $BP$ 间接寻址时操作数存放在**堆栈段**中
存储位置：操作数存放在存储器中

{% message color:info size:default%}
    Address = (DS) << 4 + (SI/DI/BX) / (SS) << 4 + (BP)
    Operand = AH << 8 + AL = Mem[Address] << 8 + Mem[Address+1]
{% endmessage %}

+ 寄存器相对寻址

{% message color:info size:default%}
    Example: MOV AX, DISP[SI]
{% endmessage %}

指令格式：指令中存放段内偏移地址的寄存器 $SI/DI/BX/BP$ + $8/16$ 位带符号相对地址偏移量 $DISP$
存储位置：操作数存放在存储器中

{% message color:info size:default%}
    Address = (DS) << 4 + (SI/DI/BX) + DISP / (SS) << 4 + (BP) + DISP
    Operand = AH << 8 + AL = Mem[Address] << 8 + Mem[Address+1]
{% endmessage %}

+ 基址、变址寻址

{% message color:info size:default%}
    Example: MOV AX, [BX][SI]
{% endmessage %}

指令格式：指令指定一个基址寄存器 $BX/BP$ 和一个变址寄存器 $SI/DI$
存储位置：操作数存放在存储器中

{% message color:info size:default%}
    Address = (DS) << 4 + (BX) + (SI/DI) / (SS) << 4 + (BP) + (SI/DI)
    Operand = AH << 8 + AL = Mem[Address] << 8 + Mem[Address+1]
{% endmessage %}

+ 基址、变址、相对寻址

{% message color:info size:default%}
    Example: MOV AX, [BX][SI]
{% endmessage %}

指令格式：指令指定一个基址寄存器 $BX/BP$、一个变址寄存器 $SI/DI$ 和相对偏移地址 $DISP$
存储位置：操作数存放在内存中

{% message color:info size:default%}
    Address = (DS) << 4 + (BX) + (SI/DI) + DISP / (SS) << 4 + (BP) + (SI/DI) + DISP
    Operand = AH << 8 + AL = Mem[Address] << 8 + Mem[Address+1]
{% endmessage %}

+ 隐含寻址

{% message color:info size:default%}
Example: 乘法指令 MUL BL
            十进制调整指令 DAA
            串传送指令 MOVSW
{% endmessage %}

指令格式：操作数的地址隐含在指令操作码中

#### 转移地址的寻址方式

+ 段内直接寻址（相对寻址）

{% message color:info size:default%}
Example: JMP DSP1
{% endmessage %}

指令格式：指令指明8位或16位带符号相对地址偏移量 $DISP$(补码表示)
指令说明：相对寻址指的是在程序计数器的基础上加上偏移量进行相对位移

{% message color:info size:default%}
Jump Address = (CS) << 4 + ((IP) + DISP)
{% endmessage %}

+ 段内间接寻址

{% message color:info size:default%}
Example: JMP CX
{% endmessage %}

指令格式：指令指明存放段内偏移地址的寄存器或存储器单元地址，按指令码中规定的寻址方式取得转移地址
指令说明：间接寻址指的是通过寄存器或存储器单元获取得到新的段内偏移地址赋值给程序计数器而段地址不变

{% message color:info size:default%}
Jump Address = (CS) << 4 + (CX)
{% endmessage %}

+ 段间直接寻址

{% message color:info size:default%}

+ Example:
  + JMP FAR PTR OPRD
+ PTR
  + 属性运算符: 给指令中的操作数指定一个临时属性，暂时忽略当前属性
  + 作用于操作数时，重载操作数的类型(字节或字)或属性(NEAR或FAR)
+ NEAR, SHORT, FAR
  + 表示操作数的属性
  + SHORT 表示段内短转移, IP 偏移量: $-128\sim127$ 字节($8$ 位，一个字节)
  + NEAR 表示段内近转移, IP 偏移量: $-32768\sim32767$ 字节($16$ 位，两个字节)
  + FAR 表示段间转移
    {% endmessage %}

指令格式：指令码指明段地址和偏移地址所在存储单元的首地址: OPRD (标号或立即数, 标号会在编译时由 CPU 转换为对应的地址)
指令说明：操作码后连续四个字节，低字表示段内转移地址，高字表示段地址，直接赋值给 IP, CS (直接寻址)
<img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Courses/image.4ghvu9vmlh40.png" alt="段间直接寻址地址对应关系" width="70%"/>

{% message color:info size:default%}
Jump Address = (CS) << 4 + (IP)
{% endmessage %}

+ 段间间接寻址

{% message color:info size:default%}
Example: JMP DWORD PTR [BP][DI]
{% endmessage %}

指令格式：依据指令码寻址方式确定存储单元的首地址，前两个单元：段内偏移地址，后两个单元：段地址
指令说明：需要确定段地址 ($16$位) 和段内偏移地址 ($16$位) $32$ 位信息，只适用于存储器寻址方式

{% message color:info size:default%}
Address = SS + BP + DI
CS = Mem[Address+3] << 8 + Mem[Address+2], IP = Mem[Address+1] << 8 + Mem[Address]
Jump Address = (CS) << 4 + (IP)
{% endmessage %}

### 第二讲 汇编语言

+ 引言：汇编语言的用途
  + 嵌入式系统中，程序大小和运行速度需要高度优化
  + 设计驱动程序、操作系统内核以及编译程序
  + 高级语言中嵌入汇编
+ 汇编语言的访问层次
  + OS函数
  + BIOS功能（一组固化到计算机内主板上一个ROM芯片上的程序，保存着计算机最重要的基本输入输出的程序、开机后自检程序和系统自启动程序）
  + 硬件

#### 汇编语言程序结构

+ 操作系统装入程序：初始化CS为正确的代码段地址，初始化SS为正确的堆栈段地址
+ ASSUME 伪指令：指明段与段寄存器的对应关系
+ 编译、链接和运行程序
  <img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Courses/图片1.5hmc3yy94mk0.webp" alt="编译、链接和运行程序" width="70%;" />

#### 汇编语言基本元素

{% message color:info size:default%}
[...] 中的参数可选，{...|...} 多选一（由'|'隔开）
{% endmessage %}

+ 整数常量

{% message color:info size:default%}

<center>digits[radix]</center>

|      radix      |   进制   | radix |  进制  | radix |  进制  | radix |  进制  | radix |   进制   |
| :--------------: | :------: | :---: | :----: | :---: | :----: | :---: | :----: | :---: | :------: |
|        h        | 十六进制 |   b   | 二进制 |  q/o  | 八进制 |   d   | 十进制 |   r   | 编码实数 |
| {% endmessage %}

+ 保留字
  {% message color:info size:default%}

  + 指令助记符: 标识特定的指令，Example: MOV, ADD
  + 伪指令: 指明 MASM 如何编译程序
  + 属性: 变量和操作数的尺寸以及使用方式的说明
  + 运算符: 常量表达式中
  + 预定义符号: Example: @data (编译时返回整数常量)
    {% endmessage %}
  + 定义数据伪指令
    <img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Courses/image.3f1810ez9co0.webp" alt="定义数据伪指令" width="70%;" />
  + 符号常量伪指令
    不占用任何实际的存储空间
    {% message color:info size:default%}
  + 等号伪指令

    + Example:
      COUNT = 500
      mov cx COUNT
  + EQU伪指令
  + TESTEQU伪指令
    {% endmessage %}
  + 操作符
    {% message color:info size:default%}
  + OFFSET

    + 返回数据标号的偏移地址（标号距数据段开始的距离，以字节为单位）
    + 保护模式下偏移: $32$ 位，实模式下偏移: $16$ 位
    + Example:
      + Address[bVal] = $00300$h

      ```x86asm
      .data
      bVal DB ?
      wVal DW ?
      mov si, OFFSET bVal   ; SI=0000H
      mov si, OFFSET wVal   ; SI=0001H
      ```
  + SEG

    + 返回变量或数据标号的段基址
    + Example:

    ```x86asm
    .data
    buffer DB ?
    mov ax, SER buffer
    mov ds, ax
    ```
  + PTR

    + 重载操作数的默认尺寸
    + 标准数据类型：DB, SDB, WORD, SWORD, DWORD, SDWORD, FWORD, QWORD, TDB
    + Example:

    ```x86asm
    .data
    myDouble DD 12345678h    ; Data Double Words: 32位数据
    .code
    mov ax, myDouble     ; ax为16位寄存器，数据长度不一致引发报错
    mov ax, DW PTR myDouble     ; Data Word: 16位数据, ax=5678h
    mov ax, DW PTR [myDouble+2]     ; ax=1234h
    mov bl, DB PTR myDouble     ; Data Byte: 8位数据, bl=78h
    ```
  + TYPE

    + 返回按字节计算的变量单个元素的大小
    + Example:

    ```x86asm
    .data
    var1 DB ?       ; Data Byte
    var2 DW ?       ; Data Word
    var3 DD ?       ; Data Double Words
    var4 DQ ?       ; Data Quanter Words
    ```

    |    表达式    |  值  |
    | :-----------: | :---: |
    | TYPE var$1$ | $1$ |
    | TYPE var$2$ | $2$ |
    | TYPE var$3$ | $4$ |
    | TYPE var$4$ | $8$ |
  + LENGTHOF

    + 计算数组中元素个数
    + Example:

    ```x86asm
    .data
    DB1 DB 10, 20, 30
    array1 DW 30 DUP(?), 0, 0
    array2 DW 5 DUP(3, DUP(?))
    array3 DD 1, 2, 3, 4
    digitStr DB "12345678", 0
    ```

    |       表达式       |      值      |
    | :-----------------: | :----------: |
    |  LENGTHOF DB$1$  |    $3$    |
    | LENGTHOF array$1$ |   $30+2$   |
    | LENGTHOF array$2$ | $5\times3$ |
    | LENGTHOF array$3$ |    $4$    |
    |  LENGTHOF digitStr  |    $9$    |
  + SIZEOF

    + SIZEOF返回值=LENGTHOF返回值 $\times$ TYPR返回值
    + Example:

    ```x86asm
    .data
    intArray DW 32 DUP(0)
    ```

    |      表达式      |   值   |
    | :---------------: | :----: |
    |   TYPE intArray   | $2$ |
    | LENGTHOF intArray | $32$ |
    |  SIZEOF intArray  | $64$ |

  {% endmessage %}

### 第三讲 指令系统

#### 数据传送指令

+ 操作数类型

  + 立即数 (Immediate)
    {% message color:info size:default%}
  + Label
    + imm: $8$、$16$、$32$ 位立即数
    + imm8: $8$ 位立即数
    + imm16: $16$ 位立即数
    + imm32: $32$ 位立即数
  + 立即数只能用作源操作数
  |立即数类型|$8$ 位| $16$ 位|
  |:-----:|:---------:|:------:|
  |无符号数 | $00H\sim FFH(0\sim 255)$ | $0000H\sim FFFFH(0\sim 65535)$  |
  |有符号数| $80H\sim 7FH(-128\sim 127)$ | $8000H\sim 7FFFH(-32768\sim 32767)$ |
  {% endmessage %}
  + 寄存器操作数 (Register)
    {% message color:info size:default%}
  + Label
    + reg: 任意的通用寄存器
    + sreg: $16$ 位段寄存器 (CS, DS, SS, ES, FS, GS)
    + r8: AH, AL, BH, BL, CH, CL, DH, DL
    + r16: AX, BX, CX, DX
    + r32: EAX, EBX, ECX, EDX, ESI, EDI, EBP, ESP
  + SI, DI, SP, BP 只能存放字操作数
  + 不允许将立即数传送到段寄存器
    {% endmessage %}
  + 内存操作数 (Memory)
    {% message color:info size:default%}
  + Label
    + mem: $8$、$16$ 或 $32$ 内存操作数
  + 不允许两个操作数同时为存储器操作数
    + Example:

    ```x86asm
    .code
    mov [8000H], [1000H]   ; Error
    mov buff1, buff2      ; Error
    mov al, [buff2]    ; 编译器自动将名称转换为地址偏移量
    mov al, [buff1+1]  ; 直接偏移操作数
    ```

    {% endmessage %}
+ 数据传送指令

  + MOV
    {% message color:info size:default%}
  + 指令格式：mov desination, source
  + Special Rules:
    + 两个操作数类型尺寸必须一致
    + 目的操作数不能是 CS 和 IP (保护程序执行)
  + 特殊用法 (AX作桥梁)

  ```x86asm
  ; 存储器 -> 存储器
  MOV AX, MEM1
  MOV MEM2, AX
  ; 段寄存器 -> 段寄存器
  MOV AX, DS
  MOV ES, AX
  ; 立即数 -> 段寄存器
  MOV AX, 1000H
  MOV DS, AX
  ```

  {% endmessage %}

  + MOVZX (move with zero-extend, 高位填充 $0$)
    {% message color:info size:default%}
  + 指令格式:
    + movzx r32, r/m8
    + movzx r32, r/m16
    + movzx r16, r/m8
  + Example:

  ```x86asm
  .code
  mov bx, 0A69Bh
  movzx eax, bx
  movzx ax, bl
  ```

  {% endmessage %}

  + MOVSX (move with sign-extend, 高位填充符号位)
    {% message color:info size:default%}
  + 指令格式:
    + movzx r32, r/m8
    + movzx r32, r/m16
    + movzx r16, r/m8
  + Example:

  ```x86asm
  .code
  mov bx, 0A69h
  movsx eax, bx   ; eax = FFFFA69Bh
  movsx edx, bl   ; edx = FFFFFF9Bh
  movsx ax, bl    ; ax =  FF9Bh
  ```

  {% endmessage %}

  + 字节-字转换命令
    {% message color:info size:default%}
  + 指令格式:
    + CBW: 把 AL 的符号位复制到 AH
    + CWD: 把 AX 的符号位复制到 DX
  + 用途: 用于有符号数的除法
    {% endmessage %}
  + XCHG (exchange data)
    {% message color:info size:default%}
  + 指令格式:
    + xchg reg, reg
    + xchg reg, mem
    + xchg mem, reg
  + Rules:
    + 两个操作数至少有一个在寄存器中
    + 操作数不能是段寄存器和立即数
    + 操作数类型一致
  + Example:

  ```x86asm
  .code
  mov ax, val1
  xchg ax, val2
  mov val1, ax
  ```

  {% endmessage %}

  + 堆栈操作指令概述
    + 以字 ($16$ 位) 为单位进行数据压入和弹出操作，但是存储单元仍是 $8$ 位
    + SS 指示堆栈段的段基址，SP 始终指向堆栈的顶部，即最后一个存储单元
    + 进栈方向从高地址向低地址发展 (SP 的初值决定了所用堆栈区的大小)
  + 堆栈用途
    + 作为临时保存区域，保存局部变量
    + 备份寄存器状态，以便恢复其原始值
    + CALL 指令执行时，CPU 用堆栈保存当前过程的返回地址
    + 调用过程中，通过堆栈传递参数
  + PUSH
    {% message color:info size:default%}
  + 指令格式: PUSH SRC (SRC 为 $16/32$ 位操作数，对应 SP 以 $2/4$ 递减)
  + Example:

  ```x86asm
  .code
  push ax
  ;  (SP-1) <- AH
  ;  (SP-2) <- AL
  ;  (SP) <- (SP)-2  小端存储，注意括号寻址
  ```

  {% endmessage %}

  + POP
    {% message color:info size:default%}
  + 指令格式: POP DEST (DEST 为 $16/32$ 位操作数，对应 SP 以 $2/4$ 递增)
  + 指令功能: 从堆栈顶部连续取两个单元的内容送到 DEST 指定的位置
  + Example:

  ```x86asm
  .code
  pop ax     ; 寄存器
  pop ds     ; 数据段寄存器
  pop [bx]   ; 内存单元
  ```

  {% endmessage %}

  + PUSHFD ($32$ 位程序)
    {% message color:info size:default%}
  + 在堆栈上压入 $32$ 位 EFLAGS 寄存器的值
  + Example:

  ```x86asm
  .data
  saveFlags DWORD ?
  .code
  pushfd           ; 标志入栈，不需指定
  pop saveFlags    ; 拷贝到变量
  push saveFlags   ; 将保存的标志入栈
  popfd            ; 恢复标志，不需指定
  ```

  {% endmessage %}

  + POPFD ($32$ 位程序)
    {% message color:info size:default%}
  + 将堆栈顶部的值弹出并送至 EFLAGS 寄存器
    {% endmessage %}
  + PUSHF (实地址模式)
    {% message color:info size:default%}
  + 在堆栈上压入 $16$ 位 FLAGS 寄存器的值
    {% endmessage %}
  + POPF (实地址模式)
    {% message color:info size:default%}
  + 将堆栈顶部的值弹出 $16$ 位的值并送至 FLAGS 寄存器
    {% endmessage %}
  + PUSHAD (Push All Data)
    {% message color:info size:default%}
  + 在堆栈上按顺序压入所有 $32$ 位通用寄存器: EAX, ECX, EDX, EBX, ESP, EBP, ESI, EDI
    {% endmessage %}
  + POPAD (Pop All Data)
    {% message color:info size:default%}
  + 在堆栈上按相反的顺序弹出所有 $32$ 位通用寄存器: EDI, ESI, EBP, ESP, EBX, EDX, ECX, EAX
    {% endmessage %}
  + PUSHA ($80286$ 处理器)
    {% message color:info size:default%}
  + 在堆栈上按顺序压入所有 $16$ 位通用寄存器: AX, CX, DX, BX, SP, BP, SI, DI
    {% endmessage %}
  + POPA ($80286$ 处理器)
    {% message color:info size:default%}
  + 在堆栈上按相反的顺序弹出所有 $16$ 位通用寄存器: DI, SI, BP, SP, BX, DX, CX, AX
    {% endmessage %}
  + LEA (Load Effective Address)
    {% message color:info size:default%}
  + 指令格式: LEA reg, mem
  + 指令功能: 将指定存储器的 $16$ 位偏移地址送到指定的寄存器
  + Example:

  ```x86asm
  LEA BX, BUFFER       ; 符号地址为 BUFFER 的存储单元的偏移地址取到 BX 中
  MOV BX, BUFFER       ; 符号地址为 BUFFER 的存储单元中内容送至 BX 中
  LEA BX, BUFFER = MOV BX, OFFSET BUFFER   ; LEA 可以取动态的地址, OFFSET 只能取静态的地址
  ```

  {% endmessage %}

  + LDS (Load DS)
    {% message color:info size:default%}
  + 指令格式: LDS reg, mem32
  + 指令功能: $mem32$ 开始的四个内存单元 $\rightarrow$ DS:reg (高 $16$ 位 $\rightarrow$ DS, 低 $16$ 位 $\rightarrow$ reg)
    {% endmessage %}
  + LES (Load ES)
    {% message color:info size:default%}
  + 指令格式: LES reg, mem32
  + 指令功能: $mem32$ 开始的四个内存单元 $\rightarrow$ ES:reg (高 $16$ 位 $\rightarrow$ ES, 低 $16$ 位 $\rightarrow$ reg)
    {% endmessage %}
  + Special Rules(LEA, LDS, LES):
    + 源操作数必须是一个内存操作数
    + 目的操作数必须是一个 $16$ 位的通用寄存器

#### 算术运算类指令

+ 加法和减法指令

  + INC
    {% message color:info size:default%}
  + 指令格式: INC OPRD
  + 指令功能: 操作数 + $1$ $\rightarrow$ 目的操作数
    {% endmessage %}
  + DEC
    {% message color:info size:default%}
  + 指令格式: DEC OPRD
  + 指令功能: 操作数 - $1$ $\rightarrow$ 目的操作数
  + Special Rules(INC, DEC):
    + 不影响 CF, 影响 OF, SF, ZF, AF, PF
      {% endmessage %}
  + ADD
    {% message color:info size:default%}
  + 指令格式: ADD DEST, SRC
  + 指令功能: 源操作数+目的操作数 $\rightarrow$ 目的操作数
    {% endmessage %}
  + ADC
    {% message color:info size:default%}
  + 指令格式: ADC DEST, SRC
  + 指令功能: 源操作数+目的操作数+ CF $\rightarrow$ 目的操作数
  + 指令用途: 多用于多字节加法运算中
  + Example: 有两个 $4$ 字节的数分别存放在 FIRST 和 SECOND 开始的两个存储区中，试将两数相加并将结果放回 FIRST 存储区中

  ```x86asm
  .code
  mov ax, FIRST     ; 第一个数的低 16 位送 ax
  add ax, SECOND    ; 两数的低 16 位相加送 ax
  mov FIRST, ax     ; 低 16 位相加结果送 FIRST 和 FIRST+1 单元 (小端存储)
  mov ax, FIRST+2   ; 第一个数的高 16 位送 ax
  adc ax, SECOND+2  ; 高 16 位同低位进位相加送 ax 
  mov FIRST+2, ax   ; 高 16 位相加的结果存入 FIRST+2 和 FIRST+3 单元
  ```

  {% endmessage %}

  + SUB
    {% message color:info size:default%}
  + 指令格式: SUB DEST, SRC
  + 指令功能: 目的操作数 - 源操作数 $\rightarrow$ 目的操作数
    {% endmessage %}
  + SBB
    {% message color:info size:default%}
  + 指令格式: SBB DEST, SRC
  + 指令功能: 目的操作数 - 源操作数 - CF $\rightarrow$ 目的操作数
  + 指令用途: 多用于多字节减法运算中
  + 影响标志位: CF, ZF, SF, OF, AF, PF
    {% endmessage %}
  + NEG
    {% message color:info size:default%}
  + 指令格式: NEG OPRD
  + 指令功能: 0 - (OPRD) $\rightarrow$ OPRD (OPRD 可以为 REG/MEM)
  + 指令说明: 将操作数按位求反、末位加一 (求补运算)
  + 影响标志位: CF, ZF, SF, OF, AF, PF
  + Example:

  ```x86asm
  .data
  val DB FCH
  .code
  mov al, val
  neg al    ; (al) = 04H, CF = 1
  ```

  {% endmessage %}

+ 乘法和除法指令

  + MUL
    {% message color:info size:default%}
  + 指令格式: MUL r/m$8$/m$16$/m$32$
  + 指令功能: 无符号乘法

    |      被乘数      |   乘数   |   积   | CF=$1$的条件 |
    | :--------------: | :-------: | :-----: | :------------: |
    |        AL        | r/m$8$ |   AX   |  AH$\neq0$  |
    |        AX        | r/m$16$ |  DX:AX  |  DX$\neq0$  |
    |       EAX       | r/m$32$ | EDX:EAX |  EDX$\neq0$  |
    {% endmessage %}
  + IMUL
    {% message color:info size:default%}
  + 指令格式: IMUL r/m$8$/m$16$/m$32$
  + 指令功能: 有符号乘法
  + 指令说明: 积的高半部分不是第半部分的符号扩展，则设置 CF 和 OF
    {% endmessage %}
  + DIV
    {% message color:info size:default%}
  + 指令格式: DIV r/m8/m16/m32)
  + 指令功能: 无符号除法，影响 OF

    |      被除数      | 除数 | 商 | 余数 |
    | :--------------: | :---: | :-: | :--: |
    |        AX        | r/m8 | AL |  AH  |
    |      DX:AX      | r/m16 | AX |  DX  |
    |     EDX:EAX     | r/m32 | EAX | EDX |
    {% endmessage %}
  + IDIV
    {% message color:info size:default%}
  + 指令格式: IDIV r/m$8$(/$16$/$32$)
  + 指令功能: 有符号除法，影响 OF

    |      被除数      | 除数 | 商 | 余数 |
    | :--------------: | :---: | :-: | :--: |
    |        AX        | r/m8 | AL |  AH  |
    |      DX:AX      | r/m16 | AX |  DX  |
    |     EDX:EAX     | r/m32 | EAX | EDX |
    {% endmessage %}
  + CBW

    + 将AL中的符号位扩展到AH
  + CWD

    + 将AX中的符号位扩展到DX
  + CDQ

    + 将EAX中的符号位扩展到EDX
  + 溢出

    + 商太大，目的操作数无法容纳 -> 使用32位除数
    + 除数 = 0 $\rightarrow$ 跳过，不执行

#### 逻辑运算和移位指令

+ 逻辑运算类指令

  + AND
    {% message color:info size:default%}
  + 指令格式: AND DEST, SRC
  + 指令功能: DEST & SRC $\rightarrow$ DEST (对特定位清零，同时保留其他位)
  + 影响的标志位: 清除 OF 和 CF, 修改 SF, ZF, PF
  + Example: 大小写字母的转换(大写转小写方法：将位 $5$ 设置为 $1$, 加32得到小写字母)
    + 'a': 61h, 即 $01\boldsymbol{1}00001$
    + 'A': 41h, 即 $01\boldsymbol{0}00001$

  ```x86asm
  .data
      array DB 50 DUP(?)
  .code
      mov cx, LENGTHOF array
      mov si, OFFSET arrray
  L1:
      and byte PTR [si], 11011111b
      inc si
      loop L1
  ```

  {% endmessage %}
  + OR
    {% message color:info size:default%}
  + 指令格式: OR DEST, SRC
  + 指令功能: DEST | SRC $\rightarrow$ DEST (对特定位设 $1$，同时保留其他位)
  + Example: $0\sim9$ 数字转换为 ASCII 码数字 (方法：将位 $4$ 和位 $5$ 设置为 $1$, 数字加48得到数字字符)
    + $05$h: 00000101b
    + $30h$: 00$\boldsymbol{11}$0000b

  ```x86asm
  .code
      mov dl, 05h
      or dl, 30h
  ```

  {% endmessage %}
  + XOR
    {% message color:info size:default%}
  + 指令格式: XOR DEST, SRC
  + 指令功能: DEST ^ SRC $rightarrow$ DEST (对特定位取反，同时保留其他位)
  + Example:
    + 判断 $16$ 位或 $32$ 位值奇偶性
    + 简单数据加密 $(X\oplus Y)\oplus Y=X$
    + 对 Reg 清零 (自身异或)
    + 把 Reg/Mem 得某几位取反 (与 $1$ 异或)

  ```x86asm
  .code
      mov ax, 64C1h
      xor ah, al
  ```

  {% endmessage %}
  + 寄存器清零
    {% message color:info size:default%}
  + MOV AX, $0$
  + XOR AX, AX
  + AND AX, $0$
  + SUB AX, AX
    {% endmessage %}
  + NOT
    {% message color:info size:default%}
  + 指令格式: NOT Reg/Mem
  + 指令功能: 对数据位取反，结果为反码 (不影响任何状态标志)
    {% endmessage %}

#### 比较测试指令
+ CMP
  {% message color:info size:default%}
+ 指令格式: CMP DEST, SRC
+ 指令功能: 目的操作数 - 源操作数, 但不回送结果只影响标志位
+ 影响标志位: CF, ZF, SF, OF, AF, PF
+ 判读比较结果

  + 两个无符号数

  | CMP Result | ZF | CF |
  | :--------: | :-: | :-: |
  | DEST < SRC | 0 | 1 |
  | DEST > SRC | 0 | 0 |
  | DEST = SRC | 1 | 0 |


  + 两个有符号数

  | CMP Result |     Sign     |
  | :--------: | :-----------: |
  | DEST < SRC | SF$\neq$ OF |
  | DEST > SRC |     SF=OF     |
  | DEST = SRC |     ZF=1     |

{% endmessage %}

+ TEST
  {% message color:info size:default%}
+ 指令格式: TEST DEST, SRC
+ 指令功能: DEST & SRC, 根据结果设置标志位但不回送结果
+ 影响标志位: 清除 OF, CF；修改 SF, ZF, PF
+ Example:
  + 测试操作数的某一位是 $0$ 或 $1$

```x86asm
.code
test al, 80H      ; 检查 al 操作数是否是负数 (D7=1)
jnz MINUS         ; 条件跳转指令, 转到 MINUS
```

```x86asm
.code
test al, 00001001H      ; 检查 al 操作数第 0 位和第 3 位是否同时为 0
```

{% endmessage %}

#### 控制转移指令

+ 转移指令概述

  + 实质: 改变 IP(或 CS) 的内容
  + 所有转移指令不会影响标志位
+ 无条件转移指令

  + JMP

    + JMP disp (段内直接转移)
      + CS 保持不变, **指令中** $8/16$ 位偏移量**加到** IP
        {% message color:info size:default%}
    + JMP SHORT OPRD
      + $8$ 位偏移地址: $-128\sim +127$
    + JMP NEAR PTR BUF
      + $16$ 位偏移地址: $-32768\sim +32767$
    + Example:

    ```x86asm
    .code
    JMP 0120H                  ; 直接转向
    JMP SHORT LR               ; 8 位偏移量
    JMP NEAR PTR BUF           ; 16 位偏移量
    ```

    {% endmessage %}
    + JMP reg/mem (段内间接转移)
      + CS 保持不变, **reg/mem** 中的 $16$ 位偏移地址**送到** IP
      + $16$ 位偏移地址指的是段内偏移地址，而不是相对于 IP 的偏移量
        {% message color:info size:default%}

    ```x86asm
    .code
    JMP [BX + DI]   ; (DS) = 3000H, (BX) = 1300H, (DI) = 1200H, (32500H) = 2350H
    ;   (IP) = 2350H
    ```

    {% endmessage %}
    + JMP segment:offset (段间直接转移)
      + **指令**中的 $16$ 位段地址和 $16$ 位偏移地址**送到** CS 和 IP (立即数)
        {% message color:info size:default%}

    ```x86asm
    .code
    JMP 2000H:1000H
    ; (CS) = 2000H, (IP) = 1000H
    ```

    注：直接地址为符号地址时，段间直接转移指令中的符号地址前应加操作符 FAR PTR
    {% endmessage %}
    + JMP mem$32$ (段间间接转移)
      + **mem$32$** 中的 $16$ 位段地址和 $16$ 位偏移地址**送到** CS 和 IP (两个相邻字)
        {% message color:info size:default%}

    ```x86asm
    .code
    JMP DWORD PTR[SI]       ;  表示转移地址为一个双字
    ; (DS) = 4000H, (SI) = 1212H, (41212H) = 1000H, (41214H) = 4A00H
    ; (IP) = 1000H, (CS) = 4A00H
    ```

    {% endmessage %}
+ 条件跳转指令

  + 声明全局变量 (变量后面紧跟 "::")
  + 基于特定 CPU 标志值的跳转指令

  | 助记符 |     标志位     | 标志值/跳转条件 |
  | :----: | :------------: | :-------------: |
  |   JZ   |  ZF(零标志位)  |        1        |
  |  JNZ  |  ZF(零标志位)  |        0        |
  |   JC   | CF(进位标志位) |        1        |
  |  JNC  | CF(进位标志位) |        0        |
  |   JO   | OF(溢出标志位) |        1        |
  |  JNO  | OF(溢出标志位) |        0        |
  |   JS   | SF(符号标志位) |        1        |
  |  JNS  | SF(符号标志位) |        0        |
  |   JP   | PF(奇偶标志位) |        1        |
  |  JNP  | PF(奇偶标志位) |        0        |


  + 依据相等比较的跳转指令
    + 指令格式:
      + CMP LeftOp, RightOp
      + Jxx Label

  | 助记符 |        跳转条件        |
  | :----: | :--------------------: |
  |   JE   |    LeftOp = RightOp    |
  |  JNE  | LeftOp$\neq$ RightOp |
  |  JCXZ  |          CX=0          |
  | JECXZ |         ECX=0         |

  + 基于无符号整数比较结果的跳转指令
    + 指令格式:
      + CMP LeftOp, RightOp
      + Jxx Label
    + 指令助记: Z:zero, E:equal, A:above, B:below

  | 助记符 |     跳转条件     |
  | :----: | :---------------: |
  |   JA   | LeftOp > RightOp |
  |  JAE  | LeftOp >= RightOp |
  |   JB   | LeftOp < RightOp |
  |  JBE  | LeftOp <= RightOp |
  |  JNA  | LeftOp <= RightOp |
  |  JNAE  | LeftOp <= RightOp |
  |  JNB  | LeftOp >= RightOp |
  |  JNBE  | LeftOp >= RightOp |

  + 基于有符号整数比较结果的跳转指令
    + 指令格式:
      + CMP LeftOp, RightOp
      + Jxx Label
    + 指令助记: Z:zero, E:equal, G:greater, L:less

  | 助记符 |     跳转条件     |
  | :----: | :---------------: |
  |   JG   | LeftOp > RightOp |
  |  JGE  | LeftOp >= RightOp |
  |   JL   | LeftOp < RightOp |
  |  JLE  | LeftOp <= RightOp |
  |  JNG  | LeftOp <= RightOp |
  |  JNGE  | LeftOp <= RightOp |
  |  JNL  | LeftOp >= RightOp |
  |  JNLE  | LeftOp >= RightOp |

  {% message color:info size:default%}

  ```x86asm
  MOV al, status
  TEST al, 00100000b   ; 测试 bit5=1
  jnz OK
  ```

  ```x86asm
  MOV al, status
  TEST al, 00010011b   ; 测试 bit0, bit1, bit4至少一个1
  jnz OK
  ```

  ```x86asm
  MOV al, status
  AND al, 10001100b   ; 取出 bit2, bit3, bit7
  CMP al, 10001100b   ; 测试 bit2, bit3, bit7不全为 1
  jnz OK
  ```

  {% endmessage %}
+ 循环指令

  + LOOP
    + LOOP Label
      + 执行操作: (CX)$-1$ $\rightarrow$ CX
      + 循环条件: (CX) $\neq0$ 转至 Label 处循环执行
      + 等价指令: DEC CX, JNZ Label
      + 循环目的地址与当前地址相距范围：$-128\sim+127$字节，机器指令平均 $3$ 字节, 单词循环平均最多包含约 $42$ 条指令
        {% message color:info title: 循环嵌套 size:default%}

    ```x86asm
    .data
    count DW ?
    .code
        mov cx, 100
    L1: mov count, cx
        mov cx, 20
    L2: .
        .
        loop L2
        mov cx, count
        loop L1
    ```

    {% endmessage %}
  + LOOPZ (Loop if zero, Loop if equal)
    + 指令等价：LOOPE
    + 指令格式: LOOPZ Label
    + 执行操作: (CX) $-1\rightarrow$ CX
    + 循环条件: (CX) $\neq0 \wedge$ ZF $=1$
  + LOOPNZ (Loop if not zero, Loop if not equal)
    + 指令等价：LOOPNE
    + 指令格式: LOOPNZ Label
    + 执行操作: (CX) $-1\rightarrow$ CX
    + 循环条件: (CX) $\neq0 \wedge$ ZF $=0$
+ 移位和循环移位指令

  | 助记符 | 指令含义 |   影响标志位   | 助记符 |     指令含义     | 影响标志位 | 助记符 |  指令含义  | 影响标志位 |
  | :----: | :------: | :------------: | :----: | :--------------: | :--------: | :----: | :--------: | :--------: |
  |  SHL  | 逻辑左移 | CF,OF,PF,SF,ZF |  ROL  |     循环左移     |   CF,OF   |  SHLD  | 双精度左移 |   CF,OF   |
  |  SHR  | 逻辑右移 | CF,OF,PF,SF,ZF |  ROR  |     循环右移     |   CF,OF   |  SHRD  | 双精度右移 |   CF,OF   |
  |  SAL  | 算术左移 | CF,OF,PF,SF,ZF |  RCL  | 带进位的循环左移 |   CF,OF   |        |            |            |
  |  SAR  | 算术右移 | CF,OF,PF,SF,ZF |  RCR  | 带进位的循环右移 |   CF,OF   |        |            |            |

  {% message color:info size:default%}


  + 算术移位——把操作数看做是有符号数
  + 逻辑移位——把操作数看做无符号数
  + $8086$/$8088$: imm8=1, $80286$ 以上: imm8=任意整数
    {% endmessage %}
  + SHL/SAL
    {% message color:info size:default%}
  + 指令格式: SHL/SAL mem/reg, imm8/CL
  + 指令功能: 逻辑左移/算术左移
  + Example

  ```x86asm
  SAL al, 1    ; 2x
  MOV ah, al
  SAL al, 1    ; 4x
  SAL al, 1    ; 8x
  ADD al, ah
  ```

  {% endmessage %}

  + ROL
    {% message color:info size:default%}
  + 指令格式: ROL mem/reg, imm8/CL
  + 指令功能: 循环左移
  + 指令特点: 循环左移出的最高位同时给 CF
  + Example

  ```x86asm
  MOV al, 10000000b
  ROL al, 1       ; 10000000b -> 00000001b, CF = 1
  ROL al, 1      ; 00000001b -> 00000010b, CF = 0
  ```

  ```x86asm
  MOV al, 26H
  MOV cl, 4
  ROL al, cl      ; AL=62H
  ```

  {% endmessage %}

  + ROR
    {% message color:info size:default%}
  + 指令格式: ROR mem/reg, imm8/CL
  + 指令功能: 循环右移
  + 指令特点: 循环右移出的最低位同时给 CF
  + Example

  ```x86asm
  MOV al, 00000001b
  ROR al, 1       ; 00000001b -> 10000000b, CF = 1
  ROR al, 1      ; 10000000b -> 01000000b, CF = 0
  ```

  ```x86asm
  MOV al, 26H
  MOV cl, 4
  ROL al, cl      ; AL=62H
  ```

  ```x86asm
  .data
      ArraySize = 3
      array DWORD ArraySize DUP(99999999H)
  .code 
      MOV esi, 0
      SHR array[esi+8], 1        ; 高位的最低位移进 CF
      RCR array[esi+4], 1        ; 低位移位必须带 CF
      RCR array[esi], 1
  ```

  {% endmessage %}

  + SHLD
    + 至少是Intel $386$处理器
    + 指令格式: SHLD DEST, SRC, CL/imm8
    + 功能: 将目的操作数左移指定的位数，低位空出来的位用源操作数的高位填充
    + 影响标志: SF, ZF, AF, PF, CF
  + SHRD
    + 至少是Intel $386$处理器
    + 指令格式: SHRD DEST, SRC, CL/imm8
    + 功能: 将目的操作数右移指定的位数，高位空出来的位用源操作数的低位填充
    + 影响标志: SF, ZF, AF, PF, CF

#### 字符串操作指令

+ 寻址方式

  + 源操作数指针 DS:SI
  + 目的操作数指针 ES:DI
  + SI是DS段中的偏移
  + DI是ES段中的偏移
  + ES ES通常开始时设为同样的段值
  + SI DI的值会自动修改（方向标志位DF=0 增；DF=1 减；CLD 清除方向标志位，STD 设置方向标志位）
+ 基本字符串指令

  + MOVSB(/SW/SD)移动
    + 拷贝DS:(E)SI寻址的内存操作数至ES:(E)DI
    + 例题
      ![mov](https://cdn.jsdelivr.net/gh/15645jngfkj/picx-images-hosting@master/Screenshot_20231011_113542_com.huawei.hinote.258ji2q263ls.webp)
  + CMPSB(/SW/SD)比较
    + SCASB(/SW/SD)扫描比较内存中由DS:(E)SI寻址和ES:(E)DI寻址的字符串。源﹣目的
    + 例题
      ![cmp](https://cdn.jsdelivr.net/gh/15645jngfkj/picx-images-hosting@master/Screenshot_20231011_113951_com.huawei.hinote.718lo3pipyo0.webp)
  + SCASB(/SW/SD)扫描
    + 扫描ES:(E)DI指向的内存字符串查找与累加器匹配的值
    + 例题
      ![sca](https://cdn.jsdelivr.net/gh/15645jngfkj/picx-images-hosting@master/Screenshot_20231011_114544_com.huawei.hinote.6hftk0t14t40.webp)
  + STOSB(/SW/SD)存储
    + 将累加器内容存储到由ES:(E)DI寻址的内存中
    + 例题
      ![sto](https://cdn.jsdelivr.net/gh/15645jngfkj/picx-images-hosting@master/Screenshot_20231011_215922_com.huawei.hinote.384b0l57qmy0.webp)
  + LODSB(/SW/SD)装入
    + 将由DS:(E)SI寻址的内存单元装入累加器中
    + 例题
      ![lod](https://cdn.jsdelivr.net/gh/15645jngfkj/picx-images-hosting@master/Screenshot_20231011_220104_com.huawei.hinote.71vqoaszln80.webp)
+ 使用重复前缀

  + REP
    + 当CX>0时重复
  + REPZ, REPE
    + 当ZF=1且CX>0时重复
  + REONZ, REPNE
    + 当ZF=0且CX>0时重复
  + 实现用一条指令处理整个数组

#### 端口、过程与逻辑运算指令

+ 使用I/O端口控制硬件
  + 端口范围：0~FFFFh
  + 端口作用：传送数据，返回状态，控制
  + 指令
    + IN 累加器，端口地址
      + 累加器：AL\AX\EAX
      + 端口地址
        + 0~FFh之间的常量
        + 包含0~FFFFh 之间值的DX寄存器
    + OUT 端口地址，累加器
    + 例题
      ![i/o](https://cdn.jsdelivr.net/gh/15645jngfkj/picx-images-hosting@master/MTXX_PT20231011_220357764.6qh8sx7bpf40.webp)
+ 过程的定义和使用
  + PROC伪指令
  + CALL与RET指令
    + CALL指令执行时，处理器自动完成压栈；RET指令执行时，处理器自动完成出栈
    + 过程可以嵌套调用
    + 局部标号(L1:)和全局标号(L1::)
    + 例题
      ![过程](https://cdn.jsdelivr.net/gh/15645jngfkj/picx-images-hosting@master/Screenshot_20231011_220708_com.huawei.hinote.3s2zt88q8aw0.webp)
+ 逻辑运算指令
  + 设置和清除单个CPU标志

    ```x86asm
    stc ;设置进位标志
    clc ;清除进位标志
    ```

    ```x86asm
    and al，0 ;设置零标志
    or al,1 ;清除零标志
    ```

    ```x86asm
    or al,80h ;设置符号标志
    and al,7Fh ;清除符号标志
    ```

    ```x86asm
    mov al,7Fh ;AL=+127
    inc al ;AL=-128, OF=1
    or eax,0 ;清除溢出标志
    ```

#### 程序设计举例

+ 顺序程序
  + 例题
    ![顺序程序](https://cdn.jsdelivr.net/gh/15645jngfkj/picx-images-hosting@master/Screenshot_20231011_220742_com.huawei.hinote.130wfurk9v1c.webp)
+ 分支程序
  + 例题1
    ![1](https://cdn.jsdelivr.net/gh/15645jngfkj/picx-images-hosting@master/Screenshot_20231011_220815_com.huawei.hinote.2b76yggvndz4.webp)
  + 例题2
    ![2](https://cdn.jsdelivr.net/gh/15645jngfkj/picx-images-hosting@master/Screenshot_20231011_220846_com.huawei.hinote.3oxya0x1946.webp)
+ 循环程序
  + 两种结构
    ![结构](https://cdn.jsdelivr.net/gh/15645jngfkj/picx-images-hosting@master/Screenshot_20231012_200347_com.huawei.hinote.5u9c6sai7hc0.webp)
    两种结构循环次数CX的设置会不同
  + 例题
    ![循环程序](https://cdn.jsdelivr.net/gh/15645jngfkj/picx-images-hosting@master/Screenshot_20231011_220924_com.huawei.hinote.73wi7lxs69g0.webp)
+ 子程序
  + 编写时应注意
    + 如何调用和返回
    + 入口条件和出口条件
    + 寄存器、保护、影响哪些标志位、出错如何处理
  + 参数传递
    + 利用寄存器
    + 利用内存单元
    + 利用堆栈
  + 例题
    ![子程序](https://cdn.jsdelivr.net/gh/15645jngfkj/picx-images-hosting@master/Screenshot_20231012_210721_com.huawei.hinote.3ew3pc4aprm0.webp)
+ 功能调用
  + 高级功能调用（DOS功能调用）

    ```x86asm
    MOV AH,功能号
    对各寄存器调用参数
    INT 21H
    ```
  + 低级功能调用（BIOS功能调用)

    ```x86asm
    MOV AH,功能类型
    对各寄存器调用参数
    INT 中断类型
    ```
  + 例题

    + 例1
      ![1](https://cdn.jsdelivr.net/gh/15645jngfkj/picx-images-hosting@master/Screenshot_20231012_205528_com.huawei.hinote.4fuqu327bca0.webp)
    + 例2
      ![2](https://cdn.jsdelivr.net/gh/15645jngfkj/picx-images-hosting@master/Screenshot_20231012_205536_com.huawei.hinote.1n57a88zsco0.webp)
    + 例3
      ![3](https://cdn.jsdelivr.net/gh/15645jngfkj/picx-images-hosting@master/Screenshot_20231012_205546_com.huawei.hinote.6kx5yq2pi6g0.webp)
    + 例4
      ![4](https://cdn.jsdelivr.net/gh/15645jngfkj/picx-images-hosting@master/Screenshot_20231012_205604_com.huawei.hinote.6uz4aoxyfio0.webp)
    + 例5
      ![5](https://cdn.jsdelivr.net/gh/15645jngfkj/picx-images-hosting@master/MTXX_PT20231012_205725903.2eqpckcazllw.webp)

#### Examples

```x86asm
TITLE Summing an Array
; This program sums an array of 16-bit integers.
.data
intarray DW 100H, 200H, 300H, 400H
.code
main PROC
    MOV di, OFFSET intarray
    MOV cx, LENGTHOF intarray
    MOV ax, 0           ; clear the accumulator
L1:
    ADD ax, [di]
    ADD di, TYPE intarray
    LOOP L1
main ENDP
END main
```

```x86asm
TITLE Copying a String
; This program copies a string.
.data
source DB "This is the source string.", 0     ; 字符串末尾加上结束符便于输出，'\0'的 ASICC 码值为 0
target DB SIZEOF source DUP(0), 0
.code
main PROC
    MOV si, 0
    MOV cx, SIZEOF source
L1:
    MOV al, source[si]
    MOV target[si], al
    LOOP L1
main ENDP
END main
```

```x86asm
TITLE Displaying Register Contents
; This program displays the contents of the general-purpose registers.
.code
main PROC
    MOV bx, 1234H
    MOV ch, 4        ; 循环计数器
ROT:
    MOV cl, 4        ; 移位计数器
    ROL bx, cl       ; 循环移位，将高4位移到低4位
    MOV al, bl
    AND al, 0FH      ; 取低4位
    ADD al, 30H      ; 将数字转换为ASCII码
    CMP al, 39H
    JBE DISP         ; 如果不大于 9 就显示
    ADD al, 7        ; 如果大于 9 则转 A-F
DISP:
    MOV dl, al
    MOV ah, 2        ; 显示
    INT 21H
    DEC CH           ; 计数4个十六进制数
    JNZ ROT

    MOV dl, 48H     ; 显示 H
    MOV ah, 2
    INT 21H
main ENDP
END main
```

```x86asm
TITLE Scanning an Array for Non-Zero Values
; This program scans an array for non-zero values.
.data
    intArray SWORD 0, 0, 0, 0, 1, 20, 35     ; single word
    nonMsg DB "A non-zero value was not found.", 0
.code
main PROC
    MOV ebx, OFFSET intArray
    MOV ecx, SIZEOF intArray    ; Loop counter
L1:
    CMP WORD PTR[ebx], 0
    JNZ Found                  ; Found a value
    ADD ebx, 2
    Loop L1
    JMP NotFound
Found:
    MOVSX eax, WORD PTR[ebx]
    call Writeint             ; Display the value in EAX
    JMP Quit
NotFound:
    MOV edx, OFFSET nonMsg
    call WriteString
Quit:
    call crlf               ; Carriage Return and linefeed
    exit
main ENDP
END main
```

```x86asm
TITLE Scanning an Array
; This program scans an array until it finds a positive value.

.data
    intArray SWORD -3, -6, -1, 0, 1, 20, 35     ; single word
    sentinel SWORD 0
.code
main PROC
    MOV esi, OFFSET intArray
    MOV ecx, LENGTHOF intArray    ; Loop counter
Next:
    TEST WORD PTR[esi], 80H        ; Testing the highest bit
    PUSHFD                     ; Pushing flags on stack
    ADD esi, TPYE intArray
    POPFD                      ; Poping flags from stack
    LOOPNZ Next                ; if not found
    JNZ Quit                   ; finished scanning and not zero that is not found
    SUB esi, TYPE intArray     ; moving to the former element

Quit:
    call crlf               ; Carriage Return and linefeed
    exit
main ENDP
END main
```

```x86asm
TITLE Compress BCD to ASCII
; This program converts a BCD number to ASCII.
.code
main PROC
    MOV si, 1000H         ; SI <- BCD 首地址
    MOV di, 2000H         ; DI <- ASCII 首地址
    MOV BX, 4             ; 4 个 BCD 码
L1:
    MOV al, [si]          ; 取 BCD 码
    AND al, 0FH           ; 屏蔽高 4 位
    OR al, 30H            ; 转换为 ASCII
    STOSB                 ; 将AL寄存器中的值存储到DI地址指向的内存单元
    LODSB                 ; SI指向的存储单元读入读入AL
    MOV cl, 4
    SHR al, cl            ; 逻辑右移 4 位
    OR al, 30H            ; 得到高 4 位 ASCII 码
    STOSB
    INC si
    DEC bx
    JNZ L1              

Quit:
    call crlf               ; Carriage Return and linefeed
    exit
main ENDP
END main
```

## 第四章 总线技术

### 第一讲 总线概述

{% message color:info %}
定义: 连接两个以上数字系统元件的公共的信息通路
{% endmessage %}

#### 总线分类

{% message color:info title:按连接的层次 %}
片内总线：连接CPU内部各功能部件的总线，例如内部的运算器、寄存器等
元件级总线：连接CPU、内存以及总线控制逻辑的板内总线
系统总线(内总线)：主机内用于连接主板、网卡、显卡等高速功能部件的总线
通信总线(外总线)：连接主机与主机、主机与外设之间的总线
{% endmessage %}

{% message color:info title:按数据传输的位数 %}
I. 并行总线

+ 多条数据线对数据各位进行同时传输
+ 仅适宜计算机内部高速部件近距离传输
+ 有时钟偏移和串扰

II. 串行总线

+ 采用一条数据线逐位传输各位数据
+ 常用于长距离通信及计算机网络，在短距离应用中性能也超过并行总线
+ 无时钟偏移和串扰
  {% endmessage %}

#### 内总线

+ PC机的内总线

  + ISA总线(Industry Standard Architecture)
    {% message color:info %}
    I. 特点
  + 16位数据总线，支持 8 位、16 位数据操作
  + 地址、数据非多路复用
  + 多主控设备总线

  II. 信号定义

  + 数据总线(System Data Bus)

    + 16位 $SD_0$ - $SD_{15}$, 8位 $SD_0$ - $SD_7$
    + 提速: 同步准备就绪信号 $\overline{SRDY}$(Synchronous Ready)又零等待状态信号 $\overline{NOWS}$(No Wait State)，表示无需额外等待即可完成一个总线周期
    + 升位: 片选信号$\overline{MEMCS16}$(Memory Chip Select 16), $\overline{IOCS16}$(I/O Chip Select 16)，指示进行16位数据操作
    + 位数可选: 系统高字节允许信号 $\overline{SBHE}$(System Byte High Enable)
      <img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Courses/image.7acz9amyug00.webp" alt="数据操作选择策略" width="70%"/>

    注: 当 $\overline{SBHE}$ 被**主控设备**置为低电平，ISA插卡必须及时将 $\overline{MEMCS16}$ 和 $\overline{IOCS16}$ 置为有效作为回应(操作16位数据)
  + 地址总线(System Address Bus)

    + 主存地址空间: $SA_0\sim SA_{19}$ 寻址 $1$MB，配合 $LA_{17}\sim LA_{23}$ 寻址能力达到16M = $2^{24}$
    + I/O地址空间: $SA_0\sim SA_{15}$ 寻址 $64$K，实际寻址 $1$K
    + 未锁定地址信号 $LA_{17}\sim LA_{23}$(Unlatched Address)
      + 不锁存时: $LA_{17}\sim LA_{19}$ (不锁存)与 $SA_{17}$ - $SA_{19}$ (锁存)重复
      + 索存时: $LA_{17}\sim LA_{23}$ 扩展地址总线
  + 中断请求

    + 11个：$IRQ_3$ - $IRQ_7$、$IRQ_9$ - $IRQ_{12}$、$IRQ_{14}$ - $IRQ_{15}$
    + $IRQ_0$ 定时器，$IRQ_1$ 键盘，$IRQ_2$ 级联，$IRQ_8$ 定时器8254，$IRQ_{13}$ 协处理器
  + DMA请求

    + 7个 $DRQ_0$ - $DRQ_3$ 、 $DRQ_5$ - $DRQ_7$、 $\overline{DACK_0}$ - $\overline{DACK_3}$ 、 $\overline{DACK_5}$ - $\overline{DACK_7}$
    + 优先级：$DRQ_0$  - $DRQ_7$递减
    + $DRQ_0$ - $DRQ_3$ （8位传输）， $DRQ_5$ - $DRQ_7$（16位传输）
  + 多主控制总线

    + $\overline{MASTER}$ 系统控制权信号
  + 速度

    + CLK
      {% endmessage %}
  + PCI总线(Peripheral Component Interconnect Local Bus)
    {% message color:info %}
    I. 特点
  + 不依赖处理器（PCI桥）
  + 扩充性好（多PCI总线结构）
  + 自动配置，即插即用
  + 数据、地址奇偶校验功能
  + 数据宽度32位，可扩展为64位
  + 信号复用，支持无限读写突发操作
  + 适应性广
  + 并行总线操作
    II. 总线命令

    | 总线命令 | 命令类型 |
    | :------: | :------: |
    |   0000   | 中断应答 |
    |   0001   | 特殊周期 |
    |   0010   |  I/O读  |
    |   0011   |  I/O写  |
    |   0100   |  MEM读  |
    |   0100   |  MEM写  |
    |   0100   |  读配置  |
    |   0100   |  写配置  |

  III. 突发成组数据传输：一个分组= 一个地址节拍 + 一个/多个数据节拍
  {% endmessage %}
+ 工控机的内总线

  + STD总线

#### 外总线

+ RS232C
  {% message color:info %}
  I. 特点
+ 传输信号线少
+ 传输距离较远
+ 采用不归零编码NRZ和负逻辑
+ 单端通信
+ 传输速率较低

II. 电气特性、引脚功能

+ DB25和DB9，相同功能针号不同
+ 信号
  + 传送信息信号：TXD RXD（逻辑1电压为负）
  + 联络信号：RTS CTSS DTR DSR DCD RI（逻辑1电压为正）
  + 全双工和半双工
  + 与TTL电平转换
+ 应用
  + 使用modem连接
  + 软硬件系统调试
  + 直接连接
    + 交叉连接方式（全双工）
    + 三线连接方式（软件无需检测CTS、DSR的状态）
+ RS423、RS422
  + RS423：单端输出、差分接收
  + RS422：差分输出、差分接收
    {% endmessage %}
+ SCSI
  {% message color:info %}
+ 使用逻辑地址而非物理地址寻址数据
+ 特点：
  + 适用范围广
  + 传输速率高
  + 提高了CPU效率，CPU占有率低
  + 支持多任务
  + 智能化
+ SCSI-1和SCSI-2
  + 通用接口，设备无关
  + 主机适配器 + 外设控制器 ≤ 8
  + 两种工作方式
    + 同步数据传输
    + 异步数据传输（速率低于同步）
  + SCSI总线上的设备无主从之分，任何设备可做启动设备也可做目标设备
  + 驱动方式（三种方式不能共存）
    + 单端
    + 差分
    + LVD
+ 总线信号
  + $DB_0$ - $DB_7$,DBP:命令、数据、状态、信息、SCSI-ID
  + REQ 、 ACK：握手信号
  + C/D   I/O  SEL  MGS
  + ATN  BSY  RST
+ 工作过程
  + 启动设备选择一个目标设备，发送一条命令
  + 目标设备被选中并接受命令（获得总线控制权），命令传到LUN（物理外设）去执行
  + 目标设备释放总线
    {% endmessage %}
+ USB总线
  {% message color:info title:特点 %}
+ 单一接口类型
+ 127个外设
+ 整个USB系统只用一个端口、一个中断，节省系统资源
+ 热插拔
+ 高速、全速、低速
+ 设备供电
+ 控制传输、同步传输、中断传输、批量传输
+ 构成
+ 硬件
  + USB主机
    + USB主集线器
    + 根集线器
    + USB设备
      + 集线器
      + 功能部件
  + 软件
    + USB设备驱动程序
    + USB驱动程序
  + USB主控制器驱动程序
    {% endmessage %}

### 第二讲 总线的驱动与控制

#### 总线竞争

+ 同一总线上，同一时刻，有两个或以上的器件输出状态
+ 防止总线竞争：用三态电路，严格控制逻辑

#### 总线负载

+ 直流负载
  + 驱动器的高电平输出电流应不小于所有负载所需高电平输入电流之和
  + 驱动器的低电平输出电流应不小于所有负载所需低电平输入电流之和
  + 扇出数：驱动同门的个数，用 $I_{OH}$ / $I_{IH}$ 和 $I_{OL}$ / $I_{IL}$，取二者的较小值
+ 交流负载
  + 对MOS电路，主要考虑电容负载
  + 扇出数：输出门的负载电容 $C_p$ / $C_{li}$
+ 总扇出数：$I_{OH}$ / $I_{IH}$  、 $I_{OL}$ / $I_{IL}$ 、 $C_p$ / $C_{li}$，三者取最小值（理想情况）

#### 总线驱动设计

+ 克服总线负载效应：用驱动器和缓冲器
  + 扇出能力大
  + 延时可忽略
  + 噪声容限较高
+ 几种常用芯片（防止总线竞争）
  + 单向驱动器（三态输出）（224）
  + 双向驱动器（三态输出）（245）
    + DIR = 0，读；DIR = 1，写
  + 锁存器（三态输出）（373）
+ 总线驱动设计
  + 内存板：20位地址（$\overline{MEMR}$ 、 $\overline{MEMW}$），接口板：16位地址（$\overline{IOR}$ 、 $\overline{IOW}$）
  + 防止总线竞争的原则：只有当CPU读本电路板内的内存地址/接口地址时，才允许双向驱动器指向系统总线的三态门是导通的
  + 步骤
    + 分析板内内存地址，找出地址特征
    + 设置译码电路，用来控制双向数据总线驱动器，使之满足防止总线竞争的原则
  + 译码方式
    + 基本门电路
    + 译码器（74LS138）
    + 译码ROM
    + 比较器（74LS688）
    + PLA  CPLD  FPGA

### 第三讲 总线的工程设计问题

+ 设计总线要考虑
  + 不发生总线竞争
  + 总线负载
  + 总线交叉串扰
  + 总线延时
  + 总线信号的反射
+ 总线交叉串扰
  + 产生原因
    + 总线间的寄生电容
    + 总线本身可看做一个小电感
  + 解决：减少总线间的寄生电容
    + 减少总线长度
    + 增加总线间距离
    + 降低总线上的负载
    + 两条信号线间加一条地线
    + 减少总线的平行走向
    + 总线优化器DS36662
    + 采用双绞线
+ 总线延时
  + 解决方法
    + 减少总线长度
    + 选用延时小、输入输出电容小、驱动能力强的元器件
+ 总线信号的反射
  + 产生原因：
    + 信号沿总线传播到达总线终端时，若总线终端负载阻抗与总线特性阻抗不匹配，信号的一部分会被反射
    + 反射回来的信号到达信号源时，若源的内部阻抗与总线的特性阻抗不匹配，又会有一部分被反射回去
    + 此过程有时需要多次才能在负载上建立所需的波形
  + 危害
    + 反射使波形变坏、延时增加
  + 克服方法
    + 降低传输信号的频率
    + 尽量使  信号源内阻、总线的特性阻抗、负载阻抗  相匹配
      + 总线匹配
        + 末端匹配
        + 源端匹配
    + 限制总线长度

{% message color:info title:来自作者的忠告 %}
从本章开始，默认读者已经掌握大部分微机原理与接口技术的基础内容，后续着重关注于较难的问题或者不易记忆的模块，此部分基于作者本身的直觉认为应当是计算机技术专业学生必须牢记的；由于作者仍在深入学习，知识储存量不能做到非常全面，所以如果您觉得部分内容有冗余，作者建议您可以选择直接略过。
{% endmessage %}

## 第五章 存储技术

### 第一讲 概述

#### 半导体存储器的基本概念

+ RAM: SRAM [异步 SRAM, 同步 SRAM], DRAM
+ ROM
  + 可一次编程 ROM: PROM(Programmable ROM);
  + 可擦写的 PROM: [EPROM(Erasable Programmable ROM),
    E2PROM/EEPROM(Electrically Erasable Programmable ROM)[传统 E2PROM, FLASH]]
+ 三级存储结构: 高速缓冲存储器、主存储器、辅助存储器

### 第二讲 常用存储器芯片及接口设计

{% message color:info title:来自作者的忠告 %}
本讲重点关注于常用存储器芯片的特性以及接口设计，在主存接口设计中必须牢记常用存储器芯片特点
{% endmessage %}

#### SRAM 及接口设计

+ 异步 SRAM: 访存独立于时钟，控制信号不需要时钟同步
  {% message color:info title:典型传统异步性SRAM芯片--6264芯片%}
+ 数据线: $D0\sim D7$ (8位存储器)
+ 地址线: $A0\sim A12\Longrightarrow 2^{13}\times 8=8K\times 8$ (存储容量)
+ 片选信号: $\overline{CS1}, CS2$
+ 使能信号: $\overline{OE}$ (输出使能), $\overline{WE}$ (写使能)
+ 时序分析

  + 写入时序
    地址 $\rightarrow$ 片选 $\rightarrow$ 数据 $\rightarrow$ 写信号 $\rightarrow\cdots\rightarrow$ 撤写信号 $\rightarrow$ 撤其他信号
  + 读出时序
    地址 $\rightarrow$ 片选 $\rightarrow$ 读信号 $\rightarrow$ 数据有效 $\rightarrow$ 撤读信号 $\rightarrow$ 撤其他信号
  + 逻辑分析
    传送地址、加载片选信号以及加载读写信号顺序保持一定，主要是使得存储器读写信号的充分有效性
+ 译码电路

  + 译码方式
    + 全地址译码: 全部的高位地址信号(除了存储单元地址以及片内地址)作为译码信号，编码存储器芯片的所有存储单元
    + 部分地址译码/部分高位地址: 部分高位地址信号进行译码，存储器芯片占据几组不同的地址范围
  + 译码电路的选择
    + 利用译码芯片(74LS139(2-4译码器), 74LS138(3-8 译码器), 74LS154(4-16 译码器))、门电路
    + 利用数字比较器芯片 74LS688
    + 利用 PROM 译码器
+ 芯片结构
  <img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Courses/image.4g94d2vgnri0.webp" alt="6264存储器($8K\times 8$)" width="10%" />
  <img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Courses/image.4yf7tzkp0240.webp" alt="6116存储器($2K\times 8$)" width="10%" />
  <img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Courses/image.3uqp9jo7sdk0.webp" alt="74LS138" width="10%" />
  <img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Courses/image.3ewhvsxrxuc0.webp" alt="63S241 ROM" width="10%" />
  <img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Courses/image.5lz6tpeh0e80.webp" alt="74LS688 数字比较器" width="30%" />
  {% endmessage %}
+ 同步 SRAM: 访存依赖于时钟，控制信号需要时钟同步

### 第三讲 Intel16/32/64 位微机系统的主存设计

+ 8088 系统存储器: 8 位数据总线，单体存储器
+ 8086/186/286 系统: 16 位数据总线，双体存储器
<img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Courses/image.4ehbe7yyl2g0.webp" alt="存储器结构" width="50%"/>
+ 80386/80486 系统: 32 位数据总线，四体存储器
+ Intel 16位微机系统的主存设计
  + 存储器的字、位扩展
  + 芯片数量计算
    + 存储容量 = (尾地址 - 首地址 + 1)$\times$ 位宽
    + 芯片数量 = 存储容量/芯片容量
+ Intel 32位微机系统的主存设计
<img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Courses/image.eyf1lz5jq8g.png" alt="接口信号" width="50%"/>

| $M/\overline{IO}$ | $D/\overline{C}$ | $W/\overline{R}$ |   总线周期   |
| :-----------------: | :----------------: | :----------------: | :----------: |
|          0          |         0         |         0         |   中断响应   |
|          0          |         0         |         1         |     停机     |
|          0          |         1         |         0         |    I/O读    |
|          0          |         1         |         1         |    I/O写    |
|          1          |         0         |         0         | 取指令操作码 |
|          1          |         0         |         1         |     保留     |
|          1          |         1         |         0         |   存储器读   |
|          1          |         1         |         1         |   存储器写   |

+ Intel 64位微机系统的主存设计
  + 体选择信号: $\overline{BE0}$ ~ $\overline{BE7}$
  + 内存由 8 个体构成，每个体对应一个体选择信号

### 第四讲 只读存储器 (ROM) 及接口设计

+ 外存平均访问时间 ms 级; 内存平均访问时间 ns 级
  {% message color:info title:EPROM--2764芯片%}
+ 地址总线: $A12\sim A0$ (8K$\times$ 8bit)
+ 数据总线: $D7\sim D0$
+ 片选信号: $\overline{CE}$
+ 输出使能信号: $\overline{OE}$
+ PGM: 编程时脉冲输入，读时为 "1"
+ 芯片结构
<img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Courses/image.ahq35h8dhzs.webp" alt="EPROM 2764" width="10%"/>
  {% endmessage %}
  {% message color:info title:EEPROM--98C64A芯片%}
+ 地址总线: $A12\sim A0$ (8K$\times$ 8bit 并行)
+ 数据总线: $D7\sim D0$
+ 片选信号: $\overline{CE}$
+ 输出使能信号: $\overline{OE}$
+ 写入使能信号: $\overline{WE}$
+ $Ready/\overline{Busy}$: 漏极开路
+ 芯片结构
<img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Courses/image.xp6whcyx44w.webp" alt="EEPROM 98C64A" width="10%"/>
  {% endmessage %}
+ EPROM

  + 需要进行擦除
+ EEPROM

  + 可单字节随机读写 (不需擦除，直接改写数据)
  + 存储密度小，单位成本高


## 第六章 输入/输出技术

### 第一讲 I/O概述

#### 主机和外设

#### I/O 方式

+ 程序控制方式
  + 无条件传送方式
  + 查询方式          不要求CPU效率  外设慢速
  + 中断方式          要求CPU效率    外设中慢速
+ DMA                                   外设高速
  从无条件传送方式到DMA，控制越来越复杂，效率越来越高

#### I/O 接口

+ 作用
  + 信息传递（利用端口——接口里的寄存器）
    + 注意区分“接口”“端口”
  + 数据格式转换
  + CPU与外设速度匹配
  + 负载匹配、时序匹配
  + 总线隔离
  + 提供中断、DMA功能
+ 信息传递

+ 编址方式
  + 统一编制
  + 独立编制（有IN OUT）

#### I/O 端口地址译码

+ 全地址译码
+ 部分地址译码

#### 基本的并行输入/输出接口

+ 输入：三态门（防止总线冲突）
+ 输出：锁存器（CPU和外设速度匹配）

### 第二讲 程序查询 I/O 方式

+ 无条件传送方式（查询方式的特例）
  + 外设时刻处于就绪状态
  + 硬件：数据端口，软件：输入输出指令
+ 查询方式
  + 外设准备就绪之后才能与微机系统进行信息交换
  + 硬件：数据端口、状态端口，软件：不断查询 ；硬件简单，软件开销大
  + 时序
  + 多外设的查询控制
    + 一定要服务优先级高的——用于优先级差别很大的

    + 服务完高优先级的再从头查询——用于优先级有一定差距的

    + 机会均等——用于优先级差别很小的


### 第三讲 中断方式

#### 中断概述

+ 中断源
  + 内部：内中断（软件中断）
  + 外部：外中断（硬件中断）
    + NMI（不可屏蔽）
    + INTR（可屏蔽）
+ 中断过程
  1. 中断源发出中断请求
  2. 满足中断条件，进行中断响应
     + 断点保护（硬件完成）
       + PSW压栈，关中断，CS压栈，IP压栈
     + 中断判优
       + 硬件判优
       + 软件判优
     + 中断源识别
       + 软件查询
       + 中断矢量法
     + 获得中断服务子程序首地址
       + 固定入口法
       + 中断向量法
  3. 中断处理——中断服务子程序
     + FAR类型，用IRET返回
     + 保护现场 → 开中断STI → 中断处理 → 关中断CLI → 恢复现场 → 中断返回IRET
     + RISC寄存器分页，中断服务程序无需PUSH  POP
  4. 中断返回 IRET
     + IRET指令使CPU把堆栈内保存的断电信息弹出到IP、CS、FLAG中
+ 中断优先级嵌套
  + 优先级原则
    + 速度快＞速度慢
    + 输入设备＞输出设备
  + 解决办法
    + 软件查询
    + 硬件链式优先级排队电路
    + 硬件优先级编码比较电路
    + 利用可编中断控制器PIC
  + 实现中需注意
    + 中断处理程序：STI开中断指令
    + 堆栈足够大
    + 正确使用堆栈（关中断之后恢复现场）

#### Intel 16位中断系统

##### 8086/8088中断系统

+ 中断源类型
+ 中断向量表IVT
  + 存放中断服务程序的入口地址
  + 00000H ~ 003FFH，1KB = 4B/入口 × 256个入口
  + 中断向量在IVT中的存放地址 = 4 × 中断类型号n
    + 4n : IP
    + 4n+2 : CS
+ 中断的响应过程
  + 注意，外部中断INTA有两次
  + 优先级从高到低：内部中断、NMI 、INTR、单步中断

##### 可编程中断控制器（PIC）8259

+ 内部结构
  + IRR     请求（=1表示有请求）
  + ISR     服务（=1表示正被服务）
  + IMR     屏蔽（=1表示被屏蔽）
  + 中断优先权判别电路
+ 引脚
+ 工作方式

+ 级联
  + 单片8259A可支持8个中断源，多片级联最多支持64个中断源，n片可支持7n+1个中断源（n≤9）
  + $\overline{\text{SP}}$ / $\overline{\text{EN}}$ :低-从片，高-主片
  + 从片中断结束，中断服务程序需发送两个EOI命令
+ 编程使用
  + 内部寄存器的寻址方法

  + 命令字

  
##### 中断方式实现方法
1. 8259连接（硬件）
2. 编写初始化程序
     + 8259初始化（初始化命令字）
     + 设置中断向量表
          + 直接写IVT
          + 利用都是功能调用：功能号25H是写IVT
3. 编写中断处理程序

### 第四讲 直接存取方式DMA
#### 工作过程
+ 特点
     + 高速外设，纯硬件控制，外设直接与存储器进行数据交换，无需CPU，传输速率高
     + 内存/外设的地址和读写控制均由DMACA提供
+ 时序   
     + AEN=0 : CPU
     + AEN=1 : DMA
+ 工作过程
1. 外设准备好，向DMA发出DRQ
2. DMA收到请求，向CPU发出HOLD
3. CPU完成当前总线周期且非总线封锁，响应HOLD信号
     + 将数据总线、地址总线、控制信号线置高阻态，放弃总线控制权
     + 向DMA发出HLDA
4. DMA收到HLDA，开始控制总线，向外设发出DACK
5. 外设与内存 或 内存与内存 直接数据传送
6. DMA自动修改地址和字节计数器，传完后，撤HOLD
7. CPU撤HLDA，下一时钟周期控制总线

#### DMA控制器8237（DMAC）
+ 特点
     + 4个独立DMA通道，级联扩展为最多16个
+ 引脚及功能
+ 工作时序
     Si：空闲状态
     S0请求状态
     S1 ~ S4传送状态
     + S1只在A15 ~ A18更新时才执行
     + 可在S3  S4之间插入Sw
     + 正常时序S2、S3、S4
     + 压缩时序S2、S4
##### 8237工作方式
+ 工作方式
     + 空闲周期
     + 工作周期
          + 传输方式
               1. 单字节传送（传完一个字节后，DREQ无效，总线控制权还给CPU）
               2. 数据块传送 （传完块数据后才释放总线，期间无论DREQ是否有效都传送）
               3. 请求传送（猝发传送）（只要DREQ有效或I/O接口的数据缓冲可用，DMA一直传送数据）
          + 连接方式
               4. 级联方式
               + 在级联方式下，当第二层8237的请求得到响应时，第一层8237仅向微处理器发出HRQ信号、对第二层的HRQ作出响应DACK而不能输出地址及控制信号，第二层的8237才是真正的主控制器
+ 传送类型：存储器  → 接口，接口 → 存储器，存储器 → 存储器
+ 优先级
     + 固定优先级
     + 循环优先级
+ 传输速率
     + 正常时序（1个DMA总线周期需4个时钟周期）
     + 压缩时序（1个DMA总线周期需2个时钟周期）
+ 内部寄存器

## 第七章 常用接口器件

### 第一讲 8255: 8位通用可编程并行接口

#### 计算机与外设之间通过接口传送数据

+ 无条件输入(三态门: 74LS244)
+ 无条件输出(锁存器: 74LS273)
+ 中断方式，单向输入/输出
+ 中断方式，双向传输(I/O)

#### 端口地址信号

| $A_1$ | $A_0$ |          选择          |
| :-----: | :-----: | :--------------------: |
|    0    |    0    | A口($PA_0\sim PA_7$) |
|    0    |    1    | B口($PB_0\sim PB_7$) |
|    1    |    0    | C口($PC_0\sim PC_7$) |
|    1    |    1    |       控制寄存器       |

A口、B口的输入和输出具有锁存能力，C口的输出有锁存能力，输入没有锁存能力

|               A组               |               B组               |
| :------------------------------: | :------------------------------: |
| $PA_0\sim PA_7, PC_4\sim PC_7$ | $PB_0\sim PB_7, PC_0\sim PC_3$ |

#### 控制字

<img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Courses/image.63dps1a3b2o0.webp" alt="8255的方式选择控制字格式" width="40%"/>
<img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Courses/image.697cgc9bu1g0.webp" alt="8255的按位操作控制字格式" width="30%"/>

{% message color:info title:控制字的部分独立性%}
A口($PA_0\sim PA_7$)、B口($PB_0\sim PB_7$)、C口低四位($PC_0\sim PC_3$)、C口高四位($PC_4\sim PC_7$)可单独定义
{% endmessage %}

#### 状态字

当8255的A口、B口工作在方式1或A口工作在方式2时，通过读C口的状态，可以检测A口和B口的状态

<img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Courses/image.2hnrjt608co0.webp" alt="A、B口均为方式1输入时的状态字" width="70%"/>

<img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Courses/image.w8t4cha3ojk.webp" alt="A、B口均为方式1输出时的状态字" width="70%"/>
<img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Courses/image.3egg3ip3w5k0.webp" alt="A口在方式2工作时的状态字 " width="70%"/>

#### 工作方式

+ 工作方式 0(基本输入/输出方式)
  A口($PA_0\sim PA_7$)、B口($PB_0\sim PB_7$)、C口低四位($PC_0\sim PC_3$)、C口高四位($PC_4\sim PC_7$)独立定义，均可做为输入或输出接口
+ 工作方式 1(选通输入/输出方式)

{% message color:info title:A、B口均输出%}

+ 固定 C 口线
  A 口使用 $PC_3(INTR_A),PC_6(\overline{ACK_A}),PC_7(\overline{OBF_A})$, B 口使用 $PC_0(INTR_B),PC_1(\overline{OBF_B}),PC_2(\overline{ACK_B})$
+ $\overline{OBF}$(Ouput Buffer): (8255端口$\rightarrow$ 外设) 输出缓冲器满信号，通知外设在规定端口上取数据: $CPU\overset{data}{\rightarrow}Buffer\overset{\overline{OBF}}{\rightarrow}Device\overset{Fetch}{\rightarrow}Data$
+ $\overline{ACK}$: (外设$\rightarrow$ 8255端口) 外设响应信号：$Device\overset{Fetch}{\rightarrow}Data\rightarrow \overline{OBF}=1$
+ $INTR$:  (8255端口$\rightarrow$ CPU) 中断请求信号，$CPU\overset{data}{\rightarrow}Buffer\rightarrow INTR\rightarrow Newdata$
+ $INTE$: 中断允许状态，A口由$PC_6$控制，B口由$PC_2$控制
  {% endmessage %}

{% message color:info title:A、B口均输入%}

+ 固定 C 口线
  A 口使用 $PC_3(INTR_A),PC_5(IBF_A),PC_4(\overline{STB_A})$, B 口使用 $PC_0(INTR_B),PC_1(IBF_B),PC_2(\overline{STB_B})$
+ $\overline{STB}$: (外设$\rightarrow$ 8255端口) 输入选通信号，将外设数据锁存于输入锁存器中
+ $\overline{IBF}$: (8255端口$\rightarrow$ 外设) 输入缓冲器满信号
+ $INTR$:  (8255端口$\rightarrow$ CPU) 中断请求信号
+ $INTE$: 中断允许状态，A口由$PC_4$控制，B口由$PC_2$控制
  {% endmessage %}
+ 工作方式 2(双向输入/输出方式，仅A口)

  + A口控制线: $PC_0\sim PC_7$
<img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Courses/image.2cqq6771kmm8.webp" width="60%"/>

#### 芯片结构连接图

{% message color:info%}
<img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Courses/image.715uf3zc9v40.webp" alt="8255与系统总线连接" width="50%"/>
<img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Courses/image.3b8299nrmm00.webp" alt="8255与外设连接" width="50%"/>
{% endmessage %}

#### 初始化程序
{% message color:info %}
8255端口地址：380H$\sim$383H
{% endmessage %}

```x86asm 方式0——手动设置外设选通信号
INIT_8255:
  MOV DX, 0383H             ; 控制寄存器
  MOV AL, 10000011B         ; 方式选择
  OUT DX, AL
  MOV AL, 00001101B         ; C口使得STROBE=1, 初始化控制信号
  OUT DX, AL

PRINT:
  MOV AL, BLAK
  MOV Cl, AL              ; 字符串长度
  MOV SI, OFFSET DATA

GOOD:
  MOV DX, 0382H           ; C口  
PWAIT:
  IN AL, DX               ; Test busy signal
  AND AL, 02H             
  JNZ PWAIT               ; Wait until ready

  MOV AL, [SI]           ; load char data
  MOV DX, 0380H          ; A口   
  OUT DX， AL            ; CPU -> A口

  MOV DX, 0382H          ; C口
  MOV AL, 00H
  OUT DX, AL             ; PC6=0 -> STROBE=0 
  CALL Delay_1us         ; A 口 -> 外设

  MOV AL, 40H
  OUT DX, AL             ; PC6=1 -> STROBE=1 Restore high level

  INC SI
  DEC CL
  JNZ GOOD
  RET
```
```x86asm 方式1——自动握手
INIT_8255:
  MOV DX, 0383H             ; 控制寄存器
  MOV AL, 10100000B         ; 方式选择
  OUT DX, AL
  MOV AL, 00001101B         ; C口 PC6=1 初始无握手信号
  OUT DX, AL

POLLPRINT:
  MOV AL, BLAK
  MOV Cl, AL              ; 字符串长度
  MOV SI, OFFSET DATA
GOOD:
  MOV DX, 0382H           ; C口
PWAIT:  
  IN AL, DX               ; Test busy signal
  AND AL, 80H             ; Test PC7 = 1 首先测试 PC7 表示 CPU 已经响应中断
  JZ PWAIT                ; Wait until ready
  MOV AL, [SI]           ; load char data
  MOV DX, 0380H          ; A口
  OUT DX， AL            ; CPU -> A口
  ; Make a short summary
  ; A 口自动传送给外设: CPU响应中断 -> data -> A口 -> 有效的OBF -> 选通外设接受数据
  ; 外设接受数据 -> 有效 ACK 握手信号 -> OBF 无效

  INC SI     
  DEC CL
  JNZ GOOD
  RET
```
### 第二讲 8253: 可编程定时器
#### 端口地址信号

| $A_1$ | $A_0$ |          选择          |
| :-----: | :-----: | :--------------------: |
|    0    |    0    | 计数器0 |
|    0    |    1    | 计数器1 |
|    1    |    0    | 计数器2 |
|    1    |    1    |       控制寄存器       |

#### 芯片结构
<img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Courses/image.1ro285x4cskg.webp" width="20%"/>

#### 计数方式
{% message color:info title:8253的计数模式%}
计数初值寄存器与减一计数器的关系: 计数初值装入初值寄存器，减一计数器每次都加载初值寄存器中的初值，启动计数。这种关系适用于所有计数模式，主要便于循环计数，启动新一轮计数.
减一计数器与计数锁存器的关系：CPU发锁存命令，使得减一计数器中的当前计数值会锁存到计数锁存器中, 可用于在计数过程中读取当前计数值.
{% endmessage %}
{% message color:info title:减1计数器%}
二进制计数：0000H$\sim$ FFFFH(65535)
+ 初值0000H为最大计数值，FFFFH为计数最小值
+ 减一运算采用补码加法：
  + FFFFH-1H = FFFFH + 0001H=0000H
  + 0000H-1H = 0000H + 0001H=0001H
  + 0001H-1H = 0001H + 0001H=0010H
{% endmessage %}
<img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Courses/image.1iatm55f1deo.webp" width="50%"/>

+ 方式0：计数结束产生中断
  + Condition: GATE 高电平，允许计数
  + OUT: 输出低电平
  + First CLK: 计数初值 N -> 初值寄存器
  + N CLKs: 减一计数
  + N+1 CLKs: 事件计数 
+ 方式1：可编程单稳
  + Condition: GATE 上升沿触发计数
  + OUT: 输出低电平
  + N CLKs: 利用 GATE 编程计数
+ 方式2：频率发生器
  + Condition: GATE 高电平，允许计数
  + OUT: 周期输出负脉冲
  + N CLKs: 减到1时送出负脉冲
+ 方式3：方波发生器
  + Condition: GATE 高电平，对称方波
  + OUT: 前 N/2 | (N+1)/2 CLKs 高电平，后 N/2 | (N-1)/2 CLKs 低电平
+ 方式4：软件触发选通
  + Condition: GATE 高电平，允许计数
  + OUT: 计数结束输出负脉冲
  + N CLKs: 技术结束下一个 CLK, 送出负脉冲
+ 方式5：硬件触发选通
  + Condition: GATE 上升沿触发计数
  + OUT: 计数结束输出负脉冲
  + N CLKs: 技术结束下一个 CLK, 送出负脉冲
+ Summary
  + 所有 GATE 上升沿触发计数均在下一个 CLK 启动计数
  + 0为计数最大值，1为计数最小值，计数为减一计数器
  + 从初值寄存器装入新的计数值，除了可编程单稳，频率发生器都需要重新开始计数
  + 所有 GATE 上升沿触发计数(可编程单稳，频率发生器)在新的上升沿到来时，均会重新开始计数

#### 控制字
<img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Courses/image.ewqsaicu928.webp" width="50%"/>

#### 芯片结构连接图
{% message color:info%}
<img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Courses/image.5j7jfwwhm3g.webp" width="50%" alt="8253与8088系统总线的连接"/>

{% endmessage %}
#### 初始化程序及其应用
{% message color:info %}
利用计数器模式设计电路，输出指定频率的信号
{% endmessage %}

```x86asm
MOV AL, 36H         ; 控制字，计数器0，双字节，方式3，二进制计数
OUT 43H, AL         ; 送入控制寄存器
MOV AL, 0
OUT 40H, AL         ; 先写低字节
OUT 40H, AL         ; 后高字节

MOV AL, 54H         ; 计数器1，低字节，方式2，二进制计数
OUT 43H, AL
MOV AL, 18
OUT 41H, AL
```
+ 计数模式1：GATE周期信号频率: $F_1$Hz, CLK输入频率: $F_2$Hz，GATE上升沿刷新计数的最大计数值为:
$$
N = \frac{F_2}{F_1}
$$

## 题型分析
{% message color:info %}
+ 仅适用于重点范围的题型分析，是个人觉得出题意义或概率相对较大的一类题目或知识模块;
+ 设定的 Todo List 仅是基于我个人的情况，不提供参考；
{% endmessage %}

### 第二章 8086/8088 CPU
- [x] 8086/8088 最小/最大模式下系统总线形成

### 第三章 8086汇编语言程序设计
- [x] 常用汇编指令
- [x] 基本汇编程序编写
  - [x] 基本表达式计算
  - [x] 数据段中数组数据进行排序

### 第四章 总线与驱动控制
- [x] 常用数据总线特点(ISA, PCI, USB)
- [x] **总线驱动控制电路设计与分析**
  - [x] 单向驱动器: 74LS224
  - [x] 双向驱动器: 74LS245
  - [x] 数据锁存器: 74LS373
- [x] 总线驱动与控制参数计算

### 第五章 存储器设计
- [x] SRAM, DRAM, EPROM, EEPROM 特点
- [x] 数字比较器、PROM 作为译码电路的实现机制
- [x] **基于 SRAM 芯片的8086/88系统主存电路设计与分析**
  - [x] 位扩展+字扩展设计方法(8086系统16位存储设计)
  - [x] 8086系统8位读写以及16位读写
  - [x] 总线驱动设计: 单向驱动与双向驱动
  - [x] 基于 EEPROM 芯片的 IO 接口设计(Busy的处理) 
  - [x] 存储器设计中总线驱动器件使能端设计

### 第六章 输入/输出技术
- [x] 中断处理响应过程
- [x] 8259工作方式(状态字)
  - [x] 级联: 特殊全嵌套与一般嵌套
  - [x] 中断结束: 自动EOI, 特殊/指定EOI, 一般/非指定EOI
  - [x] 优先级: 固定优先级, 自动循环优先级, 指定循环优先级

## 微机原理与系统课程实验
<iframe src="/pdfjs/web/viewer.html?file=/pdf/collaboration/Microcomputer.pdf" style='width:100%;height:800px'></iframe>

## 微机原理与系统课程设计
{% message color:info %}
本章节主要是课程设计部分，以《交通信号灯自动控制模拟指示系统设计》为课设项目完成的实验报告。
{% endmessage %}
<iframe src="/pdfjs/web/viewer.html?file=/pdf/collaboration/MicrocomputerProject.pdf" style='width:100%;height:800px'></iframe>

## Contributors

<div style="border: 1px solid #ccc; padding: 15px; border-radius: 5px; box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.1); margin-bottom: 15px;">
    <p>Zhihao Li</p>
    <p>
        <a href="https://zhihaoli.top">
            https://zhihaoli.top
        </a>
    </p>
</div>

## References

<div style="border: 1px solid #ccc; padding: 15px; border-radius: 5px; box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.1); margin-bottom: 15px;">
    <p>张剑贤. 2023年秋, 微机原理与系统设计, 西安电子科技大学.</p>
    <p>
        <a href="https://mooc1.chaoxing.com/mooc-ans/course/236212950.html">
            https://mooc1.chaoxing.com/mooc-ans/course/236212950.html
        </a>
    </p>
</div>

<div style="border: 1px solid #ccc; padding: 15px; border-radius: 5px; box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.1);">
    <p>微型计算机原理及接口技术(第三版)</p>
    <p>
        <a href="https://www.xduph.com/Pages/BookDetail.aspx?doi=09a5a856-0f2e-4260-b9c4-17b78c1e7701">
            https://www.xduph.com/Pages/BookDetail.aspx?doi=09a5a856-0f2e-4260-b9c4-17b78c1e7701
        </a>
    </p>
</div>
