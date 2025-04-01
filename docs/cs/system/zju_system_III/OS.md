
Resourse allocator

Devices:

Each device controller is in charge of a particular device type

Each device controller has a local buffer

I/O: between the device and local buffer of the controller

CPU moves data between main memory and device buffer by MMIO (memory-mapped I/O)

DMA (direct memory access): asynchronous
    CPU sends a request to DMA controller
    DMA controller moves data in main memory
    When DMA controller finishes, it sends an interrupt to CPU

SMMU (System-level Memory Management Unit)

Multi-core CPU:
    Synchronization problem in OS kernel

用户态抢占：运行中的用户态进程可以被其他进程抢占，以便让其他进程运行

内核抢占（kernel preemption）：内核中的进程可以被抢占，以便让其他进程运行

Dual-mode operation:
    User mode and kernel mode
    Page table management is in kernel mode

    a mode bit distinguishes when CPU is in user or kernel mode
    system call changes mode to kernel, return from call resets it to user

线程用于处理高并发的需求，避免了进程创建和调度的开销
线程共享页表
各个线程的栈是专用的（dedicated），但是不是隔离的（isolated），它们处于同一个虚拟地址空间中

宏内核
微内核：将部分非特权态的功能移到用户态，需要更多次上下文切换，但是更加安全
外内核：专用于特定应用的内核，只提供 machanism，不提供 policy，用户态程序通过内核提供的接口直接操作外设（例如数据库应用）
