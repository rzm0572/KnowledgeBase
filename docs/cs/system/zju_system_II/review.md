# Review

Address Mode

- Immediate addressing
- Register addressing
- Base-relative addressing
- PC-relative addressing

- Register operand
- Memory operand
- Immediate operand

## Pipeline

stage | segment: 指令执行的某个阶段

较长的阶段会称为流水线的瓶颈（bottleneck）.

Static pipeline
Dynamic pipeline

Linear pipeline
Nonlinear pipeline

Implementation of Pipelining

Pipeline Performance

Throughput: 执行的指令数 / 执行时间

上界是时钟周期的倒数

Speedup: 单周期 CPU 执行时间 / 流水线 CPU 执行时间

上界是流水线的段数

Efficiency: 指令占用的时空资源 / 总时空资源

上界是 1

## Pipeline Hazard

Structural Hazard: 两条指令在同一时钟周期内使用同一物理资源

解决方案：

- Stall
- 增加物理资源

Data Hazard: 指令依赖于未完成的指令的结果

解决方案：

- Forwarding
- Stall

Stall: 增加 nop 指令

Control Hazard:

解决方案：

- Forwarding Branch to earlier stage
- Branch Prediction
  - 1-bit predictor
  - 2-bit predictor
- Stall

## Nonlinear Pipeline

Reservation table
Conflict vector
State transition graph

禁止集：两条一定会冲突的指令相隔的指令数的集合
由禁止集确定初始冲突向量

## Instruction-Level Parallelism

- Pipeline
- Multiple Issue 多发射

Static multiple issue: 编译器把可以并行的指令打包成发射槽

Cons:

- Not all stalls are predictable
- Branch
- 编译器负担过重

Dynamic multiple issue:

multiple-issue precessor:

- Superscalar processor 超标量处理器
- VLIW (Very Long Instruction Word) 超长指令字
- Super pipeline

## Exceptions and Interrupts

Software-Hardware Collaboration

## OS

### System Call

Kernel 给用户态程序提供的接口

调 System Call 时，用户态的程序的上下文保存在 Per-thread kernel stack 上，

## OS Structure

System Service

## Process

进程是资源分配的单元

元信息放在 PCB (Process Control Block) 中

Process State:

- New
  Fork
  Execve

- Ready
- Running
- Waiting

- Terminated
  Signal
  Zombie
  Orphan

Context Switch:

需要切换栈和 PC.

User space context 保存在 kernel stack 上，Kernel space context 保存在 PCB 上.

## Schedule

甘特图

average waiting time: 平均等待时间
average turnaround time: 平均周转时间

## IPC

## Thread

线程是执行单元.

不同线程可以共享 Heap，但是不能共享 Stack

不能共享：

- Process ID
- Stack
- PC
- Register

weak isolation

In Linux, PID 是第一个线程的 TID.

## Synchronization

Critical Section

## Deadlock
