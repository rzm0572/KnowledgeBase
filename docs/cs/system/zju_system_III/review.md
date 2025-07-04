# Review

## Architecture

### General Idea

- Amdahl's Law

### ILP

#### Dynamic scheduling

dependencies

hazards

- out-of-order execution
    - scoreboard algorithm
    - Tomasulo algorithm
    - Reorder buffering

#### Multiple Issue

Superscalar processor

### DLP & TLP

#### SIMD

Vector processor

Array processor

#### MIMD

Consistensy (x)

Coherence

- UMA (SMP)

- NUMA

Memory consistency v.s. Cache coherence

Cache Coherence Protocol

- Snooping Protocol
- MSI Protocol
- MESI Protocol

### Memory Hierarchy

Principle of locality

Block placement
Block identification
Block replacement
Write strategy

Average Memory Access Time

Cache Performance Analysis


## OS


### Memory Allocation

主要讲的是基于页表的虚拟内存映射

Memory Allocation?

first-fit
best-fit
worst-fit

fragmentation

- external fragmentation
- internal fragmentation

Solution: paging

#### Paging

Frame & Page

Page Table

Address Translation

Save page table in memory: we need a register to locate the root page table

Speed up virtual memory access: TLB

Effective Access Time

Memory Protection: permission bits in page table entry

hashed page table
inverted page table

### Virtual Memory

主要讲 Demand Paging

Lazy swapper
pre-paging

Out of Memory? Page replacement algorithm

- FIFO
- LRU
- OPT

- Belady's Anomaly

### Mass-storage System

Disk Scheduling: minimize seek time

- SCAN (elevator algorithm)

access time
disk bandwidth

### I/O

Polling / Interrupt / DMA

### File System

#### Concepts

File attributes
File operations

- create
- open
- read / write
- close

open-file table

- per-process table
    - permission
- system-wide table
    - physical location

Protection

- ACL
- Unix Access Control (9 bit)


#### Implementation

on-disk structure & in-memory structure

inode

Virtual File System (VFS): abstruction of different physical file systems

disk block allocation

- Linked Allocation
- Indexed Allocation
    - direct blocks
    - single indirect blocks (3 次访问)
    - double indirect blocks
    - triple indirect blocks

