# 非合作博弈

## 微观经济学

### 市场失灵

福利经济学第一定理成立的假设非常多：完全竞争、完全信息、无交易成本、无外部性、无规模经济等，而这些假设在现实中很难实现。在这些条件不满足的时候，就出现了市场失灵（market failure）。

!!! definition "垄断"
    一个产品只有一家厂商生产，故该厂商具有市场势力，自身可以决定产品价格，从而会破坏完全竞争市场的福利最优性。垄断厂商会通过提高价格攫取更多的消费者剩余。

!!! definition "外部性"
    外部性指一个人或一群人的行动和决策使另一个人或一群人受损或受益的情况，即社会成员从事经济活动时其成本与后果不完全由该行为人承担。

    公共物品的提供通常会带来正外部性，但也有可能带来公地悲剧，需要政府或社会组织的干预。

!!! definition "信息不对称"
    信息不对称是指交易双方掌握的信息不对等，导致市场参与者无法做出最优决策。

    在完全竞争市场中，暗含着假定厂商的成本函数在不同厂商之间是已知的，厂商对消费者的需求曲线也是已知的，消费者对厂商生产的产品带给自己的效用在购买前也是已知的；但在现实中，厂商的成本函数有可能是商业机密，消费者的消费策略也大概率是无法准确预测的。

    !!! note "信息不对称的影响"
        劣币驱逐良币，买家无法分辨产品质量，导致支付意愿下降，低价劣质产品在竞争中更占优势。

        保险中的逆向选择和道德风险问题：
        
        - 逆向选择：保险公司的保险产品定向筛选出了高风险的客户
        - 道德风险：保险公司无法监控客户的行为，导致客户在有保险后可能采取更高风险的行为

### 数据的特性

- 卖家垄断
- 零成本复制性
    - 供给曲线失效
    - 外部性增强

在垄断 + 零成本复制性的条件下，我们可以实施价格歧视（price discrimination）：

- 一级（完全）价格歧视：厂商完全掌握消费者偏好，将每个消费者的价格定在其最大支付意愿上，完全攫取消费者剩余；
- 三级价格歧视：根据消费者一些特征，如年龄、性别、地域等，对不同消费者群体收取不同价格（大数据杀熟，学生半价）；
- 二级价格歧视：按不同的价格出售不同数量的商品，但购买相同数量产品的人都支付相同的价格。一种巧妙的策略是设计一系列产品的特定组合，使得不同消费者根据自己的需求选择不同的组合（信息甄别），从而提升整体销量和利润。

效用不确定性

## 博弈论基本概念

微观经济学主要关注个人最大化自身效用的单人决策问题，但实际情况下每个人的决策会互相影响。

$$
    \max_{x_i \in X_i} u(x_i, x_{-i})
$$

$x_{-i} = (x_1, \cdots, x_{i-1}, x_{i+1}, \cdots, x_n)$ 表示其他所有人的决策。

垄断的类型：

- 垄断：只有一个厂商，完全控制市场价格和产量；

- 寡头垄断：少数几个厂商控制市场价格和产量，彼此之间存在竞争关系；

垄断和寡头垄断下，厂商都具有市场势力，但垄断厂商做决策只需要考虑消费者需求，而寡头垄断厂商则需要考虑其他寡头策略。

卡特尔（cartel）是指多个厂商联合起来形成的垄断组织，通过协商来控制市场价格和产量，通常是非法的。

策略式博弈（stragegic game）：要求博弈的参与人是理性且智能的

!!! definition "博弈"
    博弈可被表达为一个三元组 $G = (N, (S_i)_{i \in N}, (u_i)_{i \in N})$，其中

    - $N$ 表示参与人（player）集合，参与人个体记为 $i \in N$；
    - $S_i$ 表示 $i$ 号参与人可选的策略（strategy）集合；
    - $u_i : S_1 \times S_2 \cdots \times S_n \rightarrow \mathbb{R}$ 表示 $i$ 号参与人的报酬函数（payoff function）。

!!! definition "博弈的解"
    博弈的解或解概念（solution concept）是对于一个博弈的一种预期结果，通常是一个策略组合，即参与人的行动选择，或收益的分配结果。

博弈论的分类：

- 非合作博弈
    - 四大类博弈：完全信息静态博弈，完全信息动态博弈，不完全信息静态博弈，不完全信息动态博弈
- 合作博弈

## 占优策略均衡

!!! definition "严格劣策略"
    给定参与人 $i$ 的策略 $s_i$，如果他有另一个策略 $t_i$，使得对应任意的 $s_{-i} \in S_{-i}$，都有
    
    $$
        u_i(t_i, s_{-i}) > u_i(s_i, s_{-i}),
    $$

    则称 $s_i$ 是参与人 $i$ 的一个严格劣策略（strictly dominated strategy），称 $s_i$ 被 $t_i$ 严格占优（strictly dominated）或 $t_i$ 严格占优于（strictly dominates） $s_i$。

博弈论假定，理性人不会选择严格劣策略。“参与人是理性的”是共同知识

!!! definition "弱占优"
    给定参与人 $i$ 的策略 $s_i$，如果他有另一个策略 $t_i$，满足：

    1. 对于任意的 $s_{-i} \in S_{-i}$，都有 $u_i(t_i, s_{-i}) \geqslant u_i(s_i, s_{-i})$；
    2. 至少存在一个 $s_{-i} \in S_{-i}$，使得 $u_i(t_i, s_{-i}) = u_i(s_i, s_{-i})$；

    则称 $s_i$ 是参与人 $i$ 的一个弱劣策略（weakly dominated strategy），称 $s_i$ 被 $t_i$ 弱占优（weakly dominated）或 $t_i$ 弱占优于（weakly dominates） $s_i$。

一般而言，除非强调严格占优，否则默认占优是指弱占优。如果只采用严格劣策略，则结果不依赖于剔除的顺序，但如果引入弱劣策略，则剔除顺序可能影响结果。

## 纳什均衡

并非所有博弈都有占优策略。

!!! definition "最佳应对"
    令 $s_{-i}$ 为参与人 $i$ 之外的所有参与人的策略组合，参与人 $i$ 的策略 $s_i$ 是 $s_{-i}$ 的一个最佳应对（best response），如果满足

    $$
        u_i(s_i, s_{-i}) = \max_{t_i \in S_i} u_i(t_i, s_{-i}).
    $$

!!! definition "纳什均衡"
    一个策略组合 $s^*=(s_1^*, \cdots, s_n^*)$ 是一个纳什均衡，如果对于每个参与人 $i$ 和任意的策略 $s_i \in S_i$，都有

    $$
        u_i(s^*) \geqslant u_i(s_i, s_{-i}).
    $$

纳什均衡解法：求解博弈双方最优反应的交点，此时两个人的策略互为最优反应。

## 混合策略纳什均衡

!!! definition "混合策略"
    令 $G = (N, (S_i)_{i \in N}, (u_i)_{i \in N})$ 为一个策略型博弈。一个混合策略（mixed strategy）是 $S_i$ 上的概率分布，参与人 $i$ 的混合策略集记为

    $$
        \sum_i = \left\{
                     \sigma_i:S_i \rightarrow [0, 1]: \sum_{s_i \in S_i} \sigma_i(s_i) = 1
                 \right\}
    $$

    其中 $\sigma_i(s_i)$ 表示参与人 $i$ 在该混合策略下选择 $s_i$ 的概率。

!!! definition "混合扩展"
    令 $G = (N, (S_i)_{i \in N}, (u_i)_{i \in N})$ 为一个策略型博弈。$G$ 的混合扩展（mixed extension）是一个博弈

    $$
        \Gamma = \left(N, \left(\Sigma_i \right)_{i \in N}, (U_i)_{i \in N} \right)
    $$

    其中 $\sum_i = \Delta(S_i)$ 是参与人 $i$ 的混合策略集，他的收益函数 $U_i\colon \Sigma \rightarrow \mathbb{R}$ 将每个混合策略向量 $\sigma = (\sigma_1, \cdots, \sigma_n) \in \Sigma_1 \times \ldots \times \Sigma_n$ 映射到一个实数

    $$
        U_i(\sigma) = \mathbb(E)[u_i(\sigma)] = \sum_{s \in S} \prod_{j=1}^n \sigma_j(s_j) u_i(s_1, \ldots, s_n).
    $$

混合策略纳什均衡假设双方的混合策略是相互独立的。

!!! definition "混合策略纳什均衡"
    给定一个博弈的混合扩展 $\Gamma = (N, (\sum_i)_{i \in N}, (U_i)_{i \in N})$，一个混合策略向量 $\sigma^* = (\sigma_1^*, \cdots, \sigma_n^*)$ 是一个混合策略纳什均衡，弱对于每个参与人 $i$，有

    $$
        U_i(\sigma^*) \geqslant U_i(\sigma_i, \sigma_{-i}^*), \enspace \forall \sigma_i \in \sum_i.
    $$

!!! theorem "等价条件"
    令 $G = (N, (S_i)_{i \in N}, (u_i)_{i \in N})$ 为一个策略型博弈，$\Gamma$ 为 $G$ 的混合扩展。一个混合策略向量 $\sigma^*$ 是 $\Gamma$ 的混合策略纳什均衡，当且仅当对于每个参与人 $i$ 和每一个纯策略 $s_i \in S_i$，都有

    $$
        U_i(\sigma^*) \geqslant U_i(s_i, \sigma_{-i}^*).
    $$

混合策略纳什均衡不一定是一个好的均衡，效用不能保证最大化。

!!! theorem "无差异原则"
    令 $\sigma^*$ 为一个混合策略纳什均衡，$s_i$ 和 $s'_i$ 为参与人 $i$ 的两个纯策略，若 $\sigma_i^*(s_i), \sigma_i^*(s'_i) > 0$，则 $U_i(s_i, \sigma_{-i}^*) = U_i(s'_i, \sigma_{-i}^*)$。


无差异原则是混合策略纳什均衡的必要不充分条件。

!!! theorem "纳什定理"
    每一个策略型博弈 $G$，如果参与人的个数有限，每个参与人的纯策略数目有限，那么 $G$ 至少有一个混合策略纳什均衡。


## 完全信息动态博弈

参与人多轮交互，整体博弈被表达为一颗树，这一类博弈被称为扩展式博弈（extensive-form game）。扩展式博弈每个参与人的策略是一个向量，表示其在所有可能行动的节点上的行动。

完美信息博弈（game with perfect information）：每个参与人在选择行动时，都知道他位于博弈树的哪个节点上。

!!! definition "子博弈完美均衡"
    在扩展式博弈 $\Gamma$ 中，一个策略向量 $\sigma^*$ 是子博弈完美均衡（subgame perfect equilibrium），如果对于博弈的任意子博弈 $\Gamma(x)$，局限在那个子博弈的策略向量 $\sigma^*$ 是 $\Gamma(x)$ 的纳什均衡。

均衡精炼（equilibrium refinements）：当一个博弈存在不止一个均衡时，我们希望基于合理的选择标准选择一些均衡，而剔除另一些均衡。

!!! theorem
    令 $\sigma^*$ 是扩展式博弈 $\Gamma$ 的纳什均衡，如果对所有 $x$ 都有 $P_{\sigma^*}(x) > 0$，则 $\sigma^*$ 是子博弈完美均衡。

!!! corollary
    完全混合的纳什均衡是子博弈完美均衡。

求解方法：逆向归纳法（backward induction）

!!! theorem
    每个有限完美信息扩展式博弈都至少有一个子博弈完美纯策略均衡。

!!! example "斯塔克尔伯格模型"
    斯塔克尔伯格模型（产量领导模型）常用于描述有一家厂商处于支配地位或充当自然领导者的行业。


## 不完全信息博弈

在策略式博弈的基础上，不完全信息博弈需要引入每个参与人的类型集合 $(T_i)_{i \in N}$ 和类型的先验分布 $p$（为每种类型向量 $(t_1, \cdots, t_n)$ 分别赋予一个概率）。

- 先验分布 $p$ 是所有人的共同知识

- 每个人的类型不是共同知识，参与人知道自己的类型，但参与人仍然需要为每个类型都定义策略，因为其他人不知道自己的类型。

- 参与人 $i$ 类型为 $t_i$ 下选择纯策略 $s_i$ 的概率记为 $\sigma_i(t_i; s_i)$。

- 效用与所有人的类型相关，但策略只与自己的类型相关。

参与人的期望收益函数为

$$
    U_i(t; \sigma) = \sum_{s \in S} \prod_{i=1}^n \sigma_i(t_i; s_i) u_i(t; s).
$$

由于参与人不知道其他参与人的类型，因此需要对类型取期望：

$$
    U_i(\sigma) = \mathbb{E}_{t_{-i}} U_i(t_i, t_{-i}; \sigma) = \sum_{t_{-i}} p(t_{-i} \mid t_i) U_i(t_i, t_{-i}; \sigma).
$$

!!! note "不完全信息博弈的三个阶段"
    - 事前阶段：每个人的类型被指派前的阶段
    - 事中阶段：每个人的类型被指派后的阶段
    - 事后阶段：收益确定之后的阶段
