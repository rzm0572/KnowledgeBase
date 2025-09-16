# Lecture 1

理论计算机科学研究问题的上界和下界：

- 上界研究的是算法：找到一个方法在至多某段时间内能解决这一类问题
- 下界告诉我们这个问题的极限在哪里，这决定了这个问题是否值得继续研究

## Problem

为了对问题进行研究，首先我们需要对问题进行一些定义。

问题可以被表示为从输入到输出的一个函数。

下面我们需要为问题的输入和输出给出一个通用的表达形式。

### Binary strings

!!! definition "Definition :: Binary string"
    令字符集（alphabet）$\sigma = \{0,1\}$，序列集
    
    $$
        \sigma^n = \begin{cases}
            \sigma \times \sigma \times \cdots \times \sigma = \{(a_1, a_2, \cdots, a_n) \colon a_i \in \sigma \} & \text{if } n \geqslant 1 \\
            \{e\} & \text{if } n = 0
        \end{cases}
    $$

    其中 $e$ 表示空串，再令

    $$
        \sigma^* = \bigcup_{n \geqslant 0} \sigma^n
    $$

    则 $\sigma^*$ 是由所有二进制字符串组成的集合。

!!! definition "Definition :: operation on binary strings"
    - 拼接（concatenation）：设字符串 $x = a_1 a_2 \cdots a_n, \enspace y = b_1 b_2 \cdots b_m$，定义 $x$ 与 $y$ 的拼接

    $$
        xy := a_1 a_2 \cdots a_n b_1 b_2 \cdots b_m
    $$

    - 前缀（prefix）：字符串 $x$ 是字符串 $y$ 的前缀 $\iff \exists z \in \sigma^*$, 使得 $y = xz$
    - 后缀（suffix）：字符串 $x$ 是字符串 $y$ 的后缀 $\iff \exists z \in \sigma^*$, 使得 $x = yz$


我们知道问题有很多中表达方式，设 $A$ 为问题的所有可能输入构成的集合，我们应该如何将其转换成通用的表达形式呢？

### Encoding

我们可以通过编码的方式将输入转换成二进制字符串。具体地，编码规则是一种映射 $E: A \to \sigma^*$，并且 $E$ 是单射。

!!! definition "Definition :: Encodability"
    设 $A$ 为任意集合，如果存在映射 $E: A \to \sigma^*$，使得 $E$ 是单射，则称 $A$ 可编码。

!!! example
    - 自然数集 $\mathbb{N}$ 可编码

    !!! proof
        令映射 $\mathrm{Nts}(n) = \begin{cases} 0, & n = 0 \\ 1, & n = 1 \\ \mathrm{Nts}(\left\lfloor \frac{n}{2} \right\rfloor) \cdot \mathrm{parity}(n), & n > 1 \end{cases}$，其中 $\mathrm{parity}(n)$ 表示 $n$ 的奇偶性，则 $\mathrm{Nts}$ 是从 $\mathbb{N}$ 到 $\sigma^*$ 的一个映射。下面证明 $\mathrm{Nts}$ 是单射：

        TODO

    - 自然数集的笛卡尔积 $\mathbb{N} \times \mathbb{N}$ 可编码

    !!! proof
        做两次映射，令 $f: \mathbb{N} \times \mathbb{N} \to \{0, 1, c\}^*$，满足

        $$
            f(m, n) = \mathrm{Nts}(m),\mathrm{Nts}(n)
        $$

        令 $g: \{0, 1, c\}^* \to \sigma^*$，满足

        $$
            g(x_1 x_2 \cdots x_k) = \begin{cases} 
                g(x_1 x_2 \cdots x_{k-1}) \cdot 00, & x_k = 0  \\
                g(x_1 x_2 \cdots x_{k-1}) \cdot 11, & x_k = 1  \\
                g(x_1 x_2 \cdots x_{k-1}) \cdot 01, & x_k = c  \\
            \end{cases}
        $$

        则 $g \circ f$ 是从 $\mathbb{N} \times \mathbb{N}$ 到 $\sigma^*$ 的一个映射。下面证明 $g \circ f$ 是单射：

        TODO

在构造 $\mathbb{N} \times \mathbb{N}$ 的编码时，我们发现不能直接拼接两个自然数各自的编码，因为这样构造出的映射不满足单射性质，会造成解码时的多义性，其原因是我们无法确定两个编码的长度。因此我们用了一种比较复杂的方式来构造 $\mathbb{N} \times \mathbb{N}$ 的编码。

为了简化为集合的笛卡尔积构造编码的过程，我们希望能构造出满足以下性质的编码：

!!! property "Property 1"
    设 $A$ 为可编码的集合，$E: A \to \sigma^*$ 是从 $A$ 到 $\sigma^*$ 的编码，则 $A^n$ 的编码 $\overline{E}$ 可以写成

    $$
        \overline{E}(a_1, a_2, \cdots, a_n) = E(a_1) E(a_2) \cdots E(a_n), \quad a_i \in A
    $$

即我们可以通过直接拼接 $A^n$ 中的元素的各个分量的编码来构造该元素的编码。这个优美的性质需要 $E$ 满足一定的条件：

!!! definition "Definition :: prefix-free encoding"
    设 $A$ 为可编码的集合，称编码 $E: A \to \sigma^*$ 是 prefix-free 的，如果

    $$
        \forall x, x' \in A, x \neq x',\enspace E(x) \text{ is not a prefix of } E(x').
    $$

!!! theorem "Lemma 1"
    若 $A$ 为可编码的集合，$E: A \to \sigma^*$ 是 prefix-free 的编码，则 Property 1 成立.

!!! proof

!!! theorem "Lemma 2"
    如果 $A$ 为可编码的集合，$E$ 是从 $A$ 到 $\sigma^*$ 的编码，则存在 prefix-free 的编码 $E': A \to \sigma^*$，使得

    $$
        \forall a \in A, \enspace |E'(a)| \leqslant 2|E(a)| + 2.
    $$

!!! proof

由 Lemma 1 和 Lemma 2 我们可以得到一条十分重要的结论：

!!! theorem
    设 $A$ 为可编码的集合，则 $A^*$ 可编码。

!!! proof

### Countability

可编码性对应了集合在数学上的什么性质呢？事实上，有如下结论：

!!! theorem
    $A$ 是可编码的 $\iff$ $A$ 是可数集。

为了证明这个结论，我们先复习一下可数集的定义：

!!! definition "Definition :: Countable set"
    令 $A$ 为任意集合，称 $A$ 为可数集，如果下列任意一个条件成立：

    1. $A$ 是有限集
    2. 存在映射 $f$，使得 $f$ 为 $A$ 与 $\mathbb{N}$ 间的双射

事实上，上述判定条件 2 可以进一步弱化：

!!! theorem "Theorem :: Equivalent conditions for countability"
    设 $A$ 为任意集合，则下列条件等价：

    1. $A$ 是可数集
    2. 存在映射 $g: A \to \mathbb{N}$，使得 $g$ 为单射
    3. 存在映射 $h: \mathbb{N} \to A$，使得 $h$ 为满射

有了这个定理，我们可以先证明一个引理：

!!! theorem "Lemma 3"
    $\left\{ 0, 1 \right\}^*$ 是可数集。

!!! proof
    设 $f(x) = \begin{cases} 0 , & x = e \\ 2^{|x|} - 1 + \mathrm{StN}(x) , & |x| > 0 \end{cases}$，则 $f$ 是一个从 $\sigma^*$ 到 $\mathbb{N}$ 的单射，故 $\left\{ 0, 1 \right\}^*$ 是可数集。

有了引理 3，我们就可以证明可编码性等价于可数性：

!!! proof
    TODO

我们希望我们所要解决的问题的输入和输出都是可编码的，这就意味着问题的输入集合和输出集合都是可数集。
