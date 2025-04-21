---
tags:
    - theory
---

# Main Memory

!!! note "Address Binding"
    - 源代码层面的地址通常用某个符号（变量名）表示
    - Compiler 将符号绑定到可重定位的地址
    - Linker 将可重定位的地址绑定到绝对地址

    PIE: Position Independent Executable

    - PIE 类型的可执行文件中的地址都是相对地址，由 loader 加载到内存中后才确定绝对地址

Logical address: 逻辑地址，CPU 在寻址时使用的地址

Physical address: 物理地址，实际存储单元的地址

## Memory Management Unit (MMU)

MMU 将逻辑地址转换为物理地址

最简单的方法是加入一个重定位寄存器，保存 offset. 物理地址 = 逻辑地址 + offset

### Dynamic Loading

所有的 routine 都以可重定位格式存放在磁盘上，只有某个 routine 被调用时，其代码才会被加载进内存中

可以提高内存空间的利用率

由应用程序自行设计，OS 可以提供相关库

### Dynamic Linking

Static Linking: 编译时链接，将所有代码和数据都链接到可执行文件中

Dynamic Linking: 运行时链接

- 共享库（Shared Library）: 编译时产生，包含多个 routine，可以被多个程序共享
- 桩（Stub）
- 延迟绑定（Lazy Binding）
- GOT（Global Offset Table）

loader 加载程序时，先将代码加载到内存中，然后检查 metadata 得知该程序依赖哪些外部库，然后在 `LD_PATH` 下查找外部依赖库

## Memory Allocation

有限的内存资源需要用来支撑 OS 与 Application 的运行，因此需要高效的分配方式.

### Contiguous Allocation

- 目的：限制程序对超出范围的物理地址的访问
- Base register: 基址寄存器，保存分配起始地址
- Limit register: 限制寄存器，保存分配的内存大小
- MMU 动态映射逻辑地址到物理地址

寻找可分配空间的算法：

- First-fit: 从基址开始，找到第一个大小足够的空闲空间
- Best-fit: 从基址开始，找到大小最接近要求的空闲空间
- Worst-fit: 从基址开始，找到最大的空闲空间

Best-fit 和 Worst-fit 都需要扫描整块地址空间.

以上三种方法具有内存碎片（Fragmentation）问题. First-fit 和 Best-fit 的利用率相对 worst-fit 高.

- External Fragmentation: 外部碎片，分配的内存空间不连续
- Internal Fragmentation: 内部碎片，分配的内存空间大于程序需求的空间

### Paging

分配给一个程序的物理地址可以是不连续的，这样可以解决外部碎片问题，但是仍然有内部碎片问题.

将物理内存分成多个固定大小的块，称为 Frame
将逻辑内存分成与 Frame 大小相同的块，称为 Page

OS 需要跟踪所有的空闲 Frame

通过页表来维护 Page 和 Frame 的映射关系

逻辑地址被分为页号（page number）与页内偏移（page offset）两部分

页号是页表的索引，对应某个 Frame 的编号

假设页表中页号 p 对应的 Frame 为 f, 则逻辑地址 $p + d$ 对应物理地址 $f + d$, 其中 d 为页内偏移

Frame Table: 标记物理内存中 Frame 的使用情况，通常会维护一个 Free Frame 链表.

![pEgc9I0.png](https://s21.ax1x.com/2025/04/09/pEgc9I0.png)

打开 paging 后，程序只能操控 virtual address，而不能直接操控 physical address. 因此，在打开 paging 前，我们需要让页表的虚拟地址与物理地址相等，以便打开 paging 后对页表的访问.

## Page table

Page Table 一般存储在内存中，寄存器 PTBR 指向 Page Table 的起始地址，寄存器 PTLR 存储页表的大小.

我们每条访存指令实际上要访问两次内存：

- 第一次从页表中查找逻辑地址对应的物理地址
- 第二次从物理地址读取数据

为了加快访问，我们引入 TLB（Translation Look-aside Buffer），作为页表的缓存

![pEgckzF.png](https://s21.ax1x.com/2025/04/09/pEgckzF.png)

TLB 需要保证一致性：

- TLB 与内存中的 Page Table 的一致性
- 多核 CPU 中每个 CPU 的 TLB 的一致性

每个进程都有自己的页表，操作系统内核也有自己的页表. 上下文切换时，需要更新 PTBR 寄存器，使其指向新进程的页表. TLB 也需要更新，此时有两种方式：

- 冷启动：进程切换时，将 TLB 全部清空，从页表中重新加载
- 将 TLB 中的每个项增加 ASID（Address-Space Identifier）用于区分进程

!!! definition "Effective Access Time"

    有效访问时间，指考虑页表和 TLB 的情况下，一次访存的平均耗时.

    $$
        \text{EAT} = (\text{TLB Hit Ratio} + \text{TLB Miss Rate} \times 2) \times \text{Memory Access Time}
    $$

虚拟内存对应的物理内存不一定已经被分配，在页表中不一定能查到。若查不到，就会产生缺页异常（Page Fault Exception），控制权转移到操作系统，操作系统会先对程序请求访问的地址进行合法性校验，若地址合法，则分配物理内存，并更新页表；若地址不合法，则抛出段错误（Segmentation Fault）.


### Memory Protection

页表项一般包括：

- PFN（Page Frame Number）: Page 对应的物理内存中 Frame 的编号
- valid bits：表示该页表项存储的映射关系是否存在
- permission bits（权限位）：用于控制程序对该 page 对应的物理内存的访问权限，取值由操作系统控制. 根据操作类型（Read、Write、Execution）、指令特权态（Kernel，User）等可设置不同的权限位.

当程序访问某个 page 时，若在页表中查到了对应的页表项，则会根据权限位确定程序对该地址的访问是否合法，若合法，则可以访问该地址，否则会产生异常.

### Page Sharing

多个进程可以共享同一块物理内存，只需在进程各自的页表中设置对应的映射关系即可. 可能的应用场景包括：

- 相同程序启动多个进程，代码段可以共享
- 进程间通信使用的共享内存
- 公共库（如 libc）的代码

Linux 在 x86-64 架构上的进程在内核态和用户态之间共享页表

## Structure of page table

如果 Page size 取 4 KB，对于 32 位的系统，每个进程都需要 4 MB 的物理空间来存储页表，并且其中大部分页表项都不会被使用，造成空间浪费.

为了减少页表占用的空间，有以下几种方法：

### Hierarchical Page Tables

将页表分成多个层次，高层次的页表存储低层次的页表所在的 Frame id.

![pEgR0W6.png](https://s21.ax1x.com/2025/04/09/pEgR0W6.png)

逻辑地址被分为三段：

- page directory number
- page table number
- page offset

32 位系统中，page directory number 占 10 位，page table number 占 10 位，page offset 占 12 位，共 32 位.

一次寻址流程如下：

- 首先从 outer page table 中根据 page directory number 找到 inner page table 对应的 Frame 的地址
- 再从 inner page table 中根据 page table number 找到 page 对应的 Frame 的地址
- 最后在 Frame 中根据 page offset 找到数据

### Hashed Page Tables

![pEgRzlT.png](https://s21.ax1x.com/2025/04/09/pEgRzlT.png)

### Inverted Page Tables

![pEgRxpV.png](https://s21.ax1x.com/2025/04/09/pEgRxpV.png)


## 64-bit RISC-V Linux Paging

SATP（Supervisor Address Translation and Protection）寄存器用于设置虚拟内存分页模式，其结构如下：

![pEgfpCt.png](https://s21.ax1x.com/2025/04/09/pEgfpCt.png)

- MODE：分页模式
- ASID：地址空间标识符
- PPN：顶层页表的物理页号

SV39 是 RISCV64 的一种分页模式，采用三级页表


## Demand Paging

Demand Paging 是一种动态内存分配技术，只有当某个 Page 被访问时，系统才会将其加载进内存.

两种 Demand Paging 的策略：

- Lazy swapper
- Pre-Paging：提前将进程需要的部分或全部 Page 加载进内存

### Page Fault

![pEghOkd.png](https://s21.ax1x.com/2025/04/09/pEghOkd.png)

!!! definition "EAT in demand paging"

    Page fault rate $p = \dfrac{\text{Page Fault Times}}{\text{Page Access Times}}$

    EAT in demand paging:

    $$
    \begin{align*}
        \text{EAT} &= (1 - p) \times \text{Memory Access Time} + p \times (\text{Page Fault Overhead} + \\
                   &{} \quad \text{Swap Page Out Cost} + \text{Swap Page In Cost} + \text{Instruction Restart Overhead}).
    \end{align*}
    $$

- Major page fault: 页被访问，但没有在内存中，需要从磁盘加载
- Minor page fault: 页被访问，已经在内存中，但页表映射未建立

    !!! example "Shared library for dynamic linking"
        公共库（例如 `libc.so`）在第一个调用它的进程启动时被加载到内存中，之后其他程序再次调用该公共库时，不需要再次将其加载到内存中，而只需要在新进程的页表中设置映射关系即可.

### Copy-on-Write


### Page Fault Handler

当需要将一个 page 加载进内存时：

- 在磁盘上找到该 page
- 找 Free Frame：
    - 如果存在空闲的 Frame，则分配给进程
    - 如果不存在，使用页替换策略选择一个 victim frame，如果 victim frame 被修改过，则先将其写回磁盘，然后分配给进程
- 将 page 内容加载到新分配的 Frame 中
- 重启被中断的进程

#### Page Replacement

当物理内存不足时，需要将某些 Page 从内存中换出到磁盘，以腾出空间.

Dirty Bit：当 Page 被修改时，设置 dirty bit，表示 Page 内容与磁盘中的内容不一致，需要被优先替换

替换算法：

- FIFO
- LRU
- Optimal

与 Cache 中的 [Block replacement](./chapter2.md#block-replacement) 策略类似.

LRU 实现：

- Counter-based
- Stack-based

精确实现 LRU 的开销较大，我们可以采用 LRU 的近似实现：Additional-Reference-Bit Algorithm.

Second-chance algorithm

Page-Buffering algorithm

#### Allocation of Frames

Fixed allocation

- equal allocation
- proportional allocation



## Non-Uniform Memory Access (NUMA)


## Thrashing

!!! definition "Thrashing"
    如果一个进程分配得到的 Page 过少，导致 page fault 频繁发生，就会导致 CPU 利用率降低，


## Kernel Memory Allocation

内核态的内存分配与用户态不同，不能使用 page fault 进行动态内存分配，
