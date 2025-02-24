---
tags:
    - theory
---

# Introduction

## Evolution and Emerging Trends

### Performance

过去的处理器性能提升：

- Improvements of semi-conductor technology
- Improvements in computer architectures
    - Compilers and OS
    - RISC architectures
    - The exploitation of instruction-level parallelism (ILP)
    - The use of caches

!!! note "单核处理器性能曲线"


当前体系结构发展趋势：

- 新的性能模型：需要显式的并行编程
    - Data-level parallelism (DLP)
    - Thread-level parallelism (TLP)
    - Request-level parallelism (RLP)

- 多核处理器

!!! tip "系统性能指标"
    - **scalability**: 系统的规模增长能力
    - **extensibility**: 系统的功能扩展能力

!!! tip "并行"
    - Classes of parallelism in application-level:
        - Data-level parallelism (DLP)
        - Task-level parallelism (TLP)
    - Classes of parallelism in architecture-level:
        - Instruction-level parallelism (ILP)
        - Vector architecture / Graphics processing unit (GPU)
        - Thread-level parallelism (TLP)
        - Request-level parallelism (RLP)

!!! note "Flynn's taxonomy"
    - 单指令单数据流（SISD, Single Instruction stream, Single Data stream）
    - 单指令多数据流（SIMD, Single Instruction stream, Multiple Data streams）
        - Vector architecture
        - Multimedia extensions
        - Graphics processing unit (GPU)
    - 多指令多数据流（MIMD, Multiple Instruction streams, Multiple Data streams）
        - Tightly-coupled MIMD
        - Loosely-coupled MIMD

!!! definition "Bandwidth and Latency"
    - **Bandwidth**: 带宽，即在指定时间内设备的工作量
    - **Latency**: 延迟，也称响应时间

    目前带宽的改进速度远远超过延迟的改进速度：带宽的改进速度至少是延迟改进速度的平方.

### Power and Energy

#### System Level

从整个系统的角度看，功耗与能耗有三个常用评价指标：

- 最大功耗

- 热设计功耗（Thermal Design Power, TDP）

- 能耗和能效

在工作负载是恒定的情况（完成某一给定的任务）下，对比处理器指标时应使用能耗；而功耗主要作为一种对系统的约束条件.

#### Processor Level

处理器的能耗可分为动态能耗和静态能耗：

- 动态能耗：主要是开关晶体管时消耗的能量

    每个晶体管一次状态转换的动态能耗：$E_d \propto \frac{1}{2} \times \mathrm{Capacity Load} \times V^2$

    每个晶体管一次状态转换的动态功耗：$P_d \propto \frac{1}{2} \times \mathrm{Capacity Load} \times V^2 \times f$

Techniques for reducing power:

- Do nothing well: 关闭不必要的功能，例如关闭非活动模块的时钟
- Dynamic Voltage-Frequency Scaling: 在系统活跃程度较低时主动降频或降压
- Low power state for DRAM, disks
- Overclocking, turning off cores

- 静态功耗：晶体管关闭状态下的泄漏电流导致的能耗 $E_s \propto I_s \times V$.

    静态功耗与器件数目成正比. 可通过关闭非活动模块的电源来降低静态功耗.


### Cost

### Dependability | 可信任度

- 模块可靠性（Module reliability）
    - 平均无故障时间（Mean time to failure, MTTF）
    - 平均修复时间（Mean time to repair, MTTR）
    - 平均故障间隔时间（Mean time between failures, MTBF）
        - MTBF = MTTF + MTTR
- 模块可用性
    - 对于可修复的非冗余系统，$\mathrm{Availability} = \frac{\mathrm{MTTF}}{\mathrm{MTBF}}$.

## Performance

- Execution time
    - Wall clock time
    - CPU time
- Benchmark

!!! definition "Performance"

    $$
        \mathrm{Performance} = \frac{1}{\mathrm{Execution Time}}.
    $$

## Quantitative approaches

### CPU Performance

!!! definition "CPU Time"

    $$
        \mathrm{CPU Time} = \mathrm{CPU Clock Cycles} \times \mathrm{Clock Cycle Time}
                          = \frac{\mathrm{CPU Clock Cycles}}{\mathrm{Clock Rate}}.
    $$

!!! definition "Clocks per Instruction, CPI"
    
    $$
        \mathrm{CPI} = \frac{\mathrm{CPU Clock Cycles}}{\mathrm{Number of Instructions}}
    $$

    使用 $\mathrm{CPI}$ 可以计算 $\mathrm{CPU Time}$：

    $$
        \mathrm{CPU Time} = \mathrm{Number of Instructions} \times \mathrm{CPI} \times \mathrm{Clock Cycle Time}.
    $$

若考虑到不同指令的 $\mathrm{CPI}$ 不同，$\mathrm{CPU Time}$ 的公式可以更新为：

$$
    \newcommand{\dsum}{\displaystyle\sum}
    \mathrm{CPU Time} = \left( \dsum_{i=1}^n \mathrm{CPI}_i \times \mathrm{IC}_i \right) \times \mathrm{Clock Cycle Time}.
$$

提升性能的直接途径：

- 减少指令数（IC）
- 降低 CPI
- 提升主频

具体到各个领域：

- 算法 - 直接影响 IC，可能影响 CPI
- 程序设计语言 - 影响 IC 和 CPI
- 编译器 - 影响 IC 和 CPI
- 指令集体系结构 - 影响 IC, CPI, 主频

### Principles of Computer System Design

- Parallelism

- Principles of locality | 局部性原理

    - Time locality
    - Spatial locality

- Focus on the Common Case

    !!! theorem "Amdahl's Law"
        Amdahl's Law 定义了使用特定功能（对特定部分进行优化）可获得的加速比（Speedup）：

        $$
            \mathrm{Speedup} = \frac{\mathrm{Original Time}}{\mathrm{Optimized Time}} = \frac{1}{s + \frac{1-s}{p}},
        $$

        其中 $s$ 是不能被加速部分的执行时间，$p$ 是可以被加速的部分的加速比.

    Amdahl's Law 表明，优化某个部分的性能对系统整体性能的影响有上限. 若要有效地提升系统整体性能，我们需要优化执行时间占比较高的部分.

    除了适用于性能指标，Amdahl's Law 也适用于其他指标，如提升部件可靠性对系统整体可靠性的影响.

    需要注意的是，Amdahl's Law 仅适用于任务负载固定的情况，若任务量可变，则需要使用 Gustafson's Law.
