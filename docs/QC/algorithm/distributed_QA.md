# A path towards distributed quantum annealing

## 理论部分

- 现有量子退火硬件的主要问题：embedding and connectivity
    - 目前最新的量子退火机能做到一个比特与约 15 个比特进行耦合
    - 一个解决方案是通过比特链的方法与更多的比特进行耦合，但是这样就占用了宝贵的量子比特资源，限制了可解决问题的规模

- 分布式量子操作的出现使得分布式量子计算成为可能，其中也包括分布式量子退火机
    - 需要通过经典或量子的通信信道实现，例如光纤中纠缠的光子对
    - 如何保证传输过程中的保真度是一个很大的问题

- 量子退火机由一个含时哈密顿量驱动：

    $$
    H(t) = \left(1 - \frac{t}{t_F}\right) H_0 + \frac{t}{t_F} H_F.
    \newcommand{\ket}[1]{\left|{#1}\right\rangle}
    \newcommand{\bra}[1]{\left\langle{#1}\right|}
    \newcommand{\braket}[2]{\left\langle{#1}\middle|{#2}\right\rangle}
    $$

  量子退火机的初态为 $H_0$ 的基态 $\ket{\psi_0}$，在 $t_F$ 时间内向 $H_F$ 的基态 $\ket{\psi_F}$ 演化。演化时间 $t_F$ 越长，最终状态越接近 $\ket{\psi_F}$。

- 量子退火机可以使用 transverse Ising model 来描述：

    $$
    H_0 = \sum_{i \in \Omega} \sigma_i^x \\
    H_F = \sum_{i \in \Omega} h_i \sigma_i^z + \sum_{(i,j) \in e_0} J_{ij} \sigma_i^z \sigma_j^z
    $$

    其中 $\Omega = (q_0, q_1, \ldots, q_{N - 1})$ 为退火机中量子比特的有序集，$h_i$ 描述了量子比特 i 上的本地场，$J_{ij}$ 描述了量子比特 i 和 j 之间的耦合强度，$e_0 = \{(i,j) \colon J_{ij} \neq 0\}$。

- QUBO 问题可以通过 Ising model 编码为哈密顿量，嵌入量子退火机进行求解，其中的关键步骤是将 QUBO 转化为问题图，然后再根据问题图进行哈密顿量的编码

    - 由于单个比特可以耦合的比特数较少，所以现在能解决的问题图被限制在稀疏图中

- 本文目标：提出分布式量子退火机的协议

    ![](https://s21.ax1x.com/2025/09/12/pVWuqQs.png)

- 假如我们要将原本在一个量子退火机上进行的演化拆分到两个量子退火机上运行，那么我们可以将原本的驱动哈密顿量 $H(t)$ 拆分为局部哈密顿量 $H_L(t)$ 和非局部哈密顿量 $H_N(t)$：

    $$
    H(t) = H_L(t) + H_N(t)
    $$

    我们仍然需要通过哈密顿量计算演化算符 $U(0, t_F)$。将时间分割为 $M$ 份，则

    $$
    \begin{align*}
    U(0, t_F) &= \mathcal{T}_t \exp \left\{ -\mathrm{i} \int_0^{t_F} H(t) \,\mathrm{d}t \right\} \\
                &= \mathcal{T}_{t_k} \prod_{k=0}^{M-1} U(t_k, t_{k+1})
    \end{align*}
    $$

    由于 $\Delta t_k = \dfrac{t_F}{M}$ 很短，因而 $U(t_k, t_{k+1})$ 可近似为 $U_N(t_k, t_{k+1}) \, U_F(t_k, t_{k+1})$，因此我们就有

    $$
    \widetilde{U}(0, t_F) = \mathcal{T}_{t_k} \prod_{k=0}^{M - 1} U_N(t_k, t_{k+1}) \, U_L(t_k, t_{k+1})
    $$

    $U_L$ 使用普通的量子退火机的实现即可，$U_N$ 可以使用已有的分布式量子操作协议（例如 telegate）来实现。因此，我们可以通过快速地在以上两种模式之间切换，实现分布式量子退火机

    - 这一步中引入的近似使用了 Trotter Product Formula，具有一定误差，我们需要估计其的误差界：

        假设 $H_{eq}(t_k)$ 表示采用近似手段后 $[t_k, t_{k+1}]$ 时间段内在整个退火机系统上的哈密顿量，设 $t_{k+1} - t_k = \Delta t < \Delta t_M$，其中

        $$
        \Delta t_M = \min_k \left( \Vert H_L(t_k) \Vert_L^{-1}, \Vert H_N(t_k) \Vert_L^{-1} \right)
        $$

        $\Delta t < \Delta t_M$ 是误差收敛的一个充分条件，在此条件下，$H_{eq}(t_k)$ 与真实哈密顿量 $H(t_k)$ 之间的误差满足

        $$
        \Vert H_{eq}(t_k) - H(t_k) \Vert_L \leqslant \frac{\Delta t}{(\Delta t_M)^2}, \enspace \forall \, 0 \leqslant k \leqslant M
        $$

- 为了将一个问题编码到分布式量子退火机上，我们需要对问题图进行分解：

    ![](https://s21.ax1x.com/2025/09/12/pVWuxoT.png)

    两种分解方式：

    - 边分解：将某一条边视为非局部耦合
    - 点分解：将某个节点拆分到两个退火机上，在初哈密顿量 $H_0$ 中保证两个拆分节点对齐（强纠缠）

- 分布式系统中的噪声

    - 由于 DCNOT 门中需要将一对量子比特纠缠至贝尔态再分发到两边，因而我们需要考虑传输过程中产生的噪声。考虑噪声后，这对量子比特的密度矩阵可写为：

        $$
        \begin{align*}
        \rho_\Phi &= x \ket{\Phi}\bra{\Phi} + (1-x) \left( \frac{1}{4}\ket{00}\bra{00} + \frac{1}{4}\ket{01}\bra{01} + \frac{1}{4}\ket{10}\bra{10} + \frac{1}{4}\ket{11}\bra{11} \right) \\
        &=x \ket{\Phi}\bra{\Phi} + \frac{1-x}{4} \mathbb{I}_{2\times2}
        \end{align*}
        $$

        满足保真度

        $$
        F_\Phi = \braket{\Psi}{\rho_\Psi | \Psi} = x + \frac{1-x}{4}
        $$

## 实验部分

采用 four qubit spin chain 作为实验对象

控制参数：

- 退火时间 $t_F$，控制绝热演化的速度
- Trotter 步数 $M$，决定了每个 Trotter 迭代的时间 $\Delta t = t_F / M$，控制 Trotter 演化的精度
- 分布式操作中的噪声水平，通过纠缠保真度 $F_\Phi$ 来控制

模拟指标：

- 保真度 $p_{*0}$：演化结束时的量子态与目标哈密顿量 $H_F$ 的基态的接近程度

    $$
    p_{*0} = \sum_{\ket{\phi_i} \in gs(H_F)} \braket{\phi_i}{\rho_*|\phi_i}
    $$

- 相对能量误差 $\epsilon_{*\cdot}$

    $$
    \epsilon_{*\cdot} = \frac{E_* - E_\cdot}{E_{\mathrm{mix}} - E_\cdot}
    $$

模拟结果：

![](https://s21.ax1x.com/2025/09/12/pVWKSFU.png)

- 当 $t_F$ 较小时，增加 $t_F$ 可以减小 $\epsilon_{T0}$，因为增加退火时间显然能拿到能量更低的结果
- $M$ 增大会导致 $\epsilon_{T0}$ 增大，因为每一步 Trotter 迭代都会引入纠缠误差，所以步数越多误差越大
- 在白色虚线下方 $\epsilon_{T0}$ 较上方小很多，$\Delta t$ 在白色虚线下方小于 1.0

可以发现在 $\Delta t < 1.0$ 时结果收敛，然而理论计算出的收敛界 $\Delta t_M = 0.5$
