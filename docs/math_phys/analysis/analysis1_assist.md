# 24-25 数学分析期末复习讲义（下）

!!! tip
    本讲义的主要内容包括导数的应用、定积分、反常积分和压轴题选讲. 同学们也可阅读 [数分讲义网页版](https://rzm0572.github.io/KnowledgeBase/math_phys/analysis/analysis1_assist/)，讲义发布之后可能会在这里进行更新和放出习题答案.

    如果发现讲义中有错误或需要改进的地方，欢迎联系我进行修改 (●'◡'●)

!!! note "数学分析（甲）I（H）知识结构"
    ![数学分析（甲）I（H）知识结构](/KnowledgeBase/assets/analysis/analysis_outline.png)

## 导数应用

### 微分中值定理

为了引入微分中值定理，我们先回顾一下极值点的定义（注意与高中定义的区别）：

!!! definition "Definition :: 极值点"

    若函数 $f(x)$ 在 $x_0$ 的某个去心邻域 $\mathring{U}(x_0)$ 内恒有

    $$
    \newcommand{\dd}[1]{\mathrm{d} #1}
    \newcommand{\intdd}[1]{\, \dd{#1}}
    \newcommand{\dint}{\displaystyle \int}
    \newcommand{\dsum}{\displaystyle \sum}
    f(x) \leqslant f(x_0) \quad (f(x) \geqslant f(x_0))
    $$

    则称 $x_0$ 为 $f(x)$ 的极大（小）值点，$f(x_0)$ 称为 $f(x)$ 的极大（小）值.

有了极值点的定义，我们便可以得到最基础的微分中值定理 —— Fermat 中值定理：

!!! theorem "Theorem :: 费马中值定理，Fermat"

    设 $f(x)$ 在 $U(x_0, \delta)$ 中有定义，若 $x_0$ 是 $f(x)$ 的极值点，并且 $f(x)$ 在 $x_0$ 处可导，则 $f'(x_0)=0$.

将 Fermat 中值定理的条件加强，由极值点处可导加强到区间内连续可导，可以得到 Rolle 中值定理和 Lagrange 中值定理：

!!! theorem "Theorem :: 罗尔中值定理，Rolle"

    若函数 $f(x)$ 满足以下条件：

    - $f(x)$ 在闭区间 $[a,b]$ 上连续
    - $f(x)$ 在开区间 $(a,b)$ 上可导
    - $f(a) = f(b)$

    则 $\exists \xi \in (a,b)$，使得 $f'(\xi)=0$.

    Hint: 三个条件中，如果有一个条件不满足，会发生什么？

将 Rolle 中值定理的条件减弱，去掉端点值相等的条件，可以得到 Lagrange 中值定理：

!!! theorem "Theorem :: 拉格朗日中值定理，Lagrange"

    若函数 $f(x)$ 满足以下条件：

    - $f(x)$ 在闭区间 $[a,b]$ 上连续
    - $f(x)$ 在开区间 $(a,b)$ 上可导

    则 $\exists \xi \in (a,b)$，使得 $f'(\xi) = \dfrac{f(b)-f(a)}{b-a}$.

最后，我们将 Lagrange 中值定理的分母弱化成一般的函数，得到 Cauchy 中值定理：

!!! theorem "Theorem :: 柯西中值定理，Cauchy"

    若函数 $f(x)$ 与 $g(x)$ 满足以下条件：

    - $f(x), g(x)$ 在闭区间 $[a,b]$ 上连续
    - $f(x), g(x)$ 在开区间 $(a,b)$ 上可导
    - $\forall x \in [a,b], \enspace g'(x) \neq 0$
    
    则 $\exists \xi \in (a,b)$，使得 $\dfrac{f'(\xi)}{g'(\xi)} = \dfrac{f(b)-f(a)}{g(b)-g(a)}$.

与微分中值定理相关的题目的解题方法：

- 直接利用

    !!! example
        设 $f(x)$ 在 $[a,b]$ 上连续，在 $(a,b)$ 上可导，且 $f(a) \neq f(b)$，证明：

        $$
            \exists \xi, \eta \in (a,b), \enspace f'(\xi) = \frac{a+b}{2\eta} f'(\eta).
        $$

- 设 k 法

    !!! example
        已知函数 $f(x)$ 在 $[0,1]$ 上三阶可导，且 $f(0) = -1, f(1) = 0, f'(0) = 0$，证明：$\forall x \in (0,1), \enspace \exists \xi \in (0,1)$，使得

        $$
            f(x) = -1 + x^2 + \frac{x^2 (x-1)}{3!} f'''(\xi).
        $$

- 构造函数法

    !!! example
        设函数 $f(x)$ 在 $[0,1]$ 上连续，在 $(0,1)$ 上可导，$f(0) = f(1) = 0$，并且存在 $t_0 \in (0, 1)$，使得 $f(t_0) = \alpha$. 证明：

        $$
            \exists \xi \in (0,1), \enspace f'(\xi) = \alpha.
        $$

    !!! example
        设奇函数 $f(x)$ 在 $[-1,1]$ 上具有二阶导数，且 $f(1) = 1$，证明：

        1. 存在 $\xi \in (0, 1)$，使得 $f'(\xi) = 1$.

        2. 存在 $\eta \in (-1, 1)$，使得 $f''(\eta) + f'(\eta) = 1$.

    !!! example
        设函数 $f(x)$ 在闭区间 $[-2,2]$ 上具有二阶导数，若 $(f(0))^2 + (f'(0))^2 = 4, |f(x)| \leqslant 1$，证明：

        $$
            \exists \xi \in (-2, 2), \enspace f(\xi) + f''(\xi) = 0.
        $$

    Hint：总结一下各种条件的构造函数方法.

### 导函数性质

以微分中值定理为工具，我们可以得到有关导函数性质的两条重要定理：

!!! theorem "Theorem :: 导函数极限定理"
    设函数 $f(x)$ 在 $x_0$ 的邻域内连续，在 $x_0$ 的去心邻域内可导，则

    $$
    \begin{align*}
    \lim\limits_{x \to x_0^-} f(x) = A &\implies f'_{-}(x) = A \\
    \lim\limits_{x \to x_0^+} f(x) = A &\implies f'_{+}(x) = A
    \end{align*}
    $$

    即导数的左（右）极限存在 $\implies$ 左（右）导数存在.

!!! theorem "Theorem :: 导函数介值定理，Darboux"
    设函数 $f(x)$ 在闭区间 $[a,b]$ 上可导，若 $f'_{+}(a) < f'_{-}(b)$，则 $\forall \lambda \in (f'_{+}(a), f'_{-}(b))$，都存在 $\xi \in (a,b)$，使得 $f'(\xi) = \lambda$.（$f'_{+}(a)$ 与 $f'_{-}(b)$ 的大小关系调换后同样成立）.

    注意：即使导函数在 $[a,b]$ 上不连续，导函数介值定理依然成立.

由上述两条定理我们可以得到导函数的一条重要性质：

!!! property "Property :: 导函数的间断点性质"
    导函数的间断点只可能是第二类间断点.

上述推论是我们判断某个函数是否存在原函数的重要依据.

!!! example "例题"

    1. （23-24 期末）证明导函数介值定理：若 $f(x)$ 在 $\mathbb{R}$ 上可导，且 $f'(0) < a < f'(1)$，则存在 $\xi \in (0,1)$，使得 $f'(\xi) = a$.

    2. （22-23 期末）已知 $g(x)$ 有二阶连续导数，$g(0)=1$，$g'(0)=0$，且

        $$
            f(x) = \begin{cases} \dfrac{g(x)-\cos x}{x} &\enspace, x \neq 0 \\ a &\enspace,x=0 \end{cases}
        $$

        1. 若 $f(x)$ 在 $x=0$ 处连续，求 $a$.
    
        2. 已知 $f(x)$ 在 $x=0$ 处连续，讨论 $f'(x)$ 在 $x=0$ 处的连续性.

### Taylor 公式

!!! theorem "Theorem :: Taylor 公式"
    设 $f(x)$ 在 $U(x_0)$ 内 $n$ 阶可导，则可将 $f(x)$ 展开为

    $$
        f(x) = \sum\limits_{k=0}^n \frac{f^{(k)}(x_0)}{k!} x^k + R_n(x),
    $$

    其中 $R_n(x)$ 为 Taylor 展开的余项.

Taylor 展开的余项形式：

- Peano 型余项：若 $f(x)$ 在 $U(x_0)$ 内 $n$ 阶可导，则在该邻域内，满足 $R_n(x) = o((x-x_0)^n)$.

- Lagrange 型余项：若 $f(x) \in C^n[a,b] \cap D^{n+1}(a,b)$，则对于任意定点 $x_0 \in (a,b), \forall x \in (a,b)$，都有 $R_n(x) = \dfrac{f^{(n+1)}(\xi)}{(n+1)!}(x-x_0)^n$，其中 $\xi \in (a,b)$.

- Cauchy 型余项：若 $f(x) \in C^n[a,b] \cap D^{n+1}(a,b)$，则对于任意定点 $x_0 \in (a,b), \forall x \in (a,b)$，都有 $R_n(x) = \dfrac{f^{(n+1)}(x_0 + \theta (x - x_0))}{n!} (1-\theta)^n (x-x_0)^{n+1}$, 其中 $\theta \in (0,1)$.

值得注意的是，若 $f(x)$ 在 $U(x_0)$ 内 $n$ 阶可导，则满足

$$
    f(x) = \sum\limits_{k=0}^n a_k (x-x_0)^k + o((x-x_0)^n)
$$

的展开式是唯一的，其系数 $a_k = f^{(k)}(x_0)$ 即为 Taylor 展开式的系数. 这给了我们通过初等函数的泰勒展开推导更复杂形式的函数的泰勒展开式的途径，从而解决一系列计算问题.

!!! example "有关 Taylor 公式的计算题"
    1. 求 $f(x) = \tan x$ 的麦克劳林公式（展开至 $x^5$）.
    2. 若 $f(x) = \arctan x$，求 $f^{(n)}(0)$.
    3. （23-24 期末）求极限 $\lim\limits_{x \to 1} \dfrac{x^x - x}{1 - x + \ln x}$.

在计算问题中我们常用 Peano 型余项来估阶，而在证明问题中则更常用 Lagrange 型余项来确定余项的具体范围. 事实上，Taylor 公式相当于利用了更高阶的导数提供的信息，得到了比 Lagrange 中值定理更强的结论，当题目中出现高阶导数时，通常都会使用 Taylor 展开式来求解.

!!! example
    1. 已知 $f(x)$ 在 $(-1, 2)$ 上有二阶导数，且 $f'(\frac{1}{2})=0$. 证明：

        $$
            \exists \xi \in (0, 1), \enspace |f''(\xi)| \geqslant 4|f(1) - f(0)|.
        $$

<!-- 泰勒取点在待求点 -->
!!! example
    设函数 $f(x)$ 在 $[0,1]$ 上有二阶导数，且 $|f(x)| \leqslant A, |f''(x)| \leqslant B$，证明：$\forall x \in [0,1]$，有

    $$
        |f'(x)| \leqslant 2A + \frac{B}{2}.
    $$

<!-- 泰勒取点在极值点 -->

!!! example
    设函数 $f(x)$ 在 $[a,b]$ 上连续，在 $(a,b)$ 上二阶可导，$f(a) = f(b) = 0$，且 $|f''(x)| \geqslant m > 0$，证明：

    $$
        \max\limits_{a \leqslant x \leqslant b} |f(x)| \geqslant \frac{m}{8} (b-a)^2.
    $$

<!-- 泰勒取点在区间端点 -->

!!! example
    设 $f(x)$ 在 $I$ 上二阶可导，设区间 $[a,b] \subset I$，满足 $f'(a) = f'(b) = 0$. 证明：存在 $\xi \in (a,b)$，使得

    $$
        |f''(\xi)| \geqslant \frac{4}{(b-a)^2} |f(b) - f(a)|.
    $$

### 导数与函数性态

#### 单调性

!!! theorem "Theorem :: 函数单调性与导数的关系"
    设 $f(x)$ 在 $[a,b]$ 上连续，在 $(a,b)$ 上可导，则：

    - $f(x)$ 在 $[a,b]$ 上单调递增 $\iff \forall x \in [a,b], f'(x) \geqslant 0$.
    - $f(x)$ 在 $[a,b]$ 上单调递减 $\iff \forall x \in [a,b], f'(x) \leqslant 0$.

思考：$f(x)$ 严格单调递增如何刻画？

#### 极值点和最值点

我们可以使用一阶导数描述函数的极值点：

!!! theorem "Theorem :: 极值点判定定理 I"
    设函数 $f(x)$ 在 $U(x_0)$ 上连续，在 $\mathring{U}(x_0)$ 上可导，则

    - $\exists \delta > 0, \forall x \in (x_0 - \delta, x_0)$，有 $f'(x) \geqslant 0$，而 $\forall x \in (x_0, x_0 + \delta)$，有 $f'(x) \leqslant 0 \implies x_0$ 是 $f(x)$ 的极小值点.
    - $\exists \delta > 0, \forall x \in (x_0 - \delta, x_0)$，有 $f'(x) \leqslant 0$，而 $\forall x \in (x_0, x_0 + \delta)$，有 $f'(x) \geqslant 0 \implies x_0$ 是 $f(x)$ 的极大值点.
    - $f'(x) \geqslant 0$（或 $f'(x) \leqslant 0$）在 $\mathring{U}(x_0)$ 上恒成立 $\implies x_0$ 不是 $f(x)$ 的极值点.

在补充了高阶可导的条件后，我们可以用 Taylor 公式得到以下判定定理：

!!! theorem "Theorem :: 极值点判定定理 II"
    设函数 $f(x)$ 在 $U(x_0)$ 上 $n$ 阶可导，且 $f'(x_0) = f''(x_0) = \cdots = f^{(n-1)}(x_0) = 0$，且 $f^{(n)}(x_0) \neq 0$，则：

    - 若 $n$ 为奇数，则 $x_0$ 不是 $f(x)$ 的极值点.
    - 若 $n$ 为偶数，且 $f^{(n)}(x_0) \geqslant 0$，则 $x_0$ 是 $f(x)$ 的极小值点.
    - 若 $n$ 为偶数，且 $f^{(n)}(x_0) \leqslant 0$，则 $x_0$ 是 $f(x)$ 的极大值点.

下面我们讨论函数在给定区间上的最值问题：设函数 $f(x)$ 在 $I$ 上连续，

1. 若 $I = [a,b]$，则 $f(x)$ 在 $I$ 上的最值必定存在，且只可能在以下几处取得：
    - $x=a$ 或 $x=b$
    - $f'(x) = 0$ 的点
    - $f'(x)$ 不存在的点

    即 $\max\limits_{x \in I} f(x) = \max \{ f(a), f(b), f(x_i) \mid f'(x_i) = 0 \;\text{or not exist} \}$.

2. 若 $I = (a,b)$，则 $f(x)$ 在 $I$ 上的最值有可能存在，以最大值为例：

    设 $M_0 = \max\{f(x_i) \mid f'(x_i) = 0 \;\text{or not exist}\}$.

    - 若 $\lim\limits_{x \to a^+} f(x) > M_0$ 或 $\lim\limits_{x \to b^-} f(x) > M_0$，则 $f(x)$ 在 $I$ 上的最大值不存在.
    - 反之则有 $\max\limits_{x \in I} f(x) = M_0$.

!!! example
    1. （20-21 期末 T6）设函数 $f(x)$ 满足方程 $\forall x \in \mathbb{R}, \enspace f''(x) + \left[f'(x) \right]^2 = x$，且 $f'(0) = 0$，证明 $x=0$ 不是 $f(x)$ 的极值点.

    2. （23-24 期末 T4）求函数 $f(x) = \dint_{-1}^{1} |x-t| e^{t^2} \intdd{t}$ 在 $\mathbb{R}$ 上的最小值.

#### 凹凸性

!!! Definition "Definition :: 凹凸性"

    设 $f(x)$ 在 $I$ 上有定义，若 $\forall x_1, x_2 \in I, \forall \lambda \in (0,1)$，都有

    $$
    f(\lambda x_1 + (1-\lambda) x_2) \leqslant \lambda f(x_1) + (1-\lambda) f(x_2),
    $$

    则称 $f(x)$ 在 $I$ 上是凸函数（下凸函数）；反之，若有

    $$
    f(\lambda x_1 + (1-\lambda) x_2) \geqslant \lambda f(x_1) + (1-\lambda) f(x_2),
    $$

    则称 $f(x)$ 在 $I$ 上是凹函数（上凸函数）.

!!! theorem "Theorem :: 函数凹凸性的等价条件"
    $f(x)$ 在 $I$ 上是下凸函数 $\iff \forall x_1, x_2, x_3 \in I, x_1<x_2<x_3$，都有

    $$
        \frac{f(x_2)-f(x_1)}{x_2-x_1} \leqslant \frac{f(x_3)-f(x_1)}{x_3-x_1} \leqslant \frac{f(x_3)-f(x_2)}{x_3-x_2}.
    $$

下面我们来研究具有凹凸性的函数的一些分析性质：

!!! example "Example"
    若 $f(x)$ 为开区间 $(a,b)$ 上的下凸函数，证明：

    1. $\forall x \in (a,b)$，$f'_{\pm}(x)$ 均存在.
    2. $f(x)$ 在 $(a,b)$ 上连续.

注意：函数的凹凸性并不要求函数在 $I$ 上可导（甚至没有要求连续），只要满足定义即可.

在补充了 $f(x)$ 的可导性后，凹凸性也可以使用一阶导数或二阶导数来刻画.

!!! theorem "Theorem :: 凹凸性与导数的关系"
    - 若 $f(x)$ 在 $[a,b]$ 上连续，在 $(a,b)$ 上可导，则 $f(x)$ 在 $[a,b]$ 上是下凸函数 $\iff f'(x)$ 在 $(a,b)$ 上单调递增.
    - 若 $f(x)$ 在 $[a,b]$ 上连续，在 $(a,b)$ 上二阶可导，则 $f(x)$ 在 $[a,b]$ 上是下凸函数 $\iff \forall x \in (a,b), f''(x) \geqslant 0$.

## 定积分

### 可积理论

!!! definition "Definition :: 黎曼可积"
    设 $f(x)$ 是定义在 $[a,b]$ 上的函数，若存在实常数 $A$，$\forall \varepsilon > 0, \exists \delta > 0$，使得对于满足 $\Vert \Delta \Vert < \delta$ 的任意对 $[a,b]$ 的分割 $\Delta = \{x_0 = a < x_1 < \cdots < x_n = b\}$，以及任意的介点组 $\{\xi\}$，都有

    $$
        |S_{\Delta}(f, \xi) - A| = \left| \sum_{k=1}^n f(\xi_k) \Delta x_k - A \right| < \varepsilon,
    $$

    则称 $f(x)$ 在 $[a,b]$ 上 Riemann 可积，即 $f \in R[a,b]$. $A$ 为 $f(x)$ 在 $[a,b]$ 上的定积分，记作 $A = \dint_{a}^{b} f(x) \intdd{x}$.

!!! theorem "Theorem :: Riemann 可积的充分必要条件"
    设 $f(x)$ 在 $[a,b]$ 上有定义，则下列条件等价：

    - $f(x)$ 在 $[a,b]$ 上 Riemann 可积.
    - Darboux 上积分与 Darboux 下积分相等：$\overline{A} = \underline{A}$.
    - 振幅之和趋于 0：$\forall \varepsilon > 0, \exists \delta > 0$，对于满足 $\Vert \Delta \Vert < \delta$ 的任意分割 $\Delta$，使得 $\overline{S}_{\Delta} - \underline{S}_{\Delta} = \dsum\limits_{k=1}^n \omega_k \Delta x_k < \varepsilon$.
    - 弱化后的条件：$\forall \varepsilon > 0$，存在分割 $\Delta$，使得 $\overline{S}_{\Delta} - \underline{S}_{\Delta} = \dsum\limits_{k=1}^n \omega_k \Delta x_k < \varepsilon$.
    - $\forall \varepsilon > 0, \forall \sigma > 0$，存在满足 $\Vert \Delta \Vert < \delta$ 的分割 $\Delta$，使得 $\dsum\limits_{\omega_k \geqslant \varepsilon} \Delta x_k < \sigma$.

!!! theorem "Theorem :: Riemann 可积的必要条件"
    若 $f(x)$ 在 $[a,b]$ 上 Riemann 可积，则 $f(x)$ 在 $[a,b]$ 上有界.

!!! theorem "Theorem :: Riemann 可积的充分条件"
    若在 $[a,b]$ 上有定义且有界的函数 $f(x)$ 满足下列任一条件，则 $f(x)$ 在 $[a,b]$ 上 Riemann 可积：

    - $f(x)$ 在 $[a,b]$ 上连续.
    - $f(x)$ 在 $[a,b]$ 上单调.
    - $f(x)$ 在 $[a,b]$ 上只有有限个不连续点.

!!! example "23-24 期末 T7"
    设 $f(x)$ 在 $[0,1]$ 上有连续的导函数，证明：

    $$
        \lim_{n \to +\infty} \sum_{k=1}^n \left[ f\left(\frac{k}{n}\right) - f\left(\frac{2k-1}{2n}\right) \right] = \frac{f(1)-f(0)}{2}.
    $$

### 定积分的性质

#### 基本性质

!!! property "Property :: 定积分的基本性质"
    设 $f(x), g(x)$ 在 $[a,b]$ 上 Riemann 可积.

    - 线性性：$k_1 f(x) + k_2 g(x)$ 在 $[a,b]$ 上可积，且
    
    $$
        \dint_{a}^{b} [k_1 f(x) \intdd{x} + k_2 g(x)] \intdd{x} = k_1 \dint_{a}^{b} f(x) \intdd{x} + k_2 \dint_{a}^{b} g(x) \intdd{x}.
    $$
    
    - 保号性：$f(x) \leqslant g(x) \implies \dint_{a}^{b} f(x) \intdd{x} \leqslant \dint_{a}^{b} g(x) \intdd{x}$.
    - 乘积可积性：$f(x)g(x)$ 在 $[a,b]$ 上可积.
    - 积分区间可加性：若 $a < c < b$，则 $f(x)$ 在 $[a,c]$ 和 $[c,b]$ 上可积 $\iff f(x)$ 在 $[a,b]$ 上可积，并且
    
    $$
        \dint_{a}^{b} f(x) \intdd{x} = \dint_{a}^{c} f(x) \intdd{x} + \dint_{c}^{b} f(x) \intdd{x}.
    $$

    - 绝对可积性：$|f(x)|$ 在 $[a,b]$ 上可积，且 $\left\vert \dint_{a}^{b} f(x) \intdd{x}\right\vert \leqslant \dint_{a}^{b} |f(x)| \intdd{x}$.

!!! example "23-24 期末 T6"
    设函数 $f(x) \in R[0,1]$，且 $f$ 在 $x=0$ 处右连续. 证明：函数 $\varphi(x)=\dint_0^x f(t) \intdd{t} \enspace (0 \leqslant x \leqslant 1)$ 在 $x=0$ 处的右导数等于 $f(0)$.

#### 变限积分

!!! definition "Definition :: 变上限积分"
    设 $f(x)$ 在 $[a,b]$ 上可积，称 $\dint_{a}^{x} f(t) \intdd{t}$ 为 $f(x)$ 在 $[a,b]$ 上的变上限积分.

!!! theorem "Theorem :: 变上限积分的性质"
    设 $f(x)$ 在 $[a,b]$ 上可积，记 $F(x) = \dint_{a}^{x} f(t) \intdd{t}$，则

    - $F(x)$ 在 $[a,b]$ 上 Lipschitz 连续.
    - 若 $f(x)$ 在 $x_0 \in [a,b]$ 上连续，则 $F(x)$ 在 $x_0$ 处可导.

!!! note "Corollary"
    $f(x)$ 在区间 $[a,b]$ 上的原函数存在的一个充分条件是 $f(x)$ 在 $[a,b]$ 上连续.

    进而，若 $f(x)$ 在 $[a,b]$ 上连续，则 Newton-Leibniz 公式成立.

更一般地，我们可以将变限积分写作 $F(x) = \dint_{\phi(x)}^{\psi(x)} f(t) \intdd{t}$，其中 $\phi(x)$ 和 $\psi(x)$ 分别是 $t$ 的上下界.

在 $f(x)$ 连续，$\phi(x)$ 和 $\psi(x)$ 可导的条件下，$F(x)$ 可导，我们可以给出其导数的计算方法：

$$
    \frac{\dd{F(x)}}{\dd{x}} = \frac{\dd{}}{\dd{x}} \int_{\phi(x)}^{\psi(x)} f(t) \intdd{t} = f(\psi(x)) \psi'(x) - f(\phi(x)) \phi'(x).
$$

!!! example "23-24 期末 T2.c"
    求极限 $\lim\limits_{x \to 0} \dfrac{\dint_{0}^{x^2} t^2 e^{\sin t} \intdd{x}}{\ln (1+x^6)}$.

!!! example "23-24 期末 T6"
    设函数 $f(x)\in R[0,1]$，且 $f$ 在 $x=0$ 处右连续. 证明：函数 $\varphi(x)=\displaystyle\int_0^x f(t) \dd{t} \;(0 \leqslant x \leqslant 1)$ 在 $x=0$ 处的右导数等于 $f(0)$.

!!! example
    设 $f(x)$ 在 $[0,1]$ 上可导，且 $0 \leqslant f'(x) \leqslant 1, f(0) = 0$，证明：

    $$
        \left( \int_0^1 f(x) \intdd{x} \right)^2 \geqslant \int_0^1 f^3(x) \intdd{x}.
    $$

    Hint: 将普通积分化为变上限积分.

#### 积分中值定理

!!! theorem "Theorem :: 积分第一中值定理"
    设 $f(x)$ 和 $g(x)$ 都在 $[a,b]$ 上可积，$g(x)$ 在 $[a,b]$ 上不变号，则存在 $\eta \in [m,M]$，使得

    $$
        \int_a^bf(x)g(x) \intdd{x} = \eta \int_a^b g(x) \intdd{x}.
    $$

    特别地，如果 $f(x)$ 在 $[a,b]$ 上连续，则存在 $\xi \in (a,b)$，使得

    $$
        \int_a^b f(x)g(x) \intdd{x} = f(\xi) \int_a^b g(x) \intdd{x}.
    $$

!!! theorem "Theorem :: 积分第二中值定理"
    设 $g(x)$ 在 $[a,b]$ 上可积，

    - 若 $f(x)$ 在 $[a,b]$ 上单调递减且非负，则存在 $\xi \in [a,b]$，使得 $\dint_a^b f(x)g(x) \intdd{x} = f(a) \dint_a^{\xi} g(x) \intdd{x}$.
    - 若 $f(x)$ 在 $[a,b]$ 上单调递增且非负，则存在 $\xi \in [a,b]$，使得 $\dint_a^b f(x)g(x) \intdd{x} = f(b) \dint_{\xi}^b g(x) \intdd{x}$.
    - 若 $f(x)$ 在 $[a,b]$ 上单调，则存在 $\xi \in [a,b]$，使得 $\dint_a^b f(x)g(x) \intdd{x} = f(a) \dint_a^{\xi} g(x) \intdd{x} + f(b) \dint_{\xi}^b g(x) \intdd{x}$.

!!! example
    设 $f(x)$ 在 $[0, \pi]$ 上连续，且 $\dint_0^{\pi} f(x) \intdd{x} = 0, \dint_0^{\pi} f(x) \cos x \intdd{x} = 0$，证明：$\exists \xi_1, \xi_2 \in (0, \pi)$，使得 $f(\xi_1) = f(\xi_2) = 0$.

!!! example "Riemann-Lebesque 引理"
    设 $f(x)$ 在 $[a,b]$ 上单调，证明：

    $$
    \begin{align*}
        \lim\limits_{\lambda \to +\infty} \int_{a}^{b} f(x) \cos \lambda x \intdd{x} &= 0, \\
        \lim\limits_{\lambda \to +\infty} \int_{a}^{b} f(x) \sin \lambda x \intdd{x} &= 0.
    \end{align*}
    $$

    Hint：注意单调条件的运用

#### 积分不等式

- 绝对值不等式：$\left\vert \dint_a^b f(x) \intdd{x}\right\vert \leqslant \dint_a^b |f(x)| \intdd{x}$.

- Cauchy-Schwarz 不等式：$\dint_a^b |f(x)g(x)| \intdd{x} \leqslant \left( \dint_a^b f^2(x) \intdd{x} \right) \left( \dint_a^b g^2(x) \intdd{x} \right)$.

!!! example
    设 $g(x)$ 在 $[0,1]$ 上可积，且 $\dint_0^1 g(x) \intdd{x} = 0$，证明：

    $$
        \left[ \int_0^1 x g(x) \intdd{x} \right]^2 \leqslant \frac{1}{12} \int_0^1 g^2(x) \intdd{x}.
    $$

!!! example
    设 $a>0$ 为常数，证明：

    $$
        \int_0^{\pi} x a^{\sin x} \intdd{x} \cdot \int_0^{\frac{\pi}{2}} a^{-\cos x} \geqslant \frac{\pi^3}{4}.
    $$

!!! example
    设 $f(x)$ 在 $[0,a]$ 上的导函数连续，$f(0) = 0$，证明：

    $$
        \int_0^a |f(x) f'(x)| \intdd{x} \leqslant \frac{a}{2} \int_0^a (f'(x))^2 \intdd{x}.
    $$

!!! example
    设 $f(x)$ 在 $[a,b]$ 上连续且单调递增，证明：

    $$
        \int_a^b x f(x) \intdd{x} \geqslant \frac{a+b}{2} \int_a^b f(x) \intdd{x}.
    $$

!!! example
    设函数 $f(x)$ 在 $[0,1]$ 上可导，且 $f(x)$ 不恒等于 $0$，$\int_0^1 f(x) \intdd{x} = 0$，证明：

    $$
        \int_0^1 |f(x)| \intdd{x} \cdot \int_0^1 |f'(x)| \intdd{x} \geqslant 2 \int_0^1 f^2(x) \intdd{x}.
    $$

### 定积分的计算

#### 基本方法

对于可以找到原函数的被积函数，我们可以利用微积分基本定理直接进行计算：

!!! theorem "Theorem :: Newton-Leibniz 公式"
    若函数 $f(x)$ 在区间 $[a,b]$ 上可积且有原函数 $F(x)$，则

    $$
        \int_{a}^{b} f(x) \intdd{x} = F(b) - F(a).
    $$

##### 换元积分法

!!! theorem "Theorem :: 定积分的换元积分法"
    设 $f(x)$ 在 $[a,b]$ 上连续，$\varphi(x)$ 在 $[a,b]$ 上的一阶导连续，并且 $\forall t \in (\alpha, \beta), a = \varphi(\alpha) \leqslant \varphi(t) \leqslant \varphi(\beta) = b$，则

    $$
        \int_{a}^{b} f(x) \intdd{x} = \int_{\alpha}^{\beta} f(\varphi(t)) \varphi'(t) \intdd{t}.
    $$

!!! example
    计算 $\dint_1^2 \dfrac{1}{x^{10} + x} \intdd{x}$.

##### 分部积分法

!!! theorem "Theorem :: 定积分的分部积分法"
    设 $u(x), v(x)$ 在 $[a,b]$ 上的一阶导连续，则

    $$
        \int_{a}^{b} u(x)v'(x) \intdd{x} = (u(x)v(x)) \bigg{|}_{a}^{b} - \int_{a}^{b} u'(x)v(x) \intdd{x}.
    $$

利用分部积分法可以得到几个结论：

- $I_n = \dint_{0}^{\frac{\pi}{2}} \sin^n x \intdd{x} = \dint_{0}^{\frac{\pi}{2}} \cos^n x \intdd{x}$ 的通式

    由分部积分法可得 $I_n = \dfrac{n-1}{n} I_{n-2}$，再由 $I_0 = \dfrac{\pi}{2}$，$I_1 = 1$ 可得通式：

    $$
        I_n = \begin{cases}
            \dfrac{(n-1)!!}{n!!} \dfrac{\pi}{2} &\enspace ,n = 2k \\[1em]
            \dfrac{(n-1)!!}{n!!}               &\enspace ,n = 2k+1
        \end{cases}
    $$

    此式可用于快速求解 $\sin^n x$ 和 $\cos^n x$ 的积分.

- Wallis 公式：$\lim\limits_{n \to \infty} \left( \dfrac{(2n)!!}{(2n-1)!!} \right)^2 \dfrac{1}{2n+1} = \dfrac{\pi}{2}$.

- Stirling 公式：$n! \sim \sqrt{2\pi n} \left( \dfrac{n}{e} \right)^n, \enspace n \to \infty$.

#### 特殊方法

定积分计算比不定积分计算更复杂的原因之一：对于一些难以求得原函数的被积函数，我们可以通过一些特殊方法把它在<strong>特定区间</strong>上的定积分求出来.

- 倒代换：利用被积函数在积分区间上的对称性把难以求得原函数的部分消去.

    !!! note "倒代换"
        能通过倒代换求解的定积分中的被积函数通常都有（部分）对称性，例如：

        - 被积函数关于区间中点轴对称、中心对称
        - 幂函数和三角函数组合
        
        通常来说，如果被积函数含有三角函数项且十分复杂，并且积分区间长度与三角函数周期有关，那么我们可以通过倒代换的方法来求解定积分：

        !!! example
            计算 $\dint_{-\pi}^{\pi} \dfrac{x \sin x \cdot \arctan e^x}{1 + \cos^2 x} \intdd{x}$.

        如果积分区间是 $[a,b]$，而被积函数中含有对称的 $x$ 与 $a+b-x$ 项，那么我们可以通过倒代换后进行组合的方式计算定积分：

        !!! example
            - 设 $f(x)$ 在 $[0,a]$ 上连续，并且 $f(x) + f(a-x) \neq 0$，求 $\dint_{0}^{a} \dfrac{f(x)}{f(x) + f(a-x)} \intdd{x}$.

            - 计算 $I = \dint_0^1 \dfrac{\ln (1+x)}{1 + x^2} \intdd{x}$.
            
            - 计算 $I = \dint_{-1}^1 \dfrac{\sqrt{1 - x^2}}{a - x} \intdd{x}$.
        
        补充：倒代换中常用的三角恒等式：

        - $\sin x = \cos (\dfrac{\pi}{2} - x)$
        - $\tan x \cdot \tan (\dfrac{\pi}{2} - x) = 1$
        - $\tan (\dfrac{\pi}{4} - x) = \dfrac{1 - \tan x}{1 + \tan x}$

- 周期函数：对于周期为 $T$ 的函数 $f(x)$，有以下公式成立：

    $$
        \int_{a}^{a+T} f(x) \intdd{x} = \int_{0}^{T} f(x) \intdd{x}.
    $$

    利用该公式可以将周期函数变换到一个比较容易积分的积分上进行求解.

- 奇偶分离：对于任意一个定义域关于原点对称的函数 $f(x)$，我们可以将其拆分成一个奇函数 $g(x)$ 和一个偶函数 $h(x)$ 的和，其中

    $$
        g(x) = \dfrac{f(x) - f(-x)}{2}, \enspace h(x) = \dfrac{f(x) + f(-x)}{2}.
    $$

    然后利用对称性就可以将 $g(x)$ 的积分消去，从而达到化简的目的.

    !!! example
        计算 $I = \dint_{-2}^{2} x \ln(1 + e^x) \intdd{x}$.

- 几何意义：利用定积分的几何意义简化计算.

    !!! example
        求定积分 $\dint_{0}^{a} \sqrt{a^2 - x^2} \intdd{x}$，其中 $a>0$.

### 定积分的应用

#### 定积分与极限

##### 利用定积分计算和式的极限

对于带有以下性质的和式的极限，我们可以将其化为定积分来计算：

- 假设和式的下标为 $k$，则每次 $k$ 都以 $\dfrac{k}{n}$ 的形式出现，对应积分中的 $x$.
- 可以提出一个 $\dfrac{1}{n}$ 的因子，对应积分中的 $\dd{x}$.
- 通过以上两条规则将和式化为定积分后，被积函数应可积.

这种方法实际上是利用了已知被积函数可积的情况下，分割 $\Delta$ 和介点组 $\{\xi\}$ 可以任取的性质. 需要注意的是，大多数和式并不会有这么明显的结构，我们需要做一些变形或放缩来构造.

!!! example
    1. 计算极限 $\lim\limits_{n \to +\infty} \dfrac{1}{n} \dsum_{k=1}^n \dfrac{\cos(\dfrac{k}{n})}{1 + \sin(\dfrac{k}{n})}$.
    2. 计算极限 $I_n = \lim\limits_{n \to +\infty} \dfrac{1}{n^4} \displaystyle\prod_{k=1}^{2n} (n^2 + k^2)^{\frac{1}{n}}$.
    3. 设 $f(x)$ 在 $[0,1]$ 上的一阶导数连续，证明：

        $$
            \lim\limits_{n \to \infty} \sum_{k=1}^n \left[ f\left( x + \frac{k}{n^2 + k^2} \right) - f(x) \right] = \frac{\ln 2}{2} f'(x).
        $$

##### 极限中的定积分

我们知道定积分是一个数值，如果将其中部分改为参数的话便可以构造数列或者函数. 此时如果对参数求极限，一般情况下我们不能直接将极限符号与积分符号交换，而需要采用分段放缩的方法.

!!! example
    1. 证明 $\lim\limits_{n \to +\infty} \dint_{0}^{\frac{\pi}{2}} \sin^n x \intdd{x} = 0$.
    2. 求极限 $\lim\limits_{n \to +\infty} \left( \dint_1^2 e^{-n t^2} \intdd{t} \right)^{\frac{1}{n}}$.

!!! tip
    分段放缩法的应用：

    - 证明函数可积
    - 求极限中的定积分

#### 定积分与几何

##### 平面图形面积

1. 直角坐标系

    在直角坐标系下，定积分的几何意义就是曲边梯形与坐标轴围成的有向面积.

    - $y=f(x)$ 与 $x$ 轴围成的有向面积：$S_x = \dint_{a}^{b} f(x) \intdd{x}$.
    - $x=g(y)$ 与 $y$ 轴围成的有向面积：$S_y = \dint_{c}^{d} g(y) \intdd{y}$.

2. 极坐标系

    $r=r(\theta)$ 与射线 $\theta = \alpha$，射线 $\theta = \beta$ 围成的图形面积：

    $$
        S = \int_{\alpha}^{\beta} \frac{1}{2} (r(\theta))^2 \intdd{\theta}.
    $$

3. 参数方程

    平面参数曲线 $L: \begin{cases} x = \varphi(t) \\ y = \psi(t) \end{cases}$，起点为 $(a,c)$，终点为 $(b,d)$，满足 $a = \varphi(\alpha), b = \varphi(\beta)$ 以及 $c = \psi(\alpha), d = \psi(\beta)$，则：

    - $L$ 与 $x$ 轴围成的有向面积：$S_x = \dint_{\alpha}^{\beta} \psi(t) \varphi'(t) \intdd{t}$.
    - $L$ 与 $y$ 轴围成的有向面积：$S_y = \dint_{\alpha}^{\beta} \varphi(t) \psi'(t) \intdd{t}$.

!!! tip
    具有对称性的图形可以只算部分面积，避免正负号处理.

!!! example "23-24 期末 T2.e"
    求双扭线 $r = \sqrt{\cos (2\theta)}$ 所围平面图形的面积.

##### 旋转体体积

利用截面法可以解决旋转体体积问题：设旋转轴为 $x$ 轴，设旋转体垂直 $x$ 轴的横截面积为 $S(x)$，横截面半径为 $r(x)$，则

$$
    V = \int_{a}^{b} S(x) \intdd{x} = \int_{a}^{b} \pi r^2(x) \intdd{x}.
$$

!!! example "21-22 期末 T1.d"
    求 $y=e^x$ 过点 $(0,0)$ 的切线 $L$，并求 $y=e^x$、$L$ 与 $y$ 轴所围图形绕 $x$ 轴旋转一周的体积.

##### 曲线弧长

- 显式曲线：$\Gamma: y=f(x), \enspace x \in [a,b]$，则曲线弧长

    $$
        L(\Gamma) = \int_{a}^{b} \sqrt{1 + (f'(x))^2} \intdd{x}.
    $$

- 隐式曲线（参数曲线）：$\Gamma: \begin{cases} x = \varphi(t) \\ y = \psi(t) \end{cases}, \enspace t \in [\alpha, \beta]$，则曲线弧长

    $$
        L(\Gamma) = \int_{\alpha}^{\beta} \sqrt{(\varphi'(t))^2 + (\psi'(t))^2} \intdd{t}.
    $$

- 极坐标曲线：$\Gamma: r = r(\theta), \enspace \theta \in [\alpha, \beta]$，则曲线弧长

$$
    L(\Gamma) = \int_{\alpha}^{\beta} \sqrt{(r(\theta))^2 + (r'(\theta))^2} \intdd{\theta}.
$$

!!! example
    1. （22-23 期末 T1.d）求曲线 $y = \dint_{-\sqrt{3}}^{x} \sqrt{3 - t^2} \intdd{t}$ 在 $x \in [-\sqrt{3}, \sqrt{3}]$ 上的弧长.
    2. （20-21 期末 T1.e）设 $f(x) = \dint_{0}^{x} \sqrt{\cos t} \intdd{t}$，求曲线 $y = f(x)$ 在 $x \in [0,1]$ 上的弧长.

## 反常积分

### 反常积分的定义

!!! definition "Definition :: 无穷积分"
    设 $f(x)$ 在 $[a, \infty)$ 上有定义，且在任意有限区间 $[a,b]$ 上可积，若极限

    $$
        A = \lim\limits_{b \to \infty} \int_{a}^{b} f(x) \intdd{x}
    $$

    存在，则称无穷积分 $\dint_{a}^{\infty} f(x) \intdd{x}$ 收敛到 $A$. 若 $A$ 不存在，则称 $\dint_{a}^{\infty} f(x) \intdd{x}$ 发散.

另外两类无穷积分：

- $f(x)$ 在 $(-\infty, b]$ 上有定义，且在任意有限区间 $[a,b]$ 上可积，则

    $$
        A = \lim\limits_{a \to -\infty} \dint_{a}^{b} f(x) \intdd{x} \;\text{存在} \iff \dint_{-\infty}^{a} f(x) \intdd{x} \;\text{收敛到}\; A.
    $$

- $f(x)$ 在 $(-\infty, \infty)$ 上有定义，且在任意有限区间 $[a,b]$ 上可积，则

    $$
        A^- = \lim\limits_{a \to -\infty} \dint_{a}^{c} f(x) \intdd{x} \;\text{和}\; A^+ = \lim\limits_{b \to \infty} \dint_{c}^{b} f(x) \intdd{x} \;\text{都存在}\; \iff \dint_{-\infty}^{\infty} f(x) \intdd{x} \;\text{收敛到} A^- + A^+.
    $$

!!! definition "Definition :: 瑕积分"
    设 $f(x)$ 在 $(a, b]$ 上有定义，且 $\forall \varepsilon > 0$，$f(x)$ 在 $[a+\varepsilon,b]$ 上可积，并且 $f(x)$ 在 $(a, a+\varepsilon)$ 上无界，若极限

    $$
        A = \lim\limits_{\varepsilon \to 0^+} \int_{a+\varepsilon}^{b} f(x) \intdd{x}
    $$

    存在，则称瑕积分 $\dint_{a}^{b} f(x) \intdd{x}$ 收敛到 $A$. 若 $A$ 不存在，则称 $\dint_{a}^{b} f(x) \intdd{x}$ 发散.

另外两类瑕积分：

- $b$ 是瑕点，$\forall \varepsilon > 0$，$f(x)$ 在 $[a, b-\varepsilon]$ 上可积，而在 $(b-\varepsilon, b)$ 上无界，若极限

$$
    A = \lim\limits_{\varepsilon \to 0^+} \int_{a}^{b-\varepsilon} f(x) \intdd{x}
$$

存在，则称瑕积分 $\dint_{a}^{b} f(x) \intdd{x}$ 收敛到 $A$. 若 $A$ 不存在，则称 $\dint_{a}^{b} f(x) \intdd{x}$ 发散.

- $a,b$ 均为瑕点，$c$ 是 $(a,b)$ 上任一非瑕点，若瑕积分 $\dint_{a}^{c} f(x) \intdd{x}$ 和 $\dint_{c}^{b} f(x) \intdd{x}$ 均收敛，则 $\dint_{a}^{b} f(x) \intdd{x}$ 收敛，且

$$
    \dint_{a}^{b} f(x) \intdd{x} = \dint_{a}^{c} f(x) \intdd{x} + \dint_{c}^{b} f(x) \intdd{x}.
$$

### 反常积分的性质

!!! property "Property :: 反常积分的性质"
    - 线性性：$\dint_{a}^{+\infty} f(x) \intdd{x}$ 和 $\dint_{a}^{+\infty} g(x) \intdd{x}$ 收敛 $\implies \dint_{a}^{+\infty} [k_1 f(x) + k_2 g(x)] \intdd{x} = k_1 \dint_{a}^{+\infty} f(x) \intdd{x} + k_2 \dint_{a}^{+\infty} g(x) \intdd{x}$.
    - Newton-Leibniz 公式：

        设 $f(x)$ 在 $[a, +\infty)$ 上有原函数 $F(x)$，并且 $\dint_{a}^{+\infty} f(x) \intdd{x}$ 收敛 $\implies \dint_{a}^{+\infty} f(x) \intdd{x} = F(+\infty) - F(a)$.
    
    - 换元公式：$f(x)$ 在 $[a,+\infty)$ 上可积，$\varphi(x)$ 是 $[\alpha, \beta]$ 上的连续可微严格单调函数，且 $a = \varphi(\alpha) \leqslant \varphi(t) \leqslant \lim\limits_{t \to \beta^-} \varphi(t) = +\infty$，则
        
        $$
            \int_{a}^{+\infty} f(x) \intdd{x} = \int_{\alpha}^{\beta} f(\varphi(t)) \varphi'(t) \intdd{t}.
        $$

    - 分部积分公式：设 $u(x), v(x)$ 在 $[a, +\infty)$ 上的一阶导连续，且极限 $\lim\limits_{x \to +\infty} u(x)v(x)$ 存在，则
    
        $$
            \int_{a}^{+\infty} u(x)v'(x) \intdd{x} = (u(x) v(x)) \bigg{|}_{a}^{+\infty} - \int_{a}^{+\infty} u'(x)v(x) \intdd{x}.
        $$

        上式成立要求等号两边的积分均收敛.

!!! example
    1. （23-24 期末 T2.d）求 $\dint_{1}^{+\infty} \dfrac{\arctan x}{x^2} \intdd{x}$.

### 敛散性判别

#### 无穷积分

!!! definition "Definition :: 收敛类型"
    - 若 $\dint_{a}^{+\infty} |f(x)| \intdd{x}$ 收敛，则称 $\dint_{a}^{+\infty} f(x) \intdd{x}$ 绝对收敛.
    - 若 $\dint_{a}^{+\infty} |f(x)| \intdd{x}$ 发散，而 $\dint_{a}^{+\infty} f(x) \intdd{x}$ 收敛，则称 $\dint_{a}^{+\infty} f(x) \intdd{x}$ 条件收敛.

!!! theorem "Theorem :: Cauchy 收敛准则"
    $\forall \varepsilon > 0, \exists A > a, \forall c > b > A$，满足 $\left\vert \dint_{b}^{c} f(x) \intdd{x} \right\vert < \varepsilon \iff \dint_{a}^{+\infty} f(x) \intdd{x}$ 收敛.

!!! theorem "Theorem :: 比较判别法"
    - （比较判别法 I）
        - $g(x) \geqslant 0$，若 $\exists A > a, \forall x > A, |f(x)| \leqslant g(x)$，且 $\dint_{a}^{+\infty} g(x) \intdd{x}$ 收敛，则 $\dint_{a}^{+\infty} f(x) \intdd{x}$ 绝对收敛.
        - $g(x) \geqslant 0$，若 $\exists A > a, \forall x > A, f(x) \geqslant g(x)$，且 $\dint_{a}^{+\infty} g(x) \intdd{x}$ 发散，则 $\dint_{a}^{+\infty} f(x) \intdd{x}$ 发散.
    - （比较判别法 II）设 $A = \lim\limits_{x \to +\infty} \dfrac{f(x)}{g(x)}$，其中 $A$ 为有限常数或 $\pm \infty$，$g(x) \geqslant 0$
        - $0 \leqslant A < +\infty, \dint_{a}^{+\infty} g(x) \intdd{x}$ 收敛 $\implies \dint_{a}^{+\infty} f(x) \intdd{x}$ 收敛.
        - $0 < A \leqslant +\infty, \dint_{a}^{+\infty} g(x) \intdd{x}$ 发散 $\implies \dint_{a}^{+\infty} f(x) \intdd{x}$ 发散.

    !!! eg "比较判别法的几个常用基准"
        - $\dint_{1}^{+\infty} \dfrac{1}{x^p}$：当 $p > 1$ 时收敛，当 $0 < p \leqslant 1$ 时发散.
        - $\dint_{e}^{+\infty} \dfrac{1}{x \ln^p x}$：当 $p > 1$ 时收敛，当 $0 < p \leqslant 1$ 时发散.

!!! theorem "Theorem :: Dirichlet / Abel 判别法"
    - （Dirichlet 判别法）$F(x) = \dint_{a}^{x} f(t) \intdd{t}$ 在 $[a, +\infty)$ 上有界，$g(x)$ 在 $[a, +\infty)$ 上单调趋于 0 $\implies \dint_{a}^{+\infty} f(x)g(x) \intdd{x}$ 收敛.
    - （Abel 判别法）$\dint_{a}^{+\infty} f(x) \intdd{x}$ 收敛，$g(x)$ 在 $[a, +\infty)$ 上单调有界 $\implies \dint_{a}^{+\infty} f(x)g(x) \intdd{x}$ 收敛.

!!! example "21-22 期末 T7"
    设函数

    $$
        f(x) = \begin{cases}
            \dfrac{\sin x}{x} &, x \neq 0 \\
            \,1               &, x = 0
        \end{cases}
    $$

    证明 $\dint_{0}^{+\infty} f(x) \intdd{x}$ 与 $\dint_{0}^{+\infty} f^2(x) \intdd{x}$ 均收敛，且收敛到同一值.

!!! example
    设 $\dint_{a}^{+\infty} f(x) \intdd{x}$ 收敛，且 $f(x)$ 在 $[a, +\infty)$ 上一致连续，证明 $\lim\limits_{x \to +\infty} f(x) = 0$.

## 压轴题选讲

!!! example "23-24 期末 T8"
    设函数 $f(x)$ 在 $\mathbb{R}$ 上有二阶连续的导函数，且存在常数 $C>0$，使得

    $$
        \sup_{x \in \mathbb{R}} \left( |x|^2 |f(x)| + |f''(x)| \right) \leqslant C
    $$

    证明：存在常数 $M>0$，使得 $\sup\limits_{x \in \mathbb{R}} \left(|xf'(x)|\right) \leqslant M$.

!!! example "22-23 期末 T8"
    已知 $f(x)$ 在 $[0, 1]$ 上有二阶连续导数，证明：

    $$
        \dint_0^1 x^nf(x) \intdd{x} = \dfrac{f(1)}{n} -\dfrac{f(1) + f'(1)}{n^2}+ o(\dfrac{1}{n^2}) \enspace (n\to+\infty).
    $$

!!! example "21-22 期末 T8"
    设 $f(x)$ 三阶可导，$f(0)=f'(0)=f''(0)=0$，$f'''(0)>0$，$x\in(0,1)$ 时 $f(x) \in (0,1)$，且数列 $\{x_n\}$ 满足

    $$
        x_{n+1}=x_n(1-f(x_n)).
    $$
    
    1. 证明：$\lim\limits_{n \to +\infty} x_n = 0$;
    
    2. 证明：存在 $\alpha >0$ 和常数 $c\neq 0$，使 $\lim\limits_{n \to +\infty} cn^{\alpha} x_n = 1$.

!!! example
    设 $f(x)$ 是 $\mathbb{R}$ 上具有连续导数的非负函数，且存在 $M>0$，使得 $\forall x,y \in \mathbb{R}$，有 $|f'(x) - f'(y)| \leqslant M|x-y|$.

    证明：$\forall x \in \mathbb{R}$，有
    
    $$
        (f'(x))^2 \leqslant 2M|f(x)|.
    $$

!!! example
    设 $f(x)$ 在 $x=0$ 处存在二阶导数，且 $\forall n \in \mathbb{N}^+, f(2^{-n}) = 0$，证明 $f''(0) = 0$.

## 附录

### 常用不等式

!!! theorem "theorem :: 均值不等式"
    设 $n \in \mathbb{N}^+, a_i>0 \enspace (i=1,2,\cdots,n)$，则

    $$
        \frac{n}{\frac{1}{a_1} + \frac{1}{a_2} + \cdots + \frac{1}{a_n}} \leqslant \sqrt[n]{a_1 a_2 \cdots a_n} \leqslant \frac{a_1 + a_2 + \cdots + a_n}{n} \leqslant \sqrt{\frac{a_1^2 + a_2^2 + \cdots + a_n^2}{n}}.
    $$

    等号成立的充要条件是 $a_1 = a_2 = \cdots = a_n$.

!!! theorem "theorem :: Bernoulli 不等式"
    设 $x>-1$，

    - 若 $0< \alpha < 1$，则 $(1 + x)^{\alpha} \leqslant 1 + \alpha x$.
    - 若 $\alpha < 0$ 或 $\alpha > 1$，则 $(1 + x)^{\alpha} \geqslant 1 + \alpha x$.

!!! theorem "theorem :: Young 不等式"
    若 $f(x)$ 在 $[0, +\infty)$ 上严格单调递增，且 $f(0)=0$，则 $\forall a, b > 0$，有

    $$
        ab \leqslant \int_0^a f(x) \intdd{x} + \int_0^{b} f^{-1}(y) \intdd{y}.
    $$

    等号成立的充要条件是 $b=f(a)$.

!!! theorem "theorem :: Holder 不等式"
    若 $f(x), g(x)$ 在 $[a,b]$ 上连续，且 $p,q>0, \dfrac{1}{p}+\dfrac{1}{q}=1$，则

    $$
        \int_a^b |f(x)g(x)| \intdd{x} \leqslant \left(\int_a^b |f(x)|^p \intdd{x}\right)^{\frac{1}{p}} \left(\int_a^b |g(x)|^q \intdd{x}\right)^{\frac{1}{q}}.
    $$

!!! theorem "theorem :: Jensen 不等式"
    若 $f(x)$ 是区间 $I$ 上的下凸函数，则对于任意 $x_i \in I$ 和满足 $\dsum_{i=1}^n \lambda_i = 1$ 的 $\lambda_i \geqslant 0$，有

    $$
        f \left(\dsum_{i=1}^n \lambda_i x_i\right) \leqslant \dsum_{i=1}^n \lambda_i f(x_i).
    $$

    若 $f(x)$ 是区间 $I$ 上的上凸函数，则不等号方向相反.

!!! theorem "theorem :: Hadamard 不等式"
    若 $f(x)$ 是 $[a,b]$ 上的连续下凸函数，则成立

    $$
        f \left(\frac{a+b}{2}\right) (b-a) \leqslant \int_{a}^{b} f(x) \intdd{x} \leqslant \frac{f(a) + f(b)}{2} (b-a).
    $$
