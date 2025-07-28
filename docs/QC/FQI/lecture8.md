# Open Quantum Systems and Quantum Errors

## Errors in a Circuit Model

量子计算系统的误差来源：

- 量子态制备：制备过程中量子比特需要与其他事物相互作用，并且操作有时间限制，可能会引入噪声
- 量子门误差
- 测量误差
- 量子比特退相干


## Reduced Density Matrix

假设系统 $S$ 和环境 $E$ 的密度矩阵为 $\rho$

初态下，$\rho = \rho_S(0) \otimes \rho_E(0)$

$$
\rho_S(t) = \sum\limits_{k} E_k \rho_S(0) E_k^\dagger
\newcommand{\braket}[1]{\left\langle {#1} \right\rangle}
\newcommand{\ket}[1]{\left|{#1}\right\rangle}
\newcommand{\bra}[1]{\left\langle {#1} \right|}
$$

$E_k = \braket{e_k | U(t) | e_0}$ 被称作 Kraus 算符

## Qubit Decoherence

### 自发辐射 | Amplitude damping

### 相位涨落 | Phase damping

$$
\frac{1}{T_2^*} = \frac{1}{2T_1} + \frac{1}{\phi}
$$

## Master Equation

幺正变换对相应的密度矩阵的影响，被成为 Liouville 方程。

$$
    \dot{\rho} = -i [\mathcal{H}, \rho] = -i (\mathcal{H} \rho - \rho \mathcal{H}).
$$

量子电路必须是马尔可夫链。

$$
    \dot{\rho}(t) = -i [\mathcal{H}, \rho(t)] + \sum\limits_{k>0} \left[ L_k \rho(t) L_k^\dagger - \frac{1}{2} L_k^\dagger L_k \rho(t) - \frac{1}{2} L_k^\dagger L_k \right].
$$
