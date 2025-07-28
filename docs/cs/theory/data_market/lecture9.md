# 贝叶斯劝说

## 问题定义

### 问题样例

- 参与人：导师，企业
- 策略：（导师）写推荐信，（企业）雇佣学生
- 类型：学生能力（优秀，平均）
    - 学生能力对导师来说已知，但对企业是不完全信息
- 企业和导师对学生类型有共同的先验分布
- 效用函数：用 $R$ 表示写推荐信，$H$ 表示雇佣学生，$E$ 表示学生的类型为优秀
    - 导师：$u_1(R, H) = 1, \enspace u_1(\overline{R}, H) = 0, \enspace u_1(R, \overline{H}) = 1, \enspace u_1(\overline{R}, \overline{H}) = 0$
    - 企业：$u_2(H, E) = 1, \enspace u_2(\overline{H}, E) = 0, \enspace u_2(H, \overline{E}) = -0.5, \enspace u_2(\overline{H}, \overline{E}) = 0$

导师的策略是通过好或坏的推荐信来向企业表明学生是优秀的或一般的，可被称为发信号. 形式化地，导师写推荐信的策略是两个条件概率分布 $\pi(\cdot \mid E)$ 和 $\pi(\cdot \mid \overline{E})$：

$$
    \pi(e \mid E), \enspace \pi(\overline{e} \mid E), \enspace \pi(e \mid \overline{E}), \enspace \pi(\overline{e} \mid \overline{E})
$$

其中 $e$ 表示推荐信描述学生为优秀类型，$\overline{e}$ 表示推荐信描述学生为一般类型.

**P.S.** 导师的策略是企业看到推荐信前就已知的.

### 推荐类型

!!! example "诚实推荐"
    导师为优秀的学生写好的推荐信，为一般的学生写一般的推荐信. 信号机制为：

    $$
        \pi(e \mid E) = 1, \enspace \pi(\overline{e} \mid E) = 0, \enspace
        \pi(e \mid \overline{E}) = 0, \enspace \pi(\overline{e} \mid \overline{E}) = 1
    $$

    此时企业会雇佣所有推荐信为 $e$ 的学生，不雇佣所有推荐信为 $\overline{e}$ 的学生.

    导师的效用：$\pi(E)$.

    企业的效用：$\pi(E)$.

!!! example "完全不诚实推荐"
    导师为所有的学生写好的推荐信，不管学生的类型. 信号机制为：

    $$
        \pi(e \mid E) = 1, \enspace \pi(\overline{e} \mid E) = 0, \enspace
        \pi(e \mid \overline{E}) = 1, \enspace \pi(\overline{e} \mid \overline{E}) = 0
    $$

    此时企业只能通过其掌握的先验概率来判断学生的类型，按 $\pi(E)$ 的概率来雇佣学生.

    企业雇佣一个学生的效用为 $\pi(E) - 0.5\pi(\overline{E})$，如果 $\pi(E) \leqslant \dfrac{1}{3}$，则企业不会雇佣任何学生，此时导师的效用同样为 $0$.

!!! example "部分不诚实推荐"


### 问题模型

!!! definition "贝叶斯劝说"
    贝叶斯劝说是一个不完全信息动态博弈，其要素为：

    - 两个参与人：信号发送者和信号接收者
    - 先验分布：他们对自然的真实状态 $\omega \in \Omega$ 有相同的先验分布 $\mu_0 \in \operatorname{int}(\Delta(\Omega))$. 信号发送者知道状态的实现值，但信号接收者不知道.
    - 双方都是理性的，并且按照贝叶斯公式更新信念.
    - 效用：假设发送者的效用为 $v(a, \omega)$，接收者的效用为 $u(a, \omega)$
        - 发送者的效用为 $v(H, \omega) = 1, \enspace v(\overline{H}, \omega) = 0$.
        - 接收者的效用为 $v(H, E) = 1, \enspace v(H, \overline{E}) = -0.5, \enspace v(\overline{H}, \omega) = 0$.

    博弈的行动顺序：

    - 发送者公开信号机制 $(S, \pi(s \mid \omega)), \forall s \in S, \omega \in \Omega$，其中 $S$ 为信号实现空间
    - 自然以分布 $\mu_0$ 选择 $\omega \in \Omega$
    - 类型为 $\omega$ 时发送者以概率 $\pi(s \mid \omega)$ 发送信号 $s \in S$
    - 接收者收到信号 $s$ 并选择一个行动 $a \in A$，满足

        $$
            a = \operatorname{argmax}_{a \in A} \mathbb{E}_{\mu_s} [u(a, \omega)].
        $$

    - 发送者获得效用 $v(a, \omega)$，接收者获得效用 $u(a, \omega)$

### 贝叶斯可行

!!! definition "贝叶斯可行"
    称 $\tau$ 由信号导致，如果存在信号机制 $(S, \pi(s \mid \omega))$ 对应的后验概率分布的分布为 $\tau$. 称一个后验概率分布的分布 $\tau$ 是贝叶斯可行的，如果

    $$
        \sum_{\operatorname{Supp}(\tau)} \mu \tau(\mu) = \mu_0
    $$

    即后验概率的期望等于先验概率.

!!! theorem
    一个后验概率分布的分布 $\tau \in \Delta(\Delta(\Omega))$ 是贝叶斯可行的 $\iff$ 存在一个信号机制 $(S, \pi(s \mid \omega))$ 使得 $\tau$ 是由该信号机制导致的.

