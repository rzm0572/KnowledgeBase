# File System

## Directory

目录是一种特殊的文件，存储了一个用户可读的文件名列表，以及文件名到 INode 编号的映射，而 INode 则存放了文件的元数据，包括文件大小、创建时间、访问时间、权限等。

创建文件相当于创建一个 INode，并将其链接到文件系统的目录中

- Single-level Directory: 所有的文件存放在同一个目录下，不同用户的文件也不能重名
- Two-level Directory: 每个用户拥有其自己的目录
- Tree-structured Directory: 目录可以有多层结构，每个目录可以有自己的子目录
    - 文件可以使用绝对路径或相对路径来指定位置
    - 删除目录：
        - 若目录为空，则直接删除
        - 若目录不为空，则需要先删除其下所有文件和子目录，然后再删除该目录
    - 不能跨目录共享文件
- Acyclic-Graph Directory（无环图型目录）：两个不同的目录可以存在指向同一个文件的指针，通过链接的形式来实现
    - 存在悬挂指针的问题：若两个目录存在指向同一个文件的指针，则通过某个目录的指针删除文件后，另一个目录下的指针便产生悬挂问题
    - 解决方法：
        - Backpointer
        - Reference Counting: 每个文件都有引用计数，当引用计数为 0 时，删除文件
    - Share file
        - Hardlink: 将文件名指向要链接的文件的 INode，会改变引用计数
        - Softlink: 创建一个软链接文件，拥有独立的 INode 和数据块，数据块中存放要链接的文件的目录，不改变引用计数
        - 在通过源目录中的文件名删除文件时，Hardlink 指向的文件不会被删除，并且仍然可以访问，而 Softlink 指向的文件会被删除，无法再访问
        - Softlink 访问耗时较长，但可以跨文件系统

!!! note
    ![pET3ae1.md.png](https://s21.ax1x.com/2025/04/27/pET3ae1.md.png)

## Operation

- Open: 打开文件，获得文件操作句柄
    - `open` 函数打开文件的过程：
        - 首先查找 Open File Table，检查该文件是否已经被打开
        - 

- Read: 从文件中读取数据
    - `read` 函数可以从文件中读取数据
- Seek: 移动文件指针到指定位置
    - `lseek` 函数可以移动文件指针，但事实上不会移动磁盘上的读写头
- Write: 向文件写入数据
    - `write` 函数可以向文件写入数据，但不会立即写入磁盘，而是先写入缓冲区，只有缓冲区满了才会写入磁盘
    - 若需要立即写入磁盘，可以使用 `fsync` 函数
- Delete
    - rm 命令：Unlink file，并且更新文件的引用基数
- link
    - hardlink
    - symbolic link


## File System

文件系统在被使用之前必须先被挂载

### File System Structure

操作系统一般会支持多种文件系统

文件系统一般存储在二级存储设备（如磁盘）中，磁盘向操作系统提供读写 disk block 的接口，文件系统向用户程序提供存储设备的接口，将逻辑块映射到物理块

FCB（File Control Block）：文件控制块，记录文件的元数据，包括文件名、文件大小、创建时间、访问时间、权限等，INode 就是一种 FCB

FCB 结构：

![pEHdT6x.md.png](https://s21.ax1x.com/2025/04/30/pEHdT6x.md.png)

### On-Disk File System Structures

磁盘上有几种 control block：

- boot control block：记录操作系统的启动信息，一般存在于系统卷上
- volume control block (superblock)：记录卷的元数据，如总块数，空余块数，块大小，空闲块指针，空闲 FCB 数，空闲 FCB 指针等
- directory structure
- per-file FCBs

当 FCB 指向的文件大小较小时，可以将文件内容直接存储在 FCB 的特定段中，节省空间开销

### In-Memory File System Structures

- Mount table
- system-wide open-file table
- per-process open-file table
- I/O memory buffers


## Protection

ACL（Access Control List）：为每个文件或目录分配一个 ACL，控制用户对文件和目录的访问权限

优点是可以实现细粒度的权限控制，缺点是难以管理，并且需要大量额外空间开销。

Unix Access Control: 基于用户、组、其他三种身份来控制权限，每个文件或目录都有三个权限位（Read、Write、Execute）

使用 3 位八进制数来表示权限

目录的 Execute 权限表示可以进入目录


文件描述符泄露：Fork 时，子进程会复制父进程的 File Descriptor，并且不是深拷贝，也就是说子进程和父进程的文件描述符指向 Open File Table 中的同一项，即共享同一个文件操作句柄。为了避免这种情况，需要在 fork 之后，子进程关闭父进程的 File Descriptor 再重新申请。


