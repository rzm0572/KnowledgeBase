---
tags:
    - Theory
---

# Instruction-Level Parallelism (ILP)

!!! note "指令级并行（ILP）的常见技术"
    - 流水线（Pipeline）
    - 多发射（Multiple Issue）
    - SIMD（Single Instruction Multiple Data）

在系统 II 中我们实现了流水线 CPU，其性能可以用 CPI 来衡量：

$$
\begin{align*}
    \text{Pipeline CPI} = {}& \text{Ideal pipeline CPI} + \text{Structural stalls} + \\
                            & \text{Data hazard stalls} + \text{Control stalls}.
\end{align*}
$$

`Ideal pipeline CPI` 指的是流水线在没有任何停顿情况下的 CPI，这通常与 CPU 的时钟频率有关. 在时钟频率无法大幅提高的现状下，我们希望可以尽可能减少流水线 CPU 的停顿，从而提高性能.

## Dependence

依赖（Dependence）描述了一个程序中的指令之间的固有联系，若两条指令之间存在依赖关系，那么它们便不能直接并行执行. 通常有三种类型的依赖关系：

### 数据依赖（Data dependence）

若指令 A 比指令 B 先执行，且指令 A 的目的操作数与指令 B 的源操作数相同，则称指令 B 数据依赖于指令 A.

数据依赖具有传递性：若指令 C 数据依赖于指令 B，且指令 B 数据依赖于指令 A，则指令 C 数据依赖于指令 A.

!!! example

    ```assembly
    add x2, x1, x0
    sub x3, x2, x4
    ```

    `sub` 指令的源操作数 `x2` 依赖于 `add` 指令的结果.

两条数据依赖的指令必须按顺序执行，不能同时执行或完全重叠执行.

### 名称依赖（Name dependence）

- 反依赖（Anti-dependence）

    若指令 A 比指令 B 先执行，且指令 B 的目的操作数于指令 A 的源操作数相同，则称指令 A 与指令 B 之间存在反依赖关系.

    !!! example

        ```assembly
        add x2, x6, x4    # A
        add x6, x0, x12   # B
        add x8, x6, x14   # C
        ```

        指令 A 与 指令 B 之间存在反依赖关系，指令 C 数据依赖于指令 B.

- 输出依赖（Output-dependence）

    若指令 A 与指令 B 的目的操作数相同，则称指令 A 与指令 B 之间存在输出依赖关系.

    !!! example
    
        ```assembly
        add x2, x6, x4    # A
        add x6, x0, x12
        add x2, x6, x14   # B
        ```

        指令 A 与 指令 B 之间存在输出依赖关系.

解决方法：寄存器重命名

### 控制依赖（Control dependence）



减少控制依赖的一种方法：循环展开

## Hazard

依赖（Dependence）是程序的一种属性，而是否会导致冒险（Hazard）、冒险是否会导致停顿是由流水线 CPU 的架构决定的.

### 结构冒险（Structural hazard）

结构冒险的产生原因是不同指令并行执行时请求了相同的资源，通常通过 Stall 或指令重排来解决.

### 数据冒险（Data hazard）

在流水线处理器中，数据冒险通常出现在两条具有依赖关系的相邻指令之间，其产生原因是上一条指令在 EXE 或 MEM 阶段的执行结果尚未写回寄存器时，下一条指令已经进入 ID 阶段并读取寄存器中的值.

- Read after Write (RAW) Hazard：对应数据依赖
- Write after Read (WAR) Hazard：对应反依赖
- Write after Write (WAW) Hazard：对应输出依赖

解决方案：

1. 前递（Forwarding）：将上一条指令的计算结果直接作为下一条指令的原操作数.
2. 重排序（Code Scheduling）：将指令重新排序，使得具有依赖关系的指令不相邻.

### 控制冒险（Control hazard）

控制冒险的产生原因是 `npc` 取决于分支跳转指令在 EXE 阶段的执行结果.

最简单的方法是 Stall，但是当流水线较深时，Stall 带来的性能损失较大

![pE3VHW6.png](https://s21.ax1x.com/2025/02/26/pE3VHW6.png)

因此，我们需要引入分支预测.

## 分支预测

### 静态分支预测

基于典型的分支行为进行预测，如循环结构中的跳转指令等. 通常有以下三种策略：

- Predicted-not-taken：永远预测不跳转
- Predicted-taken：永远预测跳转

    Predicted-not-taken 和 Predicted-taken 在预测正确时不需要任何停顿，而在预测错误时需要将流水线清空并重新取指.

- Delayed branch

    在每条分支跳转指令后加入一个分支延迟槽（Branch Delay Slot），槽内的指令不论是否跳转都会被执行，因此要求与分支结构内的指令无依赖关系.

### 动态分支预测

分支预测错误在较深的流水线中会带来很大的性能损失，因此我们需要尽量减少分支预测的错误频率. 动态预测通过引入 Branch History Table (BHT) 和 Branch Predictor，利用历史信息来预测是否跳转，从而减少分支预测错误.

#### Branch History Table (BHT)

BHT 记录了分支跳转指令的地址和历史跳转信息

#### Branch Predictor

=== "1-bit Predictor"

    ![pE3CJjs.png](https://s21.ax1x.com/2025/02/26/pE3CJjs.png)


=== "2-bit Predictor"

    ![pE3CfUK.png](https://s21.ax1x.com/2025/02/26/pE3CfUK.png)

!!! note "为何通常采用 2-bit Predictor"
    - 执行嵌套循环时，1-bit Predictor 在内层循环结束，外层循环继续执行的过程中会出现两次预测错误，而 2-bit Predictor 只会出现一次预测错误（外层）.
    - 实验表明当 $n > 2$ 时，n-bit Predictor 相较于 2-bit Predictor 的预测成功率提升不大，但是硬件开销大大增加，因此通常采用 2-bit Predictor.

#### Branch-Target Buffer (BTB) 

BTB 用于保存预测的分支跳转目标地址


## 动态调度

采用动态调度（Dynamic Scheduling）的处理器硬件会重新安排指令的执行顺序以减少停顿，同时保持数据流与异常行为.

通常的流水线采用顺序执行模式，当某条指令因冒险而停顿时，后面的指令都不能执行，因而我们希望可以引入乱序执行（Out-of-order Execution）模式，使得一条指令可以在其操作数可用时立即执行，从而减少停顿.

!!! tip "乱序执行的相关术语"
    - issue: 指令发射，通常位于 ID 阶段
    - execute: 执行，通常位于 EXE 阶段
    - commit: 提交，通常位于 WB 阶段

一个完备的，不存在非精确异常的乱序执行流水线应当保证：

- 顺序发射（In-order Issue）
- 乱序执行（Out-of-order Execution）：减少停顿
- 顺序提交（In-order Commit）：保证异常处理是精确的

### Scoreboard Algorithm

![pEGwB7Q.png](https://s21.ax1x.com/2025/03/03/pEGwB7Q.png)

维护两个数据结构：

- 功能单元状态（Functional Unit Status）：记录各个功能单元的占用状态
- 寄存器结果状态（Register Result Status）：记录各个寄存器是否为某个执行单元的目的操作数

!!! key-point "Key Point"
    - 一条指令能否发射，一看前面是否还有指令未发射（必须保证顺序发射）；二看是否有功能部件空闲可用（识别结构冒险），这个信息包含在功能单元状态表中；三看指令要写的寄存器是否正要被别的指令写（识别 WAW 冒险），这个信息包含在寄存器结果状态表中
    - 一条指令能否读数，要看记分牌是否提示源寄存器不可读（识别 RAW 冒险），如果不可读，就说明该寄存器将要被别的前序指令改写，现在的指令要等待前序指令写回
    - 一条指令一旦读数完成，就必然可以进行运算，运算可以是多周期的，在第一个周期结束时应该改写功能状态，表明自己不再需要读寄存器
    - 一条指令能否写回，要看是否有指令需要读即将被改写的这个寄存器（识别 WAR 冒险），具体一点来说，就是要观察标记 Yes 的 $R_j$、$R_k$ 对应的寄存器里是否有当前指令的目的寄存器，如果有，就说明有指令需要读取寄存器的旧值，这样一来我们就要等指令读完旧值之后再写回

!!! not-advice "Scoreboard Algorithm 存在的问题"
    - WAR, WAW 冒险实际上不存在真数据依赖，但 Scoreboard 算法会因这两种冒险停顿，因而无法发挥乱序执行的全部功效
    - Scoreboard 算法采用乱序提交的方式，会产生非精确异常的问题，不利于调试与中断恢复

### Tomasulo Algorithm

![pEGw2cV.png](https://s21.ax1x.com/2025/03/03/pEGw2cV.png)
