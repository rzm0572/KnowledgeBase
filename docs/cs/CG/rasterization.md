# Rasterization

光栅化（Rasterization）是将矢量图形格式描述的图像转化为光栅图像（一系列像素点）的过程，用于在显示器或打印机中输出图像，其本质上就是图像的离散化。

Rasterization 的名字来源于老式电子显示屏中，为了过滤掉电子枪打出的电子束中位置有部分偏差的电子，而在荧光屏后放置的一系列金属光栅版，这些光栅版使得电子只能打到显示屏上某些固定的位置（像素点）。

图像的光栅化操作由 Raster Graphics Packages 支撑，这些包中含有一些将基本图形高效地光栅化的算法。

## 线段的光栅化

!!! info "线段的光栅化"
    给定线段的两个端点在坐标网格中的坐标 $P_1=(x^{(1)},y^{(1)}), \enspace P_2=(x^{(2)},y_2^{(2)})$，我们需要在坐标网格中选取一系列坐标点，使得他们尽可能接近理想线段，并且画出来的线段尽可能直。

进阶的要求：

- 颜色和亮度不随线段长度和方向变化
- 绘制过程尽可能高效
- 线段的宽度和线型可调

!!! note "Digital Differential Analyzer (DDA)"
    DDA 算法的基本思想是：从线段的起点 $P_1$ 开始，沿着线段的方向以固定的步长 $\Delta x$ 或 $\Delta y$ 递增，计算出每一步对应的另一个坐标值，然后将计算出的坐标值四舍五入到最近的整数坐标点上。

    设线段的斜率为 $m=\frac{y^{(2)}-y^{(1)}}{x^{(2)}-x^{(1)}}$，并设 $(x_k, y_k)$ 为第 $k$ 次迭代的精确坐标值。
    
    如果 $|m| \leq 1$，则以 $\Delta x=1$ 为步长递增 $x$ 坐标，并计算对应的 $y$ 坐标：

    $$
        \begin{cases}
            x_{k+1} = x_k + 1 \\
            y_{k+1} = y_k + m
        \end{cases}
    $$

    如果 $|m| > 1$，则以 $\Delta y=1$ 为步长递增 $y$ 坐标，并计算对应的 $x$ 坐标：

    $$
        \begin{cases}
            x_{k+1} = x_k + \dfrac{1}{m} \\[0.5em]
            y_{k+1} = y_k + 1
        \end{cases}
    $$

    每次计算出的坐标值需要四舍五入到最近的整数坐标点上。具体地，如果 $|m| \leq 1$，则选取的像素坐标为 $(x_k, \operatorname{round}(y_k))$；如果 $|m| > 1$，则选取的像素坐标为 $(\operatorname{round}(x_k), y_k)$。

    DDA 算法简单易实现，但是由于每次都需要进行浮点数运算和四舍五入操作，效率较低。

!!! note "Bresenham's Line Algorithm"
    利用 $P_1$ 和 $P_2$ 均为整点的性质，我们可以省去 DDA 算法中所有的浮点运算，得到 Bresenham 线段算法。

    设 $\Delta x = x^{(2)} - x^{(1)}$，$\Delta y = y^{(2)} - y^{(1)}$，线段的斜率 $m = \dfrac{\Delta y}{\Delta x}$。设 $(x_k, y_k)$ 为第 $k$ 次迭代的精确坐标值。
    
    不妨设 $0 \leqslant m \leqslant 1$，设 $\hat{y_k} = \operatorname{round}(y_k)$，由于 $0 \leqslant m \leqslant 1$，因此 $\hat{y_{k+1}} -\hat{y_k} \in \left\{ 0, 1 \right\}$。

    我们设 $d_k^{(1)} = y_k - \hat{y_{k-1}}, \enspace d_k^{(2)} = \hat{y_{k-1}} + 1 - y_k$，令

    $$
        d_{k+1}^{(1)} - d_{k+1}^{(2)} = 2m(x_k + 1) + 2b - 2 \hat{y_k} - 1 > 0
    $$

    两边同乘 $\Delta x$，则

    $$
        2x_k \Delta y - 2 \hat{y_k} \Delta x + 2 \Delta y + (2b - 1) \Delta x > 0
    $$

    由此我们可以设判别式

    $$
        P_{k+1} = 2x_k \Delta y - 2 \hat{y_k} \Delta x + 2 \Delta y + (2b - 1) \Delta x
    $$

    上式还可以进一步化为

    $$
        P_{k+1} = P_k + 2 \Delta y - 2(\hat{y_k} - \hat{y_{k-1}}) \Delta x
    $$

    当 $P_k > 0$ 时，$d_k^{(1)} > d_k^{(2)}$，故有 $\hat{y_k} = \hat{y_{k-1}} + 1$；当 $P_k < 0$ 时，$d_k^{(1)} < d_k^{(2)}$，故有 $\hat{y_k} = \hat{y_{k-1}}$。

## 圆的光栅化


- 极坐标变换

- Bresenham 算法

## 多边形光栅化

!!! note "even-odd test"

    对于待测试的像素点，以其为端点向外任意作一条射线，
    
    - 若射线与多边形的边的交点个数是奇数，则该像素点在多边形内
    - 若射线与多边形的边的交点个数是偶数，则该像素点在多边形外

!!! note "winding number test"

    假设多边形为 $P_1 P_2 \cdots P_n$，待测试的像素点为 $Q$，我们依次连接 $QP_i$，并设有向角度 $\theta_i = \angle P_i Q P_{i+1}$。令

    $$
        A = \sum_{i=0}^{n-1} \theta_i
    $$

    - 若 $A = 0$，则 $Q$ 在 $P_1 P_2 \cdots P_n$ 的外部
    - 若 $A = 2\pi$，则 $Q$ 在 $P_1 P_2 \cdots P_n$ 的内部

- scan-line method

- seed fill algorithm

- clipping

