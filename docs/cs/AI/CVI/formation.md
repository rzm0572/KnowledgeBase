# Image Formation

## Linear Algebra

线性变换可以用矩阵表示，例如二维平面上的逆时针旋转可以用一个 $2 \times 2$ 矩阵表示：

$$
    M = \begin{bmatrix} 
        \cos \theta & -\sin \theta \\
        \sin \theta & \cos \theta
    \end{bmatrix}
$$

对一个向量作用线性变换可以左乘对应的矩阵：

$$
    \mathbf{x}' = M \mathbf{x}
$$

平移变换无法用线性变换来表示，我们可以用仿射变换（affine transformation）来表示平移+线性变换：

$$
    \begin{pmatrix} 
        x' \\ y' 
    \end{pmatrix} = \begin{pmatrix} 
        a & b \\ c & d
    \end{pmatrix} \cdot \begin{pmatrix} 
        x \\ y
    \end{pmatrix} + \begin{pmatrix}
        t_x \\ t_y
    \end{pmatrix}
$$

如果要表示成矩阵形式，可以用齐次坐标表示：

$$
    \begin{pmatrix} 
        x' \\ y' \\ 1 
    \end{pmatrix} = \begin{pmatrix} 
        a & b & t_x \\
        c & d & t_y \\
        0 & 0 & 1
    \end{pmatrix} \cdot \begin{pmatrix} 
        x \\ y \\ 1
    \end{pmatrix}
$$

## Camera

### Pinhole camera

!!! note "光屏上形成清晰图像的条件"
    如果光屏上的每一个点都记录了空间中唯一一点发出的光线，则光屏上形成的图像是清晰的。

如果直接将胶片放置在物体前方，则物体上多个点发出的光线可以被胶片上的同一点接收，因而无法形成清晰的图像。为了解决这个问题，我们在胶片前方放置一个带有小孔的屏，使得光屏上的点只能接收到物体上唯一一点发出的光线。

为了让成像更为清晰，通常要减小小孔直径，但是小孔直径太小会导致两个问题：

- 通过小孔的光强过低，导致成像过暗
- 光经过小孔后发生衍射

### Lens

- 透镜成像的基本关系

    $$
        \frac{1}{i} + \frac{1}{o} = \frac{1}{f}
    $$

- 成像的放大系数（Image magnification）

    $$
        m = \dfrac{h_i}{h_o} = \dfrac{i}{o}
    $$

- 图像大小

    调大焦距 $f$，由于物距 $o$ 基本不变，所以像距 $i$ 会变大，故放大系数 $m = \dfrac{i}{o}$ 变大，生成的图像就会变大

- 视场（Field of view, FOV）

    ![](https://s21.ax1x.com/2025/09/22/pV4LO3D.png)

    - 视场与焦距有关，焦距越大，放大系数越大，视场越小
    - 视场还与传感器尺寸有关，传感器尺寸越大，视场越大
    - 现在的相机上标的焦距一般是等效焦距，即对传感器（底片）尺寸进行归一化之后的焦距

- 光圈（Aperture）

    一般使用 F-Number 来表示光圈大小：

    $$
        F = \frac{f}{D}
    $$

    其中 $D$ 是光圈直径。
    
    $F$, $D$ 和图像亮度的关系：$F$ 越大，光圈直径 $D$ 越小，进光量越少，图像亮度越低。

    ![](https://s21.ax1x.com/2025/09/22/pV4LoH1.md.png)

- 景深（Depth of field, DoF）

    在确定了焦距 $f$ 和像距 $i$ 之后，使用高斯成像公式可以计算得到物距 $o$。正好位于 $o$ 处的物体在像屏上成的像是最清晰的，但是我们一般希望可以让一定距离内的物体都能清晰地成像。

    我们先对成像的清晰程度进行定量分析：
    
    !!! definition "模糊圆"
        在焦距 $f$ 和像距 $i$ 确定的情况下，令模糊圆（Blur Circle）表示像距为 $o'$ 时在光屏上成的像，如下图所示：

        ![](https://s21.ax1x.com/2025/09/22/pV4O18U.png)

    根据上图中的几何关系，我们可以得到模糊圆直径 $b$ 的表达式：

    $$
        b = D \cdot \frac{\left| i' - i \right| }{i'}
    $$

    模糊圆直径越小，成像越清晰。如果模糊圆直径小于像屏上一个像素点的大小，那么在该分辨率下，该位置的物体形成的像便是清晰的。

    !!! definition "景深"
        假设我们可以接受的最大模糊圆直径（一般就是像素点的直径）为 $c$，如果在物距 $o \in [o_1, o_2]$ 时，物体在像屏上成像的模糊圆直径 $b \leqslant c$，则 $o_2 - o_1$ 为此时图像的景深。

    ![](https://s21.ax1x.com/2025/09/22/pV4OhPf.png)

    根据上图，我们可以计算得到：

    $$
        \begin{cases}
            c = \frac{f^2 (o - o_1)}{N o_1 (o - f)} \\
            c = \frac{f^2 (o_2 - o)}{N o_2 (o - f)}
        \end{cases}
    $$

    联立可得 $o_2 - o_1 = \dfrac{2 o f^2 c N (o - f)}{f^4 - c^2 N^2 (o-f)^2}$.


## Geometric image formation

### Perspective projection

成像是一种三维空间到二维空间的投影，我们称这种投影为透视投影（Perspective Projection），现在我们希望能找到这个投影映射的数学表达。

![](https://s21.ax1x.com/2025/09/22/pV4XldI.png)

如上图，由于物距通常较大，所以我们令 $i \approx f$，此时像屏上成像点的坐标 $p$ 满足

$$
    p = \begin{bmatrix} 
        u \\ v 
    \end{bmatrix} = \begin{bmatrix} 
        \dfrac{f x}{z} \\ \dfrac{f y}{z} 
    \end{bmatrix}.
$$

我们可以将其写成一个映射，称为透视投影：

$$
\begin{aligned}
    g: \mathbb{R}^3 &\mapsto \mathbb{R}^2 \\
    \begin{bmatrix} 
        x \\ y \\ z 
    \end{bmatrix} &\to \begin{bmatrix} 
        \dfrac{f x}{z} \\ \dfrac{f y}{z}
    \end{bmatrix} 
\end{aligned}
$$

可惜的是，透视投影不是一个线性映射，但是我们可以对其进行齐次化：

$$
    \begin{bmatrix} 
        f & 0 & 0 & 0 \\
        0 & f & 0 & 0 \\
        0 & 0 & 1 & 0 
    \end{bmatrix} \begin{bmatrix} 
        x \\ y \\ z \\ 1 
    \end{bmatrix}  = \begin{bmatrix} 
        fx \\ fy \\ z 
    \end{bmatrix}  \cong \begin{bmatrix} 
        \dfrac{fx}{z}  \\ \dfrac{fy}{z} \\ 1
    \end{bmatrix} 
$$

其中，三维坐标系原点位于相机的位置，$z$ 轴为相机的主光轴，$x$ 轴和 $y$ 分别指向水平方向与竖直方向，均垂直于 $z$ 轴；二维坐标系原点位于相机主光轴与光屏的交点处，水平方向为 $u$ 轴，竖直方向为 $v$ 轴。

上述公式表明透视投影在齐次坐标系下可写为线性映射。

- 透视投影是不可逆的，三维空间的深度信息会丢失

- 到达空间中点的位置实际上反映了三维空间中的光线方向


!!! definition "Vanishing Point"
    三维空间中的两条无限延伸的平行线在经过透视投影之后会交于一点，我们称这个点为消失点（Vanishing Point）：

    ![](https://s21.ax1x.com/2025/10/10/pVHg1te.png)

!!! property "消失点的性质"
    - 任意两条相互平行的线的消失点相同
    - 从相机到消失点的连线与消失点对应的平行线平行

!!! definition "Vanishing Line"
    同一平面 $\alpha$ 内的所有直线对应的消失点在同一条直线上，该直线被称为消失线（Vanishing Line）

!!! property "消失线的性质"    
    - 消失线对应了三维空间中平面的朝向
    - 消失线与面 $\alpha$ 平行
    - 消失线与相机构成的平面与面 $\alpha$ 平行

### Distortion

#### Perspective distortion

透视畸变（Perspective Distortion）是指透视投影的结果与实际物体的形状不一致，这种现象不是由镜头本身的瑕疵造成的。

越靠近图像的边缘，透视畸变的影响越明显。

使用轴移相机（View camera）可以通过改变透镜与光屏的相对位置来消除透视畸变的影响，故其经常被用于拍摄建筑等需要保持物体形状的场景。

#### Radial distortion

径向畸变（Radial Distortion）是由镜头本身瑕疵造成的图像畸变。一种典型的径向畸变的效果是三维空间中的直线在投影后不再是直线。

径向畸变的变换公式：

$$
\begin{aligned}
    r^2 &= {x'_n}^2 + {y'_n}^2 \\
    x'_d &= x'_n (1 + \kappa_1 r^2 + \kappa_2 r^4) \\
    y'_d &= y'_n (1 + \kappa_1 r^2 + \kappa_2 r^4)
\end{aligned}
$$

使用广角镜头会产生桶形畸变（Barrel Distortion），使用长焦镜头会产生针形畸变（Pincushion Distortion）。

![](https://s21.ax1x.com/2025/10/10/pVHg3fH.png)

### Orthographic projection

正交投影（Orthographic Projection）是指将三维空间中的点投影到一个平面上，使得投影点与点本身的连线与该平面垂直。

当相机与投影平面的距离无限远时，透视投影便转化为正交投影，在实际情况下一般无法实现。

我们也可以使用齐次坐标表示正交投影：

$$
    \begin{bmatrix} 
        1 & 0 & 0 & 0 \\
        0 & 1 & 0 & 0 \\
        0 & 0 & 0 & 1 
    \end{bmatrix}  \begin{bmatrix} 
        x \\ y \\ z \\ 1 
    \end{bmatrix}  = \begin{bmatrix} 
        x \\ y \\ 1
    \end{bmatrix} 
$$


## Photometric image formation

### 光强

曝光由快门（Shutter）控制，快门时间控制了曝光时间。

在曝光过程中，传感器会持续记录照射到某个像素点的光强，并对其进行积分，最后得到该点的像素值。

曝光时间过短会导致图像欠曝，曝光时间过长会导致图像过曝或图像模糊。

除了曝光时间外，光圈大小也可以控制照射到传感器上的光强，故一般较短的曝光时间会配合较大的光圈，以获得较好的图像质量。

快门类型可分为滚动快门（Rolling Shutter）和全局快门（Global Shutter）。

- 滚动快门：一次读取一行的传感器电压值
- 全局快门：一次读取所有传感器的电压值

Rolling shutter effect：滚动快门拍摄的图像中，不同行的像素点记录的其实是不同时间的光强，因此拍摄视频时如果被拍摄物体快速转动，会出现物体图像变形的现象。

### 颜色

#### 颜色的表示

颜色通常使用颜色空间中的坐标来表示，例如 RGB 颜色空间中，我们可以使用三元组 $(R, G, B)$ 来表示颜色，其中 $R, G, B \in [0, 1]$。

除了 RGB 颜色空间外，HSV 颜色空间也常用于颜色表示。

- Hue：色调，取值范围为 $[0, 360)$，从红色开始按逆时针方向排列，红色为 0，绿色为 120，蓝色为 240，黄色为 60
- Saturation：饱和度，取值范围为 $[0, 1]$，饱和度越高，颜色越鲜艳
- Value：明度，取值范围为 $[0, 1]$，明度越高，颜色越亮

![](https://s21.ax1x.com/2025/10/10/pVHg2n0.png)

#### 颜色的记录

在现代相机中，通常使用贝尔滤波器（Bayer Filter）来记录颜色。

![](https://s21.ax1x.com/2025/10/10/pVHgRBV.png)

滤波器实际上类似滤镜，可以过滤某一种波长的光。在上面这幅图中，我们使用四个像素来记录一个位置的颜色，包括 1 个红色像素，2 个绿色像素，1 个蓝色像素。使用 2 个绿色像素是因为人眼对绿色的敏感度更高。

为了不浪费像素点，我们也可以使用线性插值的方法，使用临近像素点测量的某种颜色的值的线性组合来表示该点的该颜色的值。

#### 着色

着色（Shading）操作用于计算在物体表面反射的光。

物体表面对光的反射特性被称为物体的材质（Material），使用**双向反射分布函数（Bidirectional Reflectance Distribution Function, BRDF）**来描述。

$$
    f_r(\mathbf{\hat{v}}_i, \mathbf{\hat{v}}_r, \mathbf{\hat{n}}; \lambda)
$$

输入为入射光方向 $\mathbf{\hat{v}}_i$，反射光方向 $\mathbf{\hat{v}}_r$，物体表面的法线方向 $\mathbf{\hat{n}}$，光的波长 $\lambda$，输出为反射光的强度。
