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

    ??? proof
        待证式中含有两个未定量 $\xi, \eta$，考虑使用两次中值定理：

        令 $g(x) = x^2$，由 Cauchy 中值定理，存在 $\eta \in (a,b)$，使得 $\dfrac{f'(\eta)}{g'(\eta)} = \dfrac{f(b)-f(a)}{g(b)-g(a)}$，即

        $$
            \frac{f'(\eta)}{2\eta} = \frac{f(b)-f(a)}{b^2-a^2}.
        $$

        该式可变形为

        $$
            \frac{a + b}{2\eta} f'(\eta) = \frac{f(b) - f(a)}{b - a}.
        $$

        由 Lagrange 中值定理，存在 $\xi \in (a,b)$，使得 $f'(\xi) = \dfrac{f(b)-f(a)}{b-a} = \dfrac{a+b}{2\eta} f'(\eta)$，证毕.

- 设 k 法

    !!! example
        已知函数 $f(x)$ 在 $[0,1]$ 上三阶可导，且 $f(0) = -1, f(1) = 0, f'(0) = 0$，证明：$\forall x \in (0,1), \enspace \exists \xi \in (0,1)$，使得

        $$
            f(x) = -1 + x^2 + \frac{x^2 (x-1)}{3!} f'''(\xi).
        $$

    ??? proof
        使用设 k 法，当 $x$ 给定时，待证式中除了 $f'''(\xi)$ 外的部分都为定值，我们先将定值与不定值分离：

        $$
            f'''(\xi) = \frac{3! (f(x) + 1 - x^2)}{x^2 (x-1)}.
        $$

        设右边的定值为 $k$，再将定值 $x$ 换成变量 $t$ 并构造函数：

        $$
            g(t) = f(t) + 1 - t^2 - \frac{t^2 (t-1)}{3!} k.
        $$

        由 $f(x)$ 在 $[0,1]$ 上三阶可导，$g(t)$ 在 $[0,1]$ 上也三阶可导，故有

        $$
        \begin{align*}
            g'(t)   &= f'(t) - 2t - (\frac{1}{2} t^2 - \frac{1}{3} t)k. \\
            g''(t)  &= f''(t) - 2 - (t - \frac{1}{3})k. \\
            g'''(t) &= f'''(t) - k.
        \end{align*}
        $$

        因此我们只需要证明 $g'''(t)$ 在 $[0,1]$ 上存在一个零点 $\xi$ 即可.
        
        由条件 $f(0) = -1, f(1) = 0, f'(0) = 0$，我们有 $g(0) = 0, g(1) = 0, g'(0) = 0$. 而 $g(t)$ 的构造方法隐含了条件 $g(x) = 0$，因此 $g(t)$ 在 $[0,1]$ 上存在三个零点，由 Rolle 中值定理，$g'(t)$ 在 $(0,x)$ 和 $(x,1)$ 上各有一个零点 $\xi_1$ 和 $\xi_2$，于是 $g'(t)$ 在 $[0,1]$ 上存在三个零点 $0, \xi_1, \xi_2$，再使用两次 Rolle 中值定理，可得 $g''(t)$ 在 $(0, \xi_1)$ 和 $(\xi_1, \xi_2)$ 上各有一个零点 $\eta_1$ 和 $\eta_2$，$g'''(t)$ 在 $(\eta_1, \eta_2)$ 上有一个零点 $\xi$，证毕.

- 构造函数法

    !!! example
        设函数 $f(x)$ 在 $[0,1]$ 上连续，在 $(0,1)$ 上可导，$f(0) = f(1) = 0$，并且存在 $t_0 \in (0, 1)$，使得 $f(t_0) = \alpha$. 证明：

        $$
            \exists \xi \in (0,1), \enspace f'(\xi) = \alpha.
        $$

    ??? proof
        构造函数 $g(x) = f(x) - \alpha x$，则 $g'(x) = f'(x) - \alpha$，我们只需证明 $g'(x)$ 在 $(0,1)$ 上存在一个零点即可.

        代入条件，$g(0) = 0, g(1) = 1 - \alpha, g(t_0) = \alpha (1 - t_0)$，其中 $t_0 \in (0,1)$，故由连续函数的零点存在性定理，存在 $\eta \in (t_0, 1)$，使得 $g(\eta) = 0 = g(0)$，于是由 Rolle 中值定理，存在 $\xi \in (0, \eta)$，使得 $g'(\xi) = 0$，证毕.

    !!! example
        设奇函数 $f(x)$ 在 $[-1,1]$ 上具有二阶导数，且 $f(1) = 1$，证明：

        1. 存在 $\xi \in (0, 1)$，使得 $f'(\xi) = 1$.

        2. 存在 $\eta \in (-1, 1)$，使得 $f''(\eta) + f'(\eta) = 1$.

    ??? proof
        1. $f(x)$ 是奇函数，且在 $x=0$ 处有定义，故 $f(0) = 0$. 由 Lagrange 中值定理，存在 $\xi \in (0, 1)$，使得 $f'(\xi) = \dfrac{f(1) - f(0)}{1 - 0} = 1$.

        2. 我们希望能将待证式转化为证明某个函数的导数在 $(-1, 1)$ 上存在零点，这样便能使用 Rolle 中值定理来证明. 观察待证式为相邻阶导数相加的形式，考虑构造函数 $g(x) = e^x (f'(x) - 1)$，则 $g'(x) = e^x (f''(x) + f'(x) - 1)$.

            由第一问，$g(\xi) = e^{\xi} (f'(\xi) - 1) = 0$，我们只需再找 $g(x)$ 的一个零点即可. 由于 $f(x)$ 是奇函数，故 $f'(x)$ 是偶函数，进而 $f'(-\xi) = 0$，故 $g(-\xi) = 0$. 由 Rolle 中值定理，存在 $\eta \in (-\xi, \xi)$，使得 $g'(\eta) = e^{\eta} (f''(\eta) + f'(\eta) - 1) = 0$，而 $e^{\eta} > 0$，故 $f''(\eta) + f'(\eta) = 1$，证毕.

    !!! example
        设函数 $f(x)$ 在闭区间 $[-2,2]$ 上具有二阶导数，若 $(f(0))^2 + (f'(0))^2 = 4, |f(x)| \leqslant 1$，证明：

        $$
            \exists \xi \in (-2, 2), \enspace f(\xi) + f''(\xi) = 0.
        $$

    ??? proof
        根据条件 $(f(0))^2 + (f'(0))^2 = 4$，考虑构造函数 $g(x) = (f(x))^2 + (f'(x))^2$，则 $g'(x) = 2 f'(x) (f(x) + f''(x))$，出现了待证式的形式，因此我们需要找 $g'(x)$ 在 $(-2, 2)$ 上的一个零点 $\xi$，并证明 $f'(\xi) \neq 0$.

        要证明某个函数的导函数存在零点，除了使用 Rolle 中值定理外，还可以通过说明函数在某个闭区间上的最值在非区间端点的位置取得，从而这个最值点必为函数的极值点，再由 Fermat 中值定理得到导函数为 0 的结论. 我们这里尝试使用第二种方法：

        由于 $f(x)$ 在 $[-2,2]$ 上有二阶导，故 $f(x)$ 与 $f'(x)$ 必在 $[-2,2]$ 上连续，进而 $g(x)$ 在 $[-2,2]$ 上连续. 我们只需在 $[-2,2]$ 上取两个点 $\eta_1, \eta_2$，使得 $g(\eta_1) < g(0), g(\eta_2) < g(0)$，这样的话 $g(x)$ 在 $[\eta_1, \eta_2]$ 上的最大值存在且必然不在区间端点处取得.

        由 Lagrange 中值定理，存在 $\eta_1 \in (-2, 0), \eta_2 \in (0, 2)$，使得 $f'(\eta_1) = \dfrac{f(0) - f(-2)}{2}, f'(\eta_2) = \dfrac{f(2) - f(0)}{2}$，于是 $|f'(\eta_1)| \leqslant \dfrac{|f(0)| + |f(-2)|}{2} \leqslant 1, |f'(\eta_2)| \leqslant \dfrac{|f(2)| + |f(0)|}{2} \leqslant 1$，进而 $g(\eta_1) \leqslant 1 + 1 = 2, g(\eta_2) \leqslant 1 + 1 = 2$，于是存在 $\xi \in (\eta_1, \eta_2)$，使得 $\xi$ 为 $g(x)$ 的一个最大值点和极大值点，$g'(\xi) = 0, g(\xi) \geqslant g(0) = 4$，而 $|f(\xi)| \leqslant 1$，故 $f'(\xi) \neq 0$，证毕.

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

    1. （23-24 期末 T5）证明导函数介值定理：若 $f(x)$ 在 $\mathbb{R}$ 上可导，且 $f'(0) < a < f'(1)$，则存在 $\xi \in (0,1)$，使得 $f'(\xi) = a$.

    2. （22-23 期末 T3）已知 $g(x)$ 有二阶连续导数，$g(0)=1$，$g'(0)=0$，且

        $$
            f(x) = \begin{cases} \dfrac{g(x)-\cos x}{x} &\enspace, x \neq 0 \\ a &\enspace,x=0 \end{cases}
        $$

        1. 若 $f(x)$ 在 $x=0$ 处连续，求 $a$.
    
        2. 已知 $f(x)$ 在 $x=0$ 处连续，讨论 $f'(x)$ 在 $x=0$ 处的连续性.

??? proof "Proof :: 23-24 期末 T5"
    设 $g(x)=f(x)-ax$，则只需证明 $\exists \xi \in (0,1), g'(\xi)=0$.

    $g(x)=f(x)-ax \in C[0,1]$，由闭区间上连续函数的最值定理可知 $g(x)$ 在 $[0,1]$ 上有最小值.

    $g'(0)=\displaystyle\lim_{x\to 0} \frac{g(x)-g(0)}{x}<0$，由局部保号性可知 $\exists \delta_1>0, \forall x \in (0,\delta_1), g(x)<g(0)$.

    $g'(1)=\displaystyle\lim_{x\to 1} \frac{g(x)-g(1)}{x}>0$，由局部保号性可知 $\exists \delta_2>0, \forall x \in (1-\delta_2,1), g(x)<g(0)$.

    所以 $g(0)$ 和 $g(1)$ 都不是 $g(x)$ 在 $[0,1]$ 上的最小值，故最小值在 $(0,1)$ 上的某个点 $\xi$ 处取得，$\xi$ 同时也是极小值.

    又因为 $g(x)\in D[0,1]$，由 Fermat 引理可知 $g'(\xi)=0$，得证.

??? proof "Solution :: 22-23 期末 T3"
    1. $\lim\limits_{x\to 0} f(x) = \lim\limits_{x\to 0} \dfrac{g(0) + g'(0) x + o(x) - 1 + o(x)}{x} = f(0) = a$，故 $a=0$.

    2. 通过 Taylor 展开可得 $f'(0) = \lim\limits_{x\to 0} \dfrac{f(x) - f(0)}{x} = \lim\limits_{x\to 0} \dfrac{g(x) - \cos x}{x^2} = \dfrac{1}{2} (g''(0) + 1)$.

        同理有 $\lim\limits_{x\to 0} f'(x) = \lim\limits_{x\to 0} \dfrac{(g'(x) + \sin x) x - g(x) + \cos x}{x^2} = \dfrac{1}{2} (g''(0) + 1) = f'(0)$.

        故 $f'(x)$ 在 $x=0$ 处连续.

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

??? Proof "Solution"
    1. 由 $f(x) = \tan x = \dfrac{\sin x}{\cos x} = (\sin x)(\cos x)^{-1}$，将分子分母用 Taylor 公式展开有

        $$
            f(x) = (x - \frac{1}{6}x^3 + \frac{1}{120}x^5 + o(x^5))(1 - \frac{1}{2}x^2 + \frac{1}{24}x^4 + o(x^5))^{-1}.
        $$

        再将 $\left(1 - \dfrac{1}{2}x^2 + \dfrac{1}{24}x^4 + o(x^5)\right)^{-1}$ 在 $x=0$ 处展开有

        $$
            f(x) = \left(x - \frac{1}{6}x^3 + \frac{1}{120}x^5 + o(x^5)\right)\left(1 + \left(\frac{1}{2}x^2 + \frac{1}{24}x^4 + o(x^5)\right) + \left(\frac{1}{2}x^2 + \frac{1}{24}x^4 + o(x^5)\right)^2 + o(x^5)\right).
        $$

        化简后即有 $f(x) = x + \dfrac{1}{3}x^3 + \dfrac{2}{15}x^5 + o(x^5)$，即 $f(x)$ 展开至 $x^5$ 的麦克劳林公式.
    
    2. 对 $f(x)$ 求导有 $f'(x) = \dfrac{1}{1 + x^2}$，再对 $f'(x)$ 泰勒展开有

        $$
            f'(x) = \dsum_{k=0}^{n} (-1)^k x^{2k} + o(x^{2n+1}).
        $$

        两边求积分，并用 $f(0) = 0$ 代入得

        $$
            f(x) = \dsum_{k=0}^{n} (-1)^k \frac{x^{2k+1}}{2k+1} + o(x^{2n+2}).
        $$

        由 Taylor 展开的唯一性，上式便为 $f(x)$ 在 $x=0$ 处的 Taylor 展开，故有

        $$
            \frac{f^{(n)}(0)}{n!} = \begin{cases}
                0 \enspace &, n = 2k, k \in \mathbb{N} \\
                \dfrac{(-1)^{\frac{n - 1}{2}}}{n} \enspace &, n = 2k+1, k \in \mathbb{N}
            \end{cases}
        $$

        因此，$f^{(n)}(0) = \begin{cases}
            0 \enspace &, n = 2k, k \in \mathbb{N} \\
            (-1)^{\frac{n-1}{2}} (n-1)! \enspace &, n = 2k+1, k \in \mathbb{N}
        \end{cases}$
    
    3. 令 $t = x - 1$，则
    
        $$
        \begin{align*}
            \lim\limits_{x \to 1} \dfrac{x^x - x}{1 - x + \ln x}
            &= \lim\limits_{t \to 0} \dfrac{(t + 1)^{t + 1} - t - 1}{-t + \ln (1 + t)} \\
            &= \lim\limits_{t \to 0} \dfrac{e^{(t+1)\ln(t+1)} - t - 1}{-t + t - \frac{1}{2} t^2 + o(t^2)} \\
            &= \lim\limits_{t \to 0} \dfrac{e^{(t + 1)(t - \frac{1}{2} t^2 + o(t^2))} - t - 1}{-\frac{1}{2} t^2 + o(t^2)} \\
            &= \lim\limits_{t \to 0} \dfrac{1 + (t + \frac{1}{2} t^2 + o(t^2)) + \frac{1}{2} (t + \frac{1}{2} t^2 + o(t^2))^2 + o(t^2) - t - 1}{-\frac{1}{2} t^2 + o(t^2)} \\
            &= \lim\limits_{t \to 0} \dfrac{t^2 + o(t^2)}{-\frac{1}{2} t^2 + o(t^2)} \\
            &= -2.
        \end{align*}
        $$

在计算问题中我们常用 Peano 型余项来估阶，而在证明问题中则更常用 Lagrange 型余项来确定余项的具体范围. 事实上，Taylor 公式相当于利用了更高阶的导数提供的信息，得到了比 Lagrange 中值定理更强的结论，当题目中出现高阶导数时，通常都会使用 Taylor 展开式来求解.

!!! example
    1. 已知 $f(x)$ 在 $(-1, 2)$ 上有二阶导数，且 $f'(\frac{1}{2})=0$. 证明：

        $$
            \exists \xi \in (0, 1), \enspace |f''(\xi)| \geqslant 4|f(1) - f(0)|.
        $$

??? proof
    题目给出条件 $f'\left(\frac{1}{2}\right) = 0$，而待证式中存在二阶导数，我们尝试在 $x = \frac{1}{2}$ 处对 $f(x)$ 进行 Taylor 展开：

    $$
        f(x) = f\left(\frac{1}{2}\right) + \frac{f''(\xi)}{2} \left(x - \frac{1}{2}\right)^2.
    $$

    代入 $x = 0$ 与 $x = 1$，有

    $$
        \begin{cases}
            f(0) &= f\left(\dfrac{1}{2}\right) + \dfrac{1}{8} f''(\xi_1). \\[1em]
            f(1) &= f\left(\dfrac{1}{2}\right) + \dfrac{1}{8} f''(\xi_2).
        \end{cases}
    $$

    两式相减并取绝对值有

    $$
        |f(1) - f(0)| = \frac{1}{8} |f''(\xi_1) + f''(\xi_2)| \leqslant \frac{1}{8} (|f''(\xi_1)| + |f''(\xi_2)|).
    $$

    令 $|f''(\xi)| = \max \{|f''(\xi_1)|, |f''(\xi_2)|\}$，则有

    $$
        4|f(1) - f(0)| \leqslant |f''(\xi)|.
    $$

<!-- 泰勒取点在待求点 -->
!!! example
    设函数 $f(x)$ 在 $[0,1]$ 上有二阶导数，且 $|f(x)| \leqslant A, |f''(x)| \leqslant B$，证明：$\forall x \in [0,1]$，有

    $$
        |f'(x)| \leqslant 2A + \frac{B}{2}.
    $$

??? proof
    题目给了区间上任意点的函数值和二阶导数的上界，因此我们考虑在任意点 $x$ 处的 Taylor 展开：

    $$
        f(x_0) = f(x) + f'(x) (x_0 - x) + \frac{f''(\xi_x)}{2} (x_0 - x)^2.
    $$

    代入区间端点 $x_0 = 0$ 和 $x_0 = 1$，有

    $$
    \begin{cases}
        f(0) &= f(x) - f'(x) x + \dfrac{f''(\xi_0)}{2} x^2, \\[0.5em]
        f(1) &= f(x) + f'(x) (1 - x) + \dfrac{f''(\xi_1)}{2} (1 - x)^2.
    \end{cases}
    $$

    两式相减并取绝对值有

    $$
    \begin{align*}
        |f'(x)| &= \left| f(1) - f(0) - \dfrac{f''(\xi_1)}{2} (1-x)^2 + \dfrac{f''(\xi_0)} x^2 \right| \\
                &\leqslant |f(1)| + |f(0)| + \left|\dfrac{f''(\xi_1)}{2} \right| (1-x)^2 + \left|\dfrac{f''(\xi_0)}{2} \right| x^2 \\
                &\leqslant 2A + \frac{B}{2} ((1-x)^2 + x^2) \\
                &\leqslant 2A + \frac{B}{2}.
    \end{align*}
    $$

<!-- 泰勒取点在极值点 -->

!!! example
    设函数 $f(x)$ 在 $[a,b]$ 上连续，在 $(a,b)$ 上二阶可导，$f(a) = f(b) = 0$，且 $|f''(x)| \geqslant m > 0$，证明：

    $$
        \max\limits_{a \leqslant x \leqslant b} |f(x)| \geqslant \frac{m}{8} (b-a)^2.
    $$

??? proof
    $f(a) = f(b) = 0$，由 Rolle 中值定理可知 $\exists \xi \in (a,b)$，使得 $f'(\xi) = 0$.

    由 $|f''(x)| \geqslant m > 0$，据导函数介值定理可知 $f''(x)$ 在 $(a,b)$ 上不变号，否则就一定存在 $x_0 \in (a,b)$ 使得 $f''(x_0) = 0 < m$.

    因此，$f'(x)$ 在 $(a,b)$ 上严格单调，故 $x = \xi$ 为 $f'(x)$ 在 $(a,b)$ 上的唯一零点，$f(x)$ 在 $(a,b)$ 上的唯一极值点.

    不妨设 $f''(x) < 0$，则 $f'(x)$ 在 $(a,b)$ 上严格单调递减，$f(x)$ 在 $(a, \xi)$ 上严格单调递增，在 $(\xi, b)$ 上严格单调递减，$x = \xi$ 为 $f(x)$ 在 $(a,b)$ 上的唯一极大值点.

    因此 $\max\limits_{a \leqslant x \leqslant b} |f(x)| = f(\xi)$，我们只要证 $f(\xi) \geqslant \dfrac{m}{8} (b-a)^2$ 即可.

    将 $f(x)$ 在 $x = \xi$ 处 Taylor 展开，有

    $$
        f(x) = f(\xi) + f'(\xi) (x - \xi) + \frac{f''(\eta_x)}{2} (x - \xi)^2 = f(\xi) + \frac{f''(\eta_x)}{2} (x - \xi)^2.
    $$

    代入 $x = a$ 和 $x = b$，有

    $$
    \begin{cases}
        0 = f(a) &= f(\xi) + \dfrac{f''(\eta_0)}{2} (a - \xi)^2, \\[0.5em]
        0 = f(b) &= f(\xi) + \dfrac{f''(\eta_1)}{2} (b - \xi)^2.
    \end{cases}
    $$

    两式相加并取绝对值有

    $$
    \begin{align*}
        2|f(\xi)| &= \left|\dfrac{f''(\eta_0)}{2} (a - \xi)^2 + \dfrac{f''(\eta_1)}{2} (b - \xi)^2 \right| \\
                  &\geqslant \left|\dfrac{m}{2} \left((a - \xi)^2 + (b - \xi)^2\right) \right| \\
                  &\geqslant \frac{m}{4} (b-a)^2.
    \end{align*}
    $$

    因而 $\max\limits_{a \leqslant x \leqslant b} |f(x)| = f(\xi) \geqslant \dfrac{m}{8} (b-a)^2$.
    

<!-- 泰勒取点在区间端点 -->

!!! example
    设 $f(x)$ 在 $I$ 上二阶可导，设区间 $[a,b] \subset I$，满足 $f'(a) = f'(b) = 0$. 证明：存在 $\xi \in (a,b)$，使得

    $$
        |f''(\xi)| \geqslant \frac{4}{(b-a)^2} |f(b) - f(a)|.
    $$

??? proof
    利用 $f'(a) = f'(b) = 0$ 的条件，在 $x=a$ 和 $x=b$ 处对 $f(x)$ 进行 Taylor 展开，有

    $$
    \begin{align*}
        f(x) &= f(a) + \frac{f''(\xi_0)}{2} (x-a)^2,
        f(x) &= f(b) + \frac{f''(\xi_1)}{2} (x-b)^2.
    \end{align*}
    $$

    粮食相减并取绝对值有

    $$
    \begin{align*}
        |f(b) - f(a)| &= \left|\frac{f''(\xi_0)}{2} (x-a)^2 - \frac{f''(\xi_1)}{2} (x-b)^2\right| \\
                      &\leqslant \frac{|f''(\xi_0)|}{2} (x-a)^2 + \frac{|f''(\xi_1)|}{2} (x-b)^2 \\
                      &\leqslant \frac{|f''(\xi)|}{2} ((x-a)^2 + (x-b)^2)
                      &\leqslant \frac{|f''(\xi)|}{4} (b-a)^2.
    \end{align*}
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

??? proof "Proof :: 20-21 期末 T6"
    向 $f''(x) + \left[f'(x) \right]^2 = x$ 代入 $x=0$，有 $f''(0) = 0$.

    由 $f''(x) + \left[f'(x) \right]^2 = x$ 可知 $f(x)$ 二阶可导，将该方程变形为

    $$
        f''(x) = x - \left[f'(x) \right]^2,
    $$

    则由右边函数可导，知 $f''(x)$ 可导，即 $f(x)$ 的三阶导函数存在，且 $f'''(x) = 1 - 2f(x)f'(x)$.

    代入 $x=0$，有 $f'''(x) = 1 - 0 = 1 > 0$，而 $f''(x) = f'(x) = 0$，由极值点判定定理 II 可知 $x=0$ 不是 $f(x)$ 的极值点.

??? proof "Solution :: 23-24 期末 T4"
    分以下三种情况讨论：

    1. $x \geqslant 1$，则

        $$
            f(x) = x\int_{-1}^1 e^{t^2} \intdd{t} - \int_{-1}^1 te^{t^2} \intdd{t}
                            = x\int_{-1}^1 e^{t^2} \intdd{t} - \frac{1}{2} e^{t^2} \bigg{|}_{-1}^1
                            \geqslant \int_{-1}^1 e^{t^2} \intdd{t}
        $$

    2. $x \leqslant -1$，则

        $$
            f(x) = \int_{-1}^1 te^{t^2} \intdd{t} - x\int_{-1}^1 e^{t^2} \intdd{t}
                            = \frac{1}{2} e^{t^2} \bigg{|}_{-1}^1 - x\int_{-1}^1 e^{t^2} \intdd{t}
                            \geqslant \int_{-1}^1 e^{t^2} \intdd{t}
        $$

    3. $-1<x<1$，则

        $$
        \begin{align*}
            f(x) &= x\int_{-1}^x e^{t^2} \intdd{t} - \int_{-1}^x te^{t^2} \intdd{t} + \int_x^1 te^{t^2} \intdd{t} - x\int_x^1 e^{t^2} \intdd{t} \\
                    &= x \left(\int_{-1}^x e^{t^2}\intdd{t} - \int_x^1 e^{t^2}\intdd{t} \right) + e - e^{x^2}
        \end{align*}
        $$

        求导可得 $f'(x) = \displaystyle\int_{-1}^x e^{t^2}\intdd{t} - \int_x^1 e^{t^2}\intdd{t},\quad f''(x)=2e^{x^2}>0$，故 $f'(x)$ 单调递增.

        由 $f''(x)=2e^{x^2}$ 为偶函数，知 $f'(0)=0$，故 $f(x)$ 在 $[-1,0]$ 上单调递减，在 $[0,1]$ 上单调递增，在 $x=0$ 处取得最小值 $e-1$.

    由于 $\displaystyle\int_{-1}^1 e^{t^2} \intdd{t} = 2\int_0^1 e^{t^2} \intdd{t} > 2\int_0^1 te^{t^2} \intdd{t} = e-1$，故 $e-1$ 为 $f(x)$ 在 $\mathbb{R}$ 上的最小值.

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

??? proof
    由条件知 $f(x) \in C[0,1] \cap D(0,1)$，据 Lagrange 中值定理，有

    $$
        \displaystyle{\sum_{k=1}^n \left[ f\left(\frac{k}{n}\right) - f\left(\frac{2k-1}{2n}\right) \right] = \frac{1}{2n} \sum_{k=1}^n f'(\xi_k)},
    $$

    且有 $\xi_k \in \left( \dfrac{2k-1}{2n},\dfrac{k}{n} \right) \subset \left( \dfrac{k-1}{n}, \dfrac{k}{n} \right)$.

    取分割 $\Delta \colon x_0=0,x_1=\dfrac{1}{n},\ldots,x_k=\dfrac{k}{n},\ldots,x_n=1$ 与介点组 $\{\xi_k\}$，由 $f'(x) \in C[0,1]$，知 $f'(x)$ 黎曼可积且有原函数 $f(x)$. 故
    
    $$
        \lim_{n \to +\infty} \sum_{k=1}^n \left[ f\left(\frac{k}{n}\right) - f\left(\frac{2k-1}{2n}\right) \right]
        = \frac{1}{2} \lim_{\| \Delta \| \to 0} \sum_{k=1}^{n} f'\left(\xi_k\right) \frac{1}{n}
        = \frac{1}{2} \int_{0}^{1} f'(x) \dd{x}
        = \frac{f(1)-f(0)}{2}.
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

??? proof
    $f(x)$ 在 $x=0$ 处右连续，故 $\forall \varepsilon>0, \exists \delta>0, \forall x \in (0,\delta), |f(x)-f(0)|<\varepsilon$，即 $f(0)-\varepsilon<f(x)<f(0)+\varepsilon$.

    由定积分保序性，可知 $\forall x \in (0,\delta), (f(0)-\varepsilon)x<\varphi(x)<(f(0)+\varepsilon)x$.

    所以 $\forall \varepsilon>0, \exists \delta>0, \forall x \in (0,\delta),  f(0)-\varepsilon<\dfrac{\varphi(x)}{x}<f(0)+\varepsilon$.

    所以 $\varphi'_+(0) = \displaystyle\lim_{x\to 0+} \dfrac{\varphi(x)}{x} = f(0)$.

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

??? proof "Solution"
    设 $f(x)=\displaystyle\int_0^{x^2} t^2 e^{\sin t} \dd{x}$ ，由于被积函数在 $\mathbb{R}$ 上连续，知 $f(x) \in D( \mathbb{R} )$ ，且 $f'(x)=2x^5 e^{\sin x^2}$.

    当 $x \to 0$ 时，分子和分母都趋向0，可以使用 L'Hospital 法则：

    $$
        \text{原式} = \lim_{x \to 0} \frac{2x^5 e^{\sin x^2}}{\frac{6x^5}{1+x^6}}
                    = \lim_{x \to 0} \frac{(1+x^6) e^{\sin x^2}}{3}
                    = \frac{1}{3}.
    $$

!!! example
    设 $f(x)$ 在 $[0,1]$ 上可导，且 $0 \leqslant f'(x) \leqslant 1, f(0) = 0$，证明：

    $$
        \left( \int_0^1 f(x) \intdd{x} \right)^2 \geqslant \int_0^1 f^3(x) \intdd{x}.
    $$

    Hint: 将普通积分化为变上限积分.

??? proof
    构造函数 $F(x) = \left( \int_0^x f(t) \intdd{t} \right)^2 - \int_0^x f^3(t) \intdd{t}$，由于 $f(x)$ 在 $[0,1]$ 上连续可导，故 $F(x)$ 可导.

    $$
        F'(x) = 2 f(x) \left( \int_0^x f(t) \intdd{t} \right) - f^3(x)
              = f(x) (2 \int_0^x f(t) \intdd{t} - f^2(x)).
    $$

    设 $G(x) = 2 \int_0^x f(t) \intdd{t} - f^2(x)$，则 $G(x)$ 在 $[0,1]$ 上也可导，且

    $$
        G'(x) = 2 f(x) - 2 f(x) f'(x) = 2f(x) (1 - f'(x)).
    $$

    由于 $0 \leqslant f'(x) \leqslant 1$，故 $f(x) \geqslant f(0) = 0$，因此 $G'(x) \geqslant 0$，$G(x)$ 在 $[0,1]$ 上单调递增，进而有 $G(x) \geqslant G(0) = 0$.

    因此 $F'(x) = f(x) G(x) \geqslant 0$，$F(x)$ 在 $[0,1]$ 上单调递增，故 $F(1) \geqslant F(0) = 0$，证毕.

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

??? proof
    直接对 $\dint_0^{\pi} f(x) \intdd{x}$ 在 $[0, \pi]$ 上使用积分第一中值定理只能得到 $f(x)$ 在 $[0,\pi]$ 上存在至少一个根，而无法说明存在两个不等的根. 但这给我们了一个启发：如果我们可以找到 $[0,\pi]$ 的两个不重叠的子区间，使 $f(x)$ 在这两个区间上的定积分均为 $0$，那么这两个区间上分别存在 $f(x)$ 的一个根，这就证明了 $f(x)$ 在 $[0,\pi]$ 上存在两个不等的根.

    由 $\cos x$ 在 $[0, \pi]$ 上单调，据积分第二中值定理，存在 $\xi \in [0, \pi]$，使得

    $$
        0 = \dint_0^{\pi} f(x) \cos x \intdd{x} = cos 0 \dint_0^{\xi} f(x) \intdd{x} + cos \pi \dint_{\xi}^{\pi} f(x) \intdd{x} = \dint_0^{\xi} f(x) \intdd{x} - \dint_{\xi}^{\pi} f(x) \intdd{x}.
    $$

    而我们又有

    $$
        \dint_0^{\xi} f(x) \intdd{x} + \dint_{\xi}^{\pi} f(x) \intdd{x} = \dint_0^{\pi} f(x) \intdd{x} = 0.
    $$

    上两式联立可得 $\dint_0^{\xi} f(x) \intdd{x} = \dint_{\xi}^{\pi} f(x) \intdd{x} = 0$. 分别在 $[0, \xi]$ 和 $[\xi, \pi]$ 使用积分第一中值定理，即有 $\xi_1, \xi_2 \in (0, \pi)$，使得 $f(\xi_1) = f(\xi_2) = 0$，证毕.

!!! example "Riemann-Lebesque 引理"
    设 $f(x)$ 在 $[a,b]$ 上单调，证明：

    $$
    \begin{align*}
        \lim\limits_{\lambda \to +\infty} \int_{a}^{b} f(x) \cos \lambda x \intdd{x} &= 0, \\
        \lim\limits_{\lambda \to +\infty} \int_{a}^{b} f(x) \sin \lambda x \intdd{x} &= 0.
    \end{align*}
    $$

    Hint：注意单调条件的运用

??? proof
    由 $f(x)$ 在 $[a,b]$ 上单调，使用积分第二中值定理，则存在 $\xi \in [a,b]$，使得

    $$
    \begin{align*}
        \left|\int_a^b f(x) \cos \lambda x \intdd{x}\right| &= \left|f(a) \int_a^{\xi} \cos \lambda x \intdd{x} + f(b) \int_{\xi}^b \cos \lambda x \intdd{x} \right| \\
                                                            &= \left|\frac{1}{\lambda} (f(a) (\sin \lambda \xi - \sin \lambda a) + f(b) (\sin \lambda b - \sin \lambda \xi)) \right| \\
                                                            &\leqslant \frac{2}{\lambda} (|f(a)| + |f(b)|).
    \end{align*}
    $$

    令 $\lambda \to + \infty$，则上式趋于 0. 同理可证 $\lim\limits_{\lambda \to +\infty} \dint_a^b f(x) \sin \lambda x \intdd{x} = 0$.

#### 积分不等式

- 绝对值不等式：$\left\vert \dint_a^b f(x) \intdd{x}\right\vert \leqslant \dint_a^b |f(x)| \intdd{x}$.

- Cauchy-Schwarz 不等式：$\left( \dint_a^b |f(x)g(x)| \intdd{x} \right)^{2} \leqslant \left( \dint_a^b f^2(x) \intdd{x} \right) \left( \dint_a^b g^2(x) \intdd{x} \right)$.

!!! tip "积分不等式的三种常见解法"
    - 利用保号性对被积函数放缩
    - 定积分化变上限积分后求导研究
    - 利用已有积分不等式

!!! example
    设 $g(x)$ 在 $[0,1]$ 上可积，且 $\dint_0^1 g(x) \intdd{x} = 0$，证明：

    $$
        \left[ \int_0^1 x g(x) \intdd{x} \right]^2 \leqslant \frac{1}{12} \int_0^1 g^2(x) \intdd{x}.
    $$

??? proof
    观察待证式和条件的结构，发现待证式与 Cauchy-Schwarz 不等式形式相似，尝试直接进行放缩：

    $$
        \left[ \int_0^1 x g(x) \intdd{x} \right]^2 \leqslant \left(\int_0^1 x^2 \intdd{x} \right) \left(\int_0^1 g^2(x) \intdd{x} \right) = \frac{1}{3} \int_0^1 g^2(x) \intdd{x},
    $$

    发现放过头，需要进一步调整. 我们现在仍然未使用条件 $\int_0^1 g(x) \intdd{x} = 0$，通过该条件可以对待证式左边进行调整，再利用 Cauchy-Schwarz 不等式进行放缩：

    $$
    \begin{align*}
        \left[ \int_0^1 x g(x) \intdd{x} \right]^2
        &= \left[ \int_0^1 (x + a) g(x) \intdd{x} \right]^2 \\
        &\leqslant \left(\int_0^1 (x + a)^2 \intdd{x} \right) \left(\int_0^1 g^2(x) \intdd{x} \right) \\
        &= \frac{1}{3} \left[(1-a)^3 + a^3 \right] \int_0^1 g^2(x) \intdd{x} \\
    \end{align*}
    $$

    考虑求 $h(a) = \dfrac{1}{3}[(1-a)^3 + a^3] = a^2 - a + \dfrac{1}{3}$ 的最小值，发现当 $a = \dfrac{1}{2}$ 时，$h(a) = \dfrac{1}{12}$ 取得最小值. 故

    $$
        \left[ \int_0^1 x g(x) \intdd{x} \right]^2 = \left[ \int_0^1 \left(x + \dfrac{1}{2}\right) g(x) \intdd{x} \right]^2 \leqslant \frac{1}{12} \int_0^1 g^2(x) \intdd{x}.
    $$

!!! example
    设 $a>0$ 为常数，证明：

    $$
        \int_0^{\pi} x a^{\sin x} \intdd{x} \cdot \int_0^{\frac{\pi}{2}} a^{-\cos x} \geqslant \frac{\pi^3}{4}.
    $$

??? proof
    对待证式左边的两个积分分别进行处理：

    $$
    \begin{align*}
        \int_0^{\pi} x a^{\sin x} \intdd{x} &= \int_0^{\frac{\pi}{2}} x a^{\sin x} \intdd{x} + \int_{\frac{\pi}{2}}^{\pi} x a^{\sin x} \intdd{x} \\
                                            &= \int_0^{\frac{\pi}{2}} x a^{\sin x} \intdd{x} + \int_0^{\frac{\pi}{2}} (\pi - x) a^{\sin x} \intdd{x} \\
                                            &= \pi \int_0^{\frac{\pi}{2}} a^{\sin x} \intdd{x}.
    \end{align*}
    $$

    $$
        \int_0^{\frac{\pi}{2}} a^{-\cos x} \intdd{x} = \int_0^{\frac{\pi}{2}} a^{-\sin x} \intdd{x}.
    $$

    故

    $$
        \mathrm{LHS} = \pi \int_0^{\frac{\pi}{2}} a^{\sin x} \intdd{x} \int_0^{\frac{\pi}{2}} a^{-\sin x} \intdd{x} \geqslant \pi \left(\int_0^{\frac{\pi}{2}} \intdd{x}\right)^2 = \frac{\pi^3}{4}.
    $$

!!! example
    设 $f(x)$ 在 $[0,a]$ 上的导函数连续，$f(0) = 0$，证明：

    $$
        \int_0^a |f(x) f'(x)| \intdd{x} \leqslant \frac{a}{2} \int_0^a (f'(x))^2 \intdd{x}.
    $$

??? proof
    令 $F(x) = \int_0^a |f'(x)| \intdd{x}$，则 $F'(x) = |f'(x)|$，并且 $F(x) \geqslant \left|\dint_0^a f'(x) \intdd{x}\right| = |f(x)|$. 利用该不等式，待证式左侧可化为

    $$
        \int_0^a |f(x)||f'(x)| \intdd{x} \leqslant \int F(x) F'(x) \intdd{x} = \frac{1}{2} (F^2(a) - F^2(0)) = \frac{1}{2} F^2(a).
    $$

    利用 Cauchy-Schwarz 不等式，有

    $$
        \frac{1}{2} F^2(a) = \frac{1}{2} \left( \int_0^a |f'(x)| \intdd{x} \right)^2 \leqslant \frac{a}{2} \int_0^a (f'(x))^2 \intdd{x}.
    $$

    证毕.

!!! example
    设 $f(x)$ 在 $[a,b]$ 上连续且单调递增，证明：

    $$
        \int_a^b x f(x) \intdd{x} \geqslant \frac{a+b}{2} \int_a^b f(x) \intdd{x}.
    $$

??? proof
    没有想法时可以尝试做差法，待证式化为

    $$
        \int_a^b \left(x - \frac{a+b}{2}\right) f(x) \intdd{x} \geqslant 0.
    $$

    由于 $g(x) = x - \frac{a+b}{2}$ 关于 $[a,b]$ 的中点 $\dfrac{a+b}{2}$ 中心对称，有 $-\dint_a^{\frac{a+b}{2}} g(x) \intdd{x} = \dint_{\frac{a+b}{2}}^b g(x) \intdd{x} = \dfrac{(b-a)^2}{8} > 0$.

    根据以上性质，我们尝试将积分区间按照中点 $\dfrac{a+b}{2}$ 分割，得到

    $$
        \int_a^b \left(x - \frac{a+b}{2}\right) f(x) \intdd{x} = \int_a^{\frac{a+b}{2}} \left(x - \frac{a+b}{2}\right) f(x) \intdd{x} + \int_{\frac{a+b}{2}}^b \left(x - \frac{a+b}{2}\right) f(x) \intdd{x}.
    $$

    由于 $g(x)$ 在 $\left[a, \dfrac{a+b}{2} \right]$ 和 $\left[\dfrac{a+b}{2}, b \right]$ 上分别不变号，利用积分第一中值定理，存在 $\xi_1 \in \left(a, \dfrac{a+b}{2} \right)$ 和 $\xi_2 \in \left(\dfrac{a+b}{2}, b \right)$，使得

    $$
    \begin{align*}
        \int_a^b \left(x - \frac{a+b}{2}\right) f(x) \intdd{x}
        &= \int_a^{\frac{a+b}{2}} \left(x - \frac{a+b}{2}\right) f(x) \intdd{x} + \int_{\frac{a+b}{2}}^b \left(x - \frac{a+b}{2}\right) f(x) \intdd{x} \\
        &= f(\xi_1) \int_a^{\frac{a+b}{2}} \left(x - \frac{a+b}{2}\right) \intdd{x} + f(\xi_2) \int_{\frac{a+b}{2}}^b \left(x - \frac{a+b}{2}\right) \intdd{x} \\
        &= \frac{(b-a)^2}{8}(f(\xi_2) - f(\xi_1)).
    \end{align*}
    $$

    由 $f(x)$ 在 $[a,b]$ 上单调递增，有 $f(\xi_1) \leqslant f(\xi_2)$，故上式非负，证毕.

!!! example
    设函数 $f(x)$ 在 $[0,1]$ 上可导，且 $f(x)$ 不恒等于 $0$，$\int_0^1 f(x) \intdd{x} = 0$，证明：

    $$
        \int_0^1 |f(x)| \intdd{x} \cdot \int_0^1 |f'(x)| \intdd{x} \geqslant 2 \int_0^1 f^2(x) \intdd{x}.
    $$

??? proof
    令变上限积分 $F(x) = \dint_0^x f(t) \intdd{t}$，则由条件有 $F(0) = F(1) = 0$. 对待证式右侧进行处理：

    $$
    \begin{align*}
        \left| \int_0^1 f^2(x) \intdd{x} \right| &= \left| \int_0^1 f(x) \intdd{F(x)} \right| \\
                                                 &= \left| f(x) F(x) \bigg{|}_{0}^{1} - \int_0^1 F(x) f'(x) \intdd{x} \right| \\
                                                 &= \left|\int_0^1 F(x) f'(x) \intdd{x} \right| \\
                                                 &\leqslant \int_0^1 |F(x)| |f'(x)| \intdd{x}.
    \end{align*}
    $$

    再利用积分第一中值定理，存在 $\xi \in (0,1)$，使得

    $$
        \left| \int_0^1 f^2(x) \intdd{x} \right| \leqslant |F(\xi)| \int_0^1 |f'(x)| \intdd{x}.
    $$

    下面证明 $2|F(\xi)| \leqslant \dint_0^1 |f(x)| \intdd{x}$. 将右侧以 $\xi$ 为分界点拆成两个积分，再利用绝对值不等式进行放缩：

    $$
    \begin{align*}
        \int_0^1 |f(x)| \intdd{x} &= \int_0^{\xi} |f(x)| \intdd{x} + \int_{\xi}^1 |f(x)| \intdd{x} \\
                                  &\geqslant \left|\int_0^{\xi} f(x) \intdd{x} \right| + \left|\int_{\xi}^1 f(x) \intdd{x} \right| \\
                                  &= |F(\xi)| + |F(1) - F(\xi)| \\
                                  &= 2|F(\xi)|.
    \end{align*}
    $$

    综上，

    $$
        2 \left| \int_0^1 f^2(x) \intdd{x} \right| \leqslant 2 |F(\xi)| \int_0^1 |f'(x)| \intdd{x} \leqslant \int_0^1 |f(x)| \intdd{x} \int_0^1 |f'(x)| \intdd{x}.
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

??? proof "Solution"
    观察到分母和分子次数相差很大，因此考虑分子分母同乘 $x$ 的幂，对分子凑微分后换元降幂.

    $$
    \begin{align*}
        \int_1^2 \frac{1}{x^{10} + x} \intdd{x}
        &= \int_1^2 \frac{x^8}{x^{18} + x^9} \intdd{x} \\
        &= \frac{1}{9} \int_1^2 \frac{1}{x^{18} + x^9} \intdd{x^9} \\
        &= \frac{1}{9} \int_1^{512} \frac{1}{t(t+1)} \intdd{t} \\
        &= \frac{1}{9} \left(\ln \frac{t}{t+1} \right) \bigg{|}_{1}^{512} \\
        &= \frac{1}{9} \ln \frac{1024}{513}.
    \end{align*}
    $$

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
            \dfrac{(n-1)!!}{n!!}                &\enspace ,n = 2k+1
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
        
        ??? proof "Solution"
            设 $I = \dint_{-\pi}^{\pi} \dfrac{x \sin x \cdot \arctan e^x}{1 + \cos^2 x} \intdd{x}$，令 $t = -x$，则有

            $$
                I = \int_{\pi}^{-\pi} \frac{-t \sin (-t) \cdot \arctan e^{-t}}{1 + \cos^2 (-t)} -\intdd{t}
                  = \int_{-\pi}^{\pi} \frac{t \sin t \cdot \arctan e^{-t}}{1 + \cos^2 t} \intdd{t}.
            $$

            因此有

            $$
            \begin{align*}
                I &= \frac{1}{2} \int_{-\pi}^{\pi} \frac{x \sin x \cdot \arctan e^x}{1 + \cos^2 x} \intdd{x} + \frac{1}{2} \int_{-\pi}^{\pi} \frac{x \sin x \cdot \arctan e^{-x}}{1 + \cos^2 x} \intdd{x} \\
                  &= \frac{1}{2} \int_{-\pi}^{\pi} \frac{x \sin x}{1 + \cos^2 x} (\arctan e^x + \arctan e^{-x}) \intdd{x} \\
                  &= \frac{1}{2} \int_{-\pi}^{\pi} \frac{\pi}{2} \frac{x \sin x}{1 + \cos^2 x} \intdd{x}.
            \end{align*}
            $$

            由于 $\dfrac{x \sin x}{1 + \cos^2 x}$ 是偶函数，因此

            $$
                I = \frac{\pi}{2} \int_{0}^{\pi} \frac{x \sin x}{1 + \cos^2 x} \intdd{x}.
            $$

            设 $u = \pi - x$，则有

            $$
                I = \frac{\pi}{2} \int_{0}^{-\pi} \frac{(\pi - u) \sin u}{1 + \cos^2 u} -\intdd{u} = \frac{\pi}{2} \int_{0}^{\pi} \frac{(\pi - u) \sin u}{1 + \cos^2 u} \intdd{u}.
            $$

            上两式相加有

            $$
            \begin{align*}
                I &= \frac{\pi}{4} \int_{0}^{\pi} \frac{\pi \sin x}{1 + \cos^2 x} \intdd{x} \\
                  &= -\frac{\pi^2}{4} \int_{0}^{\pi} \frac{\dd{\cos x}}{1 + \cos^2 x} \\
                  &= -\frac{\pi^2}{4} \arctan (\cos x) \bigg{|}_{0}^{\pi} \\
                  &= \frac{\pi^3}{8}.
            \end{align*}
            $$

        如果积分区间是 $[a,b]$，而被积函数中含有对称的 $x$ 与 $a+b-x$ 项，那么我们可以通过倒代换后进行组合的方式计算定积分：

        !!! example
            - 设 $f(x)$ 在 $[0,a]$ 上连续，并且 $f(x) + f(a-x) \neq 0$，求 $\dint_{0}^{a} \dfrac{f(x)}{f(x) + f(a-x)} \intdd{x}$.

            - 计算 $I = \dint_0^1 \dfrac{\ln (1+x)}{1 + x^2} \intdd{x}$.
            
            - 计算 $I = \dint_{-1}^1 \dfrac{\sqrt{1 - x^2}}{1 - x} \intdd{x}$.
        
        ??? proof "Solution"
            1. $\dfrac{a}{2}$. (Hint: 令 $u=a-x$，换元后得到的被积函数与原来的被积函数相加后等于 $1$)
            2. $\dfrac{\pi}{8} \ln 2$. (Hint: 令 $x = \tan t$，换元后便可以通过倒代换求解)
            3. 令 $u = -x$，换元有

                $$
                \begin{align*}
                    I &= \int_{1}^{-1} \frac{\sqrt{1-u^2}}{1 + u} -\intdd{u} \\
                      &= \int_{-1}^1 \frac{\sqrt{1-u^2}}{1 + u} \intdd{u}.
                \end{align*}
                $$

                与原积分式相加有

                $$
                \begin{align*}
                    I &= \frac{1}{2} \left(\int_{-1}^1 \frac{\sqrt{1-x^2}}{1 - x} + \int_{-1}^1 \frac{\sqrt{1-x^2}}{1 + x} \right) \\
                      &= \int_{-1}^1 \frac{\sqrt{1 - x^2}}{1 - x^2} \intdd{x} \\
                      &= \arcsin x \bigg{|}_{-1}^1 \\
                      &= \pi.
                \end{align*}
                $$
        
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

    ??? proof "Solution"
        将被积函数 $f(x) = x \ln(1 + e^x)$ 分解为奇函数 $g(x) 和偶函数 $h(x)$ 的和，可得

        $$
        \begin{align*}
            g(x) &= \frac{f(x) - f(-x)}{2} = \frac{1}{2} x \left( 2\ln (1 + e^x) - x \right), \\
            h(x) &= \frac{f(x) + f(-x)}{2} = \frac{1}{2} x \ln \frac{1 + e^x}{1 + e^{-x}} = \frac{1}{2} x^2.
        \end{align*}
        $$

        发现分解出的偶函数 $h(x)$ 十分简单，而奇函数 $g(x)$ 在积分区间 $[-2,2]$ 上的积分为 $0$，因此有

        $$
            \int_{-2}^{2} x \ln(1 + e^x) \intdd{x} = \int_{-2}^{2} \frac{1}{2} x^2 \intdd{x} = \frac{8}{3}.
        $$

- 几何意义：利用定积分的几何意义简化计算.

    !!! example
        求定积分 $\dint_{0}^{a} \sqrt{a^2 - x^2} \intdd{x}$，其中 $a>0$.

    ??? proof "Solution"
        被积函数恰为圆心位于 $(0,0)$，半径为 $a$ 的圆的上半部分，因此待求定积分恰为该圆在第一象限内的面积，即

        $$
            \int_0^a \sqrt{a^2 - x^2} \intdd{x} = \frac{\pi a^2}{4}.
        $$

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
    2. 计算极限 $I = \lim\limits_{n \to +\infty} \dfrac{1}{n^4} \displaystyle\prod_{k=1}^{2n} (n^2 + k^2)^{\frac{1}{n}}$.
    3. 设 $f(x)$ 在 $[0,1]$ 上的一阶导数连续，证明：

        $$
            \lim\limits_{n \to \infty} \sum_{k=1}^n \left[ f\left( x + \frac{k}{n^2 + k^2} \right) - f(x) \right] = \frac{\ln 2}{2} f'(x).
        $$

??? proof "Solution"
    1. 设函数 $f(x) = \dfrac{\cos x}{1 + \sin x}$，则 $f(x)$ 在 $[0, 1]$ 上可积，因此

        $$
            \lim \limits_{n \to \infty} \dfrac{1}{n} \dsum_{k=1}^n \dfrac{\cos(\dfrac{k}{n})}{1 + \sin(\dfrac{k}{n})} = \int_0^1 \frac{\cos x}{1 + \sin x} \intdd{x} = \ln (1 + \sin x) \bigg{|}_{0}^1 = \ln (1 + \sin 1).
        $$
    
    2. 设
    
        $$
        \begin{align*}
            J_n &= \ln \left(\dfrac{1}{n^4} \displaystyle\prod_{k=1}^{2n} (n^2 + k^2)^{\frac{1}{n}}\right) \\
                &= \dsum_{k=1}^{2n} \frac{1}{n} \ln (n^2 + k^2) - 4 \ln n \\
                &= \frac{1}{n} \dsum_{k=1}^{2n} \left[\ln \left(1 + \frac{k^2}{n^2}\right) + 2\ln n \right] - 4 \ln n \\
                &= \frac{1}{n} \dsum_{k=1}^{2n} \ln \left(1 + \frac{k^2}{n^2}\right).
        \end{align*}
        $$

        设函数 $f(x) = \ln (1 + x^2)$，则 $f(x)$ 在 $[0, 2]$ 上可积，因此

        $$
        \begin{align*}
            \lim\limits_{n \to +\infty} J_n
            &= \int_0^2 \ln (1 + x^2) \intdd{x} \\
            &= (x \ln (1 + x^2)) \bigg{|}_{0}^{2} - \int_0^2 \frac{2x^2}{1+x^2} \intdd{x} \\
            &= 2 \ln 5 - \int_0^2 2 \intdd{x} + 2 \int_0^2 \frac{1}{1+x^2} \intdd{x} \\
            &= 2 \ln 5 - 4 + 2 \arctan 2.
        \end{align*}
        $$

        由 $g(x) = e^x$ 在 $x = 2\ln 5 - 4 + 2 \arctan 2$ 处连续，可得

        $$
            I = \lim\limits_{n \to +\infty} e^{J_n} = e^{2 \ln 5 - 4 + 2 \arctan 2} = 25 e^{2 \arctan 2 - 4}.
        $$
    
    3. 观察到 $\dfrac{k}{n^2 + k^2} \leqslant \frac{1}{2n} \to 0 \enspace (n \to +\infty)$，因此我们可以尝试将 $f(x)$ 在 $x$ 处 Taylor 展开：

        $$
            f \left(x + \frac{k}{n^2 + k^2}\right) = f(x) + \frac{k}{n^2 + k^2} f'(x) + o\left(\frac{k}{n^2 + k^2}\right).
        $$

        求和后有

        $$
        \begin{align*}
            \dsum_{k=1}^n \left[ f\left( x + \frac{k}{n^2 + k^2} \right) - f(x) \right]
            &= \dsum_{k=1}^n \left[ \frac{k}{n^2 + k^2} f'(x) + \frac{k}{n^2 + k^2} o(1) \right] \\
            &= \frac{1}{n} \dsum_{k=1}^n \frac{\frac{k}{n}}{1 + \frac{k^2}{n^2}} (f'(x) + o(1)).
        \end{align*}
        $$

        而 $g(x) = \dfrac{x}{1 + x^2}$ 在 $[0, 1]$ 上可积，因此

        $$
        \begin{align*}
            \lim\limits_{n \to \infty} \dsum_{k=1}^{2n} \left[ f\left( x + \frac{k}{n^2 + k^2} \right) - f(x) \right]
            &= f'(x) \int_0^1 \frac{x}{1 + x^2} \intdd{x} \\
            &= f'(x) \frac{1}{2} \ln (1 + x^2) \bigg{|}_{0}^1 \\
            &= \frac{\ln 2}{2} f'(x).
        \end{align*}
        $$

##### 极限中的定积分

我们知道定积分是一个数值，如果将其中部分改为参数的话便可以构造数列或者函数. 此时如果对参数求极限，一般情况下我们不能直接将极限符号与积分符号交换，而需要采用分段放缩的方法.

!!! example
    1. 证明 $\lim\limits_{n \to +\infty} \dint_{0}^{\frac{\pi}{2}} \sin^n x \intdd{x} = 0$.
    2. 求极限 $\lim\limits_{n \to +\infty} \left( \dint_1^2 e^{-n t^2} \intdd{t} \right)^{\frac{1}{n}}$.

??? proof
    1. 当 $n \to +\infty$ 时，$x = \dfrac{\pi}{2}$ 是 $f(x) = \sin^n x$ 的一个异常点，我们设 $\delta > 0$，将积分分为两部分

        $$
            \int_0^{\frac{\pi}{2}} \sin^n x \intdd{x} = \int_0^{\frac{\pi}{2} - \delta} \sin^n x \intdd{x} + \int_{\frac{\pi}{2} - \delta}^{\frac{\pi}{2}} \sin^n x \intdd{x}.
        $$

        $\forall \varepsilon > 0$，要证明 $\exists N > 0$，使得 $\forall n > N$，有 $\dint_0^{\frac{\pi}{2}} \sin^n x \intdd{x} < \varepsilon$.
    
        对于第一部分，$\forall \delta > 0$，存在 $N_1(\delta) > 0$，使得 $\forall n > N_1(\delta)$，有 $|(\sin x)^n| < \dfrac{\varepsilon}{\pi}$，因此

        $$
            \left|\int_0^{\frac{\pi}{2} - \delta} \sin^n x \intdd{x}\right| < \frac{\varepsilon}{\pi} \left(\frac{\pi}{2} - \delta\right) < \frac{\varepsilon}{2}.
        $$

        对于第二部分，取 $\delta = \dfrac{\varepsilon}{2}$，即有

        $$
            \left|\int_{\frac{\pi}{2} - \delta}^{\frac{\pi}{2}} \sin^n x \intdd{x}\right| < \left|\int_{\frac{\pi}{2} - \delta}^{\frac{\pi}{2}} \intdd{x} \right| = \delta = \frac{\varepsilon}{2}.
        $$

        因此取 $N = N_1\left(\dfrac{\varepsilon}{2}\right)$，则 $\forall n > N$，有

        $$
            \left|\int_0^{\frac{\pi}{2}} \sin^n x \intdd{x}\right| < \left|\int_0^{\frac{\pi}{2} - \delta} \sin^n x \intdd{x}\right| + \left|\int_{\frac{\pi}{2} - \delta}^{\frac{\pi}{2}} \sin^n x \intdd{x}\right| < \frac{\varepsilon}{2} + \frac{\varepsilon}{2} = \varepsilon.
        $$

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

??? proof "Solution"
    双纽线关于 $x$ 轴与 $y$ 轴对称，故我们可以只计算位于第一象限的部分面积.

    $$
        S = 4 \int_{0}^{\frac{\pi}{4}} \frac{1}{2} \cos(2\theta) \dd{\theta}
          = \sin(2\theta) \bigg{|}_0^{\frac{\pi}{4}}
          = 1
    $$

##### 旋转体体积

利用截面法可以解决旋转体体积问题：设旋转轴为 $x$ 轴，设旋转体垂直 $x$ 轴的横截面积为 $S(x)$，横截面半径为 $r(x)$，则

$$
    V = \int_{a}^{b} S(x) \intdd{x} = \int_{a}^{b} \pi r^2(x) \intdd{x}.
$$

!!! example "21-22 期末 T1.d"
    求 $y=e^x$ 过点 $(0,0)$ 的切线 $L$，并求 $y=e^x$、$L$ 与 $y$ 轴所围图形绕 $x$ 轴旋转一周的体积.

??? proof "Solution"

    $$
        V = \int_0^1 \pi e^{2x} \intdd{x} - \frac{\pi}{3} e^2 = \frac{\pi e^2}{6} - \frac{\pi}{2}.
    $$

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

??? proof "Solution"
    1. $f(x) = \dint_{-\sqrt{3}}^{x} \sqrt{3 - t^2} \intdd{t}$，求导有 $f'(x) = \sqrt{3 - x^2}$. 故曲线 $y = f(x)$ 在 $x \in [-\sqrt{3}, \sqrt{3}]$ 上的弧长为

        $$
            L = \int_{-\sqrt{3}}^{\sqrt{3}} \sqrt{1 + (f'(x))^2} \intdd{x}
              = \int_{-\sqrt{3}}^{\sqrt{3}} \sqrt{4 - x^2} \intdd{x}
              = \sqrt{3} + 4 \arcsin \frac{\sqrt{3}}{2}.
        $$

    2. $f'(x) = \sqrt{\cos x}$，故曲线 $y = f(x)$ 在 $x \in [0,1]$ 上的弧长为

        $$
            L = \int_0^1 \sqrt{1 + (f'(x))^2} \intdd{x}
              = \int_0^1 \sqrt{1 + \cos x} \intdd{x}
              = \int_0^1 \sqrt{2 \cos^2 \frac{x}{2}} \intdd{x}
              = 2\sqrt{2} \sin \frac{1}{2}.
        $$

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

??? proof "Solution"
    先证该反常积分收敛：$\dint_{1}^{+\infty} \dfrac{1}{x^2} \intdd{x}$ 收敛，而 $f(x) = \arctan x$ 在 $[1, +\infty)$ 单调有界，由 Abel 判别法可知 $\dint_{1}^{+\infty} \dfrac{\arctan x}{x^2} \intdd{x}$ 收敛.

    下面求该反常积分的值：

    $$
    \begin{align*}
        \text{原式} &= - \int_1^{+\infty} \arctan x \dd(\frac{1}{x}) \\
                    &= - \left( \frac{\arctan x}{x} \right) \bigg{|}_1^{+\infty} + \int_1^{+\infty} \frac{1}{x(1+x^2)} \dd{x} \\
                    &= \frac{\pi}{4} + \frac{1}{2} \int_1^{+\infty} \frac{1}{x^2 (1+x^2)} \dd{x}^2 \\
                    &= \frac{\pi}{4} + \frac{1}{2} \int_1^{+\infty} \left(\frac{1}{x^2}-\frac{1}{1+x^2} \right) \dd{x}^2 \\
                    &= \frac{\pi}{4} + \frac{1}{2} \left(\ln \frac{x^2}{1+x^2} \right) \bigg{|}_1^{+\infty} \\
                    &= \frac{\pi}{4} + \frac{1}{2} \ln 2.
    \end{align*}
    $$

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
        - $\dint_{1}^{+\infty} \dfrac{1}{x^p} \intdd{x}$：当 $p > 1$ 时收敛，当 $0 < p \leqslant 1$ 时发散.
        - $\dint_{e}^{+\infty} \dfrac{1}{x \ln^p x} \intdd{x}$：当 $p > 1$ 时收敛，当 $0 < p \leqslant 1$ 时发散.

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

??? proof
    由 $\dint_0^x \sin t \intdd{t} = 1 -\cos x$ 在 $[0, +\infty)$ 上有界，$\dfrac{1}{x}$ 在 $x \to +\infty$ 时单调趋于 $0$，据 Dirichlet 判别法可知 $\dint_0^{+\infty} \dfrac{\sin x}{x} \intdd{x}$ 收敛.

    要证 $\dint_0^{+\infty} f^2(x) \intdd{x}$ 收敛，且与 $\dint_0^{+\infty} f(x) \intdd{x}$ 收敛到同一值，只需证明 $\dint_0^{+\infty} (f(x) - f^2(x)) \intdd{x}$ 收敛到 $0$ 即可.

    由 $\dint_0^{+\infty} f(x) \intdd{x}$ 收敛，令 $x = 2u$，使用换元公式得

    $$
        \int_0^{+\infty} \frac{\sin x}{x} \intdd{x}
        = \int_0^{+\infty} \frac{\sin 2u}{2u} 2\intdd{u}
        = \int_0^{+\infty} \frac{\sin 2x}{x} \intdd{x}.
    $$

    对于任意 $M \in (0, +\infty)$，

    $$
    \begin{align*}
        \int_0^{M} f(x) - f^2(x) \intdd{x}
        &= \int_0^{M} \left( \frac{\sin 2x}{x} - \frac{\sin x^2}{x^2} \right) \intdd{x} \\
        &= \int_0^{M} \frac{x \sin 2x - \sin^2 x}{x^2} \intdd{x} \\
        &= \int_0^{M} \intdd{\left(\frac{\sin^2 x}{x}\right)} \\
        &= \left( \frac{\sin^2 x}{x} \right) \bigg{|}_0^{M} \\
        &= \frac{\sin^2 M}{M} - \lim\limits_{x \to 0^+} \frac{\sin^2 x}{x} \\
        &= \frac{\sin^2 M}{M}.
    \end{align*}
    $$

    令 $M \to +\infty$，可得

    $$
        \int_0^{+\infty} (f(x) - f^2(x)) \intdd{x}
        = \lim\limits_{M \to +\infty} \int_0^{M} (f(x) - f^2(x)) \intdd{x}
        = \lim\limits_{M \to +\infty} \frac{\sin^2 M}{M}
        = 0.
    $$

    证毕.

!!! example
    设 $\dint_{a}^{+\infty} f(x) \intdd{x}$ 收敛，且 $f(x)$ 在 $[a, +\infty)$ 上一致连续，证明 $\lim\limits_{x \to +\infty} f(x) = 0$.

??? proof
    $\forall \varepsilon > 0$，要证 $\exists M > a$，使得 $\forall x > M$，有 $|f(x)| < \varepsilon$.

    由 $f(x)$ 在 $[a, + \infty)$ 上一致连续，取 $\varepsilon_0 = \dfrac{\varepsilon}{2}$，则存在 $\delta > 0$，使得 $\forall x_1, x_2 \in [a, +\infty)$ 且 $|x_1-x_2| < \delta$，有 $|f(x_1) - f(x_2)| < \varepsilon_0$.

    由 $\dint_a^{+\infty} f(x) \intdd{x}$ 收敛，根据 Cauchy 收敛原理，$\forall \varepsilon_1 > 0$，存在 $M_1(\varepsilon_1) > a$，使得 $\forall c, d > M_1(\varepsilon_1)$，有

    $$
        \left| \int_c^d f(x) \intdd{x} \right| \leqslant \varepsilon_1.
    $$

    $\forall x_0 \in (c, d)$，我们可以将上式化为

    $$
        \left |\int_c^d (f(x) - f(x_0)) \intdd{x} + f(x_0) (d - c) \right| < \varepsilon_1.
    $$

    我们取 $d - c = \delta$，则 $|f(x) - f(x_0)| < \varepsilon_0$，利用绝对值不等式，有

    $$
    \begin{align*}
        |f(x_0) (d-c)|
        &< \varepsilon_1 + \left| \int_c^d (f(x) - f(x_0)) \intdd{x} \right| \\
        &\leqslant \varepsilon_1 + \int_c^d |f(x) - f(x_0)| \intdd{x} \\
        &\leqslant \varepsilon_1 + (d-c) \varepsilon_0.
    \end{align*}
    $$

    即 $|f(x_0)| < \dfrac{\varepsilon_1}{\delta} + \varepsilon_0$. 故取 $\varepsilon_1 = \dfrac{\varepsilon}{2}$，并取 $M = M_1(\varepsilon_1)$，即可证明 $\forall x > M$，有 $|f(x)| < \varepsilon$，证毕.

## 压轴题选讲

!!! example "23-24 期末 T8"
    设函数 $f(x)$ 在 $\mathbb{R}$ 上有二阶连续的导函数，且存在常数 $C>0$，使得

    $$
        \sup_{x \in \mathbb{R}} \left( |x|^2 |f(x)| + |f''(x)| \right) \leqslant C
    $$

    证明：存在常数 $M>0$，使得 $\sup\limits_{x \in \mathbb{R}} \left(|xf'(x)|\right) \leqslant M$.

??? proof
    1. $|x|\leqslant 1$

        $f(x)$ 有二阶连续导数，故 $f'(x) \in C[-1,1]$，进而 $xf'(x) \in C[-1,1]$，
        由有界性定理可知 $\exists M_1>0, \forall x \in [-1,1], |xf'(x)|\leqslant M_1$.

    2. $|x|>1$

        $f(x)$ 有二阶连续导数，$\forall |x_0|>1$，
        
        $$
        \begin{equation}
            f(x) = f(x_0) + f'(x_0)(x-x_0) + \frac{f''(\xi)}{2}(x-x_0)^2
        \end{equation}
        $$

        代入 $x=x_0+\dfrac{1}{x_0}$，有
        
        $$
        \begin{equation}
            f(x_0+\frac{1}{x_0}) = f(x_0) + \frac{f'(x_0)}{x_0} + \frac{f''(\xi)}{2x_0^2}
        \end{equation}
        $$

        两边同乘 $x_0^2$，移项得
        
        $$
        \begin{equation}
            x_0 f'(x_0) = x_0^2 f(x_0+\frac{1}{x_0}) - x_0^2 f(x_0) - \frac{f''(\xi)}{2}  \label{eq:result1}
        \end{equation}
        $$

        等式右边第二项与第三项都是有界量，故只需证明第一项也为有界量，考虑利用有界量 $(x_0+\frac{1}{x_0})^2 f(x_0+\frac{1}{x_0})$ 进行估计：

        $$
        \begin{equation} \label{eq:result2}
            \begin{aligned}
            \left| x_0^2 f(x_0+\frac{1}{x_0}) \right| &= \left| (x_0+\frac{1}{x_0})^2 f(x_0+\frac{1}{x_0}) - 2f(x_0+\frac{1}{x_0}) - \frac{f(x_0+\frac{1}{x_0})}{x_0^2} \right| \\
                                                        &\leqslant \left| (x_0+\frac{1}{x_0})^2 f(x_0+\frac{1}{x_0}) \right| + \left| 2f(x_0+\frac{1}{x_0}) \right| + \left| \frac{f(x_0+\frac{1}{x_0})}{x_0^2} \right| \\
                                                        &< \left| (x_0+\frac{1}{x_0})^2 f(x_0+\frac{1}{x_0}) \right| + 3 \left| x_0^2 f(x_0+\frac{1}{x_0}) \right| \\
                                                        &\leqslant 4C
            \end{aligned}
        \end{equation}
        $$

        利用上两式可得

        $$
        \begin{align*}
            \left| x_0 f'(x_0) \right| &\leqslant \left| x_0^2 f(x_0+\frac{1}{x_0}) \right| + \left| x_0^2 f(x_0) \right| + \frac{\left|f''(\xi)\right|}{2} \\
                                        &\leqslant 4C + C + \frac{C}{2} \\
                                        &= \frac{11C}{2}
        \end{align*}
        $$

    综上，取 $M=\max\left\{M_1,\frac{11C}{2}\right\}$，则 $\forall x\in \mathbb{R}, |xf'(x)|\leqslant M$，
    故 $M$ 为 $|xf'(x)|$ 的一个上界，必然不小于其上确界，因此 $\sup\limits_{x \in \mathbb{R}} \left(|xf'(x)|\right) \leqslant M$.

!!! example "22-23 期末 T8"
    已知 $f(x)$ 在 $[0, 1]$ 上有二阶连续导数，证明：

    $$
        \dint_0^1 x^nf(x) \intdd{x} = \dfrac{f(1)}{n} -\dfrac{f(1) + f'(1)}{n^2}+ o\left(\dfrac{1}{n^2}\right) \enspace (n\to+\infty).
    $$

??? proof
    这边提供一个与网站上的答案不同的方法：对待证式左侧多次使用分部积分，有

    $$
    \begin{align*}
        \int_0^1 x^nf(x) \intdd{x}
        &= \frac{1}{n} \int_0^1 xf(x) \intdd{x^{n}} \\
        &= \frac{f(1)}{n} - \frac{1}{n} \int_0^1 (x^n f(x) + x^{n+1} f'(x)) \intdd{x} \\
        &= \frac{f(1)}{n} - \frac{1}{n^2} \left(\int_0^1 xf(x) \intdd{x^n} + \int_0^1 x^2 f'(x) \intdd{x^n} \right) \\
        &= \frac{f(1)}{n} - \frac{1}{n^2} \left(f(1) - \int_0^1 (x^n f(x) + x^{n+1} f'(x)) \intdd{x} \right) \\
        &\quad\; - \frac{1}{n^2} \left(f'(1) - \int_0^1 (2x^{n+1}f'(x) + x^{n+2} f''(x)) \intdd{x} \right) \\
        &= \frac{f(1)}{n} - \frac{f(1)+f'(1)}{n^2} + \frac{1}{n^2} \left(\int_0^1 (x^n f(x) + 3x^{n+1} f'(x) + x^{n+2} f''(x)) \intdd{x} \right).
    \end{align*}
    $$

    由可积函数必有界，知 $f(x), f'(x), f''(x)$ 在 $[0,1]$ 上均为有界函数，设 $M = \max \left\{\sup\limits_{x\in[0,1]}|f(x)|, \sup\limits_{x\in[0,1]}|f'(x)|, \sup\limits_{x\in[0,1]}|f''(x)| \right\}$，则余项

    $$
    \begin{align*}
        \frac{1}{n^2} \left|\int_0^1 (x^n f(x) + 3x^{n+1} f'(x) + x^{n+2} f''(x)) \intdd{x}\right|
        &\leqslant \frac{M}{n^2} \left |\int_0^1 (x^n + 3x^{n+1} + x^{n+2}) \intdd{x}\right| \\
        &= \frac{M}{n^2} \left(\frac{1}{n+1} + \frac{3}{n+2} + \frac{1}{n+3} \right) \\
        &\in o \left(\frac{1}{n^2}\right).
    \end{align*}
    $$

    故当 $n \to +\infty$ 时，

    $$
        \int_0^1 x^nf(x) \intdd{x} = \frac{f(1)}{n} -\dfrac{f(1) + f'(1)}{n^2}+ o \left(\dfrac{1}{n^2} \right)
    $$

    成立.

!!! example "21-22 期末 T8"
    设 $f(x)$ 三阶可导，$f(0)=f'(0)=f''(0)=0$，$f'''(0)>0$，$x\in(0,1)$ 时 $f(x) \in (0,1)$，且数列 $\{x_n\}$ 满足

    $$
        x_{n+1}=x_n(1-f(x_n)).
    $$
    
    1. 证明：$\lim\limits_{n \to +\infty} x_n = 0$;
    
    2. 证明：存在 $\alpha >0$ 和常数 $c\neq 0$，使 $\lim\limits_{n \to +\infty} cn^{\alpha} x_n = 1$.

??? proof
    见 [21-22 数分期末答案](https://ckc-agc.bowling233.top/analysis/analysis1_paper/21exam_answer.pdf)，此处略.

!!! example
    设 $f(x)$ 是 $\mathbb{R}$ 上具有连续导数的非负函数，且存在 $M>0$，使得 $\forall x,y \in \mathbb{R}$，有 $|f'(x) - f'(y)| \leqslant M|x-y|$.

    证明：$\forall x \in \mathbb{R}$，有
    
    $$
        (f'(x))^2 \leqslant 2M|f(x)|.
    $$

??? proof
    由 $f(x)$ 在 $\mathbb{R}$ 上有连续导函数，$\forall x, h \in \mathbb{R}$，有

    $$
        f(x+h) - f(x) = \int_{x}^{x+h} f'(t) \intdd{t}.
    $$

    为了构造出条件中两导数差的形式，可对右侧积分进行拆解：

    $$
        f(x+h) = f(x) + \int_{x}^{x+h} (f'(t) - f'(x)) \intdd{t} + f'(x)h.
    $$

    而 $f(x+h)\geqslant 0$ 恒成立，将其放掉后移项有

    $$
        -hf'(x) \leqslant f(x) + \int_{x}^{x+h} (f'(t) - f'(x)) \intdd{t}
    $$

    现在希望在两边加绝对值，以用积分绝对值不等式放出 |f'(t) - f'(x)| 的形式，为了保证放缩的方向，我们必须保证 $-hf'(x) \geqslant 0$，由于 $h \in \mathbb{R}$，这是可以做到的. 于是我们有

    $$
    \begin{align*}    
        |-hf'(x)| = -hf'(x) &\leqslant \left|f(x) + \int_{x}^{x+h} (f'(t) - f'(x)) \intdd{t} \right| \\
                            &\leqslant f(x) + \left|\int_{x}^{x+h} (f'(t) - f'(x)) \intdd{t} \right| \\
                            &\leqslant f(x) + M \int_{x}^{x+h} |t - x| \intdd{t} \\
                            &= f(x) + \frac{M}{2}h^2.
    \end{align*}
    $$

    于是

    $$
        |f'(x)| = \frac{f(x)}{|h|} + \frac{M|h|}{2}
    $$

    对所有满足 $-hf'(x) \geqslant 0$ 的 $h$ 成立，故 $|f'(x)|$ 不大于右侧的最小值，即 $2\sqrt{\dfrac{Mf(x)}{2}}$. 两边平方，

    $$
        (f'(x))^2 \leqslant 2Mf(x)
    $$

    成立.

!!! example
    设 $f(x)$ 在 $x=0$ 处存在二阶导数，且 $\forall n \in \mathbb{N}^+, f(2^{-n}) = 0$，证明 $f''(0) = 0$.

??? proof
    $f(x)$ 在 $x=0$ 处二阶可导，说明存在 $\delta > 0$，使得 $f(x)$ 在邻域 $U(0, \delta)$ 内的一阶导数连续，其本身也连续.

    取 $N = \lfloor - \log_2 \delta \rfloor + 1$，并设数列 $x_n = 2^{-(n+N)}$，则由 Rolle 中值定理，$\forall k \in \mathbb{N}^+$，存在 $\xi_k \in (x_k, x_{k+1})$，使得 $f'(\xi_k) = 0$，新构成的数列 $\{\xi_n\}$ 满足 $\xi_n \to 0, \enspace n \to +\infty$.

    由 $f'(x)$ 在 $x=0$ 处连续，知 $\lim\limits_{x \to 0} f'(x) = f'(0)$ 存在，故由 Heine 定理，

    $$
        f'(0) = \lim\limits_{x \to 0} = \lim\limits_{n \to +\infty} f'(\xi_n) = 0.
    $$

    由 $f(x)$ 在 $x=0$ 处二阶可导，$f''(0) = \lim\limits_{x \to 0} \dfrac{f'(x) - f'(0)}{x} = \lim\limits_{x \to 0} \dfrac{f'(x)}{x}$.

    由 $f'(x)$ 在 $(0, \delta)$ 内连续，知 $\dfrac{f'(x)}{x}$ 在 $(0, \delta)$ 内连续，故由 Heine 定理，

    $$
        f''(0) = \lim\limits_{x \to 0} \frac{f'(x)}{x} = \lim\limits_{n \to +\infty} \frac{f'(\xi_n)}{\xi_n} = 0.
    $$

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
