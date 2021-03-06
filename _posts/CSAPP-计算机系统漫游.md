---
title: CSAPP-计算机系统漫游
tags:
  - CS
date: 2020-08-15 16:59:29
---


这一章以一个helloworld程序为例子来开始对系统的学习-----从这个程序被程序员创建开始，到在系统上面运行，输出简单的消息，然后程序终止。我们这里将沿着这个程序的生命周期，简要的介绍一些关键概念、术语和组成部分。后面的章节将围绕这些概念展开。

<!--more-->

### 1. 信息就是 位 + 上下文

系统中所有的信息-----包括磁盘文件、内存中的程序、内存中存放的用户数据以及网络上传送的数据，都是一串比特表示。区别不同数据对象的唯一方法是我们读到这些数据对象时的上下文。比如，在不同的上下文中，一个同样的字节序列可以表示一个整数、浮点数、字符串或者机器指令。

作为程序员，我们需要了解数字机器表示方法，因为它们与实际的整数和实数是不同的。它们是对真实值的有限接近值，有时会有意想不到的行为表现。

### 2. 程序被其他程序翻译成不同的格式

一个程序文件被GCC编译器读取并翻译成一个可执行文件的过程分为以下四个步骤：

  1. 预处理阶段：修改原始的文件，将include的内容添加到需要处理的文本中，通常这一步骤的结果为 `.i`文件。
  2. 编译阶段：将文本文件 `helloworld.i` 翻译成为文本文件 `helloworld.s` 。
  3. 汇编阶段：这个阶段汇编器（as）将`helloworld.s`翻译成机器语言指令，并将这些指令打包成一种叫做可重定位目标程序的格式，并将这个结果保存在目标文件`helloworld.o`中。
  4. 链接阶段：如果调用了其他库文件，这个库文件就必须以某种方式合并到我们的可执行文件中，链接器（ld）负责处理这种合并。结果得到`helloworld` 的可执行文件，可以被加载到内存中并被系统执行。

### 3. 编译系统是如何工作的

了解系统是如何工作的可以帮助我们：

  1. 优化程序性能
  2. 理解链接时出现的错误
  3. 避免安全漏洞

### 4. 处理器读取并解释存储在内存中的指令

系统主要由一些硬件设备组成，这些硬件设备主要包括：
  
  1. 总线
  2. IO设备
  3. 主存
  4. 处理器： 处理器是解释或者执行存储在主存中指令的引擎。处理器的核心是一个大小为一个字节的存储设备，称为程序计数器(PC)。在任何时候PC都指向主存中的某条机器语言指令（或含有该条指令的地址）。CPU的操作主要围绕着主存、寄存器文件和算数/逻辑单元进行，包含加载、存储、操作、跳转、等操作。

在运行 `helloworld` 程序时，shell程序将我们在键盘上输入的字符串逐一读入寄存器中，再把它存放到内存中。当在键盘上输入回车键时，shell程序知道我们已经结束输入指令。然后shell执行一系列指令来加载可执行的 `helloworld` 文件。这些指令将 `helloworld` 目标文件中的代码和数据从磁盘复制到主存。数据包括最终会被输出的字符串 “hello,world\n” 。一旦目标文件`helloworld`中的代码和数据被加载到主存，处理器就开始执行`helloworld`程序中的main程序中的机器语言指令。这些指令将“hello,world\n”字符串中的字节从主存复制到寄存器文件中，再从寄存器文件中复制到显示设备，最终显示在屏幕上。

### 5. 高速缓存

上面helloworld的例子揭示了一个重要的问题：计算机系统花费了大量的时间将信息从一个地方转移到另外的一个地方。`helloworld`程序指令最开始是存放在磁盘上的，当程序加载的时候，它们被复制到主存上；当处理器运行程序时，指令从主存复制到处理器。从程序员的角度来讲，这些复制就是开销，减慢了程序的工作，因此系统的设计者的一个目标就是使这些复制操作尽快的完成。

> 根据机械原理，较大的存储设备要比较小的存储设备的运行得慢，而快速的设备造价要远高于同类的低速设备。

针对这种处理器和主存储器之间的差异问题，系统的设计者采用了更小更快的存储设备，称为 *高速缓存存储器(cache memory，简称为cache或者高速缓存)* 。作为暂时的集结区域，存放处理器近期可能会需要的信息。位于处理器芯片上的L1 高速缓存的容量可以达到数万字节，访问这里的速度几乎和访问寄存器文件的速度一样快，同时还有一个容量为数十万到数百万字节的更大的L2高速缓存通过一条特殊的总线连接到处理器，进程访问L2高速缓存的速度大约比L1高速缓存器慢3-5倍。L1和L2高速缓存器是通过一种叫做SRAM(静态随机访问存储器)的硬件技术来实现的。


### 6. 存储设备形成的层次结构

存储器的层次结构的主要思想是上一级存储器作为低一级存储器的高速缓存，因此，寄存器文件就是L1的高速缓存，L1是L2的高速缓存....

### 7. 操作系统管理硬件

这里回到 `helloworld` 程序，当shell加载和运行 `helloworld` 程序以及`helloworld`程序运行的时候，shell和 `hello` 程序并没有访问显示器，键盘，主存和其他设备，它们都是依靠操作系统提供的服务。这里可以将操作系统看作是应用程序和硬件之间插入的一层软件，所有的应用程序对应的硬件操作尝试都必须通过操作系统软件。

操作系统软件有两个基本功能：

  1. 防止硬件被失控的应用程序滥用；
  2. 向应用程序提供简单一致的机制来控制复杂而又通常大不相同的低级硬件设备。

操作系统通过几个基本的抽象概念（进程、虚拟内存和文件）来实现这两个功能。这其中文件是对I/O设备的抽象表示，进程则是对处理器，主存和I/O设备的抽象表示，下面将讨论每种抽象表示。

#### 7.1 进程

像 `helloworld` 这样的程序在现代操作系统上运行的时候，操作系统提供了一种假象，就好像系统上只有这个程序在运行，程序看上去是在独占的使用处理器、主存和I/O设备。处理器看上去就是在不间断的一条又一条地执行程序中的指令，即该程序的代码和数据是系统内存中唯一的对象，这些假象是通过进程的概念来实现的，进程是计算机科学中最重要和最成功的概念之一。

> 进程是操作系统对一个正在运行的程序的一种抽象，在一个系统上可以同时运行多个进程，而每个进程都在独占的使用硬件。

> 并发运行，则是说一个进程的指令和另一个进程的指令是交错执行的。

这其中一个CPU看上去是在并发的执行多个进程，这是通过处理器在进程间切换来实现的。操作系统实现这种交错执行的机制称为上下文切换。

操作系统保持跟踪进程所需的所有信息。这种状态也就是 *上下文* ，包括许多信息，比如PC指针和寄存器文件的当前值，以及主存的内容，在任何一个时刻，单处理器系统都只能执行一个进程的代码，当操作系统决定要把控制权从当前系统转移到某个新进程的时候，就会进行上下文切换，即保存当前进程的上下文，恢复新进程的上下文，然后将控制权转移到新进程，新进程就会从它上次停止的地方开始。

> 从一个进程到另一个进程的转换是由操作系统内核（kernel）来管理的，内核是操作系统代码常驻主存的部分。当应用程序需要操作系统的某些操作时，比如读写文件，它就执行一条特殊的系统调用指令，将控制权传递给内核，然后内核执行被请求的操作并返回结果给应用程序。这里需要注意的是，内核不是一个独立的进程，它是系统管理全部进程所有代码和数据结构的集合。

#### 7.2 线程

在现代系统中，一个进程实际上可由多个称为线程的执行单元组成。每个线程都运行在进程的上下文中，并共享同样的代码和全局数据。一般来说多线程比多进程之间更容易共享数据，线程一般来说比进程更加高效。当多处理器可用时，多线程也是一种可以使得程序运行的更快的方法。

#### 7.3 虚拟内存

虚拟内存是一个抽象的概念，它为每个进程提供了一个假象，即每个进程都在独占地使用主存。每个进程看到的内存都是一致的，称为虚拟地址交换空间。

#### 7.4 文件

文件就是字节序列，仅此而已。每个IO设备包括磁盘、键盘、显示器、甚至网络都可以看作是文件。系统中所有输出都是通过使用一个小组称为 Unix I/O 的系统函数调用读写文件来实现的。

### 8. 系统间利用网络通信

现代计算机系统经常通过网络和其他系统连接到一起。从一个单独的系统来看，网络可以看作一个I/O设备，当系统从主存复制一串字节字节到网络适配器时，数据流经过网络到达另一台机器，而不是说到达本地磁盘驱动器，相思地，系统可以读取从其他机器发送来的数据，并把数据复制到自己的主存。

随着Internet这样的全球网络的出现，从一台主机复制信息到另外一台主机已经成为计算机系统最重要的用途之一。比如电子邮件、万维网、FTP和Telnet这样的应用都是基于网络复制信息的功能。

### 9. 重要主题

#### 9.1 Amdahl定律

Amdahl定律描述了，当我们对系统的某个部分加速时，其对系统整体性能的影响取决于该部分的重要性和加速程度。若系统执行某应用程序所需的时间为`T_old`。假设系统所某部分所需的执行时间与该时间的比例为`a`，而该部分性能提升比例为`k`。即该部分初始所需时间为`a*T_old`，现在所需时间为 `(a*T_old)/k`。这里可以计算得到加速比为 ` S = 1/((1-a) + a/k) `。 Amdahl定律的主要观点就是-----要想显著的加速整个系统，必须提升系统中相当大的部分的速度。

#### 9.2 并发与并行

这里我们通常对计算机系统有两个需求：

  1. 想要计算机能够做更多事情；
  2. 想要计算机系统运行得更快。

> 多核处理器是将多个CPU集成到一个集成电路芯片上，每个核都有自己相对独立的L1和L2高速缓存。

> 超线程，有时被称为多线程技术，该技术允许一个CPU执行多个控制流。常规处理器大约需要20000个时钟周期做不同线程之间的切换，而超线程的处理器可以在单个周期的基础上决定要执行那个线程。这使得CPU能够更好的利用它的处理资源。

#### 9.3 计算机系统中抽象的重要性

计算机系统的一个重大的主题就是提供不同层次的抽象表示，来隐藏实际实现的复杂性。例如文件是对I/O设备的抽象，虚拟内存是对程序存储器的抽象，而进程是对一个正在运行的程序的抽象， 虚拟机是对整个计算机系统的抽象。

