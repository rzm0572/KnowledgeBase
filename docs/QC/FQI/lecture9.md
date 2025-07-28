# Principles of Quantum Error Correction

## Representation of Quantum Errors

量子比特与环境的相互作用会改变量子比特和环境的量子态，从而导致误差。我们可以通过以下公式来描述这种相互作用：

$$
\newcommand{\ket}[1]{\left|{#1}\right\rangle}
\newcommand{\bra}[1]{\left\langle{#1}\right|}
\newcommand{\braket}[2]{\left\langle {#1} \right\rangle}
\begin{cases}
    \ket{e_0}\ket{0} \rightarrow \ket{e_1}\ket{0} + \ket{e_2}\ket{1} \\
    \ket{e_0}\ket{1} \rightarrow \ket{e_3}\ket{0} + \ket{e_4}\ket{1}
\end{cases}
$$

我们假设系统与环境之间的耦合是弱的，因此我们可以期待：

$$
\sqrt{\vert \braket{e_0 | e_1} \vert} \approx \sqrt{\vert \braket{e_0 | e_4} \vert} \approx 1, \\
\braket{e_2 | e_2}, \braket{e_3 | e_3} \le 1.
$$


## 3-Bit Repetition Code

考虑使用三个量子比特表示一位逻辑编码：

$$
    \ket{\overline{0}} = \ket{000}, \ket{\overline{1}} = \ket{111}.
$$

$\ket{000}, \ket{111}$ 张成编码空间，而 $\ket{001}, \ket{010}, \ket{011}, \ket{100}, \ket{101}, \ket{110}$ 张成非编码空间，若三个比特中有任意一个发生了翻转，则会从编码空间进入非编码空间。

我们可以使用以下电路来识别错误：

![pE5nKo9.png](https://s21.ax1x.com/2025/04/21/pE5nKo9.png)

![pE5nGQK.png](https://s21.ax1x.com/2025/04/21/pE5nGQK.png)


## Understanding Error Correction

如何将任意测量 U 转化为 Z 测量：

![pE5ut10.png](https://s21.ax1x.com/2025/04/21/pE5ut10.png)


