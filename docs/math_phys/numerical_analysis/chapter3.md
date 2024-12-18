---
tags:
  - Theory
---

# Interpolation and Polynomial Approximation

## Polynomial Interpolation

!!! Definition "Definition :: Interpolation"
    给定一个函数 $f(x)$ 在多个不同的点 $x_i$ 处的函数值 $f(x_i)$，插值是指构造一个近似函数 $g(x)$，使得 $g(x_i) = f(x_i)$，并且 $g(x)$ 可以较好地逼近函数 $f(x)$.

多项式插值的理论基础：Weierstrass Approximation Theorem

即任意连续函数都可以使用一个多项式来近似.

## Lagrange Polynomials

一般来说，内插的误差比外插的误差小.
