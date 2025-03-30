---
tags:
    - theory
---

# Cache

## Memory hierarchy

### Why memory hierarchy

在设计 CPU 时，我们通常会将内存视为理想的存储器，即：

- 无延迟
- 无限带宽
- 无限容量
- 成本为零

!!! definition "latency & bandwidth"
    - 延迟（Latency）：一次访存操作所需的时间.
    - 带宽（Bandwidth）：单位时间内可执行的访存操作数量.

很遗憾，现实中大容量的存储器通常延迟较高，低延迟或高带宽的存储器通常成本较高，因而我们无法造出理想的存储器. 对于目前的计算机系统，内存访问延迟比 CPU 时钟周期大两到三个数量级，因此我们需要引入内存层次结构来平滑 CPU 主频与内存访问速度之间的差异.

### How memory hierarchy works

TODO

通过利用局部性原理，我们希望：

- 使用较便宜的存储器来为用户提供尽可能多的内存容量
- 使用高速的存储器来实现高速的访存

## Cache

Cache 是一种容量小但高速的存储器，用于优化对内存的平均访问时间，它使用缓存技术提高经常访问的数据的访问速度.

广义上的 Cache 可以指任何用于优化下一级设备访问时间的设备，例如主存可以是硬盘的 Cache.

!!! note "Split cache & Unified cache"
    - Unified cache: 所有的内存请求（取指和取数据）都通过同一个 Cache 进行处理，硬件复杂度较低但速度较慢
    - Split cache: 将指令和数据存放在不同的 Cache 内，指令 Cache (I-Cache) 只读.

    现在的 CPU 通常在 L1 Cache 中采用 Split cache 策略，L2 和 L3 Cache 中采用 Unified cache 策略.

### Cache Performance

我们称 CPU 需要取的数据为 Requested Word，如果 Requested Word 存在于 Cache 中，我们称为 Cache Hit；否则称为 Cache Miss.

为衡量 Cache 的性能，我们需要定义以下几个指标：

- Hit Rate: 命中率，即在 Cache 中找到 Requested Word 的次数占总访问次数的比例.
- Miss Rate: 缺失率，即在 Cache 中未找到 Requested Word 的次数占总访问次数的比例.
- Hit Time: 命中时间，即访问 Cache 中的 Requested Word 所需的时间.
- Miss Penalty: 缺失代价，若 Cache Miss，则需要访问下一级的存储器，产生的额外时间开销被称为 Miss Penalty.

Cache 的性能可以用平均存储器访问时间来表示，即：

$$
    \text{Average access time} = \text{Hit Time} + \text{Miss Rate} \times \text{Miss Penalty}.
$$

若采用多级缓存，则可将 Miss Penalty 扩展为下一级 Cache 的 Average access time，例如：

$$
\begin{align*}
    \text{Average access time}_{L_1} &= \text{Hit Time}_{L_1} + \text{Miss Rate}_{L_1} \times ( \text{Hit Time}_{L_2} + \\
                                     & \quad\; \text{Miss Rate}_{L_2} \times \text{Miss Penalty}_{L_2} ).
\end{align*}
$$

Cache 与主存之间的数据交换通常以 Cache Block / Cache Line 为单位进行，当 Cache Miss 时，需要从主存中将包含 Requested Word 的 Cache Line 写入 Cache 中.

Miss Penalty 取决于：

- Latency: 从下一级存储器中读取 Requested Word 所属 Cache Line 的 First Word 所需的时间.
- Bandwidth: 将该 Cache Line 剩下的部分写入 Cache 所需的时间.

Cache Miss 产生的原因：

- Compulsory: 对 Cache Line 的第一次访问带来的 Miss.
- Capacity: Cache 容量不足时会选择性丢弃一部分 Cache Line，之后再次访问时会产生 Miss.
- Conflict: 
- Coherency: 多核系统中，为了保证不同核心中的 Cache 内容一致，需要进行缓存刷新

## Strategies for cache management

### Block placement

Block placement 解决 Cache 中如何存放 Cache Line 的问题.

- Direct mapped（直接映射）：内存中的每个块地址都对应 Cache 中的唯一位置

    映射模式：$\text{Block address}_{cache} = \text{Block address}_{mem} \bmod \text{num\_blocks}$

- n-way set associative（$n$ 路组相联）：内存中的每个块地址对应了 Cache 中 $n$ 个可以存放的位置

    映射模式：$\text{Block address}_{cache} \in \text{BlockSet}(\text{Block address}_{mem} \bmod \text{num\_sets})$

- fully associative（全相联）：内存中的每个块地址可以对应 Cache 中的任意位置

直接映射相当于 1 路组相联，全相联相当于 $m$ 路组相联（$m$ 为 Cache 可以容纳的块数）.

一般来说，路数 $n$ 越大，Cache Line 在 Cache 中可以存放的位置越多，因而 Cache 空间的利用率越高. 但实际上大部分的 Cache 采用的 $n \leqslant 4$，因为 $n$ 增大会导致 Block identification 变得复杂.

### Block identification

Block identification 解决如何在 Cache 中找到包含 Requested Word 的 Cache Line 的问题.

![pEsuZj0.png](https://s21.ax1x.com/2025/03/30/pEsuZj0.png)

假设内存地址位数为 $p$，Cache Line 大小为 $2^k$ Byte，Cache 大小为 $2^{k+m}$ Byte，采用 $n$ 路组相联，则

| Domain       | Size / bit        | Remark                      |
|--------------|-------------------|-----------------------------|
| Block offset | $k$               | 不属于 Block address        |
| Index        | $\frac{m}{n}$     | 决定该 Cache Line 属于哪个组 |
| Tag          | $p-k-\frac{m}{n}$ | 用于在访问时判断是否命中      |

采取不同块放置策略，得到的块识别电路如下：

=== "Direct mapped"

    ![pEsudED.png](https://s21.ax1x.com/2025/03/30/pEsudED.png)

=== "n-way set associative"

    ![pEsuwUe.png](https://s21.ax1x.com/2025/03/30/pEsuwUe.png)

=== "fully associative"

    ![pEsuDCd.png](https://s21.ax1x.com/2025/03/30/pEsuDCd.png)

可以看到，$n$ 较大的 Cache 的块识别电路复杂度较高.

### Block replacement

Block replacement 解决在 Cache 容量不足时，如何选择性地丢弃 Cache Line 的问题.

对于直接映射 Cache，由于每个块只能对应唯一的 Cache 位置，因此当 Cache 容量不足时，可丢弃的只有对应的唯一位置上的 Cache Line.

对于 $n$ 路组相联 Cache，可选的可丢弃 Cache Line 有 $n$ 个，以下是四种选择策略：

- Random replacement：随机选择一个 Cache Line 进行替换
- LRU (Least Recently Used)：选择上一次被使用的时间最早的 Cache Line 进行替换
- FIFO：选择最先进入 Cache 的 Cache Line 进行替换
- OPT（Optimal Replacement）: 选择在未来最长时间内不再被访问的 Cache Line 进行替换（一般只能进行预测）

!!! note "Belady's Anomaly"
    当 Cache 容量扩大时，有可能带来更多的 Cache Miss.

Beloady's Anomaly 导致我们无法通过单纯地增加 Cache 容量来解决性能问题，但是如果我们使用栈替换算法（Stack Replacement Algorithm），则可以解决这一问题：

!!! theorem "Stack Replacement Algorithm"
    栈替换算法保证在访问顺序相同的情况下，容量为 $n$ 的 Cache 在时刻 $t$ 时保存的 Cache Line 为容量为 $n+1$ 的 Cache 在时刻 $t$ 时保存的 Cache Line 的子集.

=== "LRU"

    LRU 是一种栈替换算法：

    ![pEsKroF.png](https://s21.ax1x.com/2025/03/30/pEsKroF.png)

=== "FIFO"

    FIFO 不是一种栈替换算法：

    ![pEsK5dO.png](https://s21.ax1x.com/2025/03/30/pEsK5dO.png)

!!! tip
    可以从访问序列的“可见窗口”的角度来区分 LRU 与 FIFO：

    - LRU 的可见窗口中包括了 $n$ 个不同的 Cache Line，且将可见窗口左端的 Cache Line 删除后，就只会包括 $n-1$ 个不同的 Cache Line.
    - FIFO 的可见窗口中包括了 $n$ 个不同的 Cache Line，且在可见窗口左端增加一个 Cache Line 后，就会包括 $n+1$ 个不同的 Cache Line.

LRU 的一种硬件实现方案是 Comparsion Pair Method:

TODO


### Write strategy

- Write-through（写直达 / 写穿透）：信息被写入 Cache 和下一级存储器

    - 可以直接把修改后的 Cache Line 从 Cache 中删除
    - Cache 控制位只需要 valid bit
    - 内存中的数据一定是最新的
    - 在 Write Miss 时采用 no-write allocate (write around) 策略，即直接将需要写入的 Cache Line 写入主存，不在 Cache 中保存

- Write-back（写回）：信息仅被写入 Cache，直到被替换出 Cache 才写入下一级存储器

    - 不能直接把修改后的 Cache Line 从 Cache 中删除
    - Cache 控制位需要 valid bit 和 dirty bit
    - 在 Write Miss 时采用 write allocate 策略，即将需要写入的 Cache Line 先写入 Cache，再在 Cache 中进行修改

=== "Write-through"

    ![pEsMpFg.png](https://s21.ax1x.com/2025/03/30/pEsMpFg.png)

=== "Write-back"

    ![pEsM9YQ.png](https://s21.ax1x.com/2025/03/30/pEsM9YQ.png)

!!! note "Write Buffer"
    TODO


