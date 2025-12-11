# Image Matching & Motion Estimation

图像匹配的任务有两个：

1. 判定两张图片的相似度
2. 找到两张图片的对应点

应用：图像拼接、对象识别、三维重建等

## Image Matching

!!! note
    图像匹配的三个步骤：

    - Detection: 找到图像中的特征点
    - Description: 对特征点的邻域进行编码，得到 descriptors
    - Matching: 匹配两张图像中的 descriptors

### Detection

我们希望可以从图像中选择一些特征点（Feature Points / Interest Points），这些特征点需要具有唯一性。

### Corner Detection

“唯一性” 指的是与周边的点不一致，我们可以用梯度来描述这种不一致性。具体地，假设当前处理的点为 $P$，我们计算其邻域 $U(P)$ 中各个点的梯度

$$
    \nabla f(x,y) = \frac{\partial f}{\partial x} \hat{i} + \frac{\partial f}{\partial y} \hat{j} = I_x(x,y) \hat{i} + I_y(x,y) \hat{j}
$$

然后我们求 $I_x, I_y$ 的协方差矩阵 $\operatorname{Cov}(I_x, I_y)$，并找到它的两个特征值和对应的特征向量，这个方法被称为主成分分析（Principal Component Analysis，PCA）。实际上，我们用的协方差矩阵是

$$
    H = \sum w(u, v) \begin{bmatrix} 
        I_x^2 & I_x I_y \\
        I_x I_y & I_y^2 
    \end{bmatrix} 
$$

其中 $w(u, v)$ 是权重函数，可以是高斯函数。

根据特征值的大小，我们可以判断 $P$ 的类型：

- 如果 $\lambda_1$ 和 $\lambda_2$ 都很小，说明 $P$ 位于区域内，因而不是特征点
- 如果 $\lambda_1 \gg \lambda_2$ 或 $\lambda_1 \ll \lambda_2$，说明 $P$ 位于边缘
- 如果 $\lambda_1$ 和 $\lambda_2$ 都很大，说明 $P$ 是角点（Corner Point），即图像的重要特征点

我们可以用 Harris detector 来检测角点：

$$
    f = \frac{\lambda_1 \lambda_2}{\lambda_1 + \lambda_2} = \frac{\det(H)}{\operatorname{Tr}(H)}
$$

$|f|$ 接近 0 说明 $P$ 不是角点，否则说明 $P$ 是角点，因而 $f$ 也被称为角点响应器（corner response）。

综上，检测特征点的过程可以分为以下步骤：

1. 对于图像中的每个点，计算该点邻域内的梯度分布 $\left\{\nabla f(x,y) \right\}_{(x,y) \in U(P)}$
2. 计算梯度分布的协方差矩阵 $H$
3. 计算 Corner Response $f$
4. 采用极大值抑制（Nonmaximum Suppression）的策略，获取区域中 $f$ 的局部最大值对应的像素点作为特征点


Harris detector 的缺点是可重复性较低。可重复性（Repeatability）指的是检测器在相对应的像素点上的取值与图像的变换（Transformation）无关。

图像的变换包括拍摄条件的变换（如曝光强度）、几何变换（如平移、旋转、缩放）等。

- 亮度变换：假设对图像的整体亮度进行线性变换 $I \to aI + b$，则 $b$ 不会影响 $f$ 的值（因为对梯度没有影响），而 $a$ 会影响 $f$ 的值。
- 平移变换：平移不会改变对应点的梯度分布，因而不会影响 $f$ 的值。
- 旋转变换：平面内的旋转不会改变协方差矩阵的特征值，只会改变特征向量的方向，因而不会影响 $f$ 的值。
- 缩放变换：相当于改变了邻域的大小，图像的整体和局部性质不一定相同，因而会改变 $f$ 的值。

我们需要解决缩放变换引起的低可重复性问题，这实际上只需要寻找合适的邻域大小即可。具体地，对于某个点 $P$，我们对不同大小的邻域 $U_r(P)$ 计算 $f_r$，选择 $r = \underset{r}{\operatorname{argmax}} f_r$ 作为角点的尺度。（Image Pyramids）

### Blob Detection

我们也可以通过计算二阶导的方式来寻找特征点。对于图像中与周围像素点差异较大的点，其通常是一个局部最值点，我们可以通过拉普拉斯算子来描述图像的局部二阶导的大小，从而发现局部最值点。

由于拉普拉斯算子对噪声敏感，我们一般会选择对原始图像进行高斯滤波后再求二阶导，而

$$
    \nabla^2 (f * g) = f * \nabla^2 g
$$

故我们可以直接使用 $\nabla^2 \operatorname{Gaussian}$（LoG）作为卷积核，与原始图像进行卷积。LoG 的大小同样可以用图像金字塔来解决。

LoG 的简化实现为 DoG，即我们可以用两个高斯核的差去近似替代 LoG，i.e. 

$$
    \nabla^2 G_{\sigma} \approx G_{\sigma_1} - G_{\sigma_2}
$$


## Description

我们需要通过一个被称为 descriptor 的特征向量来描述特征点的邻域，即一个图像块（Raw Patch）的信息。
