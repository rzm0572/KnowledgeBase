---
tags:
  - draft
---


# 人工智能引论

!!! note
    纯补天产物，仅供参考。

## 知识表达与推理

目前，代表性的知识表达方法有命题逻辑、谓词逻辑、产生式规则、框架表示法及知识图谱

### 命题逻辑

- 逻辑：进行正确推理和充分论证的研究

- 命题：能确定为真或为假的陈述句

    - 原子命题：不包含其他命题作为其组成部分的命题，又称简单命题
  
    - 复合命题：包含其他命题作为其组成部分的命

    - 命题联结词：与、或、非、条件、双向条件

    ![pE9pdRP.png](https://s21.ax1x.com/2025/01/05/pE9pdRP.png)

- 逻辑等价：给定命题 $p$ 和命题 $q$，如果 $p$ 和 $q$ 在所有情况下都具有相同的真假结果，那么 $p$ 和 $q$ 在逻辑上等价，记作 $p \equiv q$

- 逻辑推理：逻辑推理是根据某种特定策略，从前提出发推出结论的过程。用符号 $\implies$ 来表示推理过程，$\implies$ 的左侧表示推理的前提，$\implies$ 的右侧表示推理的结论。

    - 假言推理：$\alpha \rightarrow \beta, \alpha \implies \beta$.
    - 与消解：$\alpha_1 \land \alpha_2 \land \cdots \land \alpha_n \implies \alpha_1, \alpha_2, \cdots, \alpha_n$
    - 与导入：$\alpha_1, \alpha_2, \cdots, \alpha_n \implies \alpha_1 \land \alpha_2 \land \cdots \land \alpha_n$
    - 双重否定：$\lnot (\lnot \alpha) \implies \alpha$
    - 单项消解：$\alpha \lor \beta, \lnot \beta \implies \alpha$
    - 归结：$\alpha \lor \beta, \lnot \beta \lor \gamma \implies \alpha \lor \gamma$

### 谓词逻辑

- 命题逻辑无法表达局部与整体、一般与个别的关系

- 谓词逻辑：刻画主体（个体和群体）之间逻辑关系的方法，原子命题被分解为个体、谓词和量词

- 个体：所研究领域中可以独立存在的具体或抽象的概念

- 谓词：用来刻画个体属性或者描述个体之间 **关系** 存在性的元素，其值为真或为假，是一个从定义域到 $\{T, F\}$ 的映射

- 量词：用于描述群体的共同属性

    - 全称量词：表示一切的、所有的、凡是、每一个等，用符号 $\forall$ 表示
    - 存在量词：表示存在、有一个、某些等，用符号 $\exists$ 表示

- 只包含个体谓词和个体量词的谓词逻辑称为一阶谓词逻辑

- 约束变元和自由变元：约束变元受到全称量词或存在量词的约束，自由变元则不受约束

- 谓词逻辑推理：

    - 全称量词消去：$\forall x P(x) \implies P(y)$
    - 全称量词引入：$P(y) \implies \forall x P(x)$
    - 存在量词消去：$\exists x P(x) \implies P(c)$
    - 存在量词引入：$P(c) \implies \exists x P(x)$

### 知识图谱推理

- 知识图谱：有向图，用于描述现实世界中实体与实体之间的关系

    - 节点：表示客观世界中的一个实体
    - 两个节点之间的连线：表示节点之间的某种关系

- 知识图谱中存在连线的两个实体可表达为三元组形式，这种三元组也可以表示为一阶逻辑（FOL）的形式

- 知识图谱推理：根据已有的知识图谱推导出节点之间的其他关系，常见的两种方法是归纳逻辑程序设计和路径排序算法

- 归纳逻辑程序设计（ILP）：使用一阶谓词逻辑进行知识表示，通过修改和扩充逻辑表达式对现有知识归纳，完成推理任务。

- FOIL（一阶归纳学习器）：ILP 的代表性方法，通过 **序贯覆盖** 实现推理，目标是得到以目标谓词 $P$ 为结论的推理规则

    - 规则头：目标谓词 $P$，需要得到一个以 $P$ 为结论的推理规则
    - 训练样例：
        - 正例集合 $E^+$：已有知识图谱中满足 $P$ 的实例
        - 反例集合 $E^-$：已有知识图谱中与 $P$ 矛盾的实例，通常通过两个已知存在与 $P$ 矛盾的关系的实体来构造
        - 背景知识集合：知识图谱中的其他谓词实例化结果
    - 推理过程：从空集开始，逐步添加目标谓词的前提约束谓词，直到构成的推理规则不覆盖任何反例
    - 决策依据：信息增益值

        $$
            \mathrm{FOIL\_ Gain} = \hat{m_+} \cdot \left(\log_2 \frac{\hat{m_+}}{\hat{m_+} + \hat{m_-}} - \log_2 \frac{m_+}{m_+ + m_-} \right)
        $$

        每次选择信息增益值最大的谓词加入前提约束谓词集合，加入后需要将不符合该形式的实例从 $E^+$ 和 $E^-$ 中删除，进行下一轮迭代.

- 路径排序算法（PRA）：将实体之间的关联路径作为特征，来学习目标关系的分类器

    - 确定目标关系和训练样例（正例、负例）
    - 特征抽取：生成并选择路径特征集合
    - 特征计算：计算每个训练样例对应的特征值
    - 分类器训练：根据特征值和样例类型训练目标关系的分类器
    - 预测：对于测试样例，根据路径计算特征值，使用分类器进行预测

- 分布式表示下的知识推理：将个体和谓词抽象为分布式表达（向量表达），然后学习度量函数，与自然语言处理的方法类似

### 概率推理

- 概率图：用概率描述两个相连节点之间的关联，称为概率图，反映了推理过程中的不确定性. 一般分为 **贝叶斯网络** 和 **马尔可夫网络** 两大类.

- 贝叶斯网络：有向无环图，用有向边来表示节点和节点之间的单向概率依赖.

    - 贝叶斯网络满足局部马尔可夫性：给定一个节点的父节点的情况下，该父亲节点有条件地独立于它的非后代节点（Hint：计算条件概率的时候可以不用考虑其父节点之外的条件）

- 马尔可夫网络：无向图，用无向边来表示节点和节点之间的双向概率依赖.

    - 马尔可夫逻辑网络：在概率统计中引入谓词逻辑而融入结构化知识，在一阶谓词逻辑中添加不确定性而对严格推理进行松绑，可以更好的反映客观世界的复杂性
    - 给定一个由若干规则构成的集合，给集合中的每条推理规则赋予一定的权重，则可通过以下公式计算断言 $x$ 成立的概率：

        $$
            P(X = x) = \frac{1}{Z} \exp \left( \sum_{i} w_i n_i(x) \right)
        $$

        其中 $Z = \displaystyle\sum_{x \in \mathscr{X}} \exp \left( \sum_{i} w_i n_i(x) \right)$ 为归一化常数，$n_i(x)$ 为推导 $x$ 所涉及的第 $i$ 条规则的取值，$w_i$ 为规则 $i$ 的权重

        计算方法：根据给定条件确定上式中的逻辑表达式取值

### 因果推理

- 辛普森悖论：在总体样本上成立的某种关系在分组样本中不一定成立，甚至结论相反. 说明忽略某些某些因数可能会改变已有的结论

- 变量关联关系的三种来源：

    - 因果关联：$T$ 导致 $Y$，则称 $T$ 与 $Y$ 之间存在因果关联
    - 混淆关联：$T$ 与 $Y$ 有共同的原因变量，则称 $T$ 与 $Y$ 之间存在混淆关联
    - 选择关联：$T$ 与 $Y$ 有共同的结果变量，则称 $T$ 与 $Y$ 之间存在选择关联

- 因果推理：从复杂数据中甄别因果关联，常见模型有潜在结果框架（Neyman-Rubin 因果模型）和结构因果模型

- （结构因果模型中的）因果图：有向无环图

    - 使用因果图可以方便地表示多个变量的联合概率分布：

        $$
            P(x_1, x_2, \cdots, x_n) = \prod\limits_{j=1}^n P(x_j \mid x_{pa(j)})
        $$

        图模型上的独立性假设将“高维”估计问题转化为“低维”概率分布问题.

        维度灾难：当变量数较多、数据量不足、因果图结构未知时，估计联合概率分布十分困难

- 随机对照实验：影响结果的输入变量只有一个是随机变化的，其他均保持不变，那么结果变量的变化一定是由该输入变量引起的.

- 因果干预：改变明确存在关联关系的某变量取值，研究变量取值改变对结果变量的影响.

- do 算子：计算当系统中一个变量取值发生变化、其他变量保持不变时，系统输出结果是否变化

    具体地，对 $X$ 进行干预时，引入 do 算子，记作 $do(X=x)$. $P(Y=y \mid do(X=x))$ 表示对 $X$ 进行干预后，令其为 $x$ 时，$Y$ 取值为 $y$ 的概率.

- 因果效应：$P(Y=y \mid do(X=x))$

    - 可用引入干预后的操纵图模型中的条件概率（即操纵概率） $P_m(Y=y \mid X=x)$ 来表示
    - 操纵图：将因果图中所有指向被干预变量 $X$ 去除后，剩下的图被称为操纵图

    - 调整公式：

        $$
            P(Y=y \mid do(X=x))
            = P_m(Y=y \mid X=x)
            = \sum\limits_{z} P(Y=y \mid X=x, Z=z) \cdot P(Z=z)
        $$

    - 因果效应的通式：设 $PA$ 为 $X$ 的父节点集合，则 $X$ 对 $Y$ 的因果效应可以表示为：

        $$
            P(Y=y \mid do(X=x)) = \sum\limits_{z} P(Y=y \mid X=x, PA=z) \cdot P(PA=z)
        $$

??? note "因果图与操纵图"

    === "因果图"

        ![pE9Fs4H.png](https://s21.ax1x.com/2025/01/05/pE9Fs4H.png)

    === "操纵图"

        ![pE9FfDf.png](https://s21.ax1x.com/2025/01/05/pE9FfDf.png)
    
- 反事实计算步骤：溯因、动作、预测

## 搜索与问题求解

- 搜索算法的评价指标：完备性、最优性、时间复杂度、空间复杂度

- 边缘集合（开表）

- 如何扩展节点往往是搜索算法需要解决的核心问题

- 无信息搜索：DFS、BFS

- 有信息搜索（启发式搜索）：贪婪最佳优先搜索、A* 算法、IDA* 算法

- 启发函数 $h(n)$：估计从节点 $n$ 到目标节点还需付出多少代价.

- 评价函数 $f(n)$：从当前节点 $n$ 出发，根据评价函数来选择后续结点.

### 贪婪最佳优先搜索

特点：启发函数与评价函数相同，每次都选离目标节点最近的节点作为后继节点

采用排除环路的剪枝方法，是完备的，但不是最优的

时间复杂度和空间复杂度最坏情况下均为 $O(b^m)$.

### A* 算法

定义路径代价函数 $g(n)$：从起点 $s$ 到节点 $n$ 的路径代价

评价函数满足 $f(n) = g(n) + h(n)$

A* 算法的完备性和最优性取决于搜索问题和启发函数的性质：

$h*(n)$：从节点 $n$ 出发到达终止节点的最小代价

- 可容性：$h(n) \leqslant h*(n)$

- 一致性：$h(n) \leqslant c(n, a, n') + h(n')$ （满足三角不等式）

一致性蕴含可容性，可容性可以导出 A* 算法的最优性.

A* 算法的完备性条件：

- 搜索树中分支数量是有限的，即每个结点的后继结点数量是有限的

- 单步代价的下界是一个正数

- 启发函数有下界

### 最小最大搜索 Minimax

信息确定、全局可观察、竞争对手轮流行动、输赢收益零和假设下的二人博弈问题，采用对抗搜索方法

Minimax 搜索就是在计算搜索树中每个结点分数之后，每一步由玩家根据自己的角色来选择使得分数最大或分数最小的行动. 若对手总是理性地采取最优策略，那么 Minimax 算法必然会选择最优策略.

Minimax 搜索算法对游戏搜索树执行了一个完整的深度优先搜索. 时间复杂度 $O(b^m)$，空间复杂度 $O(bm)$.

### Alpha-Beta 剪枝算法

搜索树极大的情况下，Minimax 无法在合理时间内返回结果，因此引入 Alpha-Beta 剪枝算法来减少搜索树的大小.

### 蒙特卡洛树搜索 MCTS

智能体对环境并没有完整的认识，需要实际尝试后才能确定

- 奖励：从第 $i$ 个选项中获得收益分数的分布为 $D_i$，均值为 $\mu_i$

- 悔值函数（regret function）：前 $T$ 次操作中最优策略的期望得分减去智能体的实际得分，即

    $$
        \rho_T = T \mu^* - \displaystyle\sum_{t=1}^T \hat{r}_t
    $$

## 机器学习

## 神经网络与深度学习

- 赫布理论：神经元之间持续、重复的经验刺激可导致突触传递效能增加

## 强化学习

## 人工智能博弈
