# Mathematical Preliminaries

!!! note "Assumption"
    - When discussing numerical analysis, the functions we consider are continuous.

## Roundoff Errors and Computer Arithmetic

### The source of errors

In numerical analysis, there're two types of errors:

- **Truncation errors | 截断误差**: the error involved in using a truncated, or finite, summation to approximate the sum of an infinite series.

- **Roundoff errors | 舍入误差**: the error produced when performing real number calculations.  It occurs because the arithmetic performed in a machine involves numbers with only a finite number of digits.

??? note "Example"
    Suppose we want to approximate the value of $\pi$ using the following infinite series:
    
    $$
    \pi = 4 \arctan 1 = 4 \sum\limits_{n=0}^{+\infty} \frac{(-1)^n}{2n+1}
    $$

    In computer, we can't sum up infinitely many terms, so we need to use a finite summation. A simple way to do this is to discard the terms with a large index, i.e., $R_n = 4 \sum\limits_{k=n+1}^{+\infty} \frac{(-1)^k}{2k+1}$. And $R_n$ is the **truncation error** of the series.

    After discard $R_n$, we get $\pi \approx 4 \sum\limits_{k=0}^{n} \frac{(-1)^k}{2k+1}$. During the summation, each term will be replaced by its floating-point approximation, resulting in **roundoff errors**.

Suppose there's a real number $\overline{0.d_1d_2d_3 \cdots d_k d_{k+1} d_{k+2} \cdots} \times 10^n$ ,We have two ways to terminate the real number at $k$ decimal digits:

1. **chopping**: simply chops of the extra digits, i.e., $\overline{0.d_1d_2d_3 \cdots d_k} \times 10^n$.
2. **rounding**: adds $5 \times 10^{-(k+1)}$ to the number and chops off the extra digits.

Generally, we use the function $fl(x)$ to represent the floating-point approximation of $x$.

**Notation**: 
- The error of chopping may not be bigger than the error of rounding.
- In each calculation step, we will do the chopping or rounding to meet the required precision.

### Evaluation of errors

If $p^*$ is an approximation to $p$ ( $p \neq 0$ ), we have:

- **Absolute error**: $|p^* - p|$
- **Relative error**: $\dfrac{|p^* - p|}{|p|}$

!!! note "Definition :: significant digits"
    The number $p^*$ is said to approximate $p$ to $t$ significant digits (or figures) if $t$ is the largest nonnegative integer for which

    $$
    \frac{|p^* - p|}{|p|} < 5 \times 10^{-t}
    $$

### affect of roundoff errors

- Subtraction of nearly equal numbers: cancellation of significant digits
- Dividing by a number with small magnitude: enlargement of the error

So we need to simplify the expression as much as possible before performing numerical calculations to avoid roundoff errors.

??? note "Example"
    Considering equation $x^2 + 62.10 x + 1 = 0$, its smaller root is $\dfrac{-b + \sqrt{b^2 - 4ac}}{2a}$, where $a=1$, $b=62.10$, and $c=1$.

    Since $b$ is relatively larger than $a$ and $c$, $\sqrt{b^2 - 4ac}$ will be close to $b$, resulting in a subtraction of nearly equal numbers.

    To avoid this, we can rationalize the numerator:

    $$
        \frac{-b + \sqrt{b^2 - 4ac}}{2a} = \frac{4ac}{2a(-b - \sqrt{b^2 - 4ac})} = \frac{-2c}{b + \sqrt{b^2 - 4ac}}
    $$

## Algorithms and Convergence

### Stability of an algorithm

- **Stable**: An algorithm that satisfies that small changes in the initial data produce corresponding small changes in the final results is called stable, otherwise it is **unstable**.
- **Conditionally stable**: An algorithm is conditionally stable if it is stable only for certain choices of initial data.

### Evaluation of stability

We use the growth of error to evaluate the stability of an algorithm.

If $E_n$ is the error of the $n$-th iteration of the algorithm, and $E_0$ is the error of the initial data, then we have:

- **Linear growth**: $E_n \approx C \cdot nE_0$.

    Linear growth of error is usually unavoidable and generally acceptable.

- **Exponential growth**: $E_n \approx C^n \cdot E_0$.

    Exponential growth of error usually indicates that the algorithm is unstable, and may cause very large errors, which is unacceptable.
