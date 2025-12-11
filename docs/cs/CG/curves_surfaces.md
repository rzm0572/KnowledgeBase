# Curves and Surfaces

## Curves

### 曲线的表示

!!! note "曲线表示方法"
    - 显式曲线：$y = f(x)$
    - 隐式曲线：$g(x, y) = 0$
    - 参数曲线：$\begin{cases}x = f(t) \\ y = g(t)\end{cases}$

隐式曲线和参数曲线可以表示的曲线比显式曲线更多，所以我们一般使用前两者。

#### 隐式曲线

隐式曲线的优点：判断一个点是否在曲线上很方便，只需要将点坐标代入即可。

隐式曲线的缺点：遍历曲线上所有的点是困难的。

然而，遍历曲线上的点是在屏幕上显示曲线的基本要求。由于像素空间是离散的，我们可以使用 Subdivision 的方法来绘制隐式曲线，即把屏幕不断划分为更小的矩形，只处理被曲线穿过的矩形。

#### 参数曲线

在图形学中，我们通常使用以下形式的参数曲线：

$$
    C(u) = \begin{bmatrix} 
        x(u) & y(u) & z(u) 
    \end{bmatrix}.
$$

参数曲线的长度：

$$
    s = \int_{a}^{b}  \, \mathrm{d}s = \int_{a}^{b} \sqrt{(x'(u))^2 + (y'(u))^2 + (z'(u))^2} \, \mathrm{d}u
$$

### 曲线的生成

计算机只能处理离散的数值，如何通过离散的点生成曲线也是值得研究的问题。

#### 插值

如果我们希望画一条曲线经过所有给定的点，我们可以采用插值的方法：

- 最临近插值（nearest neighbor interpolation）：选择离当前点最近的插值点的值作为当前点的值，会导致曲线不连续
- 线性插值（linear interpolation）：用直线连接相邻的插值点，会导致曲线在部分点处不可导（不光滑）
- 三次赫米特插值（Cubic Hermite Interpolation）：用三次多项式连接相邻的插值点，同时保持曲线的连续性和光滑性

    给定两个相邻的插值点 $(0, h_0)$ 和 $(1, h_1)$，以及斜率 $h_2$ 和 $h_3$，我们需要找到一个函数 $P(t)$，满足：

    $$
        P(0) = y_0, \quad P(1) = y_1, \quad P'(0) = k_0, \quad P'(1) = k_1
    $$

    设 $P(t) = at^3 + bt^2 + ct + d$，则 $P'(t) = 3at^2 + 2bt + c$，代入条件有：

    $$
    \begin{cases}
        P(0) = d = h_0, \\
        P(1) = a + b + c + d = h_1, \\
        P'(0) = c = h_2, \\
        P'(1) = 3a + 2b + c = h_3
    \end{cases}
    $$

    可以得到

    $$
    \begin{bmatrix} 
        h_0 \\ h_1 \\ h_2 \\ h_3 
    \end{bmatrix} = \begin{bmatrix} 
        0 & 0 & 0 & 1 \\
        1 & 1 & 1 & 1 \\
        0 & 0 & 1 & 0 \\
        3 & 2 & 1 & 0 
    \end{bmatrix} \begin{bmatrix} 
        a \\ b \\ c \\ d 
    \end{bmatrix}
    $$

    求解逆矩阵，并通过变换可以得到

    $$
    P(t) = \begin{bmatrix} 
        a & b & c & d 
    \end{bmatrix} \begin{bmatrix} 
         t^3 \\ t^2 \\ t \\ 1 
    \end{bmatrix} = \begin{bmatrix} 
        h_0 & h_1 & h_2 & h_3 
    \end{bmatrix} \begin{bmatrix} 
        2t^3 - 3t^2 + 1 \\
        -2t^3 + 3t^2 \\
        t^3 - 2t^2 + t \\
        t^3 - t^2
    \end{bmatrix} 
    $$

    因此我们可以构造插值函数 $P(t) = \displaystyle \sum_{i=0}^3 h_i H_i(t)$，其中

    $$
    \begin{cases}
        H_0(t) = 2t^3 - 3t^2 + 1, \\
        H_1(t) = -2t^3 + 3t^2, \\
        H_2(t) = t^3 - 2t^2 + t, \\
        H_3(t) = t^3 - t^2
    \end{cases}
    $$

- Catmull-Rom 插值：若无法得到曲线在插值点处的斜率，我们可以用当前插值点左右两个插值点的连线斜率代替该点处的切线斜率。

#### Bezier curve

!!! definition "Bezier curve"
    $$
        \mathbf{C}(t) = \sum_{i=0}^n B_{i,n}(t) \mathbf{P}_i, \enspace t \in [0, 1]
    $$

    其中 $B_{i,n}(t)$ 为 Bernstein 基函数，$\mathbf{P}_i (i=0,1,\ldots,n)$ 是控制点。

!!! definition "Bernstein 基函数"
    当 $t \in (0, 1)$ 时，

    $$
        B_{i,n}(t) = \binom{n}{i} t^i (1-t)^{n-i}
    $$

    当 $t = 0$ 时，$B_{i,n}(0) = \begin{cases} 1, & i = 0 \\ 0, & i \neq 0 \end{cases}$

    当 $t = 1$ 时，$B_{i,n}(1) = \begin{cases} 1, & i = n \\ 0, & i \neq n \end{cases}$

!!! property "Bezier 曲线的性质"
    - 端点插值：经过控制点 $P_0$ 和 $P_n$
    - 端点切线斜率：$P_0$ 处的切线经过 $P_1$，$P_n$ 处的切线经过 $P_{n-1}$

        $$
        C'(t) = n \sum_{i=0}^{n-1} (\mathbf{P}_{i+1} - \mathbf{P}_i) B_{i, n-1} (t), \enspace t \in [0, 1].
        $$

    - 对称性：由控制点序列 $\{P_0, P_1, \ldots, P_n\}$ 生成的 Bezier 曲线与由控制点序列 $\{P_n, P_{n-1}, \ldots, P_0\}$ 生成的 Bezier 曲线相同

    - 仿射不变性: 贝塞尔曲线经过仿射变换后的结果与其控制点经过放射变换后生成的贝塞尔曲线相同
    - 凸包：贝塞尔曲线位于控制点 $P_0, P_1, \ldots, P_n$ 的凸包内

Bezier 曲线的特例：

- 一次 Bezier 曲线：$\mathbf{C}(t) = (1 - t)\mathbf{P}_0 + t\mathbf{P}_1$
- 二次 Bezier 曲线：$\mathbf{C}(t) = (1 - t)^2\mathbf{P}_0 + 2(1 - t)t\mathbf{P}_1 + t^2\mathbf{P}_2$
- 三次 Bezier 曲线：$\mathbf{C}(t) = (1 - t)^3\mathbf{P}_0 + 3(1 - t)^2t\mathbf{P}_1 + 3(1 - t)t^2\mathbf{P}_2 + t^3\mathbf{P}_3$


#### Rational Bezier curve

Bezier 曲线无法表示圆，但如果我们对 Bernstein 函数进行加权，生成的 Rational Bezier 曲线就可以表示圆。

$$
    \mathbf{R}(t) = \frac{\displaystyle \sum_{i=0}^n B_{i,n}(t) \omega_i \mathbf{P}_i}{\displaystyle \sum_{i=0}^n B_{i,n}(t) \omega_i}
$$

!!! property "Rational Bezier 曲线的性质"
    - 端点插值：$\mathbf{R}(0) = \mathbf{P}_0, \enspace \mathbf{R}(1) = \mathbf{P}_n$.
    - 端点切线斜率：$\mathbf{R}(t)$ 在 $\mathbf{P}_0$ 处切线经过 $\mathbf{P}_1$，$\mathbf{P}_n$ 处切线经过 $\mathbf{P}_{n-1}$

        $$
            \mathbf{R}'(0) = n \frac{\omega_1}{\omega_0} (\mathbf{P}_1 - \mathbf{P}_0), \enspace
            \mathbf{R}'(1) = n \frac{\omega_{n-1}}{\omega_n} (\mathbf{P}_n - \mathbf{P}_{n-1})
        $$

    - Bezier 曲线满足的性质仍然成立

Bezier 曲线的缺点：

- Bezier 曲线控制点个数与曲线阶数耦合
- 所有控制点都有全局影响

#### B-spline

为了解决 Bezier 曲线控制点个数与曲线阶数耦合的问题，我们采用 B-spline 基函数替换 Bernstein 基函数，得到 B-spline 曲线。

B-spline 曲线同样由控制点生成。我们先取 $n+1$ 个控制点 $\mathbf{P}_0, \mathbf{P}_1, \ldots, \mathbf{P}_n$，则 B-spline 曲线可被定义为：

$$
    \mathbf{C}(u) = \sum_{i=0}^n \mathbf{P}_i N_{i,p} (u), \enspace u \in [a, b]
$$

其中 $N_{i,p}(u)$ 为第 $i$ 个 $p$ 次 B-spline 基函数，$a$ 和 $b$ 为曲线的区间端点。

B-spline 基函数基于节点向量 $\{u_0 = a, u_1, \ldots, u_{n+k+1} = b\}$ 定义，其中 $k = p + 1$ 为需要的最高阶数。其递归定义如下：

$$
\begin{align*}
    N_{i,0}(u) &= \begin{cases}
        1, & u_i \in [u_i, u_{i+1}) \\
        0, & \text{otherwise}
    \end{cases} \\
    N_{i,p}(u) &= \frac{u - u_i}{u_{i+p} - u_i} N_{i, p-1}(u) + \frac{u_{i+p+1} - u}{u_{i+p+1} - u_{i+1}} N_{i+1, p-1}(u)
\end{align*}
$$

$N_{i,p}(u)$ 相当于 $N_{i, p-1}(u)$ 和 $N_{i+1, p-1}(u)$ 对 $u$ 的线性插值。

!!! property "B-spline 曲线的性质"
    - 局部性：$N_{i,p}(u)$ 只在 $u \in [u_i, u_{i+p+1})$ 内非 0，意味着 $P_i$ 只会影响 $\mathbf{C}(u)$ 在 $u \in [u_i, u_{i+p+1})$ 内的形状。换句话说，曲线上的任意一个点只受到 $k$ 个控制点的影响。
    - $N_{i,p}(u)$ 在 $u \in [u_i, u_{i + p + 1}]$ 内为次数不超过 $p$ 次的多项式
    - $N'_{i,p}(u) = \dfrac{p}{u_{i+p} - u_i} N_{i, p-1}(u) + \dfrac{p}{u_{i+p+1} - u_{i+1}} N_{i+1, p-1}(u)$.
    - 凸包性、放射不变性等 Bezier 曲线的性质仍然成立


#### Non-Uniform Rational B-Splines (NURBS)

B-Spline 曲线的 rational 版本，对 B-Spline 基函数进行加权：

$$
    \mathbf{C}(u) = \frac{\displaystyle \sum_{i=0}^n N_{i,p}(u) \omega_i \mathbf{P}_i}{\displaystyle \sum_{i=0}^n N_{i,p}(u) \omega_i}.
$$

其中 $\omega_i$ 为控制点权重。


## Surfaces

图形学中，曲面通常采用参数曲面的形式来表示，即：

$$
\begin{cases}
    x = x(u, v), \\
    y = y(u, v), \\
    z = z(u, v)
\end{cases}
$$

曲面的法向量求法：

$$
    N(u, v) = \frac{\partial \mathbf{S}(u,v)}{\partial u} \times \frac{\partial \mathbf{S}(u,v)}{\partial v}.
$$

一般来说，曲面有以下几种绘制方法：

### 控制点生成

我们可以用扩展 Bezier 曲线的定义，用 Bernstein 基函数对 $n \times m$ 个控制点进行插值，得到曲面：

$$
    S(u, v) = \sum_{i=0}^n \sum_{j=0}^m \mathbf{P}_{ij} B_{i,n}(u) B_{j,m}(v).
$$

相当于先对每个 $u$ 构建关于 $v$ 的 Bezier 曲线，再对每个 $v$ 把 Bezier 曲线上的点作为控制点构造关于 $u$ 的 Bezier 曲线，得到曲面。

与之相似的，使用 B-Spline 基函数对 $n \times m$ 个控制点进行插值，可以得到 B-Spline 曲面；使用加权的 B-Spline 基函数进行插值，可以得到 NURBS 曲面。

$$
\begin{align*}
    S_{\text{B-Spline}}(u, v) &= \sum_{i=0}^n \sum_{j=0}^m \mathbf{P}_{ij} N_{i,p}(u) N_{j,q}(v), \\
    S_{\text{NURBS}}(u,v) &= \frac{\displaystyle \sum_{i=0}^n \sum_{j=0}^m N_{i,p}(u) N_{j,q}(v) \omega_{ij} \mathbf{P}_{ij}}{\displaystyle \sum_{i=0}^n \sum_{j=0}^m N_{i,p}(u) N_{j,q}(v) \omega_{ij}}.
\end{align*}
$$


### 多边形网格

我们可以通过一系列曲面上的点连接而成的多边形网格来近似表示曲面，由于在很多情况下我们并不需要非常精确的曲面表示，因此使用多边形网格来近似表示曲面是很常用的方法。大多数情况下，我们会使用三角形网格，因为空间内的任意不共线三点都可以唯一确定一个三角形，方便绘制。

要表示一个多边形网格，我们需要指定顶点信息（Vertex information）和拓扑信息（Topological information），顶点信息包括每个顶点的位置，拓扑信息包括顶点之间的连接关系。

描述多边形网格的文件格式有很多种，常用的有 .obj 等。.obj 文件的格式如下：

```
# List of Vertices, with (x, y, z [, w]) coordinates, w is optional and defaults to 1.0
v 0.1 0.2 0.3 1.0
v 0.4 0.5 0.6 1.0
...

# Texture coordinates, in (u, v [, w]) coordinates, these will vary between 0 and 1, w is optional and defaults to 0.0
vt 0.500 1 [0]
...

# Normals in (x, y, z) form; normals might not be unit
vn 0.707 0.000 0.707
...

# Parameter space vertices in (u [, v] [, w]) form
vp 0.310000 3.210000 2.100000
...

# Face Definitions (vertex[/vertex_texture[/vertex_normal]])
f v1 v2 v3
f v1/vt1 v2/vt2 v3/vt3
f v1/vt1/vn1 v2/vt2/vn2 v3/vt3/vn3
f v1//vn1 v2//vn2 v3//vn3
```

### 其他生成方式

- L-system

    L-system 最初用于描述植物的生长过程，后来被引入到计算机图形学中，用于生成复杂的曲线和曲面。L-system 定义了一种语法，用字符表示生成规则，我们就可以用一个字符串来表示复杂的曲线或曲面。因此，L-system 适合用来表示在规则限制下自由生成的复杂结构，例如树和蘑菇等。

- Subdivision surfaces

    我们从一组顶点构成的图形开始，通过预先规定的规则生成新顶点替代旧顶点，然后按照规则将新顶点与其他顶点连接，构成新的图形。重复上述过程直到收敛到所需的精度。

    例如，对一个 $n$ 边形，我们取各边的三等分点，得到 $2n$ 个新顶点，然后逐个连接这 $2n$ 个顶点，便能生成一个 $2n$ 边形。从一个正三角形开始，按照上述规则重复操作，便可以得到一个圆。

- Sweeping

    我们可以在一条二维曲线 $\Gamma$ 上取一个点，将这个点沿着另一条曲线移动，移动过程中 $\Gamma$ 扫过的区域便构成了一张曲面。这种方法适合生成管道等截面具有规则形状的物体。
