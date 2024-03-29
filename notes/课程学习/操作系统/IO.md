## I/O系统

I/O系统管理的主要对象是I/O设备和相应的设备控制器。

#### I/O软件的层次结构

<img src=".\pictures\层次结构.png" alt="层次结构" style="zoom:80%;" />

1）用户层I/O软件：实现与用户交互的接口，用户可直接调用该层所提供的、与I/O操作有关的库函数对设备进行操作。

2）设备独立性软件

又称系统调用处理层、系统调用程序或设备无关性软件

用于实现用户程序与设备驱动器的统一接口、设备命名、设备的保护以及设备的分配与释放等，同时为设备管理和数据传送提供必要的存储空间。

主要功能：

* 执行所有设备的共有操作

  对设备的分配与回收

  将逻辑设备名映射为物理设备名

  对设备进行保护，禁止用户直接访问设备

  缓冲管理、差错控制、屏蔽设备之间信息交换单位大小和传输速率的差异

* 向用户层（或文件层）提供了系统调用的接口，将系统调用参数翻译成设备操作命令。

设备的独立性是指用户在变程序时适用的设备与实际设备无关。

3）设备驱动程序

与硬件直接相关，用于具体实现系统对设备发出的操作指令，驱动I/O设备工作的驱动程序。

每类设备具有一个设备驱动程序

4）中断处理程序

用于保存被中断进程的CPU环境，转入相应的中断处理程序进行处理，处理完毕再恢复被中断进程的现场后，返回到被中断的进程。

本地用户通过键盘登录系统是，首先获得键盘输入信息的程序是中断处理程序。

#### I/O系统的层次结构

三个层次：中断处理程序、设备驱动程序、设备独立性软件。

## I/O设备

I/O设备指可以将数据输入到计算机或者可以接收计算机输出数据的外部设备，属于计算机中的硬件部件。

#### 设备的分类

I/O设备按使用特性分类：1.人机交互类外部设备。传输速度慢。2.存储设备。数据传输速度快。3.网络通信设备。数据传输速度介于两者之间

I/O设备按使用传输速率分类：1.低速设备。如鼠标、键盘。2.中速设备。如激光打印机。3.高速设备。如磁盘

I/O设备按信息交换的单位分类：1.块设备。如**磁盘**，传输速率较高，**可寻址**，能指定输入的源地址或输出的目标地址，**采用DMA方式**。2.字符设备。如键盘、打印机，不可寻址，智能采取顺序存取方式，常采用中断驱动方式。

## I/O接口（设备控制器）

I/O接口又称I/O控制器、设备控制器

CPU无法直接控制I/O设备的机械部件，因此I/O设备还要有一个电子部件作为CPU和I/O设备机械部件的“中介”，用于实现CPU对设备的控制。这个电子部件就是I/O接口。CPU可控制I/O控制器，又由I/O控制器来控制设备的机械部件。

I/O设备的机械部件，如我们看得见摸得着的鼠标、键盘的按钮，显示器的LED屏、磁盘盘面。

I/O设备的电子部件，通常是一块插入主板扩充槽的印刷电路板。

一个I/O控制器可能对应多个设备。

#### 设备控制器的组成

1）设备控制器与处理机的接口

该接口用于实现CPU与设备控制器之间的通信。

在该接口中共有三类信号线：数据线、地址线和控制线。

2）设备控制器与设备的接口

在每个接口中都存在数据、控制和状态三种类型的信号。

3）I/O逻辑

用于实现对设备的控制。

#### 设备控制器的基本功能

1）接受和识别命令

2）数据交换

3）标识和报告设备的状态

4）地址识别

5）数据缓冲区

6）差错控制

## I/O端口

I/O端口是指设备控制器中可被CPU直接访问的寄存器，主要有以下三类寄存器：

1）数据寄存器

2）状态寄存器

3）控制寄存器

独立编址与统一编址：

数据寄存器、控制寄存器、状态寄存器可能有多个，且这些寄存器都要由相应的地址，才能方便CPU操作。有的计算机采用专用地址，即寄存器独立编址，需要设置专门的指令来实现对控制器的操作，不仅要指明寄存器的地址，还要指明控制器的编号；另一些计算机会让这些寄存器占用内存地址的一部分，统一编址，称为内存映像I/O，简化了指令。

## I/O控制方式

##### 使用轮询的可编程I/O方式

数据传送的单位是字

数据的流向：I/O设备⟶CPU⟶内存 或者 内存⟶CPU⟶I/O设备

CPU和I/O设备只能串行工作，CPU中无中断机构，CPU需要一直轮询检查，长期处于忙等状态，CPU利用率低

##### 使用中断的可编程I/O方式

数据传送的单位是字。引入中断机制

数据的流向：I/O设备⟶CPU⟶内存 或者 内存⟶CPU⟶I/O设备

##### 直接存储器访问方式（Direct Memory Access，DMA）

数据传送的单位是数据块

必须在DMA控制器中，设置如下四类控制器：

* 命令/状态寄存器CR
* 内存地址寄存器MAR
* 数据寄存器DR
* 数据计数器DC

数据的流向：I/O设备⟶内存 或者 内存⟶I/O设备

用户发出磁盘I/O请求后，系统的处理流程：用户程序⟶系统调用处理程序⟶中断处理程序⟶设备驱动程序

DMA控制方式与中断控制方式的主要区别：

* 中断控制方式在每个数据传送完成后中断CPU，而DMA控制方式则在所要求传送的一批数据全部传送结束时中断。
* 中断控制方式的数据传送在中断处理时由CPU控制完成，而DMA控制方式则在DMA控制器的控制下完成。不过，在DMA控制方式中，数据传送的方向、存放数据的内存始址及传送数据的长度等仍然由CPU控制。
* DMA方式以存储器为核心，中断控制方式以CPU为核心。因此DMA方式能与CPU并行工作。
* DMA方式传输批量的数据，中断控制方式的传输则以字节为单位。

##### I/O通道控制方式

I/O通道是指专门负责输入/输出的处理机。I/O通道方式是DMA方式的发展。

数据传送的单位是一组数据块

数据的流向：I/O设备⟶内存 或者 内存⟶I/O设备

虽然在CPU与I/O设备之间增加了设备控制器后，已能大大减少CPU对I/O的干预，但当主机所配置的外设很多时，CPU的负担仍然很重。为次，在CPU和设备控制器之间又增加了I/O通道。其主要目的时为了建立独立的I/O操作。

I/O通道是一种特殊的处理机，通道硬件比较简单，指令类型单一，其所能执行的指令，主要局限于与I/O操作有关的指令；I/O通道没有自己的内存，通道所执行的程序时放在主机的内存中的，换言之，是通道与CPU共享内存。通道是一种硬件机制。

通道完成了通道程序的执行时，会产生中断

通道类型：

* 字节多路通道（Byte Multiplexor Channel）

  一个主通道连接多个子通道，以时间片轮转方式共享主通道。

  每个子通道每次只传送一个字节，连接中低速设备

* 数组选择通道（Block Selector Channel）

  可以连接多台高速设备，但由于只含有一个分配型子通道，在一段时间内只能执行一道通道程序，控制一台设备进行数据传送

  通道利用率低

* 数组多路通道（Block Multiplexo Channel）

  采用分时并行传送

瓶颈问题的解决：采用复联的多通路方式

## （设备独立性软件的）设备分配

#### 设备分配中的数据结构

系统为实现对独占设备的分配，必须在系统中配置相应的数据结构。

1）设备控制表（Device Control Table，DCT）

系统为每一个设备都配置了一张设备控制表，用于记录设备的情况。

<img src=".\pictures\设备控制表.png" alt="设备控制表" style="zoom:80%;" />

2）控制器控制表（Controller Control Table，COCT）

3）通道控制表（Channel Control Table，CHCT）

4）系统设备表（System Device Table，SDT）

一个通道对应多个控制器，而一个控制器对应多个设备。系统为每个设备配置一张设备控制表（DCT）,每个设备控制器都会对应一张控制器控制表（COCT），每个通道对应一张通道控制表（CHCT）

为进程分配设备时，先根据进程请求的物理设备名查找SDT，根据SDT找到DCT，根据DCT上的信息查看该设备是否忙碌，空闲则分配给该进程，继续根据DCT找该设备的控制器，通过COCT，看是否能分配控制器给该进程，分配后再根据CHCT，看通道是否能分配给该进程。设备、控制器、通道三者分配成功，这次设配分配才算成功，之后才可启动I/O设备进行数据传输

缺点：1.用户编程时必须使用”物理设备名“，但用户不清楚底层细节，不方便编程。2.若换了一个物理设备，则程序无法运行。3.若进程请求的物理设备正在忙碌，即使系统中海油同类型的设备，进程也必须阻塞等待

计算机系统为每台设备确定一个编号以区分和识别设备，这个确定的编号为设备的**绝对号**。

改进方法：建立逻辑设备名到物理设备名的映射关系

设备独立性软件需要通过逻辑设备表(LUT，Logical Unit Table)来确定逻辑设备对应的物理设备，并找到对应的设备驱动程序

操作系统可以采用两种方式管理逻辑设备表：1.整个系统只设置一张LUT，这意味着每个物理设备的逻辑名只能有一个。该方式只适用于单用户操作系统。2.为每个用户设置一张LUT

#### 设备分配时应考虑的因素

不用考虑及时性。

1）设备的固有属性

设备的固有属性课分成三种，对它们应采取不同的分配策略：

* 独占设备的分配策略

* 共享设备的分配策略

  共享设备时指在一个时间内可被多个进程同时访问的设备，打印机不满足

* 虚拟设备的分配策略

2）设备分配算法

* 先来先服务
* 优先级告者优先

3）设备分配中的安全性

* 安全分配方式：分配设备后，让进程阻塞，直到I/O结束

* 不安全分配方式：分配设备后，进程继续运行

## 缓冲

引入缓冲的原因：

1）缓和CPU与I/O设备之间速度不匹配的矛盾

2）减少对CPU的中断频率，放宽对CPU中断相应时间的限制

缓冲区满或空时，CPU才介入

3）解决数据粒度不匹配的问题

如输出进程每次可以生成一块数据，但I/O设备每次只能输出一个字符

4）提高CPU和I/O设备之间并行性

无论是单缓冲、多缓冲还是缓冲池，由于缓冲区是一种临界资源，所以在适用缓冲区时都有一个申请和释放的问题需要考虑。所以实现进程访问缓冲区的同步问题需要重要考虑。

#### 缓冲区

输入/输出缓冲区时在内存中开辟的。

1）单缓冲区

C表示CPU处理时间，T表示设备输入到缓冲区的时间，M表示数据从缓冲区传送到工作区的时间，平均耗时max(C，T)+M

2）双缓冲区

C表示CPU处理时间，T表示设备输入到缓冲区的时间，M表示数据从缓冲区传送到工作区的时间，若T<C+M，则平均耗时C+M；若T>C+M，则平均耗时T

3）循环（环形）缓冲区

在环形缓冲中包括多个缓冲区，其每个缓冲区的大小相同。

#### 缓冲池

单缓冲区、双缓冲区、环形缓冲区三种缓冲区的组织形式仅适用于某种特定的I/O进程和计算进程，属于专用缓冲。

为了提高缓冲区的利用率，可以采用公共缓冲池技术，其中的缓冲区可为多个设备和进程服务

缓冲池与缓冲区的区别在于：缓冲区仅仅是一组内存块的链表，而缓冲池则是包含了一个管理的数据结构及一组操作函数的管理机制，用于管理多个缓冲区。

## 假脱机系统

通过多道程序技术可将物理CPU虚拟为多台逻辑CPU，从而允许多个用户共享一台主机，那么，通过假脱机（Spooling）技术，则可将一台物理I/O设备虚拟为多台逻辑I/O设备。

脱机技术 其实就是用来缓解CPU与I/O操作速度不匹配的问题 数据先通过外围控制机输入到更快得设备上，主机再从该设备读入设备，输入、输出的操作不是由主机控制的，所以称脱机技术

假脱机系统的工作原理：

<img src=".\pictures\SPOOLing的工作原理.png" alt="SPOOLing的工作原理" style="zoom:80%;" />

假脱机系统的工作部分：

1）输入井和输出井

输入井和输出井是在磁盘上开辟出来的两个存储区域。输入井用于模拟脱机输入时的磁盘，用于收容I/O设备输入的数据。

2）输入缓冲区和输出缓冲区

3）输入进程和（预）输出进程

输入进程也称预输入进程，用于模拟脱机输入时的外围控制器。

4）井管理程序

井管理程序用于控制作业与磁盘井之间的信息交换

假脱机 就是用软件，输入进程、输出进程（缓冲区）来模拟外围控制机的功能

## 盘调度算法

1）先来先服务（FCFS）算法

2）最短寻找时间优先（Shortest Seek Time First，SSTF）算法

3）扫描（SCAN）算法

4）循环扫描（Circular SCAN，C-SCAN）算法

5）NStepSCAN 算法

N步SCAN算法是将磁盘请求队列分成若干长度为N的子队列，磁盘调度按FCFS算法依次处理这些子队列。当N值取得很大时，会使N步扫描法的性能接近于SCAN算法的性能；当N=1时，N步SCAN算法蜕化为FCFS算法

6）FSCAN算法

FSCAN算法实质上使NStepSCAN 算法的简化，即FSCAN只将磁盘请求队列分成两个子队列。
