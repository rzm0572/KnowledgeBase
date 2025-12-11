# Viewing

## 2D Viewing Transformations

View 和 Window 是不同的：

- 坐标系位置和方向不同
- Window 使用的是连续坐标系，View 使用的是离散坐标系（像素坐标）

2D Viewing Transformations 的步骤：

- 平移图像，使 Screen 的中心点与原点重合
- 缩放图像

$$
\begin{aligned}
    \frac{\text{screenSize}}{2} + (x - x_C) \cdot \text{scaleFactor} \\
    \frac{\text{screenSize}}{2} - (y - y_C) \cdot \text{scaleFactor}
\end{aligned}
$$

$y$ 轴加符号的原因是 Screen 的左上角坐标为 $(0, 0)$，并且 Screen 的 $y$ 轴朝下。

如果屏幕的长宽比和图像的长宽比不同，则 $\text{scaleFactor}$ 有多种选择，如 $\dfrac{\text{screenWidth}}{\text{imageWidth}}$ 或 $\dfrac{\text{screenHeight}}{\text{imageHeight}}$。


## 3D Viewing

3D Viewing 类似于在三维空间中摆放一个虚拟的相机，然后使用该相机进行拍照，相当于 $\mathbb{R}^3$ 到 $\mathbb{R}^2$ 的投影变换。

图形学中的虚拟相机都可以被认为是小孔相机，不需要对焦。

投影可分为透视投影（Perspective Projection）和平行投影（Parallel Projection）。正交投影有几种特殊类型：

- 正交投影（Orthographic Projection）：投影方向与坐标轴平行。

- 斜平行投影（Oblique Projection）：与投影平面平行的图形形状不变（保角），大小会发生变化（不保距）；与投影平面不平行的图形形状会发生变化。

### View Specification

为了确定一个相机拍出来的图像，我们需要指定一些参数：

- view reference point ($\mathrm{vrp}$)
- view direction (-$\vec{n}$)
- up direction for viewing ($\mathrm{upVector}$)

这些参数决定了一个视图参考坐标系（也称相机坐标系）。

up vector 决定了图像中“上方”（$v$ 轴正方向）在世界坐标系中的方向。为了方便程序员的使用，up vector 不需要与 view direction 垂直。

坐标系变换：

- 将相机坐标系的原点平移到世界坐标系的原点

- 按照以下规则进行坐标系的旋转：

    $$
    \begin{aligned}
        \vec{n} &= -\mathrm{viewDirection} \\
        \vec{u} &= \mathrm{upVector} \times \vec{n} \\
        \vec{v} &= \vec{n} \times \vec{u}
    \end{aligned}
    $$

View volumn：透视投影中可以被映射到图像中的三维空间的范围，一般可以视为一个棱台。

View volumn 的作用是 clipping，与 view volume 完全没有交集的物体不需要处理。
